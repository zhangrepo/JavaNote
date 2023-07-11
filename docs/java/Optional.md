#### 创建

Optional类的实例创建有三种方式：

-   Optional.empty()：		创建一个空的 Optional 实例。
-   Optional.of(T t) ：		创建一个 Optional 实例，当 t为null时抛出异常（NullPointerException）。
-   Optional.ofNullable(T t) ：	创建一个 Optional 实例，但当 t为null时不会抛出异常，而是返回一个空的实例。

#### 获取

-   get()：获取optional实例中的对象，当optional 容器为空时报错。

#### 判断

-   isPresent()：判断optional是否为空，如果空则返回false，否则返回true
-   ifPresent(Consumer c)：如果optional不为空，则将optional中的对象传给Comsumer函数



#### orElse与orElseGet区别

#### 实际应用

**例一**

在函数方法中

以前写法

```java
public String getCity(User user) throws Exception {
  if (user != null) {
    if (user.getAddress() != null) {
      Address address = user.getAddress();
      if (address.getCity() != null) {
        return address.getCity();
      }
    }
  }
  throw new Exception("取值错误");
}
```

**JAVA8写法**

```java
public String getCity(User user) throws Exception{
  return Optional.ofNullable(user)
    .map(u-> u.getAddress())
    .map(a->a.getCity())
    .orElseThrow(()->new Exception("取指错误"));
}
```

**例二**

比如，在主程序中

以前写法

```java
if (user != null) {
    dosomething(user);
}
```

JAVA8写法

```java
Optional.ofNullable(user)
  .ifPresent(u -> {
    dosomething(u);
  });
```



例三

以前写法

```java
public User getUser(User user) throws Exception{
    if(user!=null){
        String name = user.getName();
        if("zhangsan".equals(name)){
            return user;
        }
    }else{
        user = new User();
        user.setName("zhangsan");
        return user;
    }
}
```

java8写法

```java
public User getUser(User user) {
    return Optional.ofNullable(user)
                   .filter(u->"zhangsan".equals(u.getName()))
                   .orElseGet(()-> {
                        User user1 = new User();
                        user1.setName("zhangsan");
                        return user1;
                   });
}
```
