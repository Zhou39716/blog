### 常见注册表操作

#### 1、修改hosts

```
c:\windows\system32\drivers\etc\hosts
```

#### 2、注册表隐藏磁盘

```
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer
选中Explorer，在右侧空白处右键单击选择“新建 -> 二进制值”，然后将新建的键值名称修改为“NoDrives”
```

#### 3、新机跳过联网

```
Shift  F10
cmd→OOBE\BYPASSNRO
or
cmd→taskmgr→结束“网络连接流”
or
cmd→taskkill /F/IM oobenetworkconnectionflow.exe
```

#### 4、注册表修改我的文档位置

```
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders  右侧
```

#### 5、注册表修改系统环境变量

```
HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Session  Manager\Environment   右侧
```

#### 6、注册表关闭磁盘开机自检

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager  右侧 BootExecute   ，清空BootExecute的值
```

#### 7、win11等新系统找回IE浏览器

```
C:\Program Files (x86)\Internet Explorer\iexplore.exe

1.新建一个vbs文件，内容如下

CreateObject("InternetExplorer.Application").Visible=true

2.EDG浏览器/设置/默认浏览器/让 Internet Explorer 在 Microsoft Edge 中打开网站/从不

打开VBS文件即可打开IE浏览器
```

#### 8、注册表查看IP

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces
```
