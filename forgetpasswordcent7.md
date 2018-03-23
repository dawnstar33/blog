启动系统，并在 GRUB2 启动屏显时，按下 e 键进入编辑模式。

在 `linux16/linux/linuxefi` 所在参数行尾添加以下内容： 

	init=/bin/sh

按 `Ctrl + X` 启动到 Shell。
挂载文件系统为可写模式：
>mount -o remount,rw /  

运行 `passwd`， 并按提示修改 `root` 密码。
运行命令
> exec /sbin/init   

来正常启动，或者用命令 
>exec /sbin/reboot   

重启
