---
layout: post
title:  "【Android】动态代理在 Retrofit 中的使用"
date:   2018-12-24 15:02:36
categories: Android
---

首先，什么是动态代理和为什么会有动态代理。

众所周知，Java 是一门静态语言，编写完的类，无法在运行时做动态修改。

一个简单的动态代理如下：

1、先定义一个接口，想要使用动态代理，必须先定义一个接口：

```java
public interface IHello{
    void hello();
}
```
2、再让想要动态代理的类实现接口：
```java
public class Hello implements IHello{
    public void hello(){
        System.out.print("hello");
    }
}
```
3、再定义一个实现`InvocationHandler`的类：
```java
public class MyInvocationHandler implements InvocationHandler{
    private Object obj;
    public MyInvocationHandler(Object obj){
        this.obj = obj;
    }
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable{
        //在原方法前边动态插入代码
        Object result = method.invoke(obj, args);
        //在原方法后边动态插入代码
        return result;
    }
}
```
`InvocationHandler`是 Java 提供的动态代理实现类，想要使用动态代理，必须实现该接口，重写`invoke`方法，并酌情在原方法的前边或后边插入需要的代码，`method.invoke(target, args)`会调用原方法（比如 `Hello` 类中的 `hello()`方法）。

上边就是一个简单的动态代理的例子。

Retrofit 中也使用了动态代理，我们以官方提供的示例代码为例，首先声明一个接口：

```java
public interface GitHubService {
  @GET("users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user);
}
```

然后在需要网络请求的地方调用如下代码：

```java
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com/")
    .build();

GitHubService service = retrofit.create(GitHubService.class);

Call<List<Repo>> repos = service.listRepos("octocat");
```

上边定义的`GithubService`等同于前文的`IHello`接口，一个类想要做动态代理，就必须先要定义一个接口。

然后关键代码是`retrofit`，表面上看这与上文提到的写法完全对应不上，其实这里只是封装起来了，查看它的源码就能发现端倪。

```java
public <T> T create(final Class<T> service) {
    Utils.validateServiceInterface(service);
    if (validateEagerly) {
      eagerlyValidateMethods(service);
    }
    return (T) Proxy.newProxyInstance(service.getClassLoader(), new Class<?>[] { service },
        new InvocationHandler() {
          private final Platform platform = Platform.get();

          @Override public Object invoke(Object proxy, Method method, @Nullable Object[] args)
              throws Throwable {
            // If the method is a method from Object then defer to normal invocation.
            if (method.getDeclaringClass() == Object.class) {
              return method.invoke(this, args);
            }
            if (platform.isDefaultMethod(method)) {
              return platform.invokeDefaultMethod(method, service, proxy, args);
            }
            ServiceMethod<Object, Object> serviceMethod =
                (ServiceMethod<Object, Object>) loadServiceMethod(method);
            OkHttpCall<Object> okHttpCall = new OkHttpCall<>(serviceMethod, args);
            return serviceMethod.adapt(okHttpCall);
          }
        });
  }
```

  看到`new InvocationHandler(){****}`了吧，等于变相实现了`InvocationHandler`，另外，`Proxy.newProxyInstance`最终会调用一个 native 方法，动态生成一个 `GitHubService`（我们前文声明的接口）的实现类，大致长这样(这是 Java 动态生成的，所以以下代码并不完全准确)：

```java
  public final class $Proxy0 extends Proxy implements GitHubService {  
    private static Method m1;  
    private static Method m0;  
    private static Method m3;  
    private static Method m2;  
  
    static {  
        try {  
            m1 = Class.forName("java.lang.Object").getMethod("equals",  
                    new Class[] { Class.forName("java.lang.Object") });  
  
            m0 = Class.forName("java.lang.Object").getMethod("hashCode",  
                    new Class[0]);  
  
            m3 = Class.forName("***.GitHubService").getMethod("request",  
                    new Class[0]);  
  
            m2 = Class.forName("java.lang.Object").getMethod("toString",  
                    new Class[0]);  
  
        } catch (NoSuchMethodException nosuchmethodexception) {  
            throw new NoSuchMethodError(nosuchmethodexception.getMessage());  
        } catch (ClassNotFoundException classnotfoundexception) {  
            throw new NoClassDefFoundError(classnotfoundexception.getMessage());  
        }  
    }
  
    public $Proxy0(InvocationHandler invocationhandler) {  
        super(invocationhandler);  
    }  
  
    @Override  
    public final boolean equals(Object obj) {  
        try {  
            return ((Boolean) super.h.invoke(this, m1, new Object[] { obj })) .booleanValue();  
        } catch (Throwable throwable) {  
            throw new UndeclaredThrowableException(throwable);  
        }  
    }  
  
    @Override  
    public final int hashCode() {  
        try {  
            return ((Integer) super.h.invoke(this, m0, null)).intValue();  
        } catch (Throwable throwable) {  
            throw new UndeclaredThrowableException(throwable);  
        }  
    }  
  
    public final void listRepos() {  
        try {  
            super.h.invoke(this, m3, null);  
            return;  
        } catch (Error e) {  
        } catch (Throwable throwable) {  
            throw new UndeclaredThrowableException(throwable);  
        }  
    }  
  
    @Override  
    public final String toString() {  
        try {  
            return (String) super.h.invoke(this, m2, null);  
        } catch (Throwable throwable) {  
            throw new UndeclaredThrowableException(throwable);  
        }  
    }  
} 
```
可以发现 Java 帮我们生成的类中重写了我们`GithubService`接口的`listRepos()`，该方法中会调用`super.h.invoke(this, m3, null);`，就是调用父类的`h`的`invoke()`，它的父类是`Proxy`，`h`是一个`InvocationHandler`对象；
```java
/**
* the invocation handler for this proxy instance.
* @serial
*/
protected InvocationHandler h;
```
这个`h`，就是上文提到的`new InvocationHandler(){}`；

```java
public <T> T create(final Class<T> service) {
    Utils.validateServiceInterface(service);
    if (validateEagerly) {
      eagerlyValidateMethods(service);
    }
    return (T) Proxy.newProxyInstance(service.getClassLoader(), new Class<?>[] { service },
        new InvocationHandler() {
          private final Platform platform = Platform.get();

          @Override public Object invoke(Object proxy, Method method, @Nullable Object[] args)
              throws Throwable {
            // If the method is a method from Object then defer to normal invocation.
            if (method.getDeclaringClass() == Object.class) {
              return method.invoke(this, args);
            }
            if (platform.isDefaultMethod(method)) {
              return platform.invokeDefaultMethod(method, service, proxy, args);
            }
            ServiceMethod<Object, Object> serviceMethod =
                (ServiceMethod<Object, Object>) loadServiceMethod(method);
            OkHttpCall<Object> okHttpCall = new OkHttpCall<>(serviceMethod, args);
            return serviceMethod.adapt(okHttpCall);
          }
        });
  }
```
最后，简单画一个图总结下流程：

![retrofit.jpg](https://upload-images.jianshu.io/upload_images/782269-fb29059f8e16d0fe.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
