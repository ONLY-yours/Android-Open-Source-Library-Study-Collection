# EventBus

### 1. 简介

EventBus是一种用于Android的事件发布-订阅总线，是一个异步通信策略。它简化了应用程序内各个组件之间进行通信的复杂度，尤其是碎片之间进行通信的问题，可以避免由于使用广播通信而带来的诸多不便。

虽然EventBus非常好用，但是还是有缺陷的，使用过多的订阅会导致逻辑跳转不清晰，从而增加阅读成本。

GitHub：

https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fgreenrobot%2FEventBus

### 2. 加入依赖

- Gradle

  ```
  implementation 'org.greenrobot:eventbus:3.2.0'
  ```

- Maven

  ```
  <dependency>
      <groupId>org.greenrobot</groupId>
      <artifactId>eventbus</artifactId>
      <version>3.2.0</version>
  </dependency>
  ```

### 3. 使用

分三步走

1. 定义通信主题：

```
public static class MessageEvent { /* Additional fields if needed */ }
```

2. 在需要接收的地方注册、反注册EventBus，并编写接收事件

```java
 @Override
 public void onStart() {
     super.onStart();
     EventBus.getDefault().register(this);
 }

 @Override
 public void onStop() {
     super.onStop();
     EventBus.getDefault().unregister(this);
 }
 
 @Subscribe(threadMode = ThreadMode.MAIN)  
public void onMessageEvent(MessageEvent event) {/* Do something */};
```

3. 发送事件

```java
EventBus.getDefault().post(new MessageEvent());
```

### 4. 逻辑分析

代码通过post事件将一个MessageEvent主体发送，系统去寻找接收MessageEvent的方法，然后执行里面的方法。

```java
 @Subscribe(threadMode = ThreadMode.MAIN)  
```

- 四种模式

  这串代码通过注释的方式指定运行的模式，有以下四种模式：

  **POSTING**：默认，表示事件处理函数的线程跟发布事件的线程在同一个线程。

  **MAIN**：表示事件处理函数的线程在主线程(UI)线程，因此在这里不能进行耗时操作。

  **BACKGROUND**：表示事件处理函数的线程在后台线程，因此不能进行UI操作。如果发布事件的线程是主线程(UI线程)，那么事件处理函数将会开启一个后台线程，如果果发布事件的线程是在后台线程，那么事件处理函数就使用该线程。

  **ASYNC**：表示无论事件发布的线程是哪一个，事件处理函数始终会新建一个子线程运行，同样不能进行UI操作。

- 三个角色

  **Event**：事件，它可以是任意类型，EventBus会根据事件类型进行全局的通知。

  **Subscriber**：事件订阅者，在EventBus 3.0之前我们必须定义以onEvent开头的那几个方法，分别是`onEvent`、`onEventMainThread`、`onEventBackgroundThread`和`onEventAsync`，而在3.0之后事件处理的方法名可以随意取，不过需要加上注解`@subscribe`，并且指定线程模型，默认是`POSTING`。

  **Publisher**：事件的发布者，可以在任意线程里发布事件。一般情况下，使用`EventBus.getDefault()`就可以得到一个EventBus对象，然后再调用`post(Object)`方法即可。

