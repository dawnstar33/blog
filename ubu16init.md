光盘安装时配置好网络IP，勾选openssh server后起步

## 添加或修改网络


> vi /etc/network/interfaces

典型配置

    source /etc/network/interfaces.d/*

	# The loopback network interface
	auto lo
	iface lo inet loopback

	# The primary network interface
	auto ens9f1
	iface ens9f1 inet static
        address 1.1.1.2
        netmask 255.255.255.0
        network 1.1.1.0
        broadcast 1.1.1.255
        gateway 1.1.1.1
        # dns-* options are implemented by the resolvconf package, if installed
        dns-nameservers 2.2.2.2


> /etc/init.d/networking restart   
  
多网卡时，查询网卡名：  

>cd /proc/sys/net/ipv4/conf  


## 添加内网apt源
> cd /etc/apt/
> cp sources.list sources.list.bak
> vi sources.list

    deb http://mirrors.zju.edu.cn/ubuntu xenial main universe restricted multiverse
	deb http://mirrors.zju.edu.cn/ubuntu xenial-security main universe restricted multiverse
	deb http://mirrors.zju.edu.cn/ubuntu xenial-updates main universe restricted multiverse
	deb http://mirrors.zju.edu.cn/ubuntu xenial-backports main universe restricted multiverse
	deb-src http://mirrors.zju.edu.cn/ubuntu xenial main universe restricted multiverse
	deb-src http://mirrors.zju.edu.cn/ubuntu xenial-security main universe restricted multiverse
	deb-src http://mirrors.zju.edu.cn/ubuntu xenial-updates main universe restricted multiverse
	deb-src http://mirrors.zju.edu.cn/ubuntu xenial-backports main universe restricted multiverse

> apt-get update && apt-get upgrade