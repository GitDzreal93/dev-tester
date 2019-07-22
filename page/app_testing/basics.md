# 移动端测试基础

### 移动端测试项

![](/assets/app_testing/m_test.jpg)

### 常用的测试工具

#### 抓包工具

1. **charles**

   - 跨平台（windows，Mac），易用性强

   - [官网](https://www.charlesproxy.com/)

2. **fiddler**

   - 跨平台（windows，Mac），易用性强

   - [官网](https://www.telerik.com/fiddler)

3. **anyproxy**

   - 阿里开源，可以命令行运行，也可以可视化，基于nodejs，方便二次开发
   - [GitHub](https://github.com/alibaba/anyproxy)

4. **mitmproxy**

   - 提供python API，支持python二次开发
   - [官网](https://www.mitmproxy.org/)

5. **Stream**

   - 仅支持iOS端，是端上的代理工具，可以支持4G抓包

6. **wireshark**

   - 跨平台（windows，Mac），支持抓TCP/UDP请求
   - [官网](https://www.wireshark.org/)

#### 稳定性测试工具

1. **安卓原生Monkey**
```shell
adb shell monkey –p com.lianjia.beike –-throttle 100 –-pct-touch 50 –-pct-motion 50 –v –v 1000 > c:\monkey.txt
```
2. **Maxim**
   - 智能monkey
   - [GitHub](https://github.com/zhangzhao4444/Maxim)
3. **UICrawler**
   - 支持Android、iOS和微信小程序，依赖Appium
   - [GitHub](https://github.com/lgxqf/UICrawler)
4. **AppCrawler**
   - 支持Android和iOS，依赖Appium
   - [GitHub](https://github.com/seveniruby/AppCrawler)

#### UI自动化测试工具

1. **uiautomator2**

   - 基于python，Android UI自动化
   - [GitHub](https://github.com/openatx/uiautomator2)

2. **facebook-wda**

   - 提供python-Api，iOS UI自动化
   - [GitHub](https://github.com/openatx/facebook-wda)

3. **weditor**

   - 控件采集

   - [GitHub](https://github.com/openatx/weditor)

4. **appium**

   - 跨平台，支持多种编程语言

   - [官网](http://appium.io/)
   - [GitHub](https://github.com/appium/appium)
   - [GitHub-桌面工具](https://github.com/appium/appium-desktop)

5. **macaca**

   - [官网](https://macacajs.github.io/zh/)
   - [GitHub](https://github.com/alibaba/macaca)

#### 手机群控管理

1. **atxserver2**
   - [GitHub](https://github.com/openatx/atxserver2)
2. **stf**
   - [GitHub](https://github.com/openstf/stf)

#### APP安全测试

1. **mobsf**
   - [GitHub](https://github.com/MobSF/Mobile-Security-Framework-MobSF)
   - [GitHub-汉化版](https://github.com/HackingLab/MobileSF)

#### 性能测试工具

1. **GT**
   - 腾讯开源
   - [Github](https://github.com/Tencent/GT)
2. **SoloPi**
   - 支付宝开源
   - [GitHub](https://github.com/Tencent/GT)

#### 综合测试工具

1. **appetizer**

   - PC客户端工具，很多功能都有了，挺不错的

   - [官网](https://www.appetizer.io/cn/)

#### 游戏测试工具

1. **airtest**
   - [官网](http://airtest.netease.com/)
   - [GitHub](https://github.com/AirtestProject/Airtest)