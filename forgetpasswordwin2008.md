第一步：

插入安装光盘，重光驱启动系统，在选择“安装语言”的地方，按 `shift+F10`

在弹出的CMD窗口中，输入以下地址：  
注：vmware虚机盘符会变，`dir`确认

	x:\>c:

	c:\>cd windows\system32

	c:\>ren Magnify.exe Magnify1.exe

	c:\>copy cmd.exe Magnify.exe

	c:\>exit

以上几步是用命令行替换掉放大境窗口

然后重新启动电脑，从硬盘启动

第二步：

在输入登录密码的地方

打开放大镜，会弹出CMD窗口

在其它输入：

	net user administrator Com12345  

然后关闭窗口

输入密码	 `Com12345`

注意设置密码的时候不要使用特殊符号，只要字母和数字。