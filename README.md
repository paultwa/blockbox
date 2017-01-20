# blockbox

////////////////////////////////////////////////////////////  apt-get mirror 设置 /////////////////////////////////////
安装软件之前先把网络设置好
Ubuntu 14.04 的 apt-get 設定 

#> sudo nano /etc/apt/sources.list
aliyun的mirror国内最快,更新也最快   
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse



#> sudo nano /etc/resolv.conf
#google dns
nameserver 8.8.8.8
nameserver 8.8.4.4    
#aliyun dns
nameserver 223.5.5.5   

一样的效果
/etc/resolvconf/resolv.conf.d/base
sudo resolvconf -u


sudo /etc/init.d/networking restart
sudo /etc/init.d/network restart 记得重新启动一个网络服务

网络设置是否有效, 用ping命令确定
///////////////////////////////////////////////////////////////////////////////////////////////////////


//////////////////////  ubuntu14.04 hyperledger fabric项目    预装软件  /////////////////////////////
Git client
Go - 1.7 or later (for releases before v1.0, 1.6 or later)
Docker - 1.12 or later
Pip
//////////////////////////////////////////////////////////////

///////////////////////////////////////////  ubuntu14.04 hyperledger fabric项目 获取   docker image文件///////////////////
docker-machine create --driver=virtualbox fabric-lab 
# 替你的虛擬機起個名#如果無法運作noops,pbft

docker pull hyperledger/fabric-peer         #實驗日期 2017-1-6 ID:21cb00fb27f4
docker pull hyperledger/fabric-membersrvc   #實驗日期 2017-1-6 ID:b3654d32e4f9
docker tag hyperledger/fabric-peer hyperledger/fabric-baseimage:latest
docker pull hyperledger/fabric-starter-kit  #有會員證書
/////////////////////////////////////////////////////




官方资料链接 Setting up the development environment
http://hyperledger-fabric.readthedocs.io/en/latest/dev-setup/devenv/


其他链接
https://github.com/Lursun/somefun?files=1
http://www.itdadao.com/articles/c15a818635p0.html

fabric0.6的操流程分析
http://blog.csdn.net/xjmtxwd24/article/details/54630121

//////////////////////////////////docker command////////////////////
docker run命令
https://docs.docker.com/engine/reference/commandline/run/
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]



/////////////////////////////////////////////////////////////////////

////////////////////////hyperledger fabric  peer command /////////////////////////

peer命令
https://github.com/hyperledger/fabric/blob/master/docs/Setup/Chaincode-setup.md#option-3-docker-toolbox

https://hyperledger-fabric.readthedocs.io/en/latest/Setup/Network-setup/

/////////////////////////////////////////////////////////////////////////
