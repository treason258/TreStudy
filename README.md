# TreStudy

> 专注于 Android 学习！
>
> ![img](/res/trec-144.png)
>
> [工欲善其事 必先利其器](/TOOLS.md)
>
<!-- [个人博客 - mjiayou.com](http://mjiayou.com) -->

## 目录：

- [一、Android基础](#一android基础-)
	- [系统框架](#系统框架-)
	- [Activity](#activity-)
	- [Service](#service-)
	- [BroadcastReceiver](#broadcastreceiver-)
	- [ContentProvider](#contentprovider-)
	- [Fragment](#fragment-)
	- [Intent](#intent-)
	- [permission](#permission-)
	- [Service & Thread](#service--thread-)
	- [HandlerThread & Thread](#handlerthread--thread-)
	- [IntentService & Service](#intentservice--service-)
- [二、Java基础](#二java基础-)
	- [面向对象三大特征](#面向对象三大特征-)
	- [Thread](#thread-)
	- [ExecutorService](#executorservice-)
	- [线程安全](#线程安全-)
	- [JVM](#jvm-)
	- [GC](#gc-)
	- [对象引用](#对象引用-)
	- [堆和栈](#堆和栈-)
	- [排序算法](#排序算法-)
	- [String分析](#string分析-)
	- [List & Queue & Set & Map](#list--queue--set--map-)
	- [equals & hashCode](#equals--hashcode-)
- [三、多线程编程](#三多线程编程-)
	- [Handler异步事件处理机制](#handler异步事件处理机制-)
	- [AsyncTask](#asynctask-)
- [四、进程间通信](#四进程间通信-)
	- [RPC](#rpc-)
	- [IPC](#ipc-)
	- [AIDL](#aidl-)
	- [Binder](#binder-)
	- [Messenger](#messenger-)
- [五、图形图像编程](#五图形图像编程-)
	- [自定义控件](#自定义控件-)
	- [LayoutInflate](#layoutinflate-)
	- [Canvas](#canvas-)
	- [Animation](#animation-)
	- [Shader](#shader-)
	- [OpenGL](#opengl-)
	- [View & SurfaceView](#view--surfaceview-)
	- [onMeasure & onLayout & onDraw](#onmeasure--onlayout--ondraw-)
	- [Touch事件分发机制](#touch事件分发机制-)
	- [View绘制流程](#view绘制流程-)
- [六、高性能开发](#六高性能开发-)
	- [内存溢出和内存泄露](#内存溢出和内存泄露-)
	- [APP性能优化](#app性能优化-)
	- [TraceView性能分析](#TraceView性能分析-)
	- [ListView性能优化](#listview性能优化-)
	- [WebView内存泄露分析](#webview内存泄露分析-)
- [七、文件存储](#七文件存储-)
	- [SharedPreferences](#sharedpreferences-)
	- [File](#file-)
	- [SQLite](#sqlite-)
- [八、网络编程](#八网络编程-)
	- [网络协议](#网络协议-)
	- [Socket](#socket-)
	- [TCP & UDP](#tcp--udp-)
- [九、设计模式](#九设计模式-)
	- [单例模式](#单例模式-)
	- [适配器模式](#适配器模式-)
	- [工厂模式](#工厂模式-)
	- [代理模式](#代理模式-)
	- [观察者模式](#观察者模式-)
	- [访问者模式](#访问者模式-)
	- [命令模式](#命令模式-)
	- [组合模式](#组合模式-)
	- [建造者模式](#建造者模式-)
	- [抽象工厂模式](#抽象工厂模式-)
	- [享元模式](#享元模式-)
- [十、源码分析](#十源码分析-)
	- [Gson](#gson-)
	- [Volley](#volley-)
	- [EventBus](#eventbus-)
- [X、其他未归类](#x其他未归类-)
	- [JNI & NDK](#jni--ndk-)
	- [Java & JS](#java--js-)

## 一、Android基础 <a href="#目录"><img src="/res/back-top.png" height="20" width="20"/></a>

### 系统框架 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>
![img](/res/android-framework.png)

- 底层部分
	- Linux内核：进程、线程、电源管理、驱动
	- 硬件抽象层：音视频接口、GPS接口、通话接口、WIFI接口

- 核心部分
	- 核心类库：libc、SQLite、Webkit、OpenGL、FreeType
	- 运行时：Java核心类库、虚拟机（Dalvik Virtual Machine）

- 框架层：Activity Manager、Windows Manager、Content Providers、View System、Resource Manager、Package Manager

- 应用层：桌面应用、联系人应用、通话应用、浏览器应用

### Activity <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>
- 生命周期
	- onCreate、onStart、onResume
	- onPause、onSback-top、onDestory
	- onRestart
	- onWindowFocusChanged
	- onSaveInstanceState
 	- onRestoreInstanceState

- 生命周期变化
	- 1、启动Activity：系统先调用`onCreate`方法，然后调用`onStart`方法，最后调用`onResume`方法，Activity进入运行状态。`onCreate` > `onStart` > `onResume` > `Activity is running`
	- 2、当前Activity被其他Activity覆盖或者被锁屏：系统调用`onPause`方法，暂停当前Activity的执行。`onResume` > `Another activity comes in front of the activity` > `onPause`
	- 3、当前Activity由被覆盖状态回到前台或者解锁屏：系统调用`onResume`，再次进入运行状态。`onPause ` > `The activity comes to the foreground` > `onResume`
	- 4、当前Activity转到新的Activity页面或按Home键回到主屏，自身退居后台：系统先调用`onPause`方法，然后调用`onStop`方法，进入停滞状态。`onPause ` > `The activity is no longer visible` > `onStop `
	- 5、用户后退回到此Activity：系统先调用`onRestart`，然后调用`onStart`方法，最后调用`onResume`方法，再次进入运行状态。`onStop` > `The activity comes to the foreground` > `onRestart` > `onStart`
	- 6、当前Activity处于被覆盖状态或者后台不可见状态，即第2步和第4步，系统内存不足，杀死当前Activity，而后用户退回当前Activity：再次调用`onCreate`、`onStart`、`onResume`方法，进入运行状态。`onPause/onStop` > `Other applications need memory` > `Process is killed` > `User navigates back to the activity` > `onCreate`
	- 7、用户退出当前Activity：系统先调用`onPause`方法，然后调用`onStop`方法，最后调用`onDetroy`方法，结束当前Activity。

- 其他
	- 1、`onWindowFocusChanged`：在Activity窗口获得或者失去焦点时被调用，例如创建时首次呈现在用户面前，当前Activity被其他Activity覆盖，当前Activity转到其他Activity或按Home键回到主屏，自身退居后台，用户退出当前Activity。
用处：获取特定视图组件的尺寸大小，在oCreate可能无法获取，因为窗口Windows对象还没有创建，这时候可以在`onWindowsFocusChanged`里获取。
	- 2、`onSaveInstanceState`：Activity被覆盖或者退居后台之后，系统资源不足将其杀死;用户改变屏幕方向，会销毁当前然后重建一个;跳转其他Activity货按Home键回到主屏，保存View组件状态。
调用在onPause之前。
	- 3、`onRestoreInstanceState`：对应`onSavaInstanceState`的三种A情况，调用在`onStart`之后，`onResume`之前。

- 注：
	- 1、如果`<activity>`配置了`android:screenOrientation`属性，则会使`android：configChanges="orientation"`失效。
	- 2、模拟器与真机差别很大。

- 启动模式：
	- 1、standard 默认的启动模式
		- A-A-A-A 每个实例A都是新的
		- 不管有没有已存在的实例，都会生成新的实例。
	- 2、singleTop
		- A-A-A-A 每个实例A都是不变的
		- 如果当前Activity位于栈顶，则不再生成新的，而是直接使用，调用onNewIntent。
		- A-B-A-A 每个实例A都是新的
		- 如果当前Activity不是位于栈顶，则生成一个新的实例。
	- 3、singleTask
		- A-A-A-A 每个实例A都是不变的
		- 如果当前Activity位于栈顶，则不再生成新的，而是直接使用。
		- A-B-A-B A的实例是不变的，而B的实例是新的
		- 当B打开A时，发现有A的实例，于是清除掉A之上的所有Activity实例，将A变成栈顶。
		- 如果发现有对应的Activity实例，则使此Activity实例之上的其他Activity实例统统出栈，使此Activity实例成为栈顶。
	- 4、singleIntance
		- 比较特殊，它会启用一个新的栈结构，将Activity放置于这个新的栈结构中，并保证不再有其他Activity实例进入。
		- 然后原始栈执行之前的操作不变，两个栈同时存在。
		- 比较复杂，需要借助图形。

### Service <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>
- Service的生命周期可以从两种启动模式开始说起，分别是Context.startService和Context.bindService.
	- 1、startService启动模式下的生命周期：当首次使用startService启动一个服务时，系统会实例化一个Service实例，依次调用其onCreate和onStartCommand方法，然后进入运行状态。
此后，如果再使用startService启动服务时，不再创建新的服务对象，系统会自动找到刚才创建的Service实例，调用其onStart方法。
如果想停掉一个服务，可以使用stopService方法，此时调用onDetroy方法，需要注意，不管前面使用多个startService，只需一次stopService即可停掉服务。
	- 2、bindService启动模式下的生命周期：在这种模式下，当调用者首次使用bindService绑定一个服务时，系统会实例化一个Service实例，依次调用onCreate和onBind方法。
如果我们需要接触这个服务的绑定，可使用unbindService方法，此时调用onUnbind和onDestroy方法。

- 两种方式的区别：
	- startService模式下，调用者与服务无必然联系，及时调用者结束自己的生命周期，只要没有使用stopService方法听着这个服务，服务仍会运行。
	- 通常情况下，bindService模式下服务与调用者生死与共的，在绑定结束之后，一旦调用者被销毁，服务也就立即终止。

- 注：
	- 在Android2.0系统引进了onStartCommand方法取代onStart方法。

- 使用：
	- 在AndroidManifest中声明注册

```xml
<service android：name=".TestService">
    <intent-filter>
         <action android：name="android.intent.action.MyService" />
         <category android：name="android.intent.category.DEFAULT" />
    </intent-filter>
</service>
```

- 进程内与服务通信
	- 进程内与服务通信实际就是通过bindService的方式与服务绑定，获取到通信中介Binder实例，然后通过调用这个实例的方法，完成对服务的各种操作。

- 注：
	- 与服务绑定是个异步的过程，也就是说绑定成功后下一步操作binder对象，有可能为null。

### BroadcastReceiver <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>
广播接收者，用来接收来自系统和应用中的广播
广播体现的方方面面，例如开机、网路状态变化、电池电量变化都会产生广播，接收广播可以及时的处理一些提示和保存数据等。

使用：

- 1、创建BroadcastReceiver对象，继承BroadcastReceiver，实现其onReceive方法;
在onReceive中获取的Intent数据包含很多有用信息。
- 2、静态注册，在AndroidManifest文件中配置，intent-filter中action。
这种方式是常驻形的，也就是应用关闭后，如果有广播信息传来，也会被调用运行。
- 3、动态注册，通常是在Activity或者Service注册一个广播，使用ContextWrapper.registerReceiver(receiver，filter)。
其中filter是IntentFilter.addAction(string)。
这种广播会跟随程序的生命周期，所以Activity或者Service销毁时要记得解除注册ContextWrapper.unregisterReceiver(receiver)，否者会报异常。
- 4、发送广播，sendBroadcast(intent)。

如果多个接收者都注册了相同的广播地址
普通广播：每个接收者都无需等待即可以接收到广播，接收者相互之间不会有影响，接收者无法终止广播，无法阻止其他接收者的接受动作。
有序广播：它每次只发送到优先级较高的接收者那里，然后由优先级高的接收者再传播到优先级低的接收者那里，优先级高的接收者有能力终止这个广播。
android：priority来控制优先级，sendOrderedBroadcast发送有序广播，需要定义权限，声明权限。

举例：
1、开机启动服务。
2、网络状态变化。
3、电量变化。

注：
1、需要权限声明。

### ContentProvider <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>
ContentProvider是数据包装器，适合在不同进程间实现信息共享。
例如Android中SQLite数据库是典型的数据源，我们可以把它封装到ContentProvider中，就可以很好的为其他应用提供信息共享服务。
其他应用在访问ContentProvider时，可以使用一组类似REST的URI的方式进行数据操作，大大简化了读写信息的复杂度。
例如需要从封装图书数据库的ContentProvider获取一组图书：content：//com.xx.xx.BookProvider/books
而要从图书数据库中获取指定的图书：content：//com.xx.xx.BookProvider/books/23

ContentResolver与ContentProvider是对应的关系，正是通过他来与ContentProvider进行数据交换的。

举例
/data/data/com.android.providers.contacts 联系人的数据源，对应ContactsContract
/data/data/com.android.providers.telephony 短信数据源，对应

使用：
1、创建PersonProvider类，继承ContentProvider，实现onCreate、query、insert、update、delete和getType方法。
2、在AndroidManifest中注册授权。
<provider
    android：name=".XXXProvider"
    android：authorities="com.xxx.xxx.XXXProvider"
    android：multiprocess="true" />
3、读取数据，使用ContentResolver。

### Fragment <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### Intent <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### permission <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### Service & Thread <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### HandlerThread & Thread <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### IntentService & Service <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

## 二、Java基础 <a href="#目录"><img src="/res/back-top.png" height="20" width="20"/></a>

### 面向对象三大特征 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### Thread <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### ExecutorService <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### 线程安全 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### JVM <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### GC <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### 对象引用 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### 堆和栈 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### 排序算法 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### String分析 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### List & Queue & Set & Map <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

http://blog.csdn.net/defonds/article/details/47951103

- List 提供一个有序且有索引的容器，允许重复值的出现；
	- ArrayList
	- LinkedList
- Set 提供一个无序的唯一对象的容易，不允许重复值；
	- LinkedHashSet
	- TreeSet
	- HashSet
- Map 提供一个基于键值对以及哈希的数据结构；
	- HashMap

- 重复对象：List和Set接口最主要的区别就是在于List允许有重复对象，而Set不允许你重复对象。所有的Set都必须遵循着一约束，Map的每个Entry都持有两个对象，也就是一个键一个值，Map可能会持有相同的值对象，但键对象必须是唯一的。
- 排序：List和Set接口的另一个关键区别就是List是一个有序容器，List保持了每个元素的插入顺序。Set是个无序容器，无法保证每个元素的存储顺序。但是某些Set比如LinkedHashSet还是保持了每个元素的插入顺序。此外TreeSet和TreeMap也通过Comparator或者Comparable维护了一个排序顺序呢。
- 空元素：List允许空元素，Set也允许空，Map也允许空。值得注意的是，Hashtable即不允许null键也不允许null值。但HashMap允许任意数量的null值和最多一个null键。
- 流行实现：

- 如果使用索引对元素进行访问，那么List；
- 如果需要按照插入次序有序存储，那么List；
- 如果想保证插入数据唯一性，那么Set；
- 如果以键值对的形式进行数据存储，那么Map；

ArrayList $ LinkedList & Vector
- ArrayList 是一个数组队列，相当于动态数组，由数组实现，随意访问率高，但随机插入、随即删除效率低；
- LinkedList 是一个双链表，可以当做堆栈、队列或双端队列进行操作，随即访问率低，但随即插入、随即删除消息高。
- Vector 是矢量队列，和ArrayList一样，它也是一个动态数组，由数组实现。但是ArrayList是非线程安全的，而Vector是线程安全的。

HashMap & LinkedHashMap & TreeMap
- HashMap 里面存入的键值对在取出的时候是随即的，根据键的hashCode值进行数据存储，根据键可以直接取出它的值，具有很快的访问速度。在Map中插入、删除和定位元素，HashMap值最好的选择。
- TreeMap 取出来的是排序后的键值对。如果要按照自然排序或者自定义顺序遍历键，那么TreeMap会更好。
- LinkedHashMap 是HashMap的一个子类，如果需要输出的顺序和输入的顺序相同，那么用LinkedHashMap可以实现。

### equals & hashCode <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

## 三、多线程编程 <a href="#目录"><img src="/res/back-top.png" height="20" width="20"/></a>

### Handler异步事件处理机制 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

总结：

- Handler与Looper：
	- 在主线程中可以直接创建Handler对象，而在子线程中需要先调用Looper.prepare才能创建Handler对象，否则会抛异常；
	- 每个线程中最多只能有一个Looper对象；
	- 可以通过Looper.myLooper获取当前线程的Looper实例，通过Looper.getMainLooper获取主UI线程的Looper实例；
	- 一个Looper只能对应一个MessageQueue；
	- 一个线程中只有一个Looper实例，一个MessageQueue实例，可以有多个Handler实例；

- Looper.prepare创建了一个MessageQueue；
- 通过Handler发送消息实质就是把消息Message添加到MessageQueue消息队列中的过程；
- 然后通过Looper.loop方法，然后遍历MessageQueue的next不断的把消息出队；
- 当有一个消息出队，就将它传递到dispatchMessage方法，执行message.target的handleMessage；
- 会不断循环，直到调用Looper.quit方法，实质就是调用了MessageQueue消息队列的quit方法；

- 一些优化分析：
	- new Message对象的时候尽量使用Message.obtain，他其实是从Message池中返回了一个新的Message实例，能避免创建新的对象，从而减少内存的开销。
	- 使用内部类创建来创建Handler的时候，Handler对象会隐式的持有一个外部类对象（通常是Activity）的引用。而Handler通常会伴随一个耗时的后台线程一起出现，这个后台线程在任务执行完毕之后，通过消息机制通知Handler，然后Handler把消息发送到UI线程。然而用户在耗时线程执行过程中关闭了Activity，正常情况Activity不再被使用，就应该被GC检查的时候被回收掉，但是由于这个线程尚未执行完，而该线程持有Handler的引用，而Handler又持有Activity的引用，就导致Activity暂时无法被回收。
	- 解决：
		- 1、通过逻辑保护，在关闭Activity的时候停掉后台进程，或者使用Handler的removeCallbacks方法，把消息对象从消息队列中移除。
		- 2、将Handler声明为静态类。静态类不持有外部类的引用，但是这样就不能再Handler中操作Activity对象了，所以需要在Handler中增加一个Activity的弱引用WeakReference。

	- HandlerThread其实就是Thread、Looper、Hander的组合实现，主要是对Looper进行初始化，并提供一个Looper对象给新创建的Handler对象，使得Handler处理消息事件在子线程中处理。
	- 使用HandlerThread必须在

	```java
	mHandlerThread = new HandlerThread("HandlerWorkThread");
  //必须在实例化mThreadHandler之前调运start方法，原因上面源码已经分析了
  mHandlerThread.start();
  //将当前mHandlerThread子线程的Looper传入mThreadHandler，使得
  //mThreadHandler的消息队列依赖于子线程（在子线程中执行）
  mThreadHandler = new Handler(mHandlerThread.getLooper()) {
      @Override
      public void handleMessage(Message msg) {
          super.handleMessage(msg);
          Log.i(null, "在子线程中处理！id="+Thread.currentThread().getId());
          //从子线程往主线程发送消息
          mUiHandler.sendEmptyMessage(0);
      }
  };
	```

### AsyncTask <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

总结：

- 使用AsyncTask可以实现异步处理，其实也是Thread和Handler组合实现，因为其内部使用了Java提供的线程池技术，有效的降低了线程创建数量及限定了同事运行的线程数，还有一些针对性的对线程池的优化操作。使用步骤：
	- 继承AsyncTask，三个参数分别是：
		- Void 在执行AsyncTask时需要传入的参数，可用于在后台任务中使用；
		- Integer 后台任务执行时，如果需要在界面上显示当前的进度，则使用这个；
		- Boolean 当任务执行完毕后，如果需要对结果进行返回，则使用这个；
	- 重写onPreExecute，用于一些初始化操作；
	- 重写doInBackgroud，这个方法中的所有代码都会在子线程中运行，我们应该在这里处理耗时操作；
	- 重写onProgressUpdate，当后台任务中调用了publishProgress方法，就会调用这个方法；
	- 重写onPostExecute，当后台任务执行完毕并通过reture语句返回时，这个方法就很快被调用；
	- 执行execute方法；

源码分析：

- execute执行之后，首先就调用了onPreExecute；
- AsyncTask会有个常量叫SerialExecutor，这个SerialExecutor是使用队列的形式管理Runnable对象；
- 实质上是一个线程池中执行，线程池的默认容量和最大容量都和CPU的个数有关；
- 有个子线程的WorkerRunnable，在这里调用doInBackgroud；
- 然后postResult的形式WorkerRunnable对象再通过Message的形式send回主线程：
	- 如果正在执行中，调用onProgressUpdate；
	- 如果执行完毕，调用finish，finish调用onPostExecute；

## 四、进程间通信 <a href="#目录"><img src="/res/back-top.png" height="20" width="20"/></a>

### RPC <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### IPC <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### AIDL <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### Binder <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### Messenger <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

## 五、图形图像编程 <a href="#目录"><img src="/res/back-top.png" height="20" width="20"/></a>

### 自定义控件 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### LayoutInflate <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### Canvas <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### Animation <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### Shader <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### OpenGL <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### View & SurfaceView <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

#### 1、概念
SurfaceView是View类的之类，可以直接从内存或者DMA等硬件接口取得图像数据，是个非常重要的绘图视图。它的特征是：可以在主线程之外的线程中向屏幕绘图。这样可以避免画图任务繁重的时候造成主线程阻塞，从而提高程序的反应速度。在游戏开发中多用到SurfaceView，游戏中的背景、人物、动画等等尽量在画布Canvas中画出。

SurfaceView是在一个新起的单独线程中可以重新绘制画面，而View必须在UI的主线程中更新画面。那么在UI主线程中更新画面可能会引发一些问题，比如更新画面的时间过长，那么主UI线程会被正在画的函数阻塞，那么将无法响应按键、触屏等消息。当使用SurfaceView时，由于是在一个新的线程更新画面，所以不会阻塞你的UI主线程。但这也带来了另外一个问题，就是事件同步。比如你触屏了一下，你需要SurfaceView中的Thread处理，一般就需要有一个event queue的设计来保存touch event，这会稍稍复杂一点，因为涉及到线程同步。

所以基于以上，根据游戏特点，一般分为两类：
- 1、被动更新画面。比如棋类，这种用View就OK，因为画面的更新是依赖于onTouch来更新，可以直接使用invalidate。因为这种情况下，这一次Touch和下一次的Touch需要的时间比较长，不会产生影响。
- 2、主动更新。比如一个人一直在跑动，这就需要一个单独的thread不停的重绘人的状态，避免阻塞main UI thread。所以显然View不合适，需要SurfaceView来控制。

在一般情况下，应用程序的View都是在相同的GUI线程中绘制的。这个主应用程序线程同时也用来处理所有的用户交互（例如：按钮单击或者文本输入）。我们知道可以把容易阻塞的处理移动到后台线程中，但是，对于一个View的onDraw方法，不能这么做，因为从后台线程修改一个GUI元素会被显示的禁止。

当需要快速的更新View的UI，或者当渲染代码阻塞GUI线程的时间过长的时候，SurfaceView就是解决上述问题的最佳选择。SurfaceView封装了一个Surface对象，而不是Canvas。这一点很重要，因为Surface可以使用后台线程绘制。对于那些资源敏感的操作，或者那些要求快速更新或者高速帧率的地方，例如使用3D图形，创建游戏，或者实时预览摄像头，这一点特别有用。

独立于GUI线程进行绘图的代价是额外的内存消耗，所以，虽然它是创建定制的View的有效方式（有时甚至是必须的），但是使用SurfaceView的时候仍然需要保持谨慎。

1、何时应该使用SurfaceView？

- `SurfaceView`使用的方式和任何`View`所派生的类都是完全相同的。可以像其他View那样应用动画，并把它们放在布局中。
- `SurfaceView`封装的`Surface`支持所有标准`Canvas`方法进行绘图，同时也支持完全的`OpenGL ES`库。
- 使用`OpenGL`，你可以在`Surface`上绘制任何支持的2D或者3D对象，与在2D画布上模拟相同的效果对象，这种方法可以依靠硬件加速（可用的时候）来极大的提高性能。
- 对于显示动态的3D图像来说，例如，那些使用Google Earth功能的应用程序，或者那些提供沉浸体验的交互游戏，`SurfaceView`特别有用。它还是实时显示摄像头预览的最佳选择。

2、如何创建一个新的SurfaceView控件？

- 要创建一个新的`SurfaceView`，需要创建一个新的扩展了`SurfaceView`的类，并实现`SurfaceHolder.Callback`。
- `SurfaceHolder`回调可以在底层的`Surface`被创建和销毁的时候通知`View`，并传递给它对`SurfaceHolder`对象的引用，其中包含了当前有效的`Surface`。
- 一个典型的`SurfaceView`设计模式包括一个由`Thread`所派生的类，它可以接收对当前`SurfaceHolder`的引用，并独立地更新它。

#### 2、实现

1）实现步骤

- 继承SurfaceView
- 实现SurfaceHolder.Callback接口

2）需要重写的方法

``` java
public void surfaceChanged(SurfaceHolder holder,int format,int width,int height){}　　//在surface的大小发生改变时激发

public void surfaceCreated(SurfaceHolder holder){}　　//在创建时激发，一般在这里调用画图的线程。

public void surfaceDestroyed(SurfaceHolder holder) {}　　//销毁时激发，一般在这里将画图的线程停止、释放。
```

3）SurfaceHolder

SurfaceHolder是surface的控制器，用来操纵surface。处理它的Canvas上画的效果和动画，控制表面、大小、像素等。

几个需要注意的方法：
``` java
abstract void addCallback(SurfaceHolder.Callback callback); // 给SurfaceView当前的持有者一个回调对象。

abstract Canvas lockCanvas(); // 锁定画布，一般在锁定后就可以通过其返回的画布对象Canvas，在其上面画图等操作了。

abstract Canvas lockCanvas(Rect dirty); // 锁定画布的某个区域进行画图等..因为画完图后，会调用下面的unlockCanvasAndPost来改变显示内容。相对部分内存要求比较高的游戏来说，可以不用重画dirty外的其它区域的像素，可以提高速度。

abstract void unlockCanvasAndPost(Canvas canvas); // 结束锁定画图，并提交改变。
```

4）总结整个过程

- -> 继承`Surface`并实现`SurfaceHolder.Callback`接口
- -> `surfaceView.getHolder()`获取`SurfaceHolder`对象
- -> `SurfaceHolder.addCallback(callback)`添加回调函数
- -> `SurfaceHolder.lockCanvas()`获取`Canvas`对象并锁定画布
- -> `Canvas`绘图
- -> `SurfaceHolder.unlockCanvasAndPost(Canvas canvas)`结束锁定画布，并提交改变，将图形显示。

#### 3、案例

### onMeasure & onLayout & onDraw <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### Touch事件分发机制 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

#### 主要流程

触摸控件`View`首先执行`dispatchTouchEvent()`方法。

在`dispatchTouchEvent()`方法中首先执行`onTouch()`方法，然后执行`onTouchEvent()`方法，其中包括`onClick()`，后面详解，其中`dispatchTouchEvent()`关键代码如下：

``` java
public boolean dispatchTouchEvent(MotionEvent event) {
	......
	boolean result = false;
	......
	if (onFilterTouchEventForSecurity(event)) {
	    //noinspection SimplifiableIfStatement
	    ListenerInfo li = mListenerInfo;
	    if (li != null && li.mOnTouchListener != null
	            && (mViewFlags & ENABLED_MASK) == ENABLED
	            && li.mOnTouchListener.onTouch(this, event)) {
	        result = true;
	    }

	    if (!result && onTouchEvent(event)) {
	        result = true;
	    }
	}
	......
	return result;
}
```

源码解读：

- **如果** `View`的`mOnTouchListener!=null`（`View`设置了`setOnTouchListener()`方法） **并且** `View`是`enable`（按钮控件默认都是enable的）**的情况下** 会调用`onTouch()`方法：
 	- **如果** `onTouch()`返回`false`，**则** 跳出`if`继续执行，调用`onTouchEvent()`，最终`dispatchTouchEvent()`方法的返回结果和`onTouchEvent()`返回一样。
 	- **如果** `onTouch()`返回`true`，**则** 得到`result=true`，则不会执行`onTouchEvent()`方法，最终`dispatchTouchEvent()`方法返回true。

- **如果** `View`的`mOnTouchListener==null`（`View`没有设置`setOnTouchListener()`方法）**或者** `View`不是`enable`（比如非按钮控件），**则** 不会执行`onTouch()`方法，**则** 跳出`if`继续执行，调用`onTouchEvent()`，最终`dispatchTouchEvent()`方法的返回结果和`onTouchEvent()`返回一样。

- 当`dispatchTouchEvent()`在进行事件分发的时候，只有前一个`action`返回`true`，才会触发下一个action。

然后`onTouchEvent()`关键代码如下：

```java
public boolean onTouchEvent(MotionEvent event) {
    ......
    final int viewFlags = mViewFlags;
    final int action = event.getAction();

    if ((viewFlags & ENABLED_MASK) == DISABLED) {
        if (action == MotionEvent.ACTION_UP && (mPrivateFlags & PFLAG_PRESSED) != 0) {
            setPressed(false);
        }
        // A disabled view that is clickable still consumes the touch
        // events, it just doesn't respond to them.
        return (((viewFlags & CLICKABLE) == CLICKABLE
                || (viewFlags & LONG_CLICKABLE) == LONG_CLICKABLE)
                || (viewFlags & CONTEXT_CLICKABLE) == CONTEXT_CLICKABLE);
    }
	......
    if (((viewFlags & CLICKABLE) == CLICKABLE ||
            (viewFlags & LONG_CLICKABLE) == LONG_CLICKABLE) ||
            (viewFlags & CONTEXT_CLICKABLE) == CONTEXT_CLICKABLE) {
        switch (action) {
            case MotionEvent.ACTION_UP:
                boolean prepressed = (mPrivateFlags & PFLAG_PREPRESSED) != 0;
                if ((mPrivateFlags & PFLAG_PRESSED) != 0 || prepressed) {
                    ......
                    if (!mHasPerformedLongPress && !mIgnoreNextUpEvent) {
                        // This is a tap, so remove the longpress check
                        removeLongPressCallback();

                        // Only perform take click actions if we were in the pressed state
                        if (!focusTaken) {
                            // Use a Runnable and post this rather than calling
                            // performClick directly. This lets other visual state
                            // of the view update before click actions start.
                            if (mPerformClick == null) {
                                mPerformClick = new PerformClick();
                            }
                            if (!post(mPerformClick)) {
                                performClick();
                            }
                        }
                    }
                    ......
                }
                mIgnoreNextUpEvent = false;
                break;
            case MotionEvent.ACTION_DOWN:
                ......
                break;
            case MotionEvent.ACTION_CANCEL:
                ......
                break;
            case MotionEvent.ACTION_MOVE:
                ......
                break;
        }
        return true;
    }
    return false;
}
```

源码解读：
- **如果** `View`是`disenable`状态（`enable`和`clickable`属性可以通过`Java`或者`xml`直接设置）：
	- **如果** `View`是`clickable`，**则** `onTouchEvent`直接消费事件，返回`true`。
	- **如果** `View`是`disclickable`，**则** `onTouchEvent`直接消费事件，返回`false`。
- **如果** `View`是`enable`状态：
	- **如果** `View`是`clickable`，**则** 进入`event.getAction()`的`switch()`判断中，但是最终`onTouchEvent()`都会返回`true`。`switch()`的`ACTION_DOWN`和`ACTION_MOVE`都进行了一些必要的设置和置位，接着手抬起来`ACTION_UP`时，首先判断了是否按下过，同时是不是可以获取焦点，然后尝试获取焦点，然后判断如果不是`longPressed`则通过`post`在`UI线程`中执行一个叫`PerformClick`的`Runnable`，其中`run()`执行的也就是`performClick()`方法，后面详解。
	- **如果** `View`是`disclickable`，**则** `onTouchEvent`直接消费事件，返回`false`。

然后`performClick()`关键代码如下：

```java
public boolean performClick() {
		final boolean result;
		final ListenerInfo li = mListenerInfo;
		if (li != null && li.mOnClickListener != null) {
				playSoundEffect(SoundEffectConstants.CLICK);
				li.mOnClickListener.onClick(this);
				result = true;
		} else {
				result = false;
		}

		sendAccessibilityEvent(AccessibilityEvent.TYPE_VIEW_CLICKED);
		return result;
}
```

源码解读：

- **如果** `li.mOnClickListener!=null`（`View`设置了`setOnClickListener()`方法），**则** 执行`onClick()`方法。

另外看一下`setOnClickListener()`的关键代码：

```java
public void setOnClickListener(@Nullable OnClickListener l) {
    if (!isClickable()) {
        setClickable(true);
    }
    getListenerInfo().mOnClickListener = l;
}
```

可以看到，只要调用`setOnClickListener()`方法设置监听，如果控件是`disclickable`的话，则会自动给设置成`clickable`。

至此，整个流程走完，整理一下：
- -> `dispatchTouchEvent()`
- -> `onTouch()`
- -> `onTouchEvent()`
- -> `onClick()`

整体总结：
- -> 用户点击屏幕任意，首先触发Activity的dispatchTouchEvent()，如果是ACTION_DOWN，则触发onUserInteraction()；
- -> 然后Activity的dispatchTouchEvent()其实就是调用DecorView的dispatchTouchEvent()，即走到了根节点ViewGroup的dispatchTouchEvent()；
- -> 然后会依次下发，下发的过程是调用View(ViewGroup)的dispatchTouchEvent()方法实现的。简单的说就是ViewGroup遍历它包含的子View，调用每个View的dispatchTouchEvent()方法，而当子View为ViewGroup时，又会通过调用ViewGroup的dispatchTouchEvent()方法继续调用其内部的View的dispatchTouchEvent()方法。
- -> 其中dispatchTouchEvent()方法只负责事件的分发，它拥有boolean类型的返回值，当返回true时，顺序下发会中断。
- -> View的dispatchTouchEvent()方法调用onTouch()，根据其返回值决定是否执行onTouchEvent()，在onTouchEvent()中调用onClick()；
- -> ViewGroup多了一个onInterceptTouchEvent()，它在ViewGroup的dispatchTouchEvent()中起到作用，根据返回值来决定下发是否会中断。

### View绘制流程 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

整体总结：

- 整个View的绘图流程是在ViewRootImpl类的performTraversals()方法开始的。
- 递归measure：为整个View树计算实际的大小，然后设置实际的高和宽，每个View控件的实际宽高都是由父视图和自身决定的。实际的测量是在onMeasure方法进行，所以在View的子类需要重写onMeasure方法。
	- measure过程主要就是从顶层父View向子View递归调用view.measure方法的过程（measure又回调onMeasure方法），具体measure核心主要有如下几点：
	-	MeasureSpec（View的内部类）测量规格为int，值由高2位规格模式specMode和低30位具体的specSize组成。其中specMode只有三种值：EXACTLY、AT_MOST、UNSPECIFIED。
- 递归layout：layout方法接收四个参数，分别代表左、上、右、下。
	- layout过程也是从顶层父View向子View的递归调用view.layout方法的过程，即父View根据上一步measure子View所得到的布局大小和布局参数，将子View放在合适的位置。
	- measure操作完成后得到的是对每个View经测量过的measureWidth和measureHeight，layout操作完成后得到的是对每个View进行位置分配后的mLeft、mTop、mRight、mBottom，这些值都是相对于父View来说的。
	- 使用View的getWidth和getHeight方法来获取View测量的宽高，必须保证这两个方法在onLayout流程之后被调用才能返回有效值。
- 递归draw：绘制过程就是把View对象绘制到屏幕上。
	- 如果View是一个ViewGroup，则需要递归绘制其所包含的所有子View。
	- View默认不会绘制任何内容，真正的绘制都需要在自己的子类中实现。
	- View的绘制是借助onDraw方法传入的Canvas类来进行的。
	- 区分View动画和ViewGroup动画：前者指的是View自身的动画，可以通过setAnimation添加；后者是专门针对ViewGroup现实内部子视图时设置的动画，可以在xml布局文件中对ViewGroup设置layoutAnimation属性。

- View的invalidate和postInvalidate方法：
	- View的invilidate方法是将要刷新区域直接传递给了父ViewGroup好的invalidateChild方法，在invalidate中，调用父View的invalidateChild，这是一个从当前向父View回溯的过程，每一层的父View都将自己的显示的区域传递给父View进行绘制。
	- invalidate系列方法请求重回View树，如果View大小没有发生变化就不会调用layout过程，并且只绘制那些需要重绘的View。
	- invalidate调用的invalidateChild最终会调用到scheduleTraversals，scheduleTraversals通过Handler的Runnable发送一个异步消息，调用doTraversal方法，然后最终调用performTraversals执行绘制。
	- invalidate只能在UI Thread中执行，其他线程中需要使用postInvalidate方法。实质就是通过发送异步消息，又在UI Thread中调用了View的invalidate方法。

- View的requestLayout方法：
	- 出发View的requestLayout时实质就是层层向上传递，知道ViewRootImpl为止，然后出发ViewRootImpl的requestLayout方法；
	- requestLayout方法会调用measure过程和layout过程，不会调用draw过程，也不会绘制任何View，包括该调用者本身；



## 六、高性能开发 <a href="#目录"><img src="/res/back-top.png" height="20" width="20"/></a>

### 内存溢出和内存泄露 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### APP性能优化 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### TraceView性能分析 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### ListView性能优化 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### WebView内存泄露分析 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

## 七、文件存储 <a href="#目录"><img src="/res/back-top.png" height="20" width="20"/></a>

### SharedPreferences <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### File <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### SQLite <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

## 八、网络编程 <a href="#目录"><img src="/res/back-top.png" height="20" width="20"/></a>

### 网络协议 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### Socket <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### TCP & UDP <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

## 九、设计模式 <a href="#目录"><img src="/res/back-top.png" height="20" width="20"/></a>

### 单例模式 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### 适配器模式 <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

## 十、源码分析 <a href="#目录"><img src="/res/back-top.png" height="20" width="20"/></a>

### Gson <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### Volley <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### EventBus <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

## X、其他未归类 <a href="#目录"><img src="/res/back-top.png" height="20" width="20"/></a>

### JNI & NDK <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

### Java & JS <a href="#目录"><img src="/res/back-top.png" height="15" width="15"/></a>

http://droidyue.com/blog/2014/09/20/interaction-between-java-and-javascript-in-android/

实现Java和JS交互十分便捷：
- WebView开启JavaScript脚本执行；
	- WebSettings settings = myWebView.getSettings();
	- settings.setJavaScriptEnabled(true);


- WebView设置供JavaScript调用的交互接口；
	- myWebView.addJavascriptInterface(new JsInteration(), "control");
	- JsInteration()方法下文介绍；

```java
public class JsInteration {

		@JavascriptInterface
		public void toastMessage(String message) {
				Toast.makeText(getApplicationContext(), message, Toast.LENGTH_LONG).show();
		}

		@JavascriptInterface
		public void onSumResult(int result) {
				Log.i(LOGTAG, "onSumResult result=" + result);
		}
}
```

	- 网页端代码如下：

```html
<html>
<script type="text/javascript">
    function sayHello() {
        alert("Hello")
    }

    function alertMessage(message) {
        alert(message)
    }

    function toastMessage(message) {
        window.control.toastMessage(message)
    }

    function sumToJava(number1, number2){
       window.control.onSumResult(number1 + number2)
    }
</script>
Java-Javascript Interaction In Android
</html>
```

- 客户端和网页端编写调用对方的代码；
	- Java调用JS
		- 调用js无参无返回值函数

```java
String call = "javascript:sayHello()";
webView.loadUrl(call);
```

		- 调用js有参无返回值函数

```java
String call = "javascript:alertMessage(\"" + "content" + "\")";
webView.loadUrl(call);
```

		- 调用js有参数有返回值的函数

```java
String call = "javascript:sumToJava(1,2)";
webView.loadUrl(call);
```

对于调用js有参数有返回的函数，在Android 4.4之后使用evaluateJavascript即可，如下：

```java
webView.evaluateJavascript("getGreetings()", new ValueCallback<String>() {
  @Override
  public void onReceiveValue(String value) {
      Log.i(LOGTAG, "onReceiveValue value=" + value);
  }});
```

注意：

-	设置WebChromeClient
- 网页加载完成之后调用js方法
- 自己创建一个注释接口名字为@JavascriptInterface，然后将其引入
- 代码混淆问题
- 在js调用后的Java回调线程为主线程


# THE END

# THE END

# THE END

# THE END

# THE END

# THE END
