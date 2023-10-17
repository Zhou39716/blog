## # 华为数通设备基础命令大全

### Telnet配置

###### 1、密码登录

```
[Huawei]user-interface console 0  //进入管理控制口
[Huawei-ui-console0]authentication-mode password
Please configure the login password (maximum length 16):输入密码
[Huawei-ui-console0]user privilege level ?   //设置特权等级
[Huawei-ui-console0]idle-timeout 20 0  //设置空闲超时时间为20分钟，默认为10分钟
```

###### 2、用户和密码登录

```
[Huawei]user-interface console 0  //进入管理控制口
[Huawei-ui-console0]authentication-mode aaa
[Huawei-ui-console0]quit
[Huawei]aaa
[Huawei-aaa]local-user huawei password cipher hw(密码)
[Huawei-aaa]local-user huawei service-type terminal  //从终端登录
```

###### 3、PC通过以太网口远程登录配置

```
#AAA模式远程登录
[R1]user-interface vty 0 4
[R1-ui-vty0-4]authentication-mode aaa
[R1]aaa  //进入认证、授权、计费模式（简称AAA模式）
[R1-aaa]local-user admin password cipher 123456  //配置本地用户和密码
[R1-aaa]local-user admin service-type telnet  //选择授权用户的服务类型
[R1-aaa]local-user admin privilege level 3  //设置管理员用户级别
```

```
#仅密码远程登录
[R2]user-interface vty 0 4  //虚拟用户终端接口
[R2-ui-vty0-4]authentication-mode password  //配置用户终端的认证模式
[R2-ui-vty0-4]set authentication password simple 123456  //设置登录用户密码
[R2-ui-vty0-4]user privilege level 3  //设置用户等级
[R2-ui-vty0-4]user network-admin  //设置登录用户的参数
```

###### 4、设置超级密码

```
[Huawei]super password level 3 cipher 123456(作用？)
```

### FTP配置

###### 1、将路由器作为FTP服务器

```
[R1]ftp server enable
[R1]aaa
[R1-aaa]local-user winda password cipher aqx123456(字母与数字的组合)
[R1-aaa]local-user winda privilege level 15
[R1-aaa]local-user winda service-type ftp
[R1-aaa]local-user winda ftp-directory flash:  //指定目录
```

###### 2、登录FTP服务器，并传送文件

```
<R1>ftp 10.0.12.2
User(10.0.12.2:(none)):huawei
331 Password required for huawei.
Enter password:
230 User logged in.
[R1-ftp]put portalpage.zip  //上传文件
[R1-ftp]get portalpage.zip portal.zip  //下载文件并改名字
<R1>delete /unreserved portal.zip  //彻底删除该文件
```

### 其它配置

###### 配置登录标语

```
[R1]header shell information "Welcome to HuaWei"
```

###### 指定下次启动装载的配置文件

```
<R1>startup saved-configuration iascfg.zip
```

###### 指定下次启动装载的配置文件

```
<R1>factory-configuration reset
```

###### 设置下次启动用的文件

```
<R1>startup saved-configuration 文件名
```

###### 删除保存的配置文件

```
<R1>reset saved-configuration
<R1>reboot
Warning: All the configuration will be saved to the next startup configuration. 
Continue ? [y/n]:n
```

###### 关闭消息提示功能

```
[R1]undo info-center enable  //关闭提示信息，提高配置时的视觉
或
<R1>undo terminal monitor  //关闭日志提示消息（很烦人的一个东西）
```

## 路由IP配置

###### 浮动静态路由

```
[Huawei]ip route-static 192.168.1.0 24（目标网络） 192.168.2.1（下一跳IP） preference 80
```

###### 描述接口

```
[R1-Serial1/0/0]description this port connect to R2-S1/0/0
```

###### 默认（缺省）路由

```
[R1]ip route-static 0.0.0.0 0.0.0.0 10.0.13.3
```

###### 使用带源参数的ping命令

```
[R2]ping -a 10.0.2.2 10.0.3.3
```

###### 配置从IP地址（解决RIPv1不连续子网）

```
[R1-Serial1/0/0]ip add 10.0.23.2 sub
```

###### 静态路由与BFD联动

```
#激活BFD功能
[R1]bfd
[R1-bfd]quit

#创建BFD会话，名称为ab（自定义），对端的IP地址为10.1.12.2
[R1]bfd ab bind peer-ip 10.1.12.2
```

## VLAN配置

### 端口类型配置

###### 配置端口为access

```
[Huawei]vlan 10  //创建vlan 10
[Huawei]interface GigabitEthernet 0/0/2  //进入g0/0/2端口
[Huawei-GigabitEthernet0/0/2]port link-type access  //选择端口类型
[Huawei-GigabitEthernet0/0/2]port default vlan 10  //划分VLAN

技巧：[S2]vlan batch 3 to 5  //创建VLAN3、4、5
```

###### 配置端口的Hybrid类型

```
[SW1]vlan 10
[SW1-GigabitEthernet0/0/1]port hybrid pvid vlan 10
[SW1-GigabitEthernet0/0/1]port hybrid untagged vlan 10 30  //对VLAN10和30去标签
```

###### 配置端口的Trunk类型

```
[SW1]interface g0/0/24
[SW1-GigabitEthernet0/0/24]port link-type trunk
[SW1-GigabitEthernet0/0/24]port trunk allow-pass vlan all  //允许所有VLAN通过
```

###### 更改Trunk端口的PVID

```
[SW1-GigabitEthernet0/0/24]port trunk pvid vlan 10
```

### Eth-trunk配置

###### 创建Eth-trunk 1，并将接口加入Eth-trunk 1

```
[S1]interface Eth-Trunk 1
[S1]int g0/0/9
[S1-GigabitEthernet0/0/9]eth-trunk 1
或
[S1]interface Eth-Trunk 1
[S1-Eth-Trunk1]trunkport g0/0/9
```

###### 配置Eth-trunk 1链路配置为access

```
[S1]interface Eth-Trunk 1
[S1-Eth-Trunk1]port link-type access
[S1-Eth-Trunk1]port default vlan 5
```

## STP配置

### STP配置

```
#修改桥优先级
[S1]stp priority 4096  //数值越小优先级越高

#修改端口优先级
[S1-GigabitEthernet0/0/9]stp port priority 32    //默认为128，数值越小优先级越高

#配置边缘端口（使端口快速进入转发状态）
[S3]interface Ethernet0/0/3
[S3-Ethernet0/0/3]stp edged-port enable

[S3-Ethernet0/0/3]stp cost  //设置路径开销值

[根交换机]stp timer forward-delay  //设置延迟时间

[根交换机]stp edged-port enable  //设置网络直径
```

### 配置MSTP

```
[SW1]stp region-configuration  //进入MST域
[SW1-mst-region]region-name huawei  //配置域名
[SW1-mst-region]revision-level 1  //配置修订级别为1
[SW1-mst-region]instance 1 vlan 10  //制定MSTI 1与VLAN10的映射
```

### 查看STP

```
#各接口简要STP状态
[S1]dis stp brief

#具体接口详细STP信息
[S1]dis stp interface g0/0/10

#查看当前根桥信息
[S1]dis stp

#查看实例1的信息
[SW1]dis stp instance 1 brief
```

## DHCP配置

### 配置DHCP

###### 基于接口配置DHCP功能

```
[R1]dhcp enable  //开启DHCP功能
[R1-GigabitEthernet0/0/0]dhcp select interface  //开启接口DHCP功能
[R1-GigabitEthernet0/0/0]dhcp server lease day 2【可选】    //设置租用有效期限为2天
[R1-GigabitEthernet0/0/0]dhcp server dns-list 8.8.8.8    //为PC自动分配DNS服务器地址
[R1-GigabitEthernet0/0/1]dhcp server excluded-ip-address 192.168.2.250 192.168.2.253    //配置不参与自动分配的IP地址范围
```

###### 基于全局配置DHCP

```
[R1]dhcp enable  //开启DHCP功能
[R1]ip pool huawei1  //配置全局地址池
[R1-ip-pool-huawei1]network 192.168.1.0  //指定地址池范围
[R1-ip-pool-huawei1]lease day 2【可选】  //租期为2天，默认1天
[R1-ip-pool-huawei1]gateway-list 192.168.1.254  //出口网关地址
[R1-ip-pool-huawei1]excluded-ip-address 192.168.1.250 192.168.1.253  //不参与自动分配
[R1-ip-pool-huawei1]dns-list 8.8.8.8  //配置DNS服务器地址
[R1-GigabitEthernet0/0/0]dhcp select global  //开启接口的DHCP功能
```

###### 查看

```
dis ip pool  //查看地址池地址分配情况
```

### DHCP中继

```
[R1]dhcp enable  //开启DHCP功能

#面向PC的接口：
[R1-Ethernet0/0/0]dhcp select relay
[R1-Ethernet0/0/0]dhcp relay server-ip 100.1.1.1  //指定DHCP服务器IP地址

#面向PC的接口下调用全局定义的DHCP服务器组:
[R1]dhcp server group dhcp-group
[R1-dhcp-server-group-dhcp-group]dhcp-server 100.1.1.1
[R1-Ethernet0/0/0]dhcp select relay 
[R1-Ethernet0/0/0]dhcp relay server-select dhcp-group
```

## 帧中继配置

### DCE配置

```
[R1]interface s1/0/0
[R1-Serial1/0/0]link-protocol fr
[R1-Serial1/0/0]fr interface-type dce  //接口类型
[R1-Serial1/0/0]fr dlci 100
[R1-Serial1/0/0]ip add 192.168.1.1 24
```

### DTE配置

```
[R2]interface s1/0/0
[R2-Serial1/0/0]link-protocol fr
[R2-Serial1/0/0]fr interface-type dte
```

### 其它配置

```
查看PVC
[R1]dis fr pvc-info

#查看映射
[R1]dis fr map-info

#静态映射
[R2-Serial1/0/0]undo fr inarp
[R1-Serial1/0/0]fr map ip 192.168.1.2 100 broadcast
//OSPF中使用组播的网络类型时，应在映射后面加Broadcast
```

## 单臂路由配置

###### 配置路由器

```
[R1]int g0/0/0.1  //创建并进入g0/0/0的0.1子接口
[R1-GigabitEthernet0/0/0.1]dot1q termination vid 10  //封装dot1q协议
[R1-GigabitEthernet0/0/0.1]ip address 192.168.10.254 24  //配置IP地址
[R1-GigabitEthernet0/0/0.1]arp broadcast enable  //开启arp广播
[R1-GigabitEthernet0/0/0.1]dhcp select global  //开启DHCP全局模式

[R1-GigabitEthernet0/0/0.1]int g0/0/0.2
[R1-GigabitEthernet0/0/0.2]dot1q termination vid 20
[R1-GigabitEthernet0/0/0.2]ip address 192.168.20.254 24
[R1-GigabitEthernet0/0/0.2]arp broadcast enable
[R1-GigabitEthernet0/0/0.2]dhcp select global
```

###### 配置交换机

```
[SW1]vlan batch 10 20  //创建vlan 10和vlan 20
[SW1-GigabitEthernet0/0/1]int g0/0/1  //进入g0/0/1接口
[SW1-GigabitEthernet0/0/1]port link-type access  //修改端口为access模式
[SW1-GigabitEthernet0/0/1]port default vlan 10  //将端口划分到vlan 10中

[SW1-GigabitEthernet0/0/1]int g0/0/2
[SW1-GigabitEthernet0/0/2]port link-type access
[SW1-GigabitEthernet0/0/2]port default vlan 20

[SW1-GigabitEthernet0/0/2]int g0/0/24  //交换机与路由器相连的接口需要修改为trunk模式
[SW1-GigabitEthernet0/0/24]port link-type trunk  //修改端口为trunk模式
[SW1-GigabitEthernet0/0/24]port trunk allow-pass vlan 10 20  //允许vlan 10和vlan 20的数据通过
```

## RIP配置

### 基本配置

```
[Huawei]rip 1  //启动RIP进程
[Huawei-rip-1]version 2  //选择版本（可选）
[Huawei-rip-1]net 10.0.0.0(通告自然网段)
```

### 其它配置

```
[Huawei-rip-1]summary always  //使能自动汇总[V2默认开启，但不生效]
[接口]undo rip split-horizon  //关闭接口水平分割
[Huawei-GigabitEthernet0/0/0]rip summary-address 聚合后网络 子网掩码  //手动汇总

#接口附加度量值
[接口]rip metricin x  //cost=原来的值+x
[接口]rip metricout x  //cost=0+x

#认证
[R2-Serial1/0/0]rip authentication-mode simple huawei  //明文
[R2-Serial2/0/0]rip authentication-mode md5 usual huawei  //MD5
```

```
#开启RIP调试
<R1>debug rip 1
<R1>terminal debugging
Info: Current terminal debugging is on.
<R1>terminal monitor

#抑制接口（只收不发）
[R1-GigabitEthernet0/0/0]undo rip output  //不能使用单播通信
或
[R2-rip-1]silent-interface E0/0/0  //优先级高于前者
```

```
[R2-rip-1]peer IP地址  //单播通信
[R2-rip-1]preference x  //修改优先级（只在本地有效）
[R2-rip-1]timers rip 20 120 60  //修改定时器
[R3]ip route-static 0.0.0.0 0 LoopBack 2  //发布默认路由
[R3-rip-1]default-route originate  //不需创建默认路由，也能发布
<Huawei>reset rip 1 statistics  //刷新RIP统计信息
<R1>reset ip routing-table statistics protocol rip  //清除RIP学到的路由信息
[R2]undo rip 1  //删除RIP
[R1]rip version2 multicase  //配置版本1的也能发送RIPv2报文
```

## OSPF配置

### 配置

###### 配置OSPF

```
[R1]ospf Router-id 1.1.1.1  //配置环回口IP地址
[R1-ospf-1]area 0
[R1-ospf-1-area-0.0.0.0]network 1.1.1.0 0.0.0.255
```

###### 引入路由

```
[R3-ospf-1]import-route direct  //引入直连路由
[R1-ospf-1]import-route rip 1 cost 100  //引入RIP路由
```

###### 发布默认路由

```
[R3]ip route-static 0.0.0.0 0 LoopBack 2
[R3]ospf 100
[R3-ospf-100]default-route-advertise
[R3-ospf-100]default-route-advertise always  //自动发布默认路由
```

###### OSPF验证

```
#接口认证明文：
[R1-Serial1/0/0]ospf authentication-mode simple plain huawei

#区域认证密文：
[R1-ospf-1]area 1
[R1-ospf-1-area-0.0.0.1]authentication-mode md5 1 cipher huawei
```

###### 抑制接口（不收不发）

```
R2-ospf-1]silent-interface E0/0/0

[R2-ospf-1]peer IP地址  //单播通信
```

### 查看

```
[R1]dis ospf peer brief  //查看邻居
[R1]dis ospf interface  //查看DR、BDR
[R1]reset ospf process  //重置OSPF进程
[R1]dis ip routing-table protocol ospf  //查看OSPF学到的路由
[R1]display ospf lsdb ase 172.16.0.0  //显示链路状态数据库的外接路由信息
[R1]dis ospf int G0/0/0  //查看接口的OSPF信息
```

###### 查看LSA信息

```
[R1]dis ospf lsdb router  //查看一类LSA
[R1]dis ospf lsdb netword  //查看二类LSA
[R1]dis ospf lsdb summary  //查看三类LSA
[R1]dis ospf lsdb asbr  //查看四类LSA
[R1]dis ospf lsdb ase  //查看五类LSA
[R1]dis ospf lsdb nssa  //查看七类LSA
```

### 其它

###### 修改Hello和Dead时间

```
[R1-GigabitEthernet0/0/0]ospf timer hello 15
[R1-GigabitEthernet0/0/0]ospf timer dead 60
```

###### 修改DR优先级

```
[R1-GigabitEthernet0/0/0]ospf dr-priority 200（越大优先级越高）
```

> 注：由于DR/BDR选举默认为不抢占模式，因此在修改了路由器优先级后不会自动重新选举DR，需要重置OSPF进程。

###### 修改网络类型为广播

```
[R2-Serial2/0/0]ospf network-type broadcast
```

###### 修改开销值、带宽参考值

```
[接口]ospf cost x
[R3-ospf-1]bandwidth-reference x
```

## VRRP配置

```
[SW1-Vlanif10]vrrp vrid 1 virtual-ip 10.1.1.254  //配置虚拟网关
[SW1-Vlanif10]vrrp vrid 1 priority 150  //更改优先级
[SW1-Vlanif10]vrrp vrid 1 preempt-mode disable  //关闭抢占模式
[SW1-Vlanif10]vrrp vrid 1 track interface g0/0/24 reduced 60  //跟踪上层端口
[R1-Serial2/0/0]ospf network-type p2mp  //配置接口的网络类型为Point-to-multipoint
[S1] display vrrp  //查看VRRP信息
```

## HDLC配置

```
[R1-Serial1/0/0]ip address unnumbered interface LoopBack0  //IP地址借用
[R1]ip route-static 10.1.1.0 24 Serial1/0/0
[R1-Serial1/0/0]link-protocol hdlc  //将接口改为HDLC类型
```

## PPP配置

###### 认证方

```
[R1]aaa
[R1-aaa]local-user huawei password cipher 123456
[R1-aaa]local-user huawei service-type ppp
[R1-Serial0]ppp authentication-mode pap（CHAP）
```

###### 被认证方

```
#PAP：
[R2-Serial0]ppp pap local-user huawei password cipher 123456 

#CHAP：
[R2-Serial0]ppp chap user huawei
[R2-Serial0]ppp chap password cipher 123456
```

###### 使用 CHAP 建立 PPP连接的协商过程

```
<R2>debugging ppp chap all
<R2>terminal debugging
```

## 以太网接口配置

```
[S1-G0/0/9]undo negotiation auto  //更改接口的速率和双工模式前应先关闭接口的自动协商功能
[S1-G0/0/9]speed 100  //设置为100M速率
[S1-G0/0/9]duplex full  //全双工模式
[S1]dis eth-trunk 1  //查看Eth-trunk 1配置结果
```

## 防火墙配置

```
#登录不需要用户名和密码
[FW]user-interface console 0
[FW-ui-console0]authentication-mode none

#定义时区：
<FW1>clock timezone 1 add 08:00:00
```

## 链路技术

###### 链路聚合

```
[S1]interface Eth-Trunk 1  //聚合交换机之间的Eth-Trunk端口编号相等
[S1-Eth-Trunk1]mode lacp-static  //lacp模式
[S1-Eth-Trunk1]trunkport GigabitEthernet 0/0/1 to 0/0/2  //将成员端口加入到聚合端口中
[S1-Eth-Trunk1]max active-linknumber 2  //设置活动上限阀值
[S1-Eth-Trunk1]port link-type trunk 
[S1-Eth-Trunk1]port trunk allow-pass vlan all
[S1-Eth-Trunk1]dis eth-trunk 1 verbose  //查看聚合端口的信息
[S1] lacp priority 100  //配置系统优先级使其成为主控端
[S1-G0/0/1] lacp priority 100  //配置活动链路优先级
[S1-G0/0/2] lacp priority 100  //配置活动链路优先级
```

###### Smart Link

```
[S1-G0/0/1]stp disable  //关掉相关接口的STP
[S1]smart-link group 1
[S1-smlk-group1]port GigabitEthernet 0/0/1 master  //设置主端口
[S1-smlk-group1]port g0/0/2 slave  //设置从端口
[S1-smlk-group1]flush send control-vlan 10 password simple 123  //使能smart link组1发送Flush帧的功能，携带的控制VLAN编号为10，密码是：123
[S1-smlk-group1]restore enable  //开启回切功能
[S1-smlk-group1]timer wtr 30  //回切时间为30秒
[S1-smlk-group1]smart-link enable  //使能Smart Link组1的功能
[S1-smlk-group1]dis smart-link group 1  //查看Smart Link组1的信息
[S2-GigabitEthernet0/0/1]smart-link flush receive control-vlan 10 password simple 123  //设置其他交换机可以接收和处理携带控制VLAN编号是10的Flush帧
```

## 其他配置

### 自动保存

```
#自动保存
<R1>autosave interval on
<R1>autosave interval 120  //每隔120分钟自动保存

#定点保存
<R1>autosave time on
<R1>autosave time 23:00:00  //到23点自动保存
```

### 端口镜像

```
[R1]observe-port 1 interface Ethernet4/0/1  //设置观察端口
[接口] mirror to observe-port 1 both  //将镜像端口映射到观察端口
```

### 端口绑定

```
[Huawei]user-bind static mac-address 5489-988B-5157 int e0/0/1 //绑定MAC地址
[Huawei]user-bind static ip-address 192.168.1.3  //绑定IP地址
```

### 端口安全

```
[接口]port-security enable  //启用端口安全
[接口]port-security max-mac-num 2  //自动学习MAC地址的最大数量
[接口]port-security protect-action shutdown  //保护行为为关闭端口
```

### 前缀列表

```
[R1]ip ip-prefix 1 deny 11.1.1.0 25 greater-equal 25 less-equal 25  //在RIP里面过滤掉11.1.1.0/25
[R1]ip ip-prefix 1 permit 0.0.0.0 0 less-equal 32
[R1-rip-1]filter-policy ip-prefix 1 import
```

### IPv6配置

```
[R1]ipv6  //开启全局IPv6功能
[R1-E0/0/0]ipv6 enable  //在接口（连接PC）下开启IPv6功能
[R1-E0/0/0]ipv6 address auto link-local  //自动生成链路本地地址
[R1-G0/0/0]ipv6 enable  //在接口（连接路由器）下开启IPv6功能
[R1-G0/0/0]ipv6 add 2031:0:130F::1 64  //配置全球单播地址

[R1-E0/0/0]ipv6 add 2001:3:FD:: 64 eui-64  //用EUI-64配置地址
```

## 设备版本升级

###### 检查设备剩余空间是否大于新的软件包大小

```
<H07_S5720_BMC-05>dir flash:
Directory of flash:/

  Idx  Attr     Size(Byte)  Date        Time       FileName 
    0  drw-              -  Oct 30 2019 03:37:16   dhcp
    1  drw-              -  Oct 30 2019 03:19:15   user
    2  -rw-         13,432  Oct 30 2019 03:37:25   default_ca.cer
    3  -rw-             36  Oct 30 2019 03:38:18   $_patchstate_reboot
    4  -rw-          3,684  Oct 30 2019 03:38:18   $_patch_history
    5  -rw-          1,903  Oct 30 2019 03:37:31   default_local.cer
    6  drw-              -  Oct 30 2019 03:37:42   logfile
    7  -rw-          1,111  Apr 08 2020 17:03:35   vrpcfg.zip
    8  -rw-      8,718,710  Dec 12 2013 07:53:05   s5720ei-v200r011sph008.pat
    9  drw-              -  Oct 30 2019 03:19:14   pmdata
   10  -rw-     85,051,908  Jun 28 2018 15:55:01   s5720ei-v200r011c10spc600.cc
   11  drw-              -  Oct 30 2019 03:18:44   $_install_mod
   12  -rw-            836  Mar 30 2020 10:48:44   rr.bak
   13  -rw-            836  Mar 30 2020 10:48:44   rr.dat
   14  -rw-          1,773  Apr 08 2020 17:03:36   private-data.txt
   15  drw-              -  Apr 08 2020 17:03:33   localuser
   16  drw-              -  Mar 30 2020 14:24:42   $_backup
   17  -rw-            200  Oct 30 2019 03:37:32   ca_config.ini

352,772 KB total (265,060 KB free)
```

###### 将PC作为FTP Server，并FTP到PC

```
# 连接到PC FTP Server
<H07_S5720_BMC-05>ftp 192.168.1.2

# 进行二进制编码
[ftp]bin

# 备份软件包
[ftp]put S5720EI-V200R019C00SPC500.cc S5720EI-V200R019C00SPC500.bak.cc

# 备份补丁
[ftp]put S5720EI-V200R019SPH007.pat S5720EI-V200R019SPH007.bak.pat

# 上传软件包
[ftp]get S5720EI-V200R019C00SPC500.cc

# 上传补丁
[ftp]get S5720EI-V200R019SPH007.pat

# 退出
[ftp]bye

# 设置新版软件包应用全部设备并下次启动该软件包
<H07_S5720_BMC-05>startup system-software flash:/S5720EI-V200R019C00SPC500.cc all

# 重启设备
<H07_S5720_BMC-05>reboot

# 查看堆叠状态
<H07_S5720_BMC-05>display stack

# 将补丁应用到全部并运行
<H07_S5720_BMC-05>patch load flash:/S5720EI-V200R019SPH007.pat all run 

# 查看补丁信息
<H07_S5720_BMC-05>display patch-information

# 选中补丁
<H07_S5720_BMC-05>patch load flash:/S5720EI-V200R019SPH007.pat all active

# 删除软件包
<H07_S5720_BMC-05>delete /unreserved flash:/s5720ei-v200r011c10spc600.cc

# 删除补丁包
<H07_S5720_BMC-05>delete /unreserved flash:/S5720EI-V200R019SPH007.pat
```

