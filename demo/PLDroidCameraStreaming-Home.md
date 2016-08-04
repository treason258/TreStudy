<a name="1"></a>
# 1 概述

PLDroidCameraStreaming 是一个适用于 Android 的 RTMP 直播推流 SDK，可高度定制化和二次开发。特色是同时支持 H.264 软编／硬编和 AAC 软编／硬编。支持 Android Camera 画面捕获，并进行 H.264 编码，以及支持 Android 麦克风音频采样并进行 AAC 编码；还实现了一套可供开发者选择的编码参数集合，以便灵活调节相应的分辨率和码率；同时，SDK 提供数据源回调接口，用户可进行 Filter 处理。借助 PLDroidCameraStreaming ，开发者可以快速构建一款类似 [Meerkat](https://meerkatapp.co/) 或 [Periscope](https://www.periscope.tv/) 的 Android 直播应用。

<a name="1.1"></a>
## 1.1 功能以及版本

| 功能 | 描述 |版本号 |
|---|---|---|
| H.264 和 AAC 软编|  | v1.4.1(+) |
| H.264 和 AAC 硬编 |  | v1.0.0(+) |
| RTMP 封包及推流 | |v1.0.0(+) |
| 数据源回调接口，可自定义 Filter (滤镜) 特效处理 | |v1.4.1(+) |
| 前后置摄像头，以及动态切换 | | v1.2.0(+) |
| 自动手动对焦| | v1.1.0(+) |
| 手动对焦|| v1.5.0(+) |
| 支持美颜 || v1.7.0(+) |
| Zoom 操作| | v1.5.0(+) |
| Mute/Unmute | | v1.5.0(+) |
| 闪光灯操作 | |v1.1.0(+) |
| 纯音频推流，以及后台运行| | v1.1.0(+) |
| 截帧 || v1.3.0(+) |
| 动态更改 Encoding Orientation || v1.4.5(+) |
| 动态切换横竖屏 | |v1.4.5(+) |
| ARMv7a 芯片体系架构 || v1.0.0(+) |
| ARM, ARM64v8a, X86 主流芯片体系架构 || v1.3.3(+) |

<a name="1.2"></a>
## 1.2 特性

- 支持 H.264 和 AAC 软编（推荐）
- 支持 H.264 和 AAC 硬编
- 软编支持 Android Min API 15（Android 4.0.3）及其以上版本
- 硬编支持 Android Min API 18（Android 4.3）及其以上版本
- 支持构造带安全授权凭证的 RTMP 推流地址
- 支持 RTMP 封包及推流
- 支持 RTMP 推流自适应网络质量动态切换码率或自定义策略
- 支持数据源回调接口，可自定义 Filter (滤镜) 特效处理
- 支持前后置摄像头，以及动态切换
- 支持自动对焦
- 支持手动对焦
- 支持水印
- 支持美颜，以及调节磨皮、美白、红润效果
- 支持 Encoding Mirror 设置
- 支持 Zoom 操作
- 支持 Mute/Unmute
- 支持闪光灯操作
- 支持纯音频推流，以及后台运行
- 支持截帧功能
- 支持动态更改 Encoding Orientation
- 支持动态切换横竖屏
- 支持蓝牙麦克风
- 支持 ARM, ARMv7a, ARM64v8a, X86 主流芯片体系架构

<a name="2"></a>
# 2 阅读对象

本文档为技术文档，需要阅读者：

  - 具有基本的 Android 开发能力
  - 准备接入七牛直播云或已完成七牛直播云接入

<a name="3"></a>
# 3 开发准备

<a name="3.1"></a>
## 3.1 设备以及系统

- 设备要求：搭载 Android 系统的设备
- 系统要求：Android 4.0.3(API 15) 及其以上

<a name="3.2"></a>
## 3.2 前置条件

- 已注册七牛账号
- 通过[官网](https://portal.qiniu.com)申请并已开通直播权限

<a name="3.3"></a>
## 3.3 混淆

为了保证正常使用 SDK ，请在 proguard-rules.pro 文件中添加以下代码：

```
-keep class com.pili.pldroid.streaming.** { *; }
```

<a name="3.4"></a>
## 3.4 版本升级须知

### v1.4.6
从 v1.4.6 版本开始，需要在宿主项目中的 build.gradle 中加入如下语句：

```
dependencies {
    ...
    compile 'com.qiniu:happy-dns:0.2.7'
    ...
}
```
否则，在运行时会发生找不到 happydns 相关类的错误。

### v1.6.0
从 [v1.6.0](https://github.com/pili-engineering/PLDroidCameraStreaming/releases/tag/v1.6.0) 开始，在使用 SDK 之前，需要保证 `StreamingEnv` 被正确初始化 ，否则在构造核心类 `CameraStreamingManager` 的阶段会抛出异常。具体可参看 [Demo](https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/PLDroidCameraStreamingDemo/app/src/main/java/com/pili/pldroid/streaming/camera/demo/StreamingApplication.java)。

``` java
StreamingEnv.init(getApplicationContext());
```

### v1.6.1
从 [v1.6.1](https://github.com/pili-engineering/PLDroidCameraStreaming/releases/tag/v1.6.1) 开始，为了便于用户更好地定制化，将 TransformMatrix 信息加入到 `SurfaceTextureCallback#onDrawFrame`。因此更新到 v1.6.1 版本之后，若实现了 `SurfaceTextureCallback` 接口，需要将

``` java
int onDrawFrame(int texId, int texWidth, int texHeight);
```
更改为：

``` java
int onDrawFrame(int texId, int texWidth, int texHeight, float[] transformMatrix);
```
