VMM
====

一个管理本地kvm虚拟机的小脚本，快速生成虚拟机并启动，方便在本地使用虚拟机做测试

使用方法
---------

```
Usage: vmm [OPTIONS] COMMAND [arg...]

Create and manage virtual machines.

Options:
  --help, -h                        show help
  --version, -v                     print the version

Commands:
  active        Print which vm is active
  create        Create a vm
  inspect       Inspect information about a vm
  ip            Get the IP address of a vm
  kill          Kill a vm
  ls            List vms
  restart       Restart a vm
  rm            Remove a vm
  ssh           Log into or run a command on a vm with SSH.
  start         Start a vm
  status        Get the status of a vm
  stop          Stop a vm
  console       Open console of a vm
  ping          Send ping packages to vm's ip address

e.g.:
  vmm start vm1         # start one vm
  vmm start vm{2..10}   # start more vms
  vmm stop vm{1..3}     # stop vm1 vm2 vm3

```

更多选项使用 --help 查看帮助信息

环境依赖
---------

1 手工配置好名为 br0 的网桥

```
# cat /etc/sysconfig/network-scripts/ifcfg-br0
DEVICE=br0
TYPE=Bridge
BOOTPROTO=static
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=no
IPV6_AUTOCONF=no
IPV6_DEFROUTE=no
NAME=br0
ONBOOT=yes
STP=on
DELAY=0
IPADDR=192.168.199.122
NETMASK=255.255.255.0
GATEWAY=192.168.199.1

# cat /etc/sysconfig/network-scripts/ifcfg-enp3s0f0
DEVICE=enp3s0f0
ONBOOT=yes
BRIDGE=br0
```

2 修改 vmm 脚本中可分配的起始IP地址及网关掩码等信息

配置说明
--------

支持自定义系统盘和数据盘大小，修改脚本中
VmRootDiskSize 和 VmDataDiskSize 两个变量即可

其中，centos7镜像系统盘默认进行了分区，且使用xfs文件系统，可以在镜像中修改

/etc/rc.d/rc.local增加以下命令来自动扩容系统盘容量

```
yum -y install cloud-utils-growpart
cat >>/etc/rc.d/rc.local <<EOF
growpart /dev/vda 1
xfs_growfs /dev/vda1
EOF
```

TODO
-----

支持自定义模版

支持自定义cpu 内存 磁盘
