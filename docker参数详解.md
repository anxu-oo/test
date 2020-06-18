---

---

### docker参数详解

| 参数                                             | 解释                             |
| ------------------------------------------------ | -------------------------------- |
| --storage-opt size=30G                           | 创建容器时设置docker容器磁盘大小 |
| docker ps -a --format='{{.Names}}'               | 获取所有容器名字                 |
| docker ps -f name=test --format='{{.Names}}'     | 包含test的容器名字               |
| docker ps -f status=exited --format="{{.Names}}" | 关机状态的容器                   |
| docker rm -f -v $(docker ps -a -q)               | 删除所有容器                     |
| docker rmi $(docker images -q)                   | 删除所有镜像                     |
| --restart=always                                 | 创建容器时设置容器随daemon启动   |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |
|                                                  |                                  |

### docker info 详解

```c
Server Version: 1.12.6       　　# 服务器版本：1.12.6
Storage Driver: devicemapper    # 存储驱动程序：devicemapper
Pool Name: docker-8:3-2097465-pool    # 池名称：docker-8：3-2097465-pool
Pool Blocksize: 65.54 kB    　　# Pool Blocksize：65.54 kB
Base Device Size: 107.4 GB    # 基本设备大小：107.4 GB
Backing Filesystem: ext4    　　# 支持文件系统：ext4
Data file: /dev/loop0    　　# 数据文件：/ dev / loop0
Metadata file: /dev/loop1    # 元数据文件：/ dev / loop1
Data Space Used: 5.352 GB    # 使用的数据空间：5.352 GB
Data Space Total: 107.4 GB    # 数据空间总计：107.4 GB
Data Space Available: 10.04 GB    # 可用数据空间：10.04 GB
Metadata Space Used: 7.229 MB    # 使用的元数据空间：7.229 MB
Metadata Space Total: 2.147 GB    # 元数据空间总计：2.147 GB
Metadata Space Available: 2.14 GB    # 可用元数据空间：2.14 GB
Thin Pool Minimum Free Space: 10.74 GB    # 精简池最小可用空间：10.74 GB
Udev Sync Supported: true    　　# Udev Sync支持：true
Deferred Removal Enabled: true    　　# 延迟删除已启用：true
Deferred Deletion Enabled: true    　　# 延迟删除已启用：true
Deferred Deleted Device Count: 0    　　# 延迟删除的设备数：0
Data loop file: /var/lib/docker/devicemapper/devicemapper/data    # 数据循环文件：/ var / lib / docker / devicemapper / devicemapper / data
WARNING: Usage of loopback devices is strongly discouraged for production use. Use `--storage-opt dm.thinpooldev` to specify a custom block storage device.    # 警告：强烈建议不要使用环回设备进行生产。使用`--storage-opt dm.thinpooldev`指定自定义块存储设备。
Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata    # 元数据循环文件：/ var / lib / docker / devicemapper / devicemapper / metadata
Library Version: 1.02.107-RHEL7 (2015-10-14)    版本：1.02.107-RHEL7（2015-10-14）
Logging Driver: journald    # 日志驱动程序：journald
Cgroup Driver: systemd    # Cgroup驱动程序：systemd
Plugins:    　　# 插件：
Volume: local    # 存储卷：本地
Network: bridge host null overlay    # 网络：桥接主机空覆盖
Swarm: inactive    # Swarm：不活跃
Runtimes: docker-runc runc    # 运行时：docker-runc runc
Default Runtime: docker-runc    # 默认运行时：docker-runc
Security Options: seccomp    # 安全选项：seccomp
Kernel Version: 3.10.0-327.el7.x86_64    # 内核版本：3.10.0-327.el7.x86_64
Operating System: CentOS Linux 7 (Core)    # 操作系统：CentOS Linux 7（核心）
OSType: linux    # 系统类别：linux
```