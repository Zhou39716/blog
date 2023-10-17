#### 锐捷设备命令大全

#### 01  基础命令

```
> Enable    //进入特权模式
> #Exit      //返回上一级操作模式
> #End      //返回到特权模式
> #copy running-config startup-config  //保存配置文件
> #del flash:config.text  //删除配置文件(交换机及1700系列路由器)
> #erase startup-config  //删除配置文件(2500系列路由器)
> #del flash:vlan.dat  //删除Vlan配置信息（交换机）
> #Configure terminal  //进入全局配置模式
> (config)# hostname switchA  //配置设备名称为switchA
> (config)#banner motd &    //配置每日提示信息 &为终止符
> (config)#enable secret level 1 0 star  //配置远程登陆密码为star
> (config)#enable secret level 15 0 star  //配置特权密码为star
> Level 1为普通用户级别，可选为1~15，15为最高权限级别；0表示密码不加密
> (config)#enable services web-server //开启交换机WEB管理功能
> Services 可选以下：web-server(WEB管理)、telnet-server(远程登陆)等
```

#### 02  查看信息

```
#show running-config    //查看当前生效的配置信息
#show interface fastethernet 0/3  //查看F0/3端口信息
#show interface serial 1/2   //查看S1/2端口信息
#show interface        //查看所有端口信息
#show ip interface brief     //以简洁方式汇总查看所有端口信息
#show ip interface     //查看所有端口信息
#show version        //查看版本信息
#show mac-address-table    //查看交换机当前MAC地址表信息
#show running-config    //查看当前生效的配置信息
#show vlan         //查看所有VLAN信息
#show vlan id 10     //查看某一VLAN (如VLAN10)的信息
#show interface fastethernet 0/1  //查看某一端口模式(如F 0/1)
#show aggregateport 1 summary  //查看聚合端口AG1的信息
#show spanning-tree   //查看生成树配置信息
#show spanning-tree interface fastethernet 0/1  //查看该端口的生成树状态
#show port-security   //查看交换机的端口安全配置信息
#show port-security address   //查看地址安全绑定配置信息
#show ip access-lists listname  //查看名为listname的列表的配置信息
```

#### 03 端口基本配置

```
(config)#Interface fastethernet 0/3     //进入F0/3的端口配置模式
(config)#interface range fa 0/1-2,0/5,0/7-9   //进入F0/1、F0/2、F0/5、F0/7、F0/8、F0/9的端口配置模式
(config-if)#speed 10   //配置端口速率为10M,可选10,100,auto
(config-if)#duplex full   //配置端口为全双工模式,可选full(全双工),half(半双式),auto(自适应)
(config-if)#no shutdown          //开启该端口
(config-if)#switchport access vlan 10   //将该端口划入VLAN10中,用于VLAN
(config-if)#switchport mode trunk   //将该端口设为trunk模式,可选模式为access , trunk
(config-if)#port-group 1   //将该端口划入聚合端口AG1中,用于聚合端口
```

#### 04 端口聚合配置

```
(config)# interface aggregateport 1   //创建聚合接口AG1
(config-if)# switchport mode trunk   //配置并保证AG1为 trunk 模式
(config)#int f0/23-24
(config-if-range)#port-group 1     //将端口（端口组）划入聚合端口AG1中
```

#### 05  生成树

配置多生成树协议:

```
switch(config)#spanning-tree          //开启生成树协议
switch(config)#spanning-tree mst configuration   //建立多生成树协议
switch(config-mst)#name ruijie           //命名为ruijie
switch(config-mst)#revision 1      //设定校订本为1
switch(config-mst)#instance 0 vlan 10,20   //建立实例0
switch(config-mst)#instance 1 vlan 30,40   //建立实例1
switch(config)#spanning-tree mst 0 priority 4096  //设置优先级为4096
switch(config)#spanning-tree mst 1 priority 8192  //设置优先级为8192
switch(config)#interface vlan 10
switch(config-if)#vrrp 1 ip 192.168.10.1 //此为vlan 10的IP地址
switch(config)#interface vlan 20
switch(config-if)#vrrp 1 ip 192.168.20.1 //此为vlan 20的IP地址
switch(config)#interface vlan 30
switch(config-if)#vrrp 2 ip 192.168.30.1 //此为vlan 30的IP地址(另一三层交换机)
switch(config)#interface vlan 40
switch(config-if)#vrrp 2 ip 192.168.40.1 //此为vlan 40的IP地址(另一三层交换机)
```

#### 06  vlan的基本配置

```
(config)#vlan 10    //创建VLAN10
(config-vlan)#name vlanname   // 命名VLAN为vlanname
(config-if)#switchport access vlan 10   //将该端口划入VLAN10中
某端口的接口配置模式下进行
(config)#interface vlan 10     //进入VLAN 10的虚拟端口配置模式
(config-if)# ip address 192.168.1.1 255.255.255.0   //为VLAN10的虚拟端口配置IP及掩码，二层交换机只能配置一个IP，此IP是作为管理IP使用，例如，使用Telnet的方式登录的IP地址
(config-if)# no shutdown    //启用该端口
```

#### 07  端口安全

```
(config)# interface fastethernet 0/1    //进入一个端口
(config-if)# switchport port-security   //开启该端口的安全功能
```

##### （1）配置最大连接数限制

```
(config-if)# switchport port-secruity maxmum 1 //配置端口的最大连接数为1，最大连接数为128
(config-if)# switchport port-secruity violation shutdown
//配置安全违例的处理方式为shutdown，可选为protect (当安全地址数满后，将未知名地址丢弃)、restrict(当违例时，发送一个Trap通知)、shutdown(当违例时将端口关闭，并发送Trap通知，可在全局模式下用errdisable recovery来恢复)
```

##### （2）IP和MAC地址绑定

```
(config-if)#switchport port-security mac-address xxxx.xxxx.xxxx ip-address 172.16.1.1
//接口配置模式下配置MAC地址xxxx.xxxx.xxxx和IP172.16.1.1进行绑定(MAC地址注意用小写)
```

#### 08 三层路由功能（ 针对三层交换机 ）

```
(config)# ip routing      //开启三层交换机的路由功能
(config)# interface fastethernet 0/1
(config-if)# no switchport  //开启端口的三层路由功能(这样就可以为某一端口配置IP)
(config-if)# ip address 192.168.1.1 255.255.255.0
(config-if)# no shutdown
```

#### 09 三层交换机路由协议 

```
(config)# ip route 172.16.1.0 255.255.255.0 172.16.2.1  //配置静态路由
注:172.16.1.0 255.255.255.0     //为目标网络的网络号及子网掩码
172.16.2.1 为下一跳的地址，也可用接口表示,如ip route 172.16.1.0 255.255.255.0 serial 1/2(172.16.2.0所接的端口)
(config)# router rip   //开启RIP协议进程
(config-router)# network 172.16.1.0   //宣告本设备的直连网段信息
(config-router)# version 2    //开启RIP V2，可选为version 1(RIPV1)、version 2(RIPV2)
(config-router)# no auto-summary  //关闭路由信息的自动汇总功能(只有在RIPV2支持)
(config)# router ospf  //开启OSPF路由协议进程（针对1762，无需使用进程ID）
(config)# router ospf 1  //开启OSPF路由协议进程（针对2501，需要加OSPF进程ID）
(config-router)# network 192.168.1.0 0.0.0.255 area 0
//宣告直连网段信息，并分配区域号(area0为骨干区域)
```

