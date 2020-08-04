# RxJava

### 1. GitHub地址

https://github.com/ReactiveX/RxJava

注意，这里指的RxJava并不是androidx.ui.rxjava2和androidx.paging.rxjava2

### 2. 简介

```
RxJava：a library for composing asynchronous and event-based programs using observable sequences for the Java VM
```
 翻译：RxJava 是一个在 Java VM 上使用可观测的序列来组成异步的、基于事件的程序的库

总之就是说RxJava 是一个 **基于事件流、实现异步操作**的库

详细学习请见：

https://www.jianshu.com/nb/14302692

### 3. 作用和特点

作用：实现异步操作（类似Handler和AsynTask）

特点：RxJava使用方式是基于事件流的链式调用，优点如下。随着代码逻辑变复杂，依然可读性强，好用。

- 逻辑简洁
- 实现优雅
- 使用简单

### 4. 使用方式

- 引入：implementation "io.reactivex.rxjava3:rxjava:3.0.5"

基于事件流的链式调用方式：

```java
public class MainActivity extends AppCompatActivity {

    private static final String TAG = "Rxjava";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

// RxJava的流式操作
        Observable.create(new ObservableOnSubscribe<Integer>() {
        // 1. 创建被观察者 & 生产事件
            @Override
            public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
                emitter.onNext(1);
                emitter.onNext(2);
                emitter.onNext(3);
                emitter.onComplete();
            }
        }).subscribe(new Observer<Integer>() {
            // 2. 通过通过订阅（subscribe）连接观察者和被观察者
            // 3. 创建观察者 & 定义响应事件的行为
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "开始采用subscribe连接");
            }
            // 默认最先调用复写的 onSubscribe（）

            @Override
            public void onNext(Integer value) {
                Log.d(TAG, "对Next事件"+ value +"作出响应"  );
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "对Error事件作出响应");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "对Complete事件作出响应");
            }

        });
    }
}
```

### 5.总结

`RxJava`原理可总结为：

- 被观察者 `（Observable）`  通过 订阅`（Subscribe）` **按顺序发送事件** 给观察者 `（Observer）`
- 观察者`（Observer）` **按顺序接收事件** & 作出对应的响应动作。具体如下图：