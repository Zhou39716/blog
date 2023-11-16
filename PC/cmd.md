
### 常见的批处理操作

#### 1、重置网络

```
netsh winsock reset catalog
netsh int ip reset.log
```

#### 2、ip相关配置

```
ipconfig               显示网络信息信息
ipconfig /all          显示详细网络信息
ipconfig /release      释放IP
ipconfig /renew        重新获取IP
```

#### 3、配置IP地址

```
netsh interface ipv4 set address name="接口名" source=dhcp     配置DHCP

netsh interface ipv4 set address name = "接口名" source = static 
address = 192.168.1.110 mask = 255.255.255.0 gateway=192.168.1.201      配置静态IP
```

#### 4、配置DNS

```
netsh interface ipv4 set dnsservers name="接口名"source=static address=223.5.5.5 register=primary
netsh interface ipv4 set dnsservers name="接口名"source=static address=114.114.114.114
```

#### 5、同时配置静态IP和DNS

```
netsh interface ipv4 set address name = "WLAN" source = static address = 192.168.1.110 mask = 255.255.255.0 gateway=192.168.1.201
set dnsservers name="WLAN"source=static address=192.168.1.201 register=primary
```

#### 6、静态路由相关

```
route print           打印静态路由
route add 10.10.1.0 mask 255.255.255.0 192.168.1.1 -p   添加静态路由
route delete 10.10.1.0   删除静态路由
```

#### 7、延时启动

```
timeout /t 5 /nobreak
start "" "c:\kugou.exe"
```

#### 8、安装驱动

```
pnputil -a *.inf          安装当前目录下的所有驱动
```

#### 9、复制

```
xcopy [源目录][源文件][源目录\源文件]   [目的目录][目的目录\目的文件]  [复制目录和子目录 /E] [覆盖 /Y]

eg:复制文件
xcopy a.cmd c:\b.cmd /Y

eg：复制目录
xcopy aa c:\dd\ /E
```

#### 10、创建目录

```
md c:\dd\ff
```

#### 11、添加windows凭据

```
cmdkey /add:DOMAIN /user:DOMAIN\%USERNAME% /pass:%USERNAME%

DOMAIN ：改为计算机名/域名/IP
%USERNAME% ：改为用户名
%USERNAME% ：改为实际密码

凭证无法存储重启消失，解决办法：
gpedit.msc/计算机配置/windows设置/安全设置/本地策略/安全选项/网络访问:不允许存储网络微份验证的密码和凭据/禁用
```

#### 12、删除所有打印任务

```
net stop spooler

del c:\Windows\System32\spool\PRINTERS\*.* /Q

net start spooler
```

#### 13、开启远程桌面（管理员运行）

```
@echo off
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 0 /f
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa" /v LimitBlankPasswordUse /t REG_DWORD /d 0 /f

```
