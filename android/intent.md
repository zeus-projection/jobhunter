# Intent

Intent由6部分信息组成，**Component Name**， **Action**，**Data**，**Category**，**Extras**，**Flags**。根据信息的作用，又可以分为3类

- Component Name ，Action，Data，Category为一类，这4个信息决定了Android会启动哪个组件，其中ComponentName在显式Intent中使用，Action，Data，Category，Extras，Flags用于在隐式Intent中使用。
- Extras为一类，里面包含了具体的用于组件实际处理的数据信息。
- Flags为一类，其是Intent的元数据，决定了Android对其操作的一些行为。

**Component Name**显式启动的Intent的名字，系统根据名字去启动相应的组件。

**Action** 是表示了要执行操作的字符串，比如查看或者选择，如果这个Action有多个App支持响应，会弹出选择框，让用户选择。

**Data** 值得是Uri对象和数据的MIME类型，对应 intent-filter 中的data标签 <data />

一个完成的Uri由scheme，host，port，path组成，格式<scheme>://<host>:<port>/<path>

如果只设置数据的Uri，需要调用Intent对象的setData方法；如果只是设置数据的MIME类型，需要调用Intent对象的setType方法，如果要同时设置数据的Uri和MIME类型，需要调用Intent对象的setDataAndType方法。

千万不要先后调用setData和setType因为后调用的会把先调用的置空；

**Category** category包含了关于组件如何处理Intent的一些信息，虽然可以在Intent中，加入任意数量的Category，但大多数的Intent其实不需要category。

比较常见的Category：CATEGORY_BROWSABLE，CATEGORY_LAUNCHER

**Exras**:

extras顾名思义，就是额外的数据信息，Intent中有一个Bundle对象存储着各种键值对，接受该Intent的组件可以从中读取出所需要的信息以便完成相应的工作。有的Intent需要靠Uri携带信息，有的Intent是靠extras携带数据信息。我们可以通过调用Intent对象的各种重载的putExtra(key, value)方法向Intent中添加各种键值对形式的额外数据。我们也可以直接创建一个Bundle对象，向该Bundle对象传入很多键值对，然后通过调用Intent对象的putExtra(Bundle)方法将其一块设置给Intent对象中去。

**Flags** flag起到作为Intent对象的元数据的作用，这些flag会告知Android系统如何启动Activity。

Intent的几个常用Flag

**FLAG_ACTIVITY_BROUGHT_TO_FRONT**

singleTask 模式下，程序系统自动设置。

**FLAG_ACTIVITY_CLEAR_TOP**

如果Activity已经在当前任务栈中，则不会重新启动Activity，而是clear掉top

**FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET**

在当前Task的 Activity Stack中设置还原点，当新的Activity带着flag**FLAG_ACTIVITY_RESET_TASK_IF_NEEDED**，启动的时候，这个标志的Activity以及其上的Activity都会被clear掉。

**FLAG_ACTIVITY_EXCLUDE_FROM_RECENTS**

这个Activity不会在最近的Activity列表中保存。

**FLAG_ACTIVITY_FORWARD_RESULT**

新的Activity可以直接用setResult把结果传回到这个flag标记的Activity中。

**FLAG_ACTIVITY_LAUNCHED_FROM_HISTORY**

如果Activity是从历史记录中启动的，那么，系统会给Activity设置这个Intent。

**FLAG_ACTIVITY_NEW_TASK**

隐式Intent使用

```java
Intent sendIntent = new Intent();
sendIntent.setAction(Intent.ACTION_SEND);
sendIntent.setType("text/plain");
sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);

PackageManager pm = getPackageManager();
if(pm.resolveActivity(sendIntent, 0) != null) {
  startActivity(sendIntent);
  
  //还可以调用 Intent.createChooser 强行调起app选择框。
}
```

**android:taskAffinity** 决定这个Activity跟哪个任务栈绑定。

##### Intent 和 pendingIntent

PendingIntent，是一个给外界应用（foreign application） 比如（NotificationManager，AlarmManager，Home Screen AppWidgetManager，等等），这允许别的应用通过你的应用权限来执行预定义的代码。

所以，可能有两个功能，1，权限，2，延迟

区别

- Intent是立即使用的，而PendingIntent可以等到事件发生后出发，PendingIntent可以cancel；
- Intent在程序结束后即终止，而PendingIntent在程序结束后依然有效；
- PendingIntent自带Context，而Intent需要在某个Context中运行；
- Intent在原task中运行，PendingIntent在新的task中运行。

通过把PendingIntent传给另外的application，我们给了另外的applicaiton个你的app一样的权限和身份。

即使PendingIntent本身的进程已经被杀死了，但是pendingIntent还是可以执行的。

PendingIntent例子：

```java
NotificationManager nm = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
int icon = android.R.drawable.stat_notify_chat;
long when = System.currentTimeMillis() + 2000;
Notification n = new Notification();
n.defaults = Notification.DEFAULT_SOUND;
n.flags != Notification.FLAG_AUTO_CANCEL;

Intent openintent = new Intent(this. DemoLsit.class);
PenddingIntent pi = PendingIntent.getActivity(this, 0, oepnintent, PendingIntent,FLAG_CANCEL_CURRENT);
n.setLatestEventInfo(this,,,pi);
nm.notify(0, n);
```

```java
public class IParcel implements Parcelable {
  PendingIntent pIntent = null;
  
  public IParcel(PendingIntent pi) {
    pIntent = pi;
  }
  
  public IParcel(Parcel in) {
    pIntent = (PendingIntent)in.readValue(null);
  }
  
  public static Parcelable.Creator<IParcel> CREATOR = new Parcelable.Creator<IParcel>() {
    @Override
    public IParcel createFromParcel(Parcel source) {
      return new IParcel(source);
    }
    
    @Override
    public IParcel[] newArray(int size) {
      return null;
    }
    
    @Override
    public int describeContents() {
      return 0;
    }
    
    @Override
    public void writeToParcel(Parcel dest, int flags) {
      dest.writeValue(pIntent);
    }
  }
  
  public class FirstApp extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      
      Intent myIntent = new Intent(BluetoothAdapter.ACTION_REQUEST_ENABLE);
      PendingIntent pIntent = PendingIntent.getActivity(this, 0, myIntent, PendingIntent.FLAG_ONE_SHOT);
      
      IParcel ip = new Iparcel(pIntent);
      
      Intent sndApp = new Intent();
      sndApp.setComponent(new ComponentName("com.myapp", "com.myapp,SecondApp"));
      sndApp.putExtra(obj, ip);
      startActivity(sndApp);
    }
  }
  
  public class SecondApp extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      
      IParcel pc = getIntent().getExtras().getParcelable(obj);
      if(pc != null) {
        pc.Intent.send();
      }
    }
  }
  
}
```
