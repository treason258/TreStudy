<a id="8"></a>
#8 历史记录

* 1.6.2 ([Release Notes][31])
  - 发布 pldroid-camera-streaming-1.6.2.jar
  - 修复特殊情况下导致的 crash 问题
  - 更新 Demo 代码

* 1.6.1 ([Release Notes][30])
  - 发布 pldroid-camera-streaming-1.6.1.jar
  - 新增 libpldroid_mmprocessing.so
  - 更新 libpldroid_streaming_core.so 和 libpldroid_streaming_h264_encoder.so
  - 增加水印支持
  - 优化软编 codec，提升画质和码控能力
  - 兼容特殊的直播设备
  - 新增 TransformMatrix 到 SurfaceTextureCallback#onDrawFrame
  - 修复 `CameraStreamingManager#pause` 耗时较长
  - 修复硬编纯音频无法正常停止推流
  - 修复硬编推流过程中特殊步骤导致的概率性 crash
  - 更新 Demo 代码

* 1.6.0 ([Release Notes][29])
  - 发布 pldroid-camera-streaming-1.6.0.jar
  - 更新 libpldroid_streaming_core.so 和 libpldroid_streaming_h264_encoder.so
  - 新增 mirror 支持
  - 新增 `StreamingEnv`
  - 修复特殊机型硬编闪屏问题
  - 修复禁播导致的 crash 问题
  - 改善部分机型硬编 tearing 现象
  - 兼容异常输入的情况，并提供回调
  - 新增质量上报支持
  - 修复资源泄漏问题
  - 修复特殊机型 crash 问题
  - 重构 Demo 代码

* 1.5.3 ([Release Notes][28])
  - 发布 pldroid-camera-streaming-1.5.3.jar
  - 修复特殊机型概率性 crash 问题
  - 优化 Jar 包体积

* 1.5.1 ([Release Notes][27])
  - 发布 pldroid-camera-streaming-1.5.1.jar
  - 更新 libpldroid_streaming_core.so 和 libpldroid_streaming_h264_encoder.so
  - 新增蓝牙麦克风支持
  - 新增质量上报支持
  - 优化启用／关闭输入法弹框导致的屏闪现象
  - 修复部分机型手动对焦引起的 crash 问题
  - 修复部分机型推流过程中概率性 crash 问题
  - 修复部分机型频繁切换输入法导致黑屏问题
  - 修复特殊机型硬编音画不同步问题
  - 更新 demo 样例代码

* 1.5.0 ([Release Notes][26])
  - 发布 pldroid-camera-streaming-1.5.0.jar
  - 更新 libpldroid_streaming_core.so 和 libpldroid_streaming_h264_encoder.so
  - 支持手动对焦
  - 支持 Zoom
  - 支持 mute/unmute
  - 新增 `setSendTimeoutInSecond` API
  - 对回调方法 `sortCameraPrvSize` 的行参 supportedPreviewSizeList 进行从小到大排序
  - 当 DnsManager 设置为 null 后，不进行 Dns 解析，[Issue 78](https://github.com/pili-engineering/PLDroidCameraStreaming/issues/78)
  - 优化数据源采集和显示效率，避免 UI 卡顿
  - 修复硬编模式下，重连导致概率性 crash 问题
  - 方法 onPrepare(), onResume(), onPause(), onDestroy() 分别重命名为 prepare(), resume(), pause(), destroy()
  - 更新 demo 样例代码

* 1.4.6 ([Release Notes][25])
  - 发布 pldroid-camera-streaming-1.4.6.jar
  - 更新 libpldroid_streaming_core.so，libpldroid_streaming_aac_encoder.so 和 libpldroid_streaming_h264_encoder.so
  - 提升软编编码帧率
  - 优化推流过程中前后置摄像头切换体验
  - 新增 happydns 支持，并提供 `setDnsManager` API，用户可自定义 `DnsManager`
  - 新增 `StreamStatus` 回调，实现 `StreamStatusCallback` 获取音视频帧率和码率
  - 新增 `setRecordingHint` API，可实现高帧率推流
  - 修复推流过程中，特殊操作后，推流无图像问题
  - 修复推流过程中，HOME 键退出，再次启动 app，无法切换 camera 问题
  - 修复部分机型音画不同步，包括切换前后置
  - 修复推流过程中，概率性 crash 问题
  - 更新 demo 样例代码

* 1.4.5 ([Release Notes][24])
  - 发布 pldroid-camera-streaming-1.4.5.jar
  - 更新 libpldroid_streaming_core.so，libpldroid_streaming_aac_encoder.so 和 libpldroid_streaming_h264_encoder.so
  - 新增动态更改 Encoding Orientation 支持
  - 新增动态切换横竖屏支持
  - 新增 `onPreviewSizeSelected` 支持
  - 新增 `setPreferredVideoEncodingSize` 支持
  - 新增 `VIDEO_ENCODING_HEIGHT_544` 支持
  - 优化网络传输
  - 提升画质
  - 优化前后置切换
  - 标记 `VIDEO_ENCODING_SIZE_QVGA` 等 Deprecated
  - 标记 `onPreviewFrame(byte[] datas, Camera camera)` Deprecated
  - 修复部分机型概率性 ANR
  - 更新 demo 样例代码

* 1.4.3 ([Release Notes][23])
  - 发布 pldroid-camera-streaming-1.4.3.jar
  - 更新 libpldroid_streaming_core.so，libpldroid_streaming_aac_encoder.so 和 libpldroid_streaming_h264_encoder.so
  - 新增 `SharedLibraryidHelper` 绝对路径加载方式
  - 新增 `StreamingSessionListener`，可方便安全地实现重连策略及 Audio 数据获取失败时的策略
  - 新增 `EncodingType` 支持
  - 修复硬编模式下，多次切换前后置摄像头 crash 问题
  - 修复硬编模式下，部分机型截图 crash 问题
  - 修复 metadata 格式问题
  - 修复软编模式下，推流过程中概率性 crash 问题
  - 修复概率性无视频帧问题
  - 更新 demo 展示代码
  - 增加支持的机型信息

* 1.4.1 ([Release Notes][22])
  - 发布 pldroid-camera-streaming-1.4.1.jar
  - 更新 libpldroid_streaming_core.so
  - 新增 libpldroid_streaming_aac_encoder.so 和 libpldroid_streaming_h264_encoder.so
  - 新增 H.264 和 AAC 软编支持
  - 新增软编数据源回调接口，可定制化 Filter (滤镜) 特效处理
  - 修复硬编部分机型 crash 问题
  - 修复硬编切换前后置时长异常问题
  - 更新 demo 样例代码

* 1.3.9 ([Release Notes][21])
  - 发布 pldroid-camera-streaming-1.3.9.jar
  - 更新 libpldroid_streaming_core.so
  - 增加 x86 支持
  - 新增 x86/libpldroid_streaming_core.so
  - 优化内存，减少内存抖动，增强稳定性
  - 修复 onResume 之后快速 onPause 导致的 crash 问题
  - 修复部分机型截图 crash 问题
  - 修复部分机型切换前后置摄像头之后，导致切片异常问题
  - 修复网络异常导致的 crash 问题(issue 54)

* 1.3.8 ([Release Notes][20])
  - 发布 pldroid-camera-streaming-1.3.8.jar
  - 更新 libpldroid_streaming_core.so
  - 优化切换前后置摄像头数据重发时间，增强推流过程中切换前后置摄像头的稳定性
  - 优化内存使用，避免 OOM
  - 修复部分机型概率性 crash 问题
  - 兼容 supportedPreviewSizeList 为空的机型

* 1.3.7 ([Release Notes][19])
  - 发布 pldroid-camera-streaming-1.3.7.jar
  - 更新 libpldroid_streaming_core.so
  - 修复部分机型概率性 crash 问题
  - 修复部分机型前后置 camera 切换的 crash 问题
  - 兼容无前置 camera 的机型

* 1.3.6 ([Release Notes][18])
  - 发布 pldroid-camera-streaming-1.3.6.jar
  - 更新 libpldroid_streaming_core.so
  - 优化 video stream 流畅度
  -  修复概率性断流问题
  - 修复部分机型推流过程中，概率性 crash 问题
  - 修复部分机型切换前后置摄像头过程中，概率性 crash 问题

* 1.3.5 ([Release Notes][17])
  - 发布 pldroid-camera-streaming-1.3.5.jar
  - 更新 libpldroid_streaming_core.so
  - 修复部分机型音视频不同步问题
  - 分离 preview size 与 encoding size
  - 新增 `setEncodingSizeLevel` API，并提供 encoding size 参数列表
  - 修复部分机型花屏问题
  - 修复前后置摄像头切换概率性断流问题
  - 修复概率性 crash 问题

* 1.3.4 ([Release Notes][16])
  - 发布 pldroid-camera-streaming-1.3.4.jar
  - 更新 libpldroid_streaming_core.so
  - 修复采用 ART 运行时的 Android 机型的 crash 问题
  - 修复封包不兼容的问题

* 1.3.3 ([Release Notes][15])
  - 发布 pldroid-camera-streaming-1.3.3.jar
  - 删除 arm64-v8a/libpldroid_ffmpegbridge.so 以及 armeabi-v7a/libpldroid_ffmpegbridge.so
  - 新增 armeabi 支持
  - 新增 arm64-v8a/libpldroid_streaming_core.so, armeabi-v7a/libpldroid_streaming_core.so 和 armeabi/libpldroid_streaming_core.so
  - 体积裁剪数十倍，动态链接库裁剪至 69KB
  - 完全移除 FFmpeg 依赖
  - 修复推流过程中，切换前后置断流问题
  - 修复自适应码率过程中，切换 quality 断流问题
  - 修复前后置切换概率性 crash 问题

* 1.3.2 ([Release Notes][14])
  - 发布 pldroid-camera-streaming-1.3.2.jar
  - 修复输入法弹起导致预览画面调整的问题

* 1.3.1 ([Release Notes][13])
  - 发布 pldroid-camera-streaming-1.3.1.jar
  - 增加 arm64-v8a 支持，新增 arm64-v8a/libpldroid_ffmpegbridge.so
  - 更新 armeabi-v7a/libpldroid_ffmpegbridge.so
  - 新增切换 `Stream` 接口：setStreamingProfile
  - 新增 `setLocalFileAbsolutePath` 接口
  - 修复横屏下，经过特殊操作，Camera 预览显示异常的问题

* 1.3.0 ([Release Notes][12])
  - 发布 pldroid-camera-streaming-1.3.0.jar
  - 新增自适应码率功能
  - 新增截帧接口
  - 新增 Preview Layout `REAL/FULL` mode，解决显示黑边问题
  - 修复 IOS 和 Android 使用同一个 stream 时，导致 IOS 无法正常推流的问题
  - 修复部分机型切换前后置 crash 问题
  - 新增自适应码率演示代码
  - 新增截帧演示代码
  - 新增 REAL/FULL mode 演示代码

* 1.2.3 ([Release Notes][11])
  - 发布 pldroid-camera-streaming-1.2.3.jar
  - 新增 Audio quality 和 Video quality 配置项，可自由组合音视频码率参数
  - 新增 Video quality 设置接口 `setVideoQuality`
  - 新增 Audio quality 设置接口 `setAudioQuality`
  - 优化 jar 包，减少约 30% 体积

* 1.2.2 ([Release Notes][10])
  - 发布 pldroid-camera-streaming-1.2.2.jar
  - 更新 libpldroid_ffmpegbridge.so
  - 修复概率性的 crash 问题
  - 添加 `STATE.CONNECTION_TIMEOUT` 状态
  - 修复部分机型因连接错误而导致屏幕 Hang 住
  - 在 UI 层对点击事件加入保护逻辑，避免快速点击导致应用 crash

* 1.2.1 ([Release Notes][9])
  - 发布 pldroid-camera-streaming-1.2.1.jar
  - 更新 libpldroid_ffmpegbridge.so
  - 优化内存问题，修复 OOM 异常
  - 优化 Quality 配置
  - 添加 `setNativeLoggingEnabled()` 接口

* 1.2.0 ([Release Notes][8])
  - 发布 pldroid-camera-streaming-1.2.0.jar
  - 更新 libpldroid_ffmpegbridge.so
  - 更新 Stream 设置接口：`setStream(stream)`
  - 添加 Camera 切换接口：`switchCamera`
  - 修复 Android L crash 问题
  - 添加 Camera 切换状态：`STATE.CAMERA_SWITCHED`
  - 添加 Torch 是否支持状态：`STATE.TORCH_INFO`
  - 更新状态回调接口：`onStateChanged(state, extra)`
  - 修复特殊操作的概率性 crash 问题
  - 修复部分机型 `turnLightOn` 及 `turnLightOff` 接口无效问题
  - 修复部分机型点击 Home 按键 crash 问题
  - 修复部分机型因 `PREVIEW_SIZE_LEVEL` 导致 crash 问题
  - 添加 Camera 切换操作演示代码
  - 更新 Torch 组件显示逻辑

* 1.1.0 ([Release Notes][7])
  - 发布 pldroid-camera-streaming-1.1.0.jar
  - 更新 libpldroid_ffmpegbridge.so
  - 优化 ffmpegbridge 模块，降低 libpldroid_ffmpegbridge.so 文件大小
  - 添加纯音频推流支持：添加纯音频推流 `CameraStreamingManager(Context ctx)` 构造函数
  - 纯音频推流支持后台运行
  - 添加 preview size 设定接口：`setCameraPrvSizeLevel` 及 `setCameraPrvSizeRatio`
  - 添加 torch 操作接口： `turnLightOn` 及 `turnLightOff`
  - 添加控制连续自动对焦的接口：`setContinuousFocusModeEnabled`
  - 废弃 `setCameraPreviewSize` 接口
  - 修复部分机型因 preivew size 不支持而导致的 crash 问题
  - 添加 `AudioStreamingActivity` 及 `StreamingBaseActivity`，用来演示纯音频推流
  - 添加 torch 操作演示代码

* 1.0.2 ([Release Notes][6])
  - 发布 pldroid-camera-streaming-1.0.2.jar
  - 修复无 `StreamingStateListener` 情况下的 Crash 问题
  - 修复正常启动后无 READY 消息返回问题
  - 更新 `Stream` 定义，并与服务端保持一致
  - 增加相机正常启动后即开始推流功能

* 1.0.1 ([Release Notes][5])
  - 发布 pldroid-camera-streaming-1.0.1.jar
  - 更新 `Stream` 类结构
  - 更新 `Stream` 的构造方式

* 1.0.0 ([Release Notes][4])
  - 发布 PLDroidCameraStreaming v1.0.0

[4]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.0.0.md
[5]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.0.1.md
[6]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.0.2.md
[7]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.1.0.md
[8]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.2.0.md
[9]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.2.1.md
[10]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.2.2.md
[11]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.2.3.md
[12]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.3.0.md
[13]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.3.1.md
[14]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.3.2.md
[15]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.3.3.md
[16]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.3.4.md
[17]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.3.5.md
[18]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.3.6.md
[19]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.3.7.md
[20]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.3.8.md
[21]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.3.9.md
[22]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.4.1.md
[23]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.4.3.md
[24]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.4.5.md
[25]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.4.6.md
[26]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.5.0.md
[27]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.5.1.md
[28]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.5.3.md
[29]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.6.0.md
[30]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.6.1.md
[31]: https://github.com/pili-engineering/PLDroidCameraStreaming/blob/master/ReleaseNotes/release-notes-1.6.2.md
