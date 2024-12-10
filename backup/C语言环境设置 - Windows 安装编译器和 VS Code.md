> 本文截取自 https://www.bilibili.com/video/BV112z3YUEmU
# 下载 C 编译器
为了能在 Windows 上使用 GCC，我们可以下载 MSYS2，它包含了 GCC 编译器的 Windows 版本。而且是完全开源免费的。
1. 进入网页 https://www.msys2.org ，找到 Installation，下载。
2. 点击安装，可以自行指定路径，注意使用英文路径。安装完成后，默认勾选立即启动。
3. 在弹出的窗口输入`pacman -Syu`更新软件包数据库和基础包。
4. 运行完成后，再输入`pacman -Su`更新其余软件包。
5. 运行完成后，再输入`pacman -S --needed base-devel mingw-w64-x86_64-toolchain`。出现 Enter a selection 按回车选择默认即可。

安装完成后，找到刚刚设置的路径，找到`msys64\mingw64\bin`文件夹，往下翻可以看到`gcc.exe`文件。接下来需要把路径添加到环境变量的 Path 里。
1. 右键此电脑->高级系统设置->环境变量->系统变量->Path->编辑，粘贴路径。
2. 进入 cmd，输入`gcc -v`，出现 gcc version 等字样即配置成功。

# 安装 VS Code
1. 安装软件（略）。
2. 打开 VS Code，点击插件栏，搜索 C/C++，安装插件。
3. 搜索 Code runner，安装插件。
