# libimobiledevice 使用说明

***Q*：libimobiledevice 是什么？**

***A*：iOS 命令行调试工具**

------

### 常用命令

`brew install libimobiledevice`  安装

`brew install ideviceinstaller`



`ideviceinstaller -u [udid] -i [xxx.ipa]`   给指定连接的设备安装应用

`ideviceinstaller -u [udid] -U [bundleId]`  给指定连接的设备卸载应用

`ideviceinstaller -u [udid] -l`   指定设备，查看安装的第三方应用 

`ideviceinstaller -u [udid] -l -o list_user`   指定设备，查看安装的第三方应用 

`ideviceinstaller -u [udid] -l -o list_system`   指定设备，查看安装的系统应用

` ideviceinstaller -u [udid] -l -o list_all`   指定设备，查看安装的系统应用和第三方应用



`idevice_id -l`  查看当前已连接的设备的UUID

`ideviceinfo`	获取设备信息

`ideviceinfo -u [udid]`   指定设备，获取设备信息

`ideviceinfo -u [udid] -k DeviceName`  指定设备，获取设备名称：iPhone6s

`idevicename -u [udid]`  指定设备，获取设备名称：iPhone6s

`ideviceinfo -u [udid] -k ProductVersion`  指定设备，获取设备版本：10.3.1

`ideviceinfo -u [udid] -k ProductType`  指定设备，获取设备类型：iPhone8,1 

`ideviceinfo -u [udid] -k ProductName`  指定设备，获取设备系统名称：iPhone OS