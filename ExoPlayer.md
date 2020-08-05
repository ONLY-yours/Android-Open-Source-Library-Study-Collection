# 视频播放ExoPlayer

### GitHub

https://github.com/google/ExoPlayer

### 1. 介绍

ExoPlayer是一款适用于Android的应用程序级媒体播放器。它为Android的MediaPlayer API提供了一个替代方案，可以在本地和互联网上播放音频和视频。ExoPlayer支持Android的MediaPlayer API目前不支持的功能，包括DASH和SmoothStreaming自适应回放。与MediaPlayer API不同，ExoPlayer易于定制和扩展，并且可以通过Play Store应用程序更新进行更新。

### 2. 简单使用

相比于原生的videoview，非常重要的一点就是播放播放的资源不是直接通过videoview.setUri（）方法直接实现。exoplayer有一个专门管理播放资源的东西MediaSource。

##### 1 导入依赖

最新的依赖版本请见GitHub，演示采用2.11.7版本

```java
//exoplayer
implementation 'com.google.android.exoplayer:exoplayer:2.11.7'
```

##### 2 编写界面

```xml
<com.google.android.exoplayer2.ui.PlayerView
    android:id="@+id/exo_playerview"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginBottom="40dp"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"
    app:layout_constraintTop_toTopOf="parent"
    app:layout_constraintBottom_toTopOf="@id/playercontrol"/>
```

##### 3 设置播放（最简单）

- 1. 设置player参数，使用SimpleExoPlayer
- 2. 设置MediaSource播放资源
- 3. 为playerview设置资源

```java
// 1.设置player参数
private SimpleExoPlayer player;
 /** The scheme part of a raw resource URI. */
public static final String RAW_RESOURCE_SCHEME = "rawresource";


private void initializePlayer() {
        if (player==null){
            player = ExoPlayerFactory.newSimpleInstance(this);
            exoPlayerView.setPlayer(player);					//这个exoPlayer就是界面上的
            //设置播放（准备好立刻播放）
            player.setPlayWhenReady(playWhenReady);
            player.seekTo(currentWindow, playbackPosition);
        }
    
    
    // Produces DataSource instances through which media data is loaded.
        DataSource.Factory dataSourceFactory = new DefaultDataSourceFactory(this,
                Util.getUserAgent(this, String.valueOf(getApplication())));
        // This is the MediaSource representing the media to be played.
        ExtractorsFactory extractorsFactory = new DefaultExtractorsFactory();
		
    	// 2.设置MediaSource播放资源
        MediaSource videoSource = new ExtractorMediaSource(Uri.parse(Uri.parse(RAW_RESOURCE_SCHEME + ":///" + R.raw.media_test).toString()),
                dataSourceFactory, extractorsFactory, null, null);
        // Prepare the player with the source.
    
        // 3.为playerview设置资源
        player.prepare(videoSource);
}
```

### 3. 总结

ExoPlayer定制化其实很高，可以像上面那样简单实现，也可以个性化定制，具体可见源码。

同时也有大佬为这个东西写了一个简单的封装工具类：

https://www.jianshu.com/p/547dc4a8ebe4

```java
public class ExoPlayerManger {
    private static final String TAG = "ExoPlayerManger";
    private Context mContext;
    private BandwidthMeter bandwidthMeter = new DefaultBandwidthMeter();
    // 创建轨道选择工厂
    private TrackSelection.Factory videoTrackSelectionFactory = new AdaptiveTrackSelection.Factory(bandwidthMeter);
    // 创建轨道选择器实例
    private TrackSelector trackSelector = new DefaultTrackSelector(videoTrackSelectionFactory);
    private SimpleExoPlayer simpleExoPlayer;
    private DataSource.Factory dataSourceFactory;
    private String mVideoUrl;
    private SimpleCache simpleCache;
    private Uri playVideoUri;
    private ExtractorMediaSource mediaSource;


    /**
     * @param context 传入context
     */
    public void setBuilderContext(Context context) {
        mContext = context;
        dataSourceFactory = new DefaultDataSourceFactory(mContext, "seyed");
    }

    /**
     * @param videoUrl 传入视频路径
     */
    public void setVideoUrl(String videoUrl) {
        this.mVideoUrl = videoUrl;
        simpleCache = VideoCache.getInstance(mContext);
        playVideoUri = Uri.parse(mVideoUrl);
    }


    /**
     * @return 返回exoPlayer对象
     */
    public SimpleExoPlayer create() {
        try {
            simpleExoPlayer = ExoPlayerFactory.newSimpleInstance(mContext, trackSelector);
            dataSourceFactory = new CacheDataSourceFactory(simpleCache, dataSourceFactory);
            mediaSource = new ExtractorMediaSource.Factory(dataSourceFactory).createMediaSource(playVideoUri);
            simpleExoPlayer.prepare(mediaSource);

        } catch (Exception e) {

        }
        return simpleExoPlayer;
    }


}
```

```java
public class VideoCache {
    private static SimpleCache sDownloadCache;

    /**
     * @param context
     * @return
     */
    public static SimpleCache getInstance(Context context) {
        if (sDownloadCache == null) {
            sDownloadCache = new SimpleCache(new File(getMediaCacheFile(context), "StoryCache"), new LeastRecentlyUsedCacheEvictor(512 * 1024 *1024));

        }
        return sDownloadCache;
    }

    public static File getMediaCacheFile(Context context) {
        String directoryPath = "";
        String childPath = "exoPlayer";
        if (Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED)) {
            // 外部储存可用
            directoryPath = File.separator + context.getExternalFilesDir(childPath).getAbsolutePath();
        } else {
            directoryPath = File.separator + context.getFilesDir().getAbsolutePath() + File.separator + childPath;
        }
        File file = new File(directoryPath);
        //判断文件目录是否存在
        if (!file.exists()) {
            file.mkdirs();
        }

        return file;
    }


}
```

使用如下：

```java
		ExoPlayerManger exoPlayerManger = new ExoPlayerManger();
        exoPlayerManger.setBuilderContext(this);
        //设置Uri
		// exoPlayerManger.setVideoUrl(playVideoUrl);
        //设置从raw下读取的文件路径
        exoPlayerManger.setVideoUrl(RawResourceDataSource.buildRawResourceUri(R.raw.media_test).toString());


        SimpleExoPlayer simpleExoPlayer = exoPlayerManger.create();
        //设置音量
        simpleExoPlayer.setVolume(10);
        simpleExoPlayer.setVolume(0);
		// simpleExoPlayer.setRepeatMode(1);

        playerView.setPlayer(simpleExoPlayer);
        //监听（可自定义拓展）
		//simpleExoPlayer.addListener(this);
        //开启播放
        simpleExoPlayer.setPlayWhenReady(true);
```

