3) 最短“出图验证”（不依赖 RealSense SDK）

刷好系统、插好相机后，在 Jetson 上跑：

3.1 看系统有没有枚举出摄像头节点

ls /dev/video*

v4l2-ctl --list-devices


只要出现新的 /dev/videoX，基本说明 GMSL→反序列化→CSI→驱动/DT 这条链路在工作。

3.2 用 GStreamer 直接拉流（最快看到画面）

（下面是通用思路，不同系统可能要换成你实际的 /dev/videoX）

gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! autovideosink


v4l2-ctl 是 Linux 上 V4L2（Video4Linux2） 的命令行工具，用来查看/控制摄像头设备（/dev/video*），比如列出设备、看支持的格式/分辨率、调参数、做简单抓帧测试。

常用速查（直接复制用）：

# 1) 列出所有视频设备（摄像头/采集卡）
v4l2-ctl --list-devices

# 2) 查看某个节点的详细信息
v4l2-ctl -d /dev/video0 --all
