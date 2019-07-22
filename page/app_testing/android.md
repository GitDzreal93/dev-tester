# Android知识汇总

### 干货

[Android校招面试指南](https://lrh1993.gitbooks.io/android_interview_guide/content/)

[AndroidNote](https://github.com/linsir6/AndroidNote)

[Android面试](https://hadyang.github.io/interview/)

------

### 自己总结

##### 一、Android Crash分类

1. **Java crash**：Java 代码导致 jvm 退出。弹出“程序已经崩溃”的对话框，最终用户点击关闭后，进程退出。Logcat 会在 “AndroidRuntime” 的 tag 下输出 Java 的调用栈

   **特点**：

   - 这类错误一般是由Java层代码触发的 
   - 一般情况下程序出错时会弹出提示框，JVM虚拟机退出 
   - 一般的Crash工具都能够捕获，系统也提供了API

2. **native crash**：在实际Android开发的时候，可能会引入第三方的一些so库或者自己开发相应的so库供程序使用，然而so库一般是通过**c或者c++**开发的。Android中使用so库的时候发生的Crash，就是我们所说的Native Crash。

   **特点：**

   - 出错时界面不会弹出提示框提醒程序崩溃（Android 5.0以下）
   - 出错时会弹出提示框提醒程序崩溃（Android 5.0以上）
   - 程序会直接闪退到系统桌面
   - 这类错误一般是由C++层代码错误引起的
   - 绝大部分Crash工具不能够捕获

##### 二、常见的Android Crash

1. **NullPointerException** 空指针
2. **ClassCastException** 类型转换异常
3. **IndexOutOfBoundsException** 下标越界异常
4. **ActivityNotFoundException** Activity未找到异常
5. **IllegalStateException** 非法状态异常
6. **ArrayIndexOutOfBoundsException** 数组越界异常
7. **SecurityException** 安全异常
8. **ClassNotFoundException** 类找不到
9. **FileNotFoundException** 文件找不到
10. **OutOfMemoryError** 内存溢出
11. **SQLiteDiskIOException** 数据库磁盘读写异常
12. **JSONException** json异常

##### 三、Android Activity 生命周期

**七大** 生命周期：

（1）**onCreate()**

（2）**onStart()**

（3）**onRestart()**

（4）**onResume()**

（5）**onPause()**

（6）**onStop()**

（7）**onDestroy()**

写Activity时，需要继承Activity类，而Activity类又继承于ApplicationContext

```java
public class Activity extends ApplicationContext {  
       protected void onCreate(Bundle savedInstanceState);  
         
       protected void onStart();     
         
       protected void onRestart();  
         
       protected void onResume();  
         
       protected void onPause();  
         
       protected void onStop();  
         
       protected void onDestroy();  
   }  
```

![](/assets/app_testing/activity_life_cycle.jpg)

##### 四、Android 四大控件

| **名称**               | **功能**                                                     |
| ---------------------- | ------------------------------------------------------------ |
| **Activity**           | 也就是我们常见的页面（窗口）                                 |
| **Service**            | 用于在后台完成用户指定的操作                                 |
| **Content Provider**   | Android提供的第三方应用数据的访问方案                        |
| **Broadcast Receiver** | 对外部事件进行过滤，只对感兴趣的外部事件(如当电话呼入时，或者数据网络可用时)进行接收并做出响应。 |

##### 五、Android 五大存储

1. SharedPreferences（Android提供用来存储一些简单的配置信息的一种机制）
2. 文件存储
3. SQLite[数据库](http://lib.csdn.net/base/14)方式
4. 内容提供器（content provider）
5. 网络

##### 六、Android 六大布局

1.  **LinearLayout 线性布局**
2. **TableLayout 表格布局**
3. **FrameLayout 帧布局**
4. **RelativeLayout 相对布局**
5. **GridLayout 网格布局**
6. **AbsoluteLayout 绝对布局**

##### 七、monkey

**命令**

```shell
# -p 包名
# -v 最高警告的错误日志才输出
# 500 执行500次
# 日志重定向到 monkey.log
adb shell monkey -p <packageName> -v 500  > monkey.log
```

**monkey 参数**

*-p*	包

*-s*	设置种子

*--ignore-crashes*	出现 crash 继续执行

*--ignore-timeouts*	出现 anr 继续执行	

*--pct-touch <rateNum>*	 触摸事件占比（手指放下，抬起）

*--pct-motion <rateNum>*	动作事件占比（手指放下，移动，抬起）

*–pct-trackball <rateNum>*	轨迹球事件占比（单纯的move）

*–pct-nav <rateNum>* 	基本导航事件，用于方向输入设备的上下左右操作

*–pct-syskeys <rateNum>* 	系统按键事件。

*–pct-appswitch <rateNum>*	应用启动事件

*–pct-anyevent <rateNum>*	其他未提及事件