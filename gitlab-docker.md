## GITLAB安装（docker）

###docker安装

> yum install -y docker  
> docker pull sameersbn/gitlab:latest  

##

>docker ps -a

显示容器和镜像

>docker rm  xxxxx  


删除容器
>docker images  


显示镜像
>docker rmi xxxxxx  


删除镜像

>systemctl enable docker  

开机启动docker


###gitlab ce 安装

>sudo docker run --detach \  
    --hostname itcgit.zju.edu.cn \  
    --publish 443:443 --publish 80:80 --publish 22:22 \  
    --name gitlab \  
    --restart always \  
    --volume /srv/gitlab/config:/etc/gitlab \  
    --volume /srv/gitlab/logs:/var/log/gitlab \  
    --volume /srv/gitlab/data:/var/opt/gitlab \  
    gitlab/gitlab-ce:latest  