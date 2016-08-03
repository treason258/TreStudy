TreStudy
====================
> 专注于 Android 学习。  
>
> ![img](http://ww4.sinaimg.cn/thumb180/005E1iJQjw1enf3nmjm9qj30b30b3q30.jpg)
>
> 网站：[www.mjiayou.com](http://www.mjiayou.com)  

## 目录：
> [一、Android基础](https://github.com/treason258/TreStudy#一android基础)
>
> 包括：[Android系统框架](https://github.com/treason258/TreStudy#android系统框架)、[Activity](https://github.com/treason258/TreStudy#activity)、[Service](https://github.com/treason258/TreStudy#service)、[BroadcastReceiver](https://github.com/treason258/TreStudy#broadcastreceiver)、[ContentProvider](https://github.com/treason258/TreStudy#contentprovider)、[Intent](https://github.com/treason258/TreStudy#intent)

> [二、进程间通信](https://github.com/treason258/TreStudy#二进程间通信)
>
> 包括：[IPC](https://github.com/treason258/TreStudy#ipc)、[Binder](https://github.com/treason258/TreStudy#binder)、[Messenger](https://github.com/treason258/TreStudy#messenger)、[AIDL](https://github.com/treason258/TreStudy#aidl)

## 一、Android基础
### Android系统框架
Linux内核:进程、线程、电源管理、驱动
硬件抽象层:音视频接口、GPS接口、通话接口、WIFI接口

核心类库：libc、SQLite、Webkit、OpenGL、FreeType
运行时：Java核心类库、虚拟机（Dalvik Virtual Machine）

框架层：Activity Manager、Windows Manager、Content Providers、View System、Resource Manager、Package Manager

应用层：桌面应用、联系人应用、通话应用、浏览器应用

### Activity
生命周期
onCreate、onStart、onResume
onPause、onStop、onDestory
onRestart

onWindowFocusChanged
onSaveInstanceState
onRestoreInstanceState

1、启动Activity:系统先调用onCreate方法,然后调用onStart方法,最后调用onResume方法,Activity进入运行状态。
onCreate -> onStart -> onResume -> Activity is running
2、当前Activity被其他Activity覆盖或者被锁屏:系统调用onPause方法,暂停当前Activity的执行。
onResume -> Another activity comes in front of the activity -> onPause
3、当前Activity由被覆盖状态回到前台或者解锁屏:系统调用onResume,再次进入运行状态。
onPause -> The activity comes to the foreground -> onResume
4、当前Activity转到新的Activity页面或按Home键回到主屏,自身退居后台:系统先调用onPause方法,然后调用onStop方法,进入停滞状态。
onPause -> The activity is no longer visible -> onStop
5、用户后退回到此Activity:系统先调用onRestart,然后调用onStart方法,最后调用onResume方法,再次进入运行状态。
onStop -> The activity comes to the foreground -> onRestart -> onStart
6、当前Activity处于被覆盖状态或者后台不可见状态,即第2步和第4步,系统内存不足,杀死当前Activity,而后用户退回当前Activity:再次调用onCreate、onStart、onResume方法,进入运行状态。
onPause/onStop -> Other applications need memory -> Process is killed -> User navigates back to the activity -> onCreate
7、用户退出当前Activity:系统先调用onPause方法,然后调用onStop方法,最后调用onDetroy方法,结束当前Activity。

1、onWindowFocusChanged方法:在Activity窗口获得或者失去焦点时被调用,例如创建时首次呈现在用户面前,当前Activity被其他Activity覆盖,当前Activity转到其他Activity或按Home键回到主屏,自身退居后台,用户退出当前Activity。
用处:获取特定视图组件的尺寸大小,在oCreate可能无法获取,因为窗口Windows对象还没有创建,这时候可以在onWindowsFocusChanged里获取。
2、onSaveInstanceState:Activity被覆盖或者退居后台之后,系统资源不足将其杀死;用户改变屏幕方向,会销毁当前然后重建一个;跳转其他Activity货按Home键回到主屏,保存View组件状态。
调用在onPause之前。
3、onRestoreInstanceState:对应onSavaInstanceState的三种A情况,调用在onStart之后,onResume之前。

注:
1、如果<activity>配置了android:screenOrientation属性，则会使android:configChanges="orientation"失效。
2、模拟器与真机差别很大

启动模式:
1、standard 默认的启动模式
A-A-A-A 每个实例A都是新的
不管有没有已存在的实例,都会生成新的实例。
2、singleTop
A-A-A-A 每个实例A都是不变的
如果当前Activity位于栈顶,则不再生成新的,而是直接使用,调用onNewIntent。
A-B-A-A 每个实例A都是新的
如果当前Activity不是位于栈顶,则生成一个新的实例。
3、singleTask
A-A-A-A 每个实例A都是不变的
如果当前Activity位于栈顶,则不再生成新的,而是直接使用。
A-B-A-B A的实例是不变的,而B的实例是新的
当B打开A时,发现有A的实例,于是清除掉A之上的所有Activity实例,将A变成栈顶。
如果发现有对应的Activity实例,则使此Activity实例之上的其他Activity实例统统出栈,使此Activity实例成为栈顶。
4、singleIntance
比较特殊,它会启用一个新的栈结构,将Activity放置于这个新的栈结构中,并保证不再有其他Activity实例进入。
然后原始栈执行之前的操作不变，两个栈同时存在。
比较复杂,需要借助图形。

### Service
声明周期
Service的生命周期可以从两种启动模式开始说起,分别是Context.startService和Context.bindService.
1、startService启动模式下的生命周期:当首次使用startService启动一个服务时,系统会实例化一个Service实例,依次调用其onCreate和onStartCommand方法,然后进入运行状态。
此后,如果再使用startService启动服务时,不再创建新的服务对象,系统会自动找到刚才创建的Service实例,调用其onStart方法。
如果想停掉一个服务,可以使用stopService方法,此时调用onDetroy方法,需要注意,不管前面使用多个startService,只需一次stopService即可停掉服务。
2、bindService启动模式下的生命周期:在这种模式下,当调用者首次使用bindService绑定一个服务时,系统会实例化一个Service实例,依次调用onCreate和onBind方法。
如果我们需要接触这个服务的绑定,可使用unbindService方法,此时调用onUnbind和onDestroy方法。

两种方式的区别:
startService模式下,调用者与服务无必然联系,及时调用者结束自己的生命周期,只要没有使用stopService方法听着这个服务,服务仍会运行。
通常情况下,bindService模式下服务与调用者生死与共的,在绑定结束之后,一旦调用者被销毁,服务也就立即终止。

注:
在Android2.0系统引进了onStartCommand方法取代onStart方法。

使用:
在AndroidManifest中声明注册
``` xml
<service android:name=".TestService">
    <intent-filter>
         <action android:name="android.intent.action.MyService" />
         <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</service>
```
进程内与服务通信
进程内与服务通信实际就是通过bindService的方式与服务绑定,获取到通信中介Binder实例,然后通过调用这个实例的方法,完成对服务的各种操作。

注:
与服务绑定是个异步的过程,也就是说绑定成功后下一步操作binder对象,有可能为null。

### BroadcastReceiver
广播接收者,用来接收来自系统和应用中的广播
广播体现的方方面面,例如开机、网路状态变化、电池电量变化都会产生广播,接收广播可以及时的处理一些提示和保存数据等。

使用:
1、创建BroadcastReceiver对象,继承BroadcastReceiver,实现其onReceive方法;
在onReceive中获取的Intent数据包含很多有用信息。
2、静态注册,在AndroidManifest文件中配置,intent-filter中action。
这种方式是常驻形的,也就是应用关闭后,如果有广播信息传来,也会被调用运行。
3、动态注册,通常是在Activity或者Service注册一个广播,使用ContextWrapper.registerReceiver(receiver,filter)。
其中filter是IntentFilter.addAction(string)。
这种广播会跟随程序的生命周期,所以Activity或者Service销毁时要记得解除注册ContextWrapper.unregisterReceiver(receiver),否者会报异常。
4、发送广播,sendBroadcast(intent)。

如果多个接收者都注册了相同的广播地址
普通广播:每个接收者都无需等待即可以接收到广播,接收者相互之间不会有影响,接收者无法终止广播,无法阻止其他接收者的接受动作。
有序广播:它每次只发送到优先级较高的接收者那里,然后由优先级高的接收者再传播到优先级低的接收者那里,优先级高的接收者有能力终止这个广播。
android:priority来控制优先级,sendOrderedBroadcast发送有序广播,需要定义权限,声明权限。

举例:
1、开机启动服务。
2、网络状态变化。
3、电量变化。

注:
1、需要权限声明。

### ContentProvider
ContentProvider是数据包装器,适合在不同进程间实现信息共享。
例如Android中SQLite数据库是典型的数据源,我们可以把它封装到ContentProvider中,就可以很好的为其他应用提供信息共享服务。
其他应用在访问ContentProvider时,可以使用一组类似REST的URI的方式进行数据操作,大大简化了读写信息的复杂度。
例如需要从封装图书数据库的ContentProvider获取一组图书:content://com.xx.xx.BookProvider/books
而要从图书数据库中获取指定的图书:content://com.xx.xx.BookProvider/books/23

ContentResolver与ContentProvider是对应的关系,正是通过他来与ContentProvider进行数据交换的。

举例
/data/data/com.android.providers.contacts 联系人的数据源,对应ContactsContract
/data/data/com.android.providers.telephony 短信数据源,对应

使用:
1、创建PersonProvider类,继承ContentProvider,实现onCreate、query、insert、update、delete和getType方法。
2、在AndroidManifest中注册授权。
<provider
    android:name=".XXXProvider"
    android:authorities="com.xxx.xxx.XXXProvider"
    android:multiprocess="true" />
3、读取数据,使用ContentResolver。

### Intent

## 二、进程间通信
### IPC

### Binder

### Messenger

### AIDL
