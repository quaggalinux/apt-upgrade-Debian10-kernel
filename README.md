# apt-upgrade-Debian10-kernel
用标准apt方式升级Debian 10内核 

前阵子被玩家埋怨,说他装的是Debian 10,虽然都是用apt命令,但是完全用不了ubuntu的内核升级命令,要我重新弄个几键出来,唉 (>v<)   
只能认命继续磨刀   
  
转用root用户,注意一定要有 - 号,否则的话可能没有root用户的变量环境  

$su -  

编辑sources.list文件  

#nano /etc/apt/sources.list  
  
文件最后加入下面两行  
  
#Debian Buster Backports.  
deb http://deb.debian.org/debian buster-backports main  
  
保存并退出  
  
更新apt软件库  
  
#apt update  
  
搜索最新内核映像版本  
  
#apt search linux-image  
  
可能会输出很多内核映像版本名字,如果只是要列出最新内核映像版本名字,用下面命令过滤  
  
#apt search linux-image | grep buster-backports  
  
输出大概是下面的样子:  
  
linux-headers-5.8.0-0.bpo.2-amd64/buster-backports 5.8.10-1~bpo10+1 amd64  
linux-headers-5.8.0-0.bpo.2-cloud-amd64/buster-backports 5.8.10-1~bpo10+1 amd64  
linux-image-5.8.0-0.bpo.2-amd64/buster-backports 5.8.10-1~bpo10+1 amd64  
linux-image-5.8.0-0.bpo.2-amd64-dbg/buster-backports 5.8.10-1~bpo10+1 amd64  
linux-image-5.8.0-0.bpo.2-amd64-unsigned/buster-backports 5.8.10-1~bpo10+1 amd64  
linux-image-5.8.0-0.bpo.2-cloud-amd64/buster-backports 5.8.10-1~bpo10+1 amd64  
linux-image-5.8.0-0.bpo.2-cloud-amd64-dbg/buster-backports 5.8.10-1~bpo10+1 amd64  
linux-image-5.8.0-0.bpo.2-cloud-amd64-unsigned/buster-backports 5.8.10-1~bpo10+1 amd64  
  
符号"/"后面的是附加描述没有用,只要"/"前面的名字即可,名字里面没有cloud字样的是带图形桌面驱动的映像,有cloud字样的是不带图形桌面驱动的映像,一般情况下无论是桌面版或服务器版都是安装没有cloud字样的映像比较保险,另外也确保安装标头映像,下面是安装新内核命令  
   
#apt install linux-image-5.8.0-0.bpo.2-amd64 -y   
   
#apt install linux-headers-5.8.0-0.bpo.2-amd64 -y  
   
更新启动的GRUB  
#update-grub  
  
重启机器  
#reboot  
  
重启完成后可能先要删除旧内核文件,因为旧内核文件占好几百兆的硬盘空间  
  
转用root用户,注意一定要有 - 号,否则的话可能没有root用户的变量环境    
  
$su -  
  
检查内核版本,确认内核版本应该是下面输出的最新版本号,否则不能删除旧内核,因为这样机器会启动不了  
  
#uname -a  
  
然后查系统有多少内核文件  
  
#dpkg -l | grep linux-image | awk '{print$2}'  
  
输出大概是下面的样子  
  
linux-image-4.19.0-11-amd64  
linux-image-4.19.0-12-amd64  
linux-image-5.8.0-0.bpo.2-amd64  
linux-image-amd64  
  
删除旧内核  
  
#apt remove --purge linux-image-4.19.0-11-amd64  
  
#apt remove --purge linux-image-4.19.0-12-amd64  
  
再查系统有多少内核文件  
  
#dpkg -l | grep linux-image | awk '{print$2}'  
  
确认都删除好了,重启机器确保能够启动  
  
#reboot  
  

