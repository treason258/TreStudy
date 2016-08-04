- 4 [快速开始](#4)
	- 4.1 [开发环境配置](#4.1)
	- 4.2 [创建新工程](#4.2)
	- 4.3 [导入SDK](#4.3)
	- 4.4 [创建基础播放实例](#4.4)
		- 4.4.1 [添加相关权限并注册 Activity](#4.4.1)
		- 4.4.2 [添加 happy-dns 依赖](#4.4.2)
		- 4.4.3 [实现自己的 Application](#4.4.3)
		- 4.4.4 [创建主界面](#4.4.4)
		- 4.4.5 [创建主界面布局文件](#4.4.5)
		- 4.4.6 [创建推流界面](#4.4.6)
		- 4.4.7 [创建推流界面布局文件](#4.4.7)
		- 4.4.8 [测试播放效果](#4.4.8)
    - 4.5 [从 SDK DEMO 开始使用](#4.5)
	   - 4.5.1 [下载 SDK DEMO](#4.5.1)
	   - 4.5.2 [导入 Project 到 Android Studio](#4.5.2)
	   - 4.5.3 [已有工程导入 SDK](#4.5.3)

<a name="4"></a>
#4 快速开始

<a name="4.1"></a>
##4.1 开发环境配置

* 已全部完成 BOOK - I 中的所有操作。搭建好带有 Pili server sdk 的业务服务端，SDK 推流信息的输入来自服务端返回的 StreamJson
* Android Studio 开发工具。官方[下载地址](http://developer.android.com/intl/zh-cn/sdk/index.html)
* 下载 Android 官方开发SDK 。官方[下载地址](https://developer.android.com/intl/zh-cn/sdk/index.html#Other)。PLDroidCameraStreaming 软编要求 Min API 15 和硬编要求 Android Min API 18
* 下载 PLDroidCameraStreaming 最新的 JAR 和 SO 文件。[下载地址](https://github.com/pili-engineering/PLDroidCameraStreaming/tree/master/releases)
* 请用真机调试代码，模拟器无法调试。

<a name="4.2"></a>
##4.2 创建新工程

* 通过 Androname Studio 创建 Project，
![创建新工程](http://7xuil4.com1.z0.glb.clouddn.com/sbs_new_project_1.png)

* 设置新项目
    * 填写 Application id
    * 填写 Company Domain
    * 填写 Package id
    * 选择 Project location
    * 可以使用默认的填写项

![设置新项目 ](http://7xuil4.com1.z0.glb.clouddn.com/sbs_configure_new_project_2.png)

* 选择 Target Android Devices

本例中选择使用 MinimumSDK  API 18（软编要求 MinimumSDK API 15 ； 硬编要求 MinimumSDK API 18）
![MinimumSDK API 18](http://7xuil4.com1.z0.glb.clouddn.com/sbs_target_android_devices_3.png)

* 选择 Empty Activity
![选择 Empty Activity](http://7xuil4.com1.z0.glb.clouddn.com/sbs_empty_activity_4.png)

* 填写 Main Activity 信息，作为 `android.intent.action.MAIN`
![填写 Main Activity 信息](http://7xuil4.com1.z0.glb.clouddn.com/sbs_set_main_activity_5.png)

* 完成创建
![完成创建](http://7xuil4.com1.z0.glb.clouddn.com/sbs_new_project_done_6.png)

<a name="4.3"></a>
##4.3 导入 SDK

* 将右侧文件夹目录切换为 `Project` 视图

![Project](http://7xuil4.com1.z0.glb.clouddn.com/sbs_resource_view_selected_7.png)

* 在 app/src/main 目录下创建 jniLibs 目录。按图所示，将文件导入对应的目录。

![导入路径](http://7xuil4.com1.z0.glb.clouddn.com/sbs_import_jar_so_8.png)

* 选中 lib 目录下 pldroid-camera-streaming-1.4.6.jar，右键添加新建库，如图所示

![添加新库](http://7xuil4.com1.z0.glb.clouddn.com/add-jar_9.png)
* 导入完成，双击 `build.gradle` 文件查看内容，lib 目录中的文件已经自动导入，涉及的文件名如下：

``` java
// JAR
pldroid-camera-streaming-1.4.6.jar

// SO
libpldroid_streaming_aac_encoder.so
libpldroid_streaming_core.so
libpldroid_streaming_h264_encoder.so
```

<a name="4.4"></a>
##4.4 创建基础播放实例

<a name="4.4.1"></a>
###4.4.1 添加相关权限并注册 Activity

* 在 app/src/main 目录中的 AndroidManifest.xml 中增加 `uses-permission` 和 `uses-feature` 声明，并注册推流 Activity : SWCameraStreamingActivity

``` xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.pili.demo.streamingsdk" >

    <uses-permission android:id="android.permission.INTERNET" />
    <uses-permission android:id="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:id="android.permission.RECORD_AUDIO" />
    <uses-permission android:id="android.permission.CAMERA" />
    <uses-permission android:id="android.permission.WAKE_LOCK" />
    <uses-feature android:id="android.hardware.camera.autofocus" />
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />
    <uses-permission android:id="android.permission.ACCESS_NETWORK_STATE"/>

    <application
        android:allowBackup="true"
        android:name=".StreamingApplication"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_id"
        android:supportsRtl="true"
        android:theme="@style/AppTheme" >
        <activity android:id=".MainActivity" >
            <intent-filter>
                <action android:id="android.intent.action.MAIN" />

                <category android:id="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity android:id=".SWCameraStreamingActivity" >
        </activity>
    </application>

</manifest>
```

<a name="4.4.2"></a>
###4.4.2 添加 happy-dns 依赖

* 检查在 app 目录下的 `build.gradle` ，并且按照如下修改：
    * 确认在 app 目录下
    * 打开如图所示目录文件

``` java
apply plugin: 'com.android.application'

android {
    compileSdkVersion 22
    buildToolsVersion "22.0.1"

    defaultConfig {
        applicationId "com.pili.demo.streamingsdk"
        minSdkVersion 18
        targetSdkVersion 22
        versionCode 1
        versionid "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.qiniu:happy-dns:0.2.7'
    compile 'com.android.support:appcompat-v7:22.0.0'
    compile files('libs/pldroid-camera-streaming-1.6.2.jar')
}

```

<a name="4.4.3"></a>
###4.4.3 实现自己的 Application

``` java
public class StreamingApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        StreamingEnv.init(getApplicationContext());
    }
}
```

<a name="4.4.4"></a>
###4.4.4 创建主界面

* 查看 app/src/main/java 目录中的 MainActivity.java 文件。为了演示方便，将文件中的 `MainActivity` 父类 `AppCompatActivity` 更改为 `Activity`，即 `public class MainActivity extends AppCompatActivity`，修改为 `public class MainActivity extends Activity`，`MainActivity` 其主要工作包括：
    * 通过 start Button 去 app server 异步请求 stream Json
    * stream json 获取成功之后，启动 `SWCameraStreamingActivity`

``` java
public class MainActivity extends Activity {
    private static final String TAG = "MainActivity";

    private String requestStreamJson() {
        try {
            // Replace "Your app server" by your app sever url which can get the StreamJson as the SDK's input.
            HttpURLConnection httpConn = (HttpURLConnection) new URL("Your app server").openConnection();
            httpConn.setConnectTimeout(5000);
            httpConn.setReadTimeout(10000);
            int responseCode = httpConn.getResponseCode();
            if (responseCode != HttpURLConnection.HTTP_OK) {
                return null;
            }

            int length = httpConn.getContentLength();
            if (length <= 0) {
                return null;
            }
            InputStream is = httpConn.getInputStream();
            byte[] data = new byte[length];
            int read = is.read(data);
            is.close();
            if (read <= 0) {
                return null;
            }
            return new String(data, 0, read);
        } catch (Exception e) {
            showToast("Network error!");
        }
        return null;
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button btn = (Button) findViewById(R.id.start);
        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        try {
                            // Get the stream json from http
                            String streamJson = requestStreamJson();

                            Intent intent = new Intent(MainActivity.this, SWCameraStreamingActivity.class);
                            intent.putExtra("stream_json_str", streamJson);
                            startActivity(intent);
                        } catch (Exception e) {
                            e.printStackTrace();
                        }
                    }
                }).start();
            }
        });
    }
}
```

<a name="4.4.5"></a>
###4.4.5 创建主界面布局文件
* 查看 app/src/main/res/layout 目录下的 `activity_main.xml`
* 底部 TAB 切换至 Text 面板，粘贴如下代码

``` xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">

    <Button
        android:id="@+id/start"
        android:text="start"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
</RelativeLayout>
```

<a name="4.4.6"></a>
###4.4.6 创建推流界面

* 创建名为 `SWCameraStreamingActivity` 的 Empty Activity，`SWCameraStreamingActivity` 的主要工作包括：
    - `SWCameraStreamingActivity` 获取 `MainActivity` 从 app server 获取到的 stream json
    - 在 `onCreate` 通过 stream json 初始化推流 SDK 的核心类 `CameraStreamingManager`
    - 在 `onResume` 中调用 `mCameraStreamingManager.onResume();`
    - 在接收到 `STATE.READY` 之后，开始推流 `mCameraStreamingManager.startStreaming();`，`startStreaming` 需要在非 UI 线程中进行操作。

* 在 app/src/main/java 目录下创建 `SWCameraStreamingActivity` 文件，代码如下：

``` java
public class SWCameraStreamingActivity extends Activity
        implements CameraStreamingManager.StreamingStateListener {
    private JSONObject mJSONObject;
    private CameraStreamingManager mCameraStreamingManager;
    private StreamingProfile mProfile;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_camera_streaming);

        AspectFrameLayout afl = (AspectFrameLayout) findViewById(R.id.cameraPreview_afl);

        // Decide FULL screen or real size
        afl.setShowMode(AspectFrameLayout.SHOW_MODE.REAL);
        GLSurfaceView glSurfaceView = (GLSurfaceView) findViewById(R.id.cameraPreview_surfaceView);

        String streamJsonStrFromServer = getIntent().getStringExtra("stream_json_str");
        try {
            mJSONObject = new JSONObject(streamJsonStrFromServer);
            StreamingProfile.Stream stream = new StreamingProfile.Stream(mJSONObject);
            mProfile = new StreamingProfile();
            mProfile.setVideoQuality(StreamingProfile.VIDEO_QUALITY_HIGH1)
                    .setAudioQuality(StreamingProfile.AUDIO_QUALITY_MEDIUM2)
                    .setEncodingSizeLevel(StreamingProfile.VIDEO_ENCODING_HEIGHT_480)
                    .setEncoderRCMode(StreamingProfile.EncoderRCModes.QUALITY_PRIORITY)
                    .setStream(stream);  // You can invoke this before startStreaming, but not in initialization phase.

            CameraStreamingSetting setting = new CameraStreamingSetting();
            setting.setCameraId(Camera.CameraInfo.CAMERA_FACING_BACK)
                    .setContinuousFocusModeEnabled(true)
                    .setCameraPrvSizeLevel(CameraStreamingSetting.PREVIEW_SIZE_LEVEL.MEDIUM)
                    .setCameraPrvSizeRatio(CameraStreamingSetting.PREVIEW_SIZE_RATIO.RATIO_16_9);

            mCameraStreamingManager = new CameraStreamingManager(this, afl, glSurfaceView, CameraStreamingManager.EncodingType.SW_VIDEO_WITH_SW_AUDIO_CODEC);  // soft codec

            mCameraStreamingManager.prepare(setting, mProfile);
            mCameraStreamingManager.setStreamingStateListener(this);
        } catch (JSONException e) {
            e.printStackTrace();
        }
    }

    @Override
    protected void onResume() {
        super.onResume();
        mCameraStreamingManager.resume();
    }

    @Override
    protected void onPause() {
        super.onPause();
        // You must invoke pause here.
        mCameraStreamingManager.pause();
    }

    @Override
    public void onStateChanged(int state, Object info) {
        switch (state) {
            case CameraStreamingManager.STATE.PREPARING:
                break;
            case CameraStreamingManager.STATE.READY:
                // start streaming when READY
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        if (mCameraStreamingManager != null) {
                            mCameraStreamingManager.startStreaming();
                        }
                    }
                }).start();
                break;
            case CameraStreamingManager.STATE.CONNECTING:
                break;
            case CameraStreamingManager.STATE.STREAMING:
                // The av packet had been sent.
                break;
            case CameraStreamingManager.STATE.SHUTDOWN:
                // The streaming had been finished.
                break;
            case CameraStreamingManager.STATE.IOERROR:
                // Network connect error.
                break;
            case CameraStreamingManager.STATE.SENDING_BUFFER_EMPTY:
                break;
            case CameraStreamingManager.STATE.SENDING_BUFFER_FULL:
                break;
            case CameraStreamingManager.STATE.AUDIO_RECORDING_FAIL:
                // Failed to record audio.
                break;
            case CameraStreamingManager.STATE.OPEN_CAMERA_FAIL:
                // Failed to open camera.
                break;
            case CameraStreamingManager.STATE.DISCONNECTED:
                // The socket is broken while streaming
                break;
        }
    }

    @Override
    public boolean onStateHandled(int i, Object o) {
        return false;
    }
}
```

<a name="4.4.7"></a>
###4.4.7 创建推流界面布局文件

* 查看 app/src/main/res/layout 中的 `activity_swcamera_streaming.xml`
* 切换至 Text 面板，粘贴如下内容

``` xml
<?xml version="1.0" encoding="utf-8"?>

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/content"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/background_floating_material_dark"
    tools:context=".SWCameraStreamingActivity" >

    <com.pili.pldroid.streaming.widget.AspectFrameLayout
        android:id="@+id/cameraPreview_afl"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_centerHorizontal="true"
        android:layout_alignParentTop="true">

        <android.opengl.GLSurfaceView
            android:id="@+id/cameraPreview_surfaceView"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_gravity="center" />

    </com.pili.pldroid.streaming.widget.AspectFrameLayout>
</RelativeLayout>
```

启动 APP 之后，当点击 start button，就可以开始推流了。

<a name="4.4.8"></a>
###4.4.8 测试播放效果
* 测试方法: 从 app server 获取到推流对应的播放地址，输入到播放器中进行播放。

<a name="4.5"></a>
##4.5 从 SDK DEMO 开始使用

<a name="4.5.1"></a>
###4.5.1 下载 SDK DEMO
* 从[这里](https://github.com/pili-engineering/PLDroidCameraStreaming/releases)下载 SDK Demo。

* 假设下载的是 v1.4.1 版本的 `PLDroidCameraStreaming-1.4.1.zip`，解压 zip 包：
```
$ unzip -n PLDroidCameraStreaming-1.4.1.zip -d ~/workdir/workspace/sdk/
```
或者解压到您的工作目录下。

<a name="4.5.2"></a>
###4.5.2 导入 Project 到 Android Studio

  - 启动 Android Studio 并选择 `Open an existing Android Studio project`。
![Android Studio 打开](http://7xuil4.com1.z0.glb.clouddn.com/startup-android-studio_10.png)

  - 选择 PLDroidCameraStreamingDemo 工程。从目录 `~/workdir/workspace/sdk/PLDroidCameraStreaming-1.4.1/` 中选择 `DroidCameraStreamingDemo` 工程，选择完成后，点击 `Choose` 按钮。
![DroidCameraStreamingDemo](http://7xuil4.com1.z0.glb.clouddn.com/choose-demo-project_11.png)
  - 恭喜你，导入完成。
![完成](http://7xuil4.com1.z0.glb.clouddn.com/import-completed_12.png)

<a name="4.5.3"></a>
###4.5.3 已有工程导入 SDK
* SDK [下载地址](https://github.com/pili-engineering/PLDroidCameraStreaming)。
* Demo Project 目录结构说明。如下图所示：
  - 红色框：是 Demo 依赖 SDK 的 JAR 文件。您也可以在 build.gradle 中自定义其路径：
  ``` java
  dependencies {
      compile fileTree(dir: 'libs', include: ['*.jar'])
      compile 'com.qiniu:happy-dns:0.2.7'
      compile files('libs/pldroid-camera-streaming-1.4.1.jar')
    }
  ```
  - 绿色框：是 Demo Java 代码部分。
    - HWCodecCameraStreamingActivity 是硬编的样例代码；
    - SWCodecCameraStreamingActivity 是软编的样例代码；
    - AudioStreamingActivity 是纯音频的样例代码

  - 橙色框：是 Demo 依赖 SDK 的动态链接库文件。目前 SDK 支持主流的 ARM, ARMv7a, ARM64v8a, X86 芯片体系架构。
  - 蓝色框：是 Demo 的布局文件。
![demo](http://7xuil4.com1.z0.glb.clouddn.com/demo-project-structure_13.png)