## 使用 Google 验证器进行二次认证


安装 Google-Authenticator

>yum install google-authenticator  

将 Google-Authenticator 和设备绑定  
运行 google-authenticator，类似如下界面

	Do you want authentication tokens to be time-based (y/n) y
	#是否想做一个基于时间的令牌

	二维码区域  
	二维码区域  
	二维码区域  
	二维码区域  

	Your new secret key is: FFFFFFFFFFFFFFFFFFFFFFFFFF
	Your verification code is 666666
	Your emergency scratch codes are:
 	 11111111
 	 22222222
	 33333333
 	 44444444
 	 55555555

	Do you want me to update your "/root/.google_authenticator" file? (y/n) y
	#是否更新你的google认证文件，由于第一次设置，所以一定选y　
	Do you want to disallow multiple uses of the same authentication
	 token? This restricts you to one login about every 30s, but it increases
	 your chances to notice or even prevent man-in-the-middle attacks (y/n) y
	#是否禁止口令多用，这里选择y，禁止它，以防止中间人欺骗。
	By default, tokens are good for 30 seconds. In order to compensate for
 	possible time-skew between the client and the server, we allow an extra
	 token before and after the current time. If you experience problems with
 	poor time synchronization, you can increase the window from its default
 	size of +-1min (window size of 3) to about +-4min (window size of
 	17 acceptable tokens).
 	Do you want to do so? (y/n) n
	#默认情况，1个口令的有效期是30s，这里是为了防止主机时间和口令客户端时间不一致，设置的误差，可以选择y，也可选n，看要求严谨程度
	If the computer that you are logging into isn't hardened against brute-force
 	login attempts, you can enable rate-limiting for the authentication module.
 	By default, this limits attackers to no more than 3 login attempts every 30s.
 	Do you want to enable rate-limiting (y/n) y
	#是否打开尝试次数限制，默认情况，30s内不得超过3次登陆测试，防止别人暴力破解。
	#上面这些设置将被存储在用户的〜/.google_authenticator文件中，emergency scratch codes 中的5个代码是紧急代码，务必牢记，这是在你的动态口令无法使用的情况下使用的，记住，用一个失效一个。后期可以登陆上去后，重新生成！！

修改 PAM 配置文件添加以允许 Google 验证器方式认证  
>vi /etc/pam.d/sshd

	#%PAM-1.0
	auth required pam_google_authenticator.so
	auth substack password-auth

>vi /etc/ssh/sshd_config

	#允许返回认证提示消息
 	ChallengeResponseAuthentication yes
	#认证类型 (密码+Google 验证器)
	 AuthenticationMethods keyboard-interactive

最后重启 SSH 服务

>systemctl restart sshd.service

###其他认证类型修改

GoogleAuthenticator 在 SSH 认证中会自带密码认证，所以有以下问题

1、证书+密码+Google 验证器

	#%PAM-1.0 
	auth required pam_google_authenticator.so
	auth substack password-auth
	AuthenticationMethods publickey,keyboard-interactive

2、证书+Google 验证器

	#%PAM-1.0 
	auth required pam_google_authenticator.so
	#auth substack password-auth
	AuthenticationMethods publickey,keyboard-interactive

3、密码+Google 验证器

	#%PAM-1.0 
	auth required pam_google_authenticator.so
	auth substack password-auth
	AuthenticationMethods keyboard-interactive



参考文章 https://blog.starryvoid.com/archives/247.html