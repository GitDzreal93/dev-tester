## 自动化测试用例脚本应该怎么写？

（以下用伪代码来写）

```python
import uiautomator2 as u2

# 开始测试需要做的事
def setUP():
    '''
    （1）创建设备连接对象
    （2）关闭所有应用
    （3）[下载安装包apk]
    （4）[自动安装测试包apk]
    （5）打开测试包
    :return:
    '''
    # 调用 判断是否需要下载apk 的函数，这里不去细究如何实现，大概就是判断已安装的包的md5和需要下载的包是否相等，不相等就下载apk，该函数返回值是bool值
    is_need_download = check_download()
    download_url = 'http://xxx/xxx/xxxx.apk'
    pkg_name = 'com.xxx.xxx'
    # 连接设备
    d = u2.connect()
    # 关闭所有应用
    d.app_stop_all()
    # 假如需要下载的话，就下载app
    if is_need_download:
        # 调用apk下载器，返回下载好的安装包的路径
        apk_path = do_download(download_url);
        # 安装apk
        d.app_install()
    d.app_start(pkg_name)

# 结束测试需要做的事
def tearDown():
    '''
    （1）关闭app
    （2）[卸载app]
    （3）生成测试报告    
    :return:
    '''
    # 包名应该放在全局配置中，此处为了方便大家理解上下文，抽离出来了
    pkg_name = 'com.xxx.xxx'
    # check_uninstall函数会去读取全局配置，判断是否需要做卸载app测试，返回值是bool值
    is_need_uninstall = check_uninstall()
    # 关闭测试app
    d.app_stop()
    if is_need_uninstall:
        d.uninstall(pkg_name)
    # 生成测试报告
    make_report()
    
def test_swipe_high_1()
    '''
    :title: 滑动测试
    :desc: 第1条用例--滑动测试
    :priority: high
    '''
    d.swipe()

def test_click_mid_2()
    '''
    :title: 点击测试
    :desc: 第2条用例--点击测试
    :priority: mid
    '''
    d(id='wwwwaaaddd').click()

def test_input_low_3()
    '''
    :title: 输入表单验证
    :desc: 第3条用例--输入表单验证
    :priority: mid
    '''
    # 聚焦表单
    d(id='wwwwaaaddd').click()
    # 填写表单
    d(id='wwwwaaaddd').send_keys("asdasdasdasd")
    # 提交表单
    d(text='提交').click()
    
# 代码执行顺序
setUp() #1
test_swipe_high_1() #2
test_click_mid_2() #3
test_input_low_3() #4
tearDown() #5
```