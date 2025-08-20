问题：

由于此设备上的安全设置已更改，你的PIN不再可用。单击以重新设置PIN。

解决方式

1、

对于 Windows 11 和 10 系统，可在登录界面长按 Shift 键的同时点击 “重启”，进入高级启动界面，选择 “疑难解答”>“高级选项”>“命令提示符”，用命令提示符将 utilman.exe 换为 cmd.exe，然后重启，打开注册表编辑器，将 HKEY\_LOCAL\_MACHINE\\SOFTWAREMicrosoft\\Windows NT\\CurrentVersion\\PasswordLess\\Device 项目下的 DevicePasswordLessBuildVersion 数值修改为 0，再重启使用密码登录系统并删除 PIN