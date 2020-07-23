# SwipeRefreshLayout

SwipeRefreshLayout作为谷歌官方推荐的下拉刷新控件，同时简单而又不失优雅的风格，让许多app都使用了这一控件，今天记录下SwipeRefreshLayout在项目中的实际运用。
 首先，我们在布局文件中使用：

```xml
     <android.support.v4.widget.SwipeRefreshLayout
            android:id="@+id/swipeRefreshLayout"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            >
        <ListView
            android:id="@+id/list"
            android:layout_width="match_parent"
            android:layout_height="match_parent">

        </ListView>
        </android.support.v4.widget.SwipeRefreshLayout>
```

一般来说SwipeRefreshLayout中嵌套一个listview或是recyclerview即可。

### 设置SwipeRefreshLayout 的颜色

在res/values/color中定义自己需要的颜色

```objectivec
     <color name="blue">#5BC0DE</color>
     <color name="red">#FF4081</color>
     <color name="black">#000000</color>
```

然后在java代码中设置颜色：

```css
swipeRefreshLayout.setColorSchemeResources(R.color.blue);
```

### 设置SwipeRefreshLayout 下拉刷新功能的实现

在SwipeRefreshLayout 的回调方法中编写。

```java
swipeRefreshLayout.setOnRefreshListener(new SwipeRefreshLayout.OnRefreshListener() {
           @Override
           public void onRefresh() {
                 //这里获取数据的逻辑
               swipeRefreshLayout.setRefreshing(false);
           }
       });
```

swipeRefreshLayout.setRefreshing(false)这句话传入一个布尔变量，false代表停止执行，这样，当我们执行完毕获取数据的过程后，就可以将一直转的下拉动画给取消掉啦，而且呢， swipeRefreshLayout.setRefreshing(）这个方法也可以实现第一次打开页面自动下拉刷新的逻辑，具体实现请问度娘。

### SwipeRefreshLayout 的其他几个方法

```java
//设置进度View样式的大小，只有两个值DEFAULT和LARGE，表示默认和较大
swipeRefreshLayout.setSize(DEFAULT);
//设置触发下拉刷新的距离
swipeRefreshLayout.setDistanceToTriggerSync(300);
//设置动画样式下拉的起始点和结束点，scale 是指设置是否需要放大或者缩小动画。
swipeRefreshLayout.setProgressViewOffset(boolean scale, int start, int end)
//设置动画样式下拉的结束点，scale 是指设置是否需要放大或者缩小动画
swipeRefreshLayout.setProgressViewEndTarget(boolean scale, int end);
//如果自定义了swipeRefreshLayout，可以通过这个回调方法决定是否可以滑动。
setOnChildScrollUpCallback(@Nullable OnChildScrollUpCallback callback)
```

