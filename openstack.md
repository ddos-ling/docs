# OpenStack 安装教程 <!-- {docsify-ignore} --> 
## 1. 准备工作
### 1.1. 配置ip
#### 1）添加2个网卡（外网、内网）
#### 2）修改/etc/sysconfig/network-scripts/ifcfg-ens*（具体的网口）文件
```
[root@localhost ~]# vi /etc/sysconfig/network-scripts/ifcfg-ens192
```
修改以下配置（ip地址和网关根据实际情况进行填写）
```
TYPE=Ethernet
BOOTPROTO=static
ONBOOT=yes
IPADDR=172.16.1.101
PREFIX=24
GATEWAY=172.16.1.254
```
**提醒：无需配置dns**
#### 3）重启网卡
```
[root@openstack ~]# systemctl restart network
```
### 1.2. 上传yum源
通过SecureFXPortable把CentOS源和OpenStack源上传到服务器的/root目录
![logo](openstack-img/1.2-1.png ':size=100')
### 1.3. 关闭防火墙、修改主机名
#### 1）关闭防火墙
```
[root@localhost ~]# systemctl stop firewalld
[root@localhost ~]# systemctl disable firewalld
Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
```
#### 2）关闭selinux
```
[root@localhost ~]# setenforce 0
[root@localhost ~]# vi /etc/selinux/config
# ......
SELINUX=permissive
# ......
SELINUXTYPE=targeted
```
#### 3）更改主机名
```
[root@localhost ~]# hostnamectl set-hostname openstack
[root@localhost ~]# logout
```
重新登录后配置hosts
```
[root@openstack ~]# vi /etc/hosts
```
在文件末尾增加以下内容（ip根据实际情况更改）
```
172.16.1.101 openstack
```
### 1.4. 配置本地yum源
#### 1）新建本地yum源文件夹
```
[root@openstack ~]# cd /opt/
[root@openstack opt]# mkdir centos openstack
```
#### 2）挂载iso镜像并拷贝到本地
```
[root@openstack opt]# mount /root/CentOS-7-x86_64-DVD-1804.iso /mnt/   #挂载镜像
mount: /dev/loop0 is write-protected, mounting read-only
[root@openstack opt]# cp -rp /mnt/* centos/   #复制镜像的内容到本地
[root@openstack opt]# umount /mnt/   #卸载镜像

[root@openstack opt]# mount /root/IaaS-OpenStack-x86-64_v1.0.iso /mnt/
mount: /dev/loop0 is write-protected, mounting read-only
[root@openstack opt]# cp -rp /mnt/* openstack/
[root@openstack opt]# umount /mnt/
```
#### 3）配置yum源
```
[root@openstack ~]# mv /etc/yum.repos.d/* /mnt/
[root@openstack ~]# vi /etc/yum.repos.d/local.repo
```
在新文件local.repo中加入以下内容
```
[centos]
name = centos
gpgcheck = 0
enabled = 1
baseurl = file:///opt/centos

[openstack]
name = openstack
gpgcheck = 0
enabled = 1
baseurl = file:///opt/openstack/iaas-repo
```
> 参数说明
> name: yum源名称
> gpgcheck: 是否开启gpg验证
> enabled: 是否启用这个源
> baseurl: 本地源绝对路径
刷新yum源缓存
```
[root@openstack ~]# yum clean all
[root@openstack ~]# yum makecache 
```
### 1.5. 配置OpenStack相关ip、账号、密码
安装OpenStack一键安装脚本
```
[root@openstack ~]# yum install iaas-openstack -y
```
配置OpenStack一键安装脚本
```
[root@openstack ~]# cd /etc/iaas-openstack/
[root@openstack iaas-openstack]# cp openrc.sh openrc.sh.bak   #备份脚本
[root@openstack iaas-openstack]# sed -i /=/s/#//g openrc.sh   #去除脚本注释
[root@openstack iaas-openstack]# vi openrc.sh
```
openrc.sh配置参考
```
##--------------------system Config--------------------##
##Controller Server Manager IP. example:x.x.x.x
HOST_IP=172.16.1.101

##Controller HOST Password. example:000000 
HOST_PASS=000000

##Controller Server hostname. example:controller
HOST_NAME=openstack

##Compute Node Manager IP. example:x.x.x.x
HOST_IP_NODE=172.16.1.101

##Compute HOST Password. example:000000 
HOST_PASS_NODE=000000

##Compute Node hostname. example:compute
HOST_NAME_NODE=openstack

##--------------------Chrony Config-------------------##
##Controller network segment IP.  example:x.x.0.0/16(x.x.x.0/24)
network_segment_IP=172.16.1.0/24

##--------------------Rabbit Config ------------------##
##user for rabbit. example:openstack
RABBIT_USER=openstack

##Password for rabbit user .example:000000
RABBIT_PASS=000000

##--------------------MySQL Config---------------------##
##Password for MySQL root user . exmaple:000000
DB_PASS=000000

##--------------------Keystone Config------------------##
##Password for Keystore admin user. exmaple:000000
DOMAIN_NAME=xiandian
ADMIN_PASS=000000
DEMO_PASS=000000

##Password for Mysql keystore user. exmaple:000000
KEYSTONE_DBPASS=000000

##--------------------Glance Config--------------------##
##Password for Mysql glance user. exmaple:000000
GLANCE_DBPASS=000000

##Password for Keystore glance user. exmaple:000000
GLANCE_PASS=000000

##--------------------Nova Config----------------------##
##Password for Mysql nova user. exmaple:000000
NOVA_DBPASS=000000

##Password for Keystore nova user. exmaple:000000
NOVA_PASS=000000

##--------------------Neturon Config-------------------##
##Password for Mysql neutron user. exmaple:000000
NEUTRON_DBPASS=000000

##Password for Keystore neutron user. exmaple:000000
NEUTRON_PASS=000000

##metadata secret for neutron. exmaple:000000
METADATA_SECRET=000000

##Tunnel Network Interface. example:x.x.x.x
INTERFACE_IP=172.16.1.101

##External Network Interface. example:eth1
INTERFACE_NAME=ens192

##External Network The Physical Adapter. example:provider
Physical_NAME=provider

##First Vlan ID in VLAN RANGE for VLAN Network. exmaple:101
minvlan=1

##Last Vlan ID in VLAN RANGE for VLAN Network. example:200
maxvlan=1000

##--------------------Cinder Config--------------------##
##Password for Mysql cinder user. exmaple:000000
CINDER_DBPASS=000000

##Password for Keystore cinder user. exmaple:000000
CINDER_PASS=000000

##Cinder Block Disk. example:md126p3
BLOCK_DISK=sdb

##--------------------Swift Config---------------------##
##Password for Keystore swift user. exmaple:000000
SWIFT_PASS=000000

##The NODE Object Disk for Swift. example:md126p4.
OBJECT_DISK=sdc

##The NODE IP for Swift Storage Network. example:x.x.x.x.
STORAGE_LOCAL_NET_IP=172.16.1.101

##--------------------Heat Config----------------------##
##Password for Mysql heat user. exmaple:000000
HEAT_DBPASS=000000

##Password for Keystore heat user. exmaple:000000
HEAT_PASS=000000

##--------------------Zun Config-----------------------##
##Password for Mysql Zun user. exmaple:000000
ZUN_DBPASS=000000

##Password for Keystore Zun user. exmaple:000000
ZUN_PASS=000000

##Password for Mysql Kuryr user. exmaple:000000
KURYR_DBPASS=000000

##Password for Keystore Kuryr user. exmaple:000000
KURYR_PASS=000000

##--------------------Ceilometer Config----------------##
##Password for Gnocchi ceilometer user. exmaple:000000
CEILOMETER_DBPASS=000000

##Password for Keystore ceilometer user. exmaple:000000
CEILOMETER_PASS=000000

##--------------------AODH Config----------------##
##Password for Mysql AODH user. exmaple:000000
AODH_DBPASS=000000

##Password for Keystore AODH user. exmaple:000000
AODH_PASS=000000

##--------------------Barbican Config----------------##
##Password for Mysql Barbican user. exmaple:000000
BARBICAN_DBPASS=000000

##Password for Keystore Barbican user. exmaple:000000
BARBICAN_PASS=000000
```
## 2. 安装OpenStack
### 2.1. 环境配置
```
[root@openstack ~]# iaas-pre-host.sh
[root@openstack ~]# iaas-install-mysql.sh
```
### 2.2. 安装OpenStack组件
#### 2.2.1. 安装Keystone 认证服务
```
[root@openstack ~]# iaas-install-keystone.sh
```
#### 2.2.2. 安装Glance 镜像服务
```
[root@openstack ~]# iaas-install-glance.sh
```
上传CentOS7.5镜像
```
[root@openstack ~]# cd /opt/openstack/images/
[root@openstack images]# source /etc/keystone/admin-openrc.sh 
[root@openstack images]# openstack image create --file CentOS_7.5_x86_64_XD.qcow2 --container-format bare --disk-format qcow2 centos7.5
[root@openstack images]# openstack image list
```
#### 2.2.3. 安装Nova 计算资源管理
```
[root@openstack ~]# iaas-install-nova-controller.sh
[root@openstack ~]# iaas-install-nova-compute.sh
```
#### 2.2.4. 安装Neutron 网络
```
[root@openstack ~]# iaas-install-neutron-controller.sh
[root@openstack ~]# iaas-install-neutron-compute.sh
```
#### 2.2.5. 安装Dashboard Web控制台
```
[root@openstack ~]# iaas-install-dashboard.sh
```
#### 2.2.6. 安装Cinder 存储
```
[root@openstack ~]# iaas-install-cinder-controller.sh
[root@openstack ~]# iaas-install-cinder-compute.sh
```
#### 2.2.7. 安装Swift 对象存储
```
[root@openstack ~]# iaas-install-swift-controller.sh
[root@openstack ~]# iaas-install-swift-compute.sh
```
### 2.3. 重启服务器
```
[root@openstack ~]# reboot
```
## 3. 管理OpenStack