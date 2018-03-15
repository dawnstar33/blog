


## 光盘安装选项：可调整默认磁盘分区

## 添加网络


> vi /etc/sysconfig/network-scripts/ifcfg-ens160

多网卡配置，单网卡直接写ipaddr不带0；多网卡用ip命令

    TYPE=Ethernet  
    PROXY_METHOD=none  
    BROWSER_ONLY=no  
    BOOTPROTO=static  
    DEFROUTE=yes  
    IPV4_FAILURE_FATAL=no  
    IPV6INIT=yes  
    IPV6_AUTOCONF=yes  
    IPV6_DEFROUTE=yes  
    IPV6_FAILURE_FATAL=no  
    IPV6_ADDR_GEN_MODE=stable-privacy  
    NAME=ens160  
    UUID=4ec31c2d-76b0-4de8-84a0-232ef19eba1b  
    DEVICE=ens160  
    ONBOOT=yes  
    IPADDR0=1.1.1.3  
    IPADDR1=1.1.1.4  
    PREFIX0=24  
    PREFIX1=24  
    #GATEWAY0=1.1.1.1
    #GATEWAY1=1.1.1.2
    DNS1=10.10.0.21  
    DNS2=10.10.2.53

> service network restart

## 添加内网yum源及epel源
> cd /etc/yum.repos.d/  
> mkdir old  
> mv * old/  
> vi CentOS-Base.repo  

    [base]
    name=CentOS-$releasever - Base
    baseurl=http://mirrors.zju.edu.cn/centos/$releasever/os/$basearch/
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    
    [updates]
    name=CentOS-$releasever - Updates
    baseurl=http://mirrors.zju.edu.cn/centos/$releasever/updates/$basearch/
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    
    [extras]
    name=CentOS-$releasever - Extras
    baseurl=http://mirrors.zju.edu.cn/centos/$releasever/extras/$basearch/
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    
> yum update -y   
> yum install epel-release -y  
> vi /etc/yum.repos.d/epel.repo 
    
    [epel]
    name=Extra Packages for Enterprise Linux 7 - $basearch
    baseurl=http://mirrors.zju.edu.cn/epel/7/$basearch
    #mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch
    failovermethod=priority
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
     
    [epel-debuginfo]
    name=Extra Packages for Enterprise Linux 7 - $basearch - Debug
    baseurl=http://mirrors.zju.edu.cn/epel/7/$basearch/debug
    #mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-debug-7&arch=$basearch
    failovermethod=priority
    enabled=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
    gpgcheck=1
     
    [epel-source]
    name=Extra Packages for Enterprise Linux 7 - $basearch - Source
    baseurl=http://mirrors.zju.edu.cn/epel/7/SRPMS
    #mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-source-7&arch=$basearch
    failovermethod=priority
    enabled=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
    gpgcheck=1

> vi /etc/yum.repos.d/epel-testing.repo

    [epel-testing]
    name=Extra Packages for Enterprise Linux 7 - Testing - $basearch
    baseurl=http://mirrors.zju.edu.cn/epel/testing/7/$basearch
    #mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=testing-epel7&arch=$basearch
    failovermethod=priority
    enabled=0
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
    
    [epel-testing-debuginfo]
    name=Extra Packages for Enterprise Linux 7 - Testing - $basearch - Debug
    baseurl=http://mirrors.zju.edu.cn/epel/testing/7/$basearch/debug
    #mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=testing-debug-epel7&arch=$basearch
    failovermethod=priority
    enabled=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
    gpgcheck=1
    
    [epel-testing-source]
    name=Extra Packages for Enterprise Linux 7 - Testing - $basearch - Source
    baseurl=http://mirrors.zju.edu.cn/epel/testing/7/SRPMS
    #mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=testing-source-epel7&arch=$basearch
    failovermethod=priority
    enabled=0
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
    gpgcheck=1

> yum repolist  
> yum clean all && yum makecache

## selinux关闭
> setenforce 0  
> vi /etc/selinux/config

## 时间同步
### ntpdate可能引发时间倒退，待修改
> yum install ntpdate -y  
> /usr/sbin/ntpdate time.zju.edu.cn  
> crontab -e  
    1 1 * * 0 /usr/sbin/ntpdate time.zju.edu.cn

## 防火墙
### 使用iptables
> systemctl stop firewalld   
> systemctl disable firewalld   
> yum install iptables-services -y  
> systemctl start iptables   
> systemctl enable iptables

如果需要web加80，或修改

> /sbin/iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT  
> /sbin/iptables -A INPUT -i lo -j ACCEPT  
> /sbin/iptables -I INPUT -p tcp --dport 22 -j ACCEPT  
> /sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT  
> /sbin/iptables -I INPUT -p tcp --dport 443 -j ACCEPT  
> /sbin/iptables -P INPUT DROP  
> /sbin/iptables -P FORWARD DROP  
> service iptables save

###使用firewalld
> 待补充


## 防暴力破解sshd
### 会导致root锁死，待思考更优化方式
> 
> vi /etc/pam.d/login  
> vi /etc/pam.d/sshd

加在第一行

    auth  required  pam_tally2.so   deny=10 lock_time=600 even_deny_root root_unlock_time=600

查看锁定的账户信息

> pam_tally2 --user

解锁账户

> pam_tally2 -r -u xxxx

## SSH允许列表（若需要）
> vi /etc/hosts.allow

    sshd:1.1.1.*:allow
    sshd:1.1.2.2:allow
    sshd:all:deny
