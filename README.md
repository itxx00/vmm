VMM
====

一个管理本地kvm虚拟机的小脚本，快速生成虚拟机并启动，方便在本地使用虚拟机做测试

使用方法
---------

```
  vmm create vm1  创建名为vm1的虚拟机
  vmm ssh vm1     自动SSH登陆vm1
  vmm rm vm1      销毁vm1的配置和存储
  vmm ls          查看所有虚拟机


```

更多选项使用 --help 查看帮助信息

环境依赖
---------

1 手工配置好名为 br0 的网桥

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
