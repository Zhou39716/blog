# MACOS 安装相关
## 1、进入恢复模式（联网安装）
https://support.apple.com/zh-cn/HT204904
## 2、带启动的镜像引导（刻录到U盘）
https://www.apple114.com/pages/macos/

下载带启动的原版镜像，开机安装option，插入U盘，按照提示安装。

如果提示副本损坏，可能是时间问题，打开终端调整时间。

```
时间对应系统版本如下：

sudo date 010514102017.30（macOS Sierra 10.12适用）

sudo date 062614102014.30 （macOS Mojave 10.13、10.14适用）

sudo date 121212122019 （macOS Catalina 10.15）

sudo date 121212122020 （macOS 11，Big Sur）

sudo date 121212122021 （macOS 12，Monterey）

```


## 3、关于混合硬盘相关

	官方文档：https://support.apple.com/zh-cn/HT207584

## 4、双系统安装
	双系统安装一定是用bootcamp启动转换助理来安装，windows镜像必须是原版镜像。

## 5、双系统改单系统
	①改单MACOS系统：
		需要用bootcamp移除windows系统（时间可能较长，请耐心等待）

	②改单windows系统：
		可直接在PE或者windows下将MAC硬盘格式化，合并到windows系统即可。
	
