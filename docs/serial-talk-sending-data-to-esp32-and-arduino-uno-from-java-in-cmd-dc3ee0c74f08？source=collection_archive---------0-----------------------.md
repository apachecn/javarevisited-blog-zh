# 串行通话-从 cmd 中的 Java 向 ESP32(和 Arduino Uno)发送数据

> 原文：<https://medium.com/javarevisited/serial-talk-sending-data-to-esp32-and-arduino-uno-from-java-in-cmd-dc3ee0c74f08?source=collection_archive---------0----------------------->

当你在玩 ESP32 的时候，你会发现每次**你都必须绘制草图，编译并上传来运行**你想要的电子任务。回想一下，我们之前讨论过处理 [ESP32](/@himnickson/beginning-with-esp32-ba8366da7a42) 和 [Arduino Uno](/@himnickson/understanding-an-arduino-board-with-an-led-792958f833f6) 的内置 LED。我们在 ESP32 中制作了 LED，它依靠 2 号引脚**每 1 秒闪烁一次。对于那个小品，是**固定**。要改变持续时间，我们必须再次编辑草图，编译并上传。**

多么累人的工作啊？