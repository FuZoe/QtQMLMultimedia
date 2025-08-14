a). 源代码和运行效果如下：
代码
main.qml - https://github.com/simonqin09/QtQMLMultimedia/blob/master/main.qml
main.cpp - https://github.com/simonqin09/QtQMLMultimedia/blob/master/main.cpp

b). 具体要点说明如下
由于 Qt Multimedia 组件在底层也是调用 Gstreamer playbin 元件进行媒体播放操作，因此在编程之前，首先可以直接使用 Gstreamer pipeline 在嵌入式系统先验证下，确保媒体可以正常播放，没有解码器缺失的情况。
$ gst-launch-1.0 playbin uri=file:///home/root/ready-player-one-trailer-2_h720p.mov


main.cpp 文件用于识别输入变量并通过下面代码将其传递给QML来指定媒体文件位置。
QQmlApplicationEngine engine;
engine.rootContext()->setContextProperty("mysource", source);


main.qml 文件用于实现video player界面以及媒体播放。
- 媒体播放主要由 ”Mediaplayer” 元素和 ”VideoOutput” 元素来配合实现。然后将其显示在定义好的 640x480分 辨率的 rectangle 中。
- 定义了一些控制播放和音量的按键，当点击时候会有颜色的变化指示。
- 使用了 Connections 用于信号触发操作，这里对应为当发现媒体播放停止的时候自动将 Play 按键的颜色变更为初始状态。
- 在页面切换按键处，使用了 loader 功能来加载 Camera Capture 页面，需要注意的是同时也需要在 main.qml 中创建一个 CameraMode.qml 的实例以便于操作。
CameraMode {
id: cameramode1
anchors.fill: parent
z: 1
}
