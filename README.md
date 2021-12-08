## Welcome to my obscured blog!

<div style="width: 150pt">

### Use autossh

疫情期间一直在家办公，每天需要走openvpn用ssh连接公司网络内机器干活，但是这种连接一般不太稳定ssh经常用着用着就没反应了，后来改用autossh方便了很多，它可以在ssh连接中断后自动重连。
我的办公环境是OpenSUSE TumbleWeed，autossh使用很简单:
```
⚡=❯ rpm -qi autossh
Name        : autossh
Version     : 1.4g
Release     : 2.8
Architecture: x86_64
Install Date: Fri 24 Sep 2021 10:54:48 AM CST
Group       : Productivity/Networking/SSH
Size        : 61277
License     : BSD-3-Clause
Signature   : RSA/SHA256, Sun 19 Sep 2021 12:13:37 AM CST, Key ID b88b2fd43dbdc284
Source RPM  : autossh-1.4g-2.8.src.rpm
Build Date  : Sun 19 Sep 2021 12:13:12 AM CST
Build Host  : lamb55
Packager    : https://bugs.opensuse.org
Vendor      : openSUSE
URL         : https://www.harding.motd.ca/autossh/
Summary     : Automatically restart SSH sessions and tunnels
Description :
Autossh is a program to start a copy of ssh and monitor it, restarting
it as necessary should it die or stop passing traffic. The idea and
the mechanism are from rstunnel (Reliable SSH Tunnel), but implemented
in C. The author's view is that it is not as fiddly as rstunnel to get
to work. Connection monitoring using a loop of port forwardings. Backs
off on rate of connection attempts when experiencing rapid failures
such as connection refused.
Distribution: openSUSE Tumbleweed

⚡=❯ cat ~/.ssh/config

Host youtube-socks5
	HostName  [IP of remote host]		
	User			ydfan
	Port			22
	IdentityFile		~/.ssh/id_rsa
	DynamicForward		8081
	ControlMaster		yes
	ControlPath		~/.ssh/ubuntu.ctl
	ServerAliveInterval	30
	ServerAliveCountMax	60	


⚡=❯ cat ~/.fishrc | grep autossh
alias socks5='autossh -M 9091 -N -f youtube-socks5'
```
按照以上配置后autossh就可以正常运行了，只需执行socks5这个别名，启动后进程显示如下：
```
⚡=❯ ps aux | grep ssh
[...]
ydfan     19082  0.0  0.0   2800   124 ?        Ss   21:08   0:00 autossh -M 9091 -N    youtube-socks5
ydfan     19083  0.0  0.0   9988  6300 ?        S    21:08   0:00 /usr/bin/ssh -L 9091:127.0.0.1:9091 -R 9091:127.0.0.1:9092 -N youtube-socks5
[...]
```
