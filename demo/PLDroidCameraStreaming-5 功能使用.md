- 5 [功能使用](#5)
    - 5.1 [摄像头参数配置](#5.1)
	    - 5.1.1 [前置／后置摄像头配置](#5.1.1)
	    - 5.1.2 [Camera 对焦相关](#5.1.2)
	    - 5.1.3 [Camera 预览 size](#5.1.3)
	    - 5.1.4 [RecordingHint](#5.1.4)
	    - 5.1.5 [Encoding Mirror](#5.1.5)
    - 5.2 [麦克风参数配置](#5.2)
	    - 5.2.1 [是否开启蓝牙麦克风支持](#5.2.1)
    - 5.3 [推流参数设置](#5.3)
	    - 5.3.1 [推流地址配置](#5.3.1)
	    - 5.3.2 [ VideoQuality](#5.3.2)
	    - 5.3.3 [AudioQuality](#5.3.3)
	    - 5.3.4 [AVProfile](#5.3.4)
	    - 5.3.5 [HappyDns 支持](#5.3.5)
	    - 5.3.6 [软编的 EncoderRCModes](#5.3.6)
	    - 5.3.7 [Encoding size 的设定](#5.3.7)
	    - 5.3.8 [StreamStatus 反馈配置](#5.3.8)
	    - 5.3.9 [动态更改 Orientation](#5.3.9)
	- 5.4 [水印设置](#5.4)
		 - 5.4.1 [水印位置信息](#5.4.1)
		 - 5.4.2 [水印显示大小](#5.4.2)
		 - 5.4.3 [构造WatermarkSetting](#5.4.3)
	- 5.5 [核心类 CameraStreamingManager](#5.5)
		 - 5.5.1 [构造 CameraStreamingManager](#5.5.1)
		 - 5.5.2 [设置 Listener](#5.5.2)
		 - 5.5.3 [resume](#5.5.3)
		 - 5.5.4 [开始推流](#5.5.4)
		 - 5.5.5 [手动对焦](#5.5.5)
		 - 5.5.6 [Zoom](#5.5.6)
		 - 5.5.7 [闪光灯操作](#5.5.7)
		 - 5.5.8 [切换摄像头](#5.5.8)
		 - 5.5.9 [禁音推流](#5.5.9)
		 - 5.5.10 [截帧](#5.5.10)
		 - 5.5.11 [停止推流](#5.5.11)
		 - 5.5.12 [Log 管理](#5.5.12)
		 - 5.5.13 [pause](#5.5.13)
		 - 5.5.14 [destroy](#5.5.14)
	- 5.6 [视频滤镜渲染](#5.6)
		 - 5.6.1 [软编模式滤镜实现](#5.6.1)
		 - 5.6.2 [硬编模式滤镜实现](#5.6.2)

<a name="5"></a>
#5 功能使用

<a name="5.1"></a>
##5.1 摄像头参数配置

所有摄像头相关的配置，都在 CameraStreamingSetting 类中进行。

<a name="5.1.1"></a>
###5.1.1 前置／后置摄像头配置

前后置摄像头配置：

``` java
mCameraStreamingSetting.setCameraId(Camera.CameraInfo.CAMERA_FACING_FRONT); // 前置摄像头
mCameraStreamingSetting.setCameraId(Camera.CameraInfo.CAMERA_FACING_BACK); // 后置摄像头
```
注：默认 `CAMERA_FACING_BACK`

若设定的摄像头打开失败，将会回调 `CameraStreamingManager.STATE.OPEN_CAMERA_FAIL` .

<a name="5.1.2"></a>
###5.1.2 Camera 对焦相关
* `setContinuousFocusModeEnabled`
若希望关闭自动对焦功能，可以：

``` java
mCameraStreamingSetting.setContinuousFocusModeEnabled(false);
```
注：默认开启

* 通过 `setFocusMode` 设置对焦模式

您可以设置不同的对焦模式，目前 SDK 支持的对焦模式有:

``` java
- CameraStreamingSetting.FOCUS_MODE_CONTINUOUS_PICTURE // 自动对焦（Picture）
- CameraStreamingSetting.FOCUS_MODE_CONTINUOUS_VIDEO   // 自动对焦（Video）
- CameraStreamingSetting.FOCUS_MODE_AUTO               // 手动对焦
```

`FOCUS_MODE_CONTINUOUS_PICTURE` 与 `FOCUS_MODE_CONTINUOUS_VIDEO` 最大的区别在于，`FOCUS_MODE_CONTINUOUS_PICTURE` 对焦会比 `FOCUS_MODE_CONTINUOUS_VIDEO` 更加频繁，功耗会更高，建议使用 `FOCUS_MODE_CONTINUOUS_VIDEO`。

可以通过 `CameraStreamingSetting#setFocusMode` 来设置不同的对焦模式：

``` java
mCameraStreamingSetting.setFocusMode(CameraStreamingSetting.FOCUS_MODE_CONTINUOUS_PICTURE);
mCameraStreamingSetting.setFocusMode(CameraStreamingSetting.FOCUS_MODE_CONTINUOUS_VIDEO);
mCameraStreamingSetting.setFocusMode(CameraStreamingSetting.FOCUS_MODE_AUTO);
```
注：默认值为 `CameraStreamingSetting.FOCUS_MODE_CONTINUOUS_VIDEO`

* `setResetTouchFocusDelayInMs`
当对焦模式处理 `CameraStreamingSetting.FOCUS_MODE_AUTO`，并触发了手动对焦之后，若希望隔一段时间恢复到自动对焦模式，就可以调用：

``` java
mCameraStreamingSetting.setResetTouchFocusDelayInMs(3000); // 单位毫秒
```
注：默认值为 3000 ms

<a name="5.1.3"></a>
###5.1.3 Camera 预览 size
为了兼容更多的 Camera ，SDK 采用了相关的措施：

- 使用 `PREVIEW_SIZE_LEVEL` 和 `PREVIEW_SIZE_RATIO` 共同确定一个 Preview Size.
- 用户也可以通过 `StreamingSessionListener#onPreviewSizeSelected` 自定义选择一个合适的 Preview Size，`onPreviewSizeSelected` 的参数 list 是 Camera 系统支持的 preview size 列表（Camera.Parameters#getSupportedPreviewSizes()），SDK 内部会对 list 做从小到大的排序，可以安全地使用 list 中的 preview size。如果 `onPreviewSizeSelected` 返回为 null，代表放弃自定义选择，那么 SDK 会使用前面的策略选择一个合适的 Preview Size，否则使用 `onPreviewSizeSelected` 的返回值。

SDK 目前分别支持的 `PREVIEW_SIZE_LEVEL` 和 `PREVIEW_SIZE_RATIO` 分别是：
- SMALL
- MEDIUM
- LARGE

- RATIO_4_3
- RATIO_16_9

需要注意的是：在某些机型上，list 可能为 null。

<a name="5.1.4"></a>
###5.1.4 RecordingHint
可以通过 CameraStreamingSetting 对象设置 Recording hint，以此来提升数据源的帧率。

``` java
CameraStreamingSetting setting = new CameraStreamingSetting();
setting.setRecordingHint(false);
```

需要注意的是，在部分机型开启 Recording Hint 之后，会出现画面卡帧等风险，所以请慎用该 API。如果需要实现高 fps 推流，可以考虑开启并加入白名单机制。

<a name="5.1.5"></a>
###5.1.5 Encoding Mirror
对于有对前置摄像头进行 Mirror 操作的用户来说，只需通过 `CameraStreamingSetting#setFrontCameraMirror` 设置即可。该操作目前仅针对播放端有效。可以避免前置摄像头拍摄字体镜像反转问题。

``` java
boolean isMirror = xxx; // false as default
mCameraStreamingSetting.setFrontCameraMirror(isMirror);
```
<a name="5.2"></a>
##5.2 麦克风参数配置

所有麦克风相关的配置，都在 `MicrophoneStreamingSetting` 类中进行。

<a name="5.2.1"></a>
###5.2.1 是否开启蓝牙麦克风支持
若希望增加蓝牙麦克风的支持，需要：

``` java
mMicrophoneStreamingSetting.setBluetoothSCOEnabled(true);
```
注：默认值为 false

<a name="5.3"></a>
##5.3 推流参数设置
所有推流相关的参数配置，都在 `StreamingProfile` 类中进行。

<a name="5.3.1"></a>
###5.3.1 推流地址配置
streamJsonStrFromServer 是由服务端返回的一段 JSON String，该 JSON String 描述了 Stream 的结构。通常，您可以使用 Pili 服务端 SDK 的 getStream(streamId) 方法来获取一个 `Stream` 对象，在服务端并将该对象以 JSON String 格式输出，该输出即是 streamJsonStrFromServer 变量的内容。从业务服务器获取对应的 Stream 可参考 [MainActivity.java](https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/PLDroidCameraStreamingDemo/app/src/main/java/com/pili/pldroid/streaming/camera/demo/MainActivity.java) 中的 requestByHttpPost 方法。

``` java
String streamJsonStrFromServer = "stream json string from your server";

JSONObject streamJson = null;
try {
    streamJson = new JSONObject(streamJsonStrFromServer);
} catch (JSONException e) {
    e.printStackTrace();
}
mStreamingProfile.setStream(new Stream(streamJson));
```

设置 Stream 只需要保证在 `CameraStreamingManager#startStreaming()` 之前调用即可，SDK 接受以下调用流程：
``` java
mStreamingProfile.setStream(new Stream(mJSONObject1));
mCameraStreamingManager.setStreamingProfile(mProfile);
mCameraStreamingManager.startStreaming();
```

<a name="5.3.2"></a>
###5.3.2 VideoQuality
VideoQuality 是对视频质量相关参数的一个抽象，它的值对应的参数表如下：

| Level | Fps | Video Bitrate(Kbps) |
|---|---|---|
|VIDEO_QUALITY_LOW1|12|150|
|VIDEO_QUALITY_LOW2|15|264|
|VIDEO_QUALITY_LOW3|15|350|
|VIDEO_QUALITY_MEDIUM1|30|512|
|VIDEO_QUALITY_MEDIUM2|30|800|
|VIDEO_QUALITY_MEDIUM3|30|1000|
|VIDEO_QUALITY_HIGH1|30|1200|
|VIDEO_QUALITY_HIGH2|30|1500|
|VIDEO_QUALITY_HIGH3|30|2000|

你只需要通过 `StreamingProfile#setVideoQuality` 进行设置即可，如：

``` java
mStreamingProfile.setVideoQuality(StreamingProfile.VIDEO_QUALITY_MEDIUM1);
```
其含义为，设置视频的 fps 为 30，码率为 512 kbps

<a name="5.3.3"></a>
###5.3.3 AudioQuality
AudioQuality 是对音频质量相关参数的一个抽象，它的值对应的参数表如下：

| Level | Audio Bitrate(Kbps) | Audio Sample Rate(Hz)|
|---|---|---|
|AUDIO_QUALITY_LOW1|18|44100|
|AUDIO_QUALITY_LOW2|24|44100|
|AUDIO_QUALITY_MEDIUM1|32|44100|
|AUDIO_QUALITY_MEDIUM2|48|44100|
|AUDIO_QUALITY_HIGH1|96|44100|
|AUDIO_QUALITY_HIGH2|128|44100|

你只需要通过 `StreamingProfile#setAudioQuality` 进行设置即可，如：

``` java
mStreamingProfile.setAudioQuality(StreamingProfile.AUDIO_QUALITY_HIGH1);
```
其含义为，设置音频的采样率为 44100 HZ，码率为 96 kbps。

<a name="5.3.4"></a>
###5.3.4 AVProfile
当需要自定义 video 的 fps/video bitrate/GOP size 或者 audio 的 sample rate/audio bitrate，可以通过设置 AVProfile。

``` java
// audio sample rate is 44100, audio bitrate is 96 * 1024 bps
StreamingProfile.AudioProfile aProfile = new StreamingProfile.AudioProfile(44100, 96 * 1024);

// fps is 30, video bitrate is 1000 * 1024 bps, maxKeyFrameInterval is 48
StreamingProfile.VideoProfile vProfile = new StreamingProfile.VideoProfile(30, 1000 * 1024, 48);

StreamingProfile.AVProfile avProfile = new StreamingProfile.AVProfile(vProfile, aProfile);

mStreamingProfile.setAVProfile(avProfile)
```
注：44100 是 Android 平台唯一保证所以设备支持的采样率，为了避免音频兼容性问题，建议设置为 44100。

`StreamingProfile#setAVProfile` 的优先级高于 Quality，也就是说，当同时调用了 Quality 和 AVProfile 的设置，AVProfile 会覆盖 Quality 的设置值，比如：

``` java
StreamingProfile.AudioProfile aProfile = new StreamingProfile.AudioProfile(44100, 48 * 1024);
StreamingProfile.VideoProfile vProfile = new StreamingProfile.VideoProfile(30, 1000 * 1024, 48);
StreamingProfile.AVProfile avProfile = new StreamingProfile.AVProfile(vProfile, aProfile);

mStreamingProfile.setAVProfile(avProfile)
                .setVideoQuality(StreamingProfile.VIDEO_QUALITY_MEDIUM1) // |30|512|90|
                .setAudioQuality(StreamingProfile.AUDIO_QUALITY_HIGH1);  // |96|44100|
```
最终设定的值应该为：
- 音频：48 * 1024, 44100
- 视频：30, 1000 * 1024, 48

<a name="5.3.5"></a>
###5.3.5 HappyDns 支持
为了防止 Dns 被劫持，SDK 加入了 HappyDns 支持，可以从[这里](https://github.com/qiniu/happy-dns-android)查阅源码。 从 v1.4.6 版本开始，需要在宿主项目中的 build.gradle 中加入如下语句：

```
dependencies {
    ...
    compile 'com.qiniu:happy-dns:0.2.7'
    ...
}
```

通过 `StreamingProfile` 设定自定义 `DnsManager`，如下：

``` java
public static DnsManager getMyDnsManager() {
      IResolver r0 = new DnspodFree();
      IResolver r1 = AndroidDnsServer.defaultResolver();
      IResolver r2 = null;
      try {
          r2 = new Resolver(InetAddress.getByName("119.29.29.29"));
      } catch (IOException ex) {
          ex.printStackTrace();
      }
      return new DnsManager(NetworkInfo.normal, new IResolver[]{r0, r1, r2});
  }


  StreamingProfile mProfile = new StreamingProfile();
  // Setting null explicitly, means give up {@link DnsManager} and access by the original host.
  mStreamingProfile.setDnsManager(getMyDnsManager()); // set your DnsManager
```

若显示地设置为 null，即：

``` java
mStreamingProfile.setDnsManager(null);
```
SDK 会使用系统 DNS 解析，而不会使用 `DnsManager` 来进行 Dns 解析。

若不调用 `StreamingProfile#setDnsManager` 方法，SDK 会默认的创建一个 `DnsManager` 来对 Dns 进行解析。

<a name="5.3.6"></a>
###5.3.6 软编的 EncoderRCModes
目前 RC mode 支持的类型：

- EncoderRCModes.QUALITY_PRIORITY: 质量优先，实际的码率可能高于设置的码率
- EncoderRCModes.BITRATE_PRIORITY: 码率优先，更精确地码率控制

可通过 StreamingProfile 的 setEncoderRCMode 方法进行设置，如下：

``` java
mStreamingProfile.setEncoderRCMode(StreamingProfile.EncoderRCModes.QUALITY_PRIORITY);
```

注：默认值为 `EncoderRCModes.QUALITY_PRIORITY`

<a name="5.3.7"></a>
###5.3.7 Encoding size 的设定
SDK 将 Preview size 和 Encoding size 已经分离，他们之间互不影响。Preview size 代表的是 Camera 本地预览的 size，encoding size 代表的是编码时候的 size，即播放端不做处理时候看到视频的 size。

有两种方式可以设置 Encoding size：
- 使用内置的 encoding size level：`StreamingProfile#setEncodingSizeLevel`
- 设定一个 encoding size 偏好值：`StreamingProfile#setPreferredVideoEncodingSize(int, int)`

SDK 会秉持一个原则，用户偏好值的优先级会高于内置的设置。若同时调用了上述 api，偏好设置会覆盖内置的 encoding size level。

内置的 Encoding size level 对应的分辨率：

| Level | Resolution(16:9) | Resolution(4:3)|
|---|---|---|
|VIDEO_ENCODING_HEIGHT_240|424 x 240|320 x 240|
|VIDEO_ENCODING_HEIGHT_480|848 x 480|640 x 480|
|VIDEO_ENCODING_HEIGHT_544|960 x 544|720 x 544|
|VIDEO_ENCODING_HEIGHT_720|1280 x 720|960 x 720|
|VIDEO_ENCODING_HEIGHT_1088|1920 x 1088|1440 x 1088|

<a name="5.3.8"></a>
###5.3.8 StreamStatus 反馈配置
对于推流信息的反馈，可以通过 `StreamingProfile#setStreamStatusConfig` 来设定其反馈的频率，

``` java
mStreamingProfile#setStreamStatusConfig(new StreamingProfile.StreamStatusConfig(3)); // 单位为秒
```

其含义为，若注册了 `mCameraStreamingManager.setStreamStatusCallback` ，每隔 3 秒回调 StreamStatus 信息。

<a name="5.3.9"></a>
###5.3.9 动态更改 Orientation
动态更改 Orientation，包括动态更改 Encoding Orientation 以及动态切换横竖屏。
* 动态更改 Encoding Orientation

更改的是编码后图像的方向，但需要重新推流才会生效；目前支持的 ENCODING_ORIENTATION 的类型有：PORT 和 LAND

用户不设置的情况下， Encoding Orientation 会依赖 Activity 的 Orientation，即有如下对应关系：
// 不调用 `setEncodingOrientation` 情况下，SDK 会默认根据如下关系进行设置 Encoding Orientation

| Activity Orientation | -> | Encoding Orientation |
|---|---|---|
|ActivityInfo.SCREEN_ORIENTATION_PORTRAIT|->|PORT|
|ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE|->|LAND|

设置 `ENCODING_ORIENTATION.PORT` 之后，播放端会观看竖屏的画面；
设置 `ENCODING_ORIENTATION.LAND` 之后，播放端会观看横屏的画面。

其设置方式如下：

``` java
mProfile.setEncodingOrientation(StreamingProfile.ENCODING_ORIENTATION.PORT);
mCameraStreamingManager.setStreamingProfile(mProfile); // notify CameraStreamingManager that StreamingProfile had been changed.
```

* 动态切换横竖屏
用户切换 Activity 方向后，相应地调整 Camera 的预览显示效果。
在更改了 Activity Orientation 之后，需要调用 `notifyActivityOrientationChanged` 通知 `CameraStreamingManager`。

比如：

``` java
setRequestedOrientation(isEncOrientationPort ? ActivityInfo.SCREEN_ORIENTATION_PORTRAIT : ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);
mCameraStreamingManager.notifyActivityOrientationChanged();
```

需要注意的是，为了防止 `setRequestedOrientation` 调用后 Activity 的重建，需要在 AndroidManifest.xml 里面配置对应的 Activity 的如下属性：

``` java
android:configChanges="orientation|screenSize"
```

可查看 Demo 中的 `.SWCodecCameraStreamingActivity` 的[配置](https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/PLDroidCameraStreamingDemo/app/src/main/AndroidManifest.xml)。

<a name="5.4"></a>
##5.4 水印设置
所有水印相关的配置，都在 `WatermarkSetting` 类中进行。

<a name="5.4.1"></a>
###5.4.1 水印位置信息
水印的位置信息，目前内置四个方位，如：

``` java
public enum WATERMARK_LOCATION {
    NORTH_WEST,
    NORTH_EAST,
    SOUTH_WEST,
    SOUTH_EAST,
}
```

分别在屏幕的位置如下图所示：

```
/**
 * define the relative location of watermark on the screen when start streaming
 *
 *    |  NorthWest     |                |     NorthEast
 *    |                |                |
 *    |                |                |
 *    |  --------------|----------------|--------------
 *    |                |                |
 *    |                |                |
 *    |                |                |
 *    |  --------------|----------------|--------------
 *    |                |                |
 *    |                |                |
 *    |   SouthWest    |                |     SouthEast
 *
 */
```

<a name="5.4.2"></a>
###5.4.2 水印显示大小
``` java
/**
 * define de relative size of watermark
 */
 public enum WATERMARK_SIZE {
     LARGE,
     MEDIUM,
     SMALL,
 }
```
<a name="5.4.3"></a>
###5.4.3 构造 `WatermarkSetting`
传入 drawable 对象作为水印资源：

``` java
WatermarkSetting watermarksetting = new WatermarkSetting(this, R.drawable.qiniu_logo, WatermarkSetting.WATERMARK_LOCATION.SOUTH_WEST, WatermarkSetting.WATERMARK_SIZE.MEDIUM, 100); // 100 为 alpha 值
```

传入图片的绝对路径作为水印资源：

``` java
WatermarkSetting watermarksetting = WatermarkSetting(this, "watermark resource absolute path", WatermarkSetting.WATERMARK_LOCATION.SOUTH_WEST, WatermarkSetting.WATERMARK_SIZE.MEDIUM, 100) {
```

<a name="5.5"></a>
##5.5 核心类 CameraStreamingManager
所有音视频推流相关的具体操作，都在 CameraStreamingManager 中进行。

<a name="5.5.1"></a>
###5.5.1 构造 CameraStreamingManager

在构造 CameraStreamingManager 阶段会确定其编码的类型，目前 SDK 支持的编码类型有：

``` java
- EncodingType.HW_VIDEO_WITH_HW_AUDIO_CODEC, // 视频硬编，音频硬编
- EncodingType.SW_VIDEO_WITH_HW_AUDIO_CODEC, // 视频软编，音频硬编
- EncodingType.SW_VIDEO_WITH_SW_AUDIO_CODEC, // 视频软编，音频软编
- EncodingType.SW_AUDIO_CODEC,               // 纯音频硬编
- EncodingType.HW_AUDIO_CODEC,               // 纯音频硬编
- EncodingType.SW_VIDEO_CODEC,               // 纯视频硬编
- EncodingType.HW_VIDEO_CODEC;               // 纯视频硬编
```

构造带有视频 CameraStremaingManager，需要传入 AspectFrameLayout 和 GLSurfaceView

``` java
mCameraStreamingManager = new CameraStreamingManager(this, afl, cameraPreviewFrameView,
                EncodingType.SW_VIDEO_WITH_SW_AUDIO_CODEC);
```

构造完毕后，需调用 `CameraStreamingManager#prepare` 向 SDK 提供对应的配置信息，以 v1.6.2 版本为例：

``` java
mCameraStreamingManager.prepare(mCameraStreamingSetting, mMicrophoneStreamingSetting, watermarksetting, mProfile);
```

<a name="5.5.2"></a>
###5.5.2 设置 Listener
为了更好的和 SDK 交互，接受各种状态和其他信息，需要注册对应的 Listener:

``` java
mCameraStreamingManager.setStreamingStateListener(this);
mCameraStreamingManager.setStreamingSessionListener(this);
mCameraStreamingManager.setStreamStatusCallback(this);
```

* StreamingStateListener
接口原型如下：

``` java
/**
 * Callback interface for Streaming State.
 * <p>
 *
 * Called on an "arbitrary thread".
 *
 * */
 public interface StreamingStateListener {
     /**
      * Invoked if the {@link STATE} changed
      *
      * @param status the specified {@link STATE}
      * @param extra the extra information
      * */
     void onStateChanged(int status, Object extra);

     @Deprecated
     boolean onStateHandled(int status, Object extra);
 }
```

onStateChanged 中 status 对应的含义分别为：

``` java
/**
 * STATE that notified by {@link CameraStreamingManager}.
 *
 * */
public static final class STATE {
    /**
     * The initial state.
     *
     * */
    public static final int UNKNOWN               = -1;

    /**
     * Preparing the environment for network connection.
     * <p>
     * The first state after calling {@link #startStreaming()}
     *
     * */
    public static final int PREPARING             = 0;

    /**
     * <ol>
     *     <li>{@link #resume()} done in pure audio streaming</li>
     *     <li>{@link #resume()} done and camera be activated in AV streaming.</li>
     * </ol>
     * */
    public static final int READY                 = 1;

    /**
     * Being connecting.
     *
     * */
    public static final int CONNECTING            = 2;

    /**
     * The av datas start sending successfully.
     *
     * */
    public static final int STREAMING             = 3;

    /**
     * Streaming has been finished, and you can {@link #startStreaming()} again.
     *
     * */
    public static final int SHUTDOWN              = 4;

    /**
     * Connect error.
     *
     * The following is the possible case:
     *
     * <ol>
     *     <li>Stream is invalid</li>
     *     <li>Network is unreachable</li>
     * </ol>
     *
     * */
    public static final int IOERROR               = 5;

    /** @hide */
    @Deprecated
    public static final int NETBLOCKING           = 6;

    /**
     * Notify the camera switched.
     * <p>
     * extra will including the info the new camera id.
     *
     * <ol>
     *     <li>Camera.CameraInfo.CAMERA_FACING_FRONT</li>
     *     <li>Camera.CameraInfo.CAMERA_FACING_BACK</li>
     * </ol>
     *
     * <pre>
     *     <code>
     *         Log.i(TAG, "current camera id:" + (Integer)extra);
     *     </code>
     * </pre>
     * */
    public static final int CAMERA_SWITCHED       = 7;

    /**
     * Notify the torch info after camera be active.
     * <p>
     * extra will including the info if the device support the Torch.
     *
     * <ol>
     *     <li>true, supported</li>
     *     <li>false, unsupported</li>
     * </ol>
     *
     * <pre>
     *     <code>
     *         final boolean isSupportedTorch = (Boolean) extra;
     *     </code>
     * </pre>
     *
     * */
    public static final int TORCH_INFO            = 8;

    /** @hide */
    @Deprecated
    public static final int CONNECTION_TIMEOUT    = 9;

    /**
     * Sending buffer is empty.
     *
     * */
    public static final int SENDING_BUFFER_EMPTY  = 10;

    /**
     * Sending buffer have been full.
     *
     * */
    public static final int SENDING_BUFFER_FULL   = 11;

    /**
     * Sending buffer have few items witch waiting to be sent.
     *
     * */
    public static final int SENDING_BUFFER_HAS_FEW_ITEMS    = 12;

    /**
     * Sending buffer have many items witch waiting to be sent.
     *
     * */
    public static final int SENDING_BUFFER_HAS_MANY_ITEMS   = 13;

    /**
     * The network connection has been broken.
     *
     * */
    public static final int DISCONNECTED          = 14;

    /**
     * if the device hasn't the supported preview size, then it will select the default preview
     * size which mismatch the specified {@link CameraStreamingSetting.PREVIEW_SIZE_RATIO}.
     *
     * */
    public static final int NO_SUPPORTED_PREVIEW_SIZE = 15;

    /**
     * {@link AudioRecord#startRecording()} failed.
     *
     * */
    public static final int AUDIO_RECORDING_FAIL     = 16;

    /**
     * camera open failed.
     *
     * */
    public static final int OPEN_CAMERA_FAIL         = 17;

    /**
     * Do not support NV21 preview format.
     *
     * */
    public static final int NO_NV21_PREVIEW_FORMAT   = 18;

    /**
     * Invalid streaming url.
     *
     * Gets the message after call {@link #setStreamingProfile(StreamingProfile)} if streaming
     * url is invalid. Also gets the url as the extra info.
     * */
    public static final int INVALID_STREAMING_URL    = 19;

    /**
     * Invalid streaming url.
     *
     * Gets the message after call {@link #setStreamingProfile(StreamingProfile)} if streaming
     * url is invalid. Also gets the url as the extra info.
     * */
    public static final int UNAUTHORIZED_STREAMING_URL    = 20;

}
```

* StreamingSessionListener
接口原型如下：

``` java
/**
 * Callback interface for some particular Streaming incidents.
 * <p>
 *
 * Called on an "arbitrary thread".
 *
 * */
public interface StreamingSessionListener {
    /**
     * Invoked when audio recording failed.
     * <p>
     *
     * @param code error code. <b>Unspecified now</b>.
     *
     * @return true means you handled the event; otherwise, given up.
     *
     * */
    boolean onRecordAudioFailedHandled(int code);

    /**
     * Restart streaming notification.
     * <p>
     *
     * When the network-connection is broken, {@link STATE#DISCONNECTED} will notified first,
     * and then invoked this method if the environment of restart streaming is ready.
     *
     * <p>
     * SDK won't limit the number of invocation.
     *
     * @param code error code. <b>Unspecified now</b>.
     *
     * @return true means you handled the event; otherwise, given up and then {@link STATE#SHUTDOWN}
     *              will be notified.
     *
     * */
    boolean onRestartStreamingHandled(int code);

    /**
     * Invoked after camera object constructed.
     * <p>
     * The supported list exists in following cases:
     * <ol>
     *     <li>If didn't set the
     *     {@link CameraStreamingSetting#setCameraPrvSizeRatio} when
     *     initialize {@link CameraStreamingSetting},
     *     the whole supported list would be passed</li>
     *     <li>If {@link CameraStreamingSetting#setCameraPrvSizeRatio}
     *     was set, the supported preview size which filtered by the specified ratio would be passed</li>
     * </ol>
     *
     * @param list supported camera preview list which sorted from smallest to largest. The list maybe null.
     *
     * @return null means you give up selection and SDK will help you select a proper preview
     *          size; otherwise, the returned size will be effective.
     *
     * */
    Camera.Size onPreviewSizeSelected(List<Camera.Size> list);
}
```

可以在 StreamingSessionListener 处理一些重连、音频读取失败、preview size 的自定义操作。

``` java
@Override
public boolean onRecordAudioFailedHandled(int err) {
    mCameraStreamingManager.updateEncodingType(CameraStreamingManager.EncodingType.SW_VIDEO_CODEC);
    mCameraStreamingManager.startStreaming();
    return true;
}

@Override
public boolean onRestartStreamingHandled(int err) {
    return mCameraStreamingManager.startStreaming();
}

@Override
public Camera.Size onPreviewSizeSelected(List<Camera.Size> list) {
    if (list != null) {
        for (Camera.Size s : list) {
            Log.i(TAG, "w:" + s.width + ", h:" + s.height);
        }
//            return "your choice";
    }
    return null;
}
```
在消费了 onRecordAudioFailedHandled 或 onRestartStreamingHandled 之后，您应该返回 true 通知 SDK；若不做任何处理，返回 false。

* StreamStatusCallback
接口原型如下：

``` java
/**
 * Callback interface used to notify {@link StreamingProfile.StreamStatus}.
 */
public interface StreamStatusCallback {

   /**
    * Called per the {@link StreamingProfile.StreamStatusConfig#getIntervalMs}
    *
    * @param status the new {@link StreamingProfile.StreamStatus}
    *
    * */
    void notifyStreamStatusChanged(final StreamingProfile.StreamStatus status);
}
```

注意：notifyStreamStatusChanged 运行在非 UI 线程中。

StreamStatus 的定义如下：

``` java
/**
 * The nested class is for feedbacking the av status in real time.
 *
 * <p>
 * You can set the {@link StreamStatusConfig} to get the preferred callback frequency.
 *
 * */
public static class StreamStatus {
    /**
     * Audio frame per second.
     * */
    public int audioFps;

    /**
     * Video frame per second.
     * */
    public int videoFps;

    /**
     * Audio and video total bits per second.
     * */
    public int totalAVBitrate;  // bps

    /**
     * Audio and video total bits per second.
     * */

    /**
     * Audio bits per second.
     * */
    public int audioBitrate;  // bps

    /**
     * Video bits per second.
     * */
    public int videoBitrate;  // bps
}
```

<a name="5.5.3"></a>
###5.5.3 resume
`CameraStreamingManager#resume` 会进行 Camera 的打开操作，当成功打开后，会返回 STATE.READY 消息，用户可以在接受到 STATE.READY 之后，安全地进行推流操作。

``` java
mCameraStreamingManager.resume();
```

若在一个 Activity 中进行推流操作，建议 `mCameraStreamingManager.resume()` 在 `Activity#onResume` 中被调用。

<a name="5.5.4"></a>
###5.5.4 开始推流

由于 startStreaming 会进行网络链接相关操作，因此需要将 startStreaming 运行在非 UI 线程，否则可能会发生崩溃现象。

``` java
mCameraStreamingManager.startStreaming();
```

<a name="5.5.5"></a>
###5.5.5 手动对焦
对焦之前传入 Focus Indicator , 如果不进行设置，对焦过程中将会没有对应的 UI 显示。

``` java
// You should call this after getting {@link STATE#READY}.
mCameraStreamingManager.setFocusAreaIndicator(mRotateLayout,
                    mRotateLayout.findViewById(R.id.focus_indicator));
```

点击屏幕触发手动对焦，并设置对应的坐标值。

``` java
// You should call this after getting {@link STATE#READY}.
mCameraStreamingManager.doSingleTapUp((int) e.getX(), (int) e.getY());
```

<a name="5.5.6"></a>
###5.5.6 Zoom
Camera Zoom 操作。

``` java
// mCurrentZoom must be in the range of [0, mCameraStreamingManager.getMaxZoom()]
// You should call this after getting {@link STATE#READY}.
if (mCameraStreamingManager.isZoomSupported()) {
  mCameraStreamingManager.setZoomValue(mCurrentZoom);
}
```

可以获取到当前的 Zoom 值：

``` java
mCameraStreamingManager.getZoom();
```

<a name="5.5.7"></a>
###5.5.7 闪光灯操作
开启闪光灯。

``` java
mCameraStreamingManager.turnLightOn();
```

关闭闪光灯。

``` java
mCameraStreamingManager.turnLightOff();
```

<a name="5.5.8"></a>
###5.5.8 切换摄像头
切换摄像头。

``` java
mCameraStreamingManager.switchCamera();
```
<a name="5.5.9"></a>
###5.5.9 禁音推流
在推流过程中，将声音禁用掉：

``` java
mCameraStreamingManager.mute(true);
```
恢复声音：

```java
mCameraStreamingManager.mute(false);
```

注：默认为 false

<a name="5.5.10"></a>
###5.5.10 截帧
在 Camera 正常预览之后，可以正常进行截帧功能。

在调用 captureFrame 的时候，您需要传入 width 和 height，以及 FrameCapturedCallback，如果传入的 width 或者 height 小于等于 0，SDK 返回的 Bitmap 将会是预览的尺寸 。SDK 完成截帧之后，会回调 onFrameCaptured，并将结果以参数的形式返回给调用者。

``` java
mCameraStreamingManager.captureFrame(w, h, new FrameCapturedCallback() {
    @Override
    public void onFrameCaptured(Bitmap bmp) {

    }
}
```

注意：调用者有义务对 Bitmap 进行回收释放。截帧失败，bmp 会为 null。

<a name="5.5.11"></a>
###5.5.11 停止推流
停止当前推流。

``` java
mCameraStreamingManager.stopStreaming();
```
<a name="5.5.12"></a>
###5.5.12 Log 管理
当 enabled 设置为 true ，SDK Native 层的 log 将会被打开；当设置为 false，SDK Native 层的 log 将会被关闭。默认处于打开状态。

``` java
mCameraStreamingManager.setNativeLoggingEnabled(false);
```
注：默认值为 true。建议 Release 版本置为 false。

<a name="5.5.13"></a>
###5.5.13 pause
退出 CameraStreamingManager，该操作会主动断开当前的流链接，并关闭 Camera 和释放相应的资源。

``` java
mCameraStreamingManager.pause();
```
<a name="5.5.14"></a>
###5.5.14 destroy
释放不紧要资源。

``` java
mCameraStreamingManager.destroy();
```

<a name="5.6"></a>
##5.6 自定义滤镜

<a name="5.6.1"></a>
###5.6.1 软编模式滤镜实现
需要分别处理预览显示的 filter 效果和 encoding 的 filter 效果：预览显示通过实现 SurfaceTextureCallback interface；encoding 通过实现 `StreamingPreviewCallback` interface。两者分别实现，互不影响。

* encoding 部分

``` java
public interface StreamingPreviewCallback {
    public boolean onPreviewFrame(byte[] bytes, int width, int height);
}
```

onPreviewFrame 会回调 NV21 格式的 YUV 数据，进行 filter 算法处理后的结果仍然保存在 bytes 数组中，SDK 会将 bytes 中的数据当作数据源进行后续的编码和封包等操作；onPreviewFrame 运行在名称为 CameraManagerHt 的子线程中；onPreviewFrame 仅在 STATE.STREAMING 状态下被回调。
* 预览显示部分

``` java
public interface SurfaceTextureCallback {
    void onSurfaceCreated();
    void onSurfaceChanged(int width, int height);
    void onSurfaceDestroyed();
    int onDrawFrame(int texId, int width, int height);
}
```

四个回调均执行在 GL rendering thread；如果 onDrawFrame 直接返回 texId，代表放弃 filter 处理，否则 onDrawFrame 应该返回一个 filter 算法处理过的纹理 id； 自定义的 Texture id，即 onDrawFrame 返回的纹理 id， 必须是 GLES20.GL_TEXTURE_2D 类型；SDK 回调的纹理 id，即 onDrawFrame 的参数 texId 类型为 GLES11Ext.GL_TEXTURE_EXTERNAL_OES。

<a name="5.6.2"></a>
###5.6.2 硬编模式滤镜实现
硬编模式下仅需要实现 SurfaceTextureCallback interface 就可实现预览显示和 streaming。