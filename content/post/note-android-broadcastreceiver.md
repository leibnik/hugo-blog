---
date: 2015-06-02
categories: android
tags: [android,note]
title: 【笔记】BroadcastReceiver
layout: post
---

《Android第一行代码》的学习笔记

### 一、 接收广播

#### **动态注册广播**

通过Toast提醒网络发生变化

```java
public class MainActivity extends Activity {

    private IntentFilter intentFilter;

    private NetworkChangeReceiver networkChangeReceiver;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        intentFilter = new IntentFilter();
        intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
        networkChangeReceiver = new NetworkChangeReceiver();
        registerReceiver(networkChangeReceiver,intentFilter);
    }
    
    @Override
    protected void onDestroy() {
        super.onDestroy();
        unregisterReceiver(networkChangeReceiver);
    }

    class NetworkChangeReceiver extends BroadcastReceiver{
        @Override
        public void onReceive(Context context, Intent intent) {
            Toast.makeText(context, "network changes",Toast.LENGTH_LONG).show();
        }
    }
}
``` 

<!-- more -->
对程序进行改进准确告知用户网络的状态

```java
class NetworkChangeReceiver extends BroadcastReceiver{
        @Override
        public void onReceive(Context context, Intent intent) {
            //系统服务类，用于管理网络连接
            ConnectivityManager connectivityManager = (ConnectivityManager)
                    getSystemService(Context.CONNECTIVITY_SERVICE);
            NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();
            if(networkInfo != null && networkInfo.isAvailable()){
                Toast.makeText(context,"network is available",Toast.LENGTH_SHORT)
                        .show();
            }else{
                Toast.makeText(context,"network is unavailable",Toast.LENGTH_SHORT)
                        .show();
            }
//            Toast.makeText(context, "network changes",Toast.LENGTH_LONG).show();
        }
    }
```

添加权限

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

#### **静态注册实现开机启动**

> 动态注册的广播接收器可以自由地控制注册与注销，在灵活性方面有很大的优势，但是它也存在着一个缺点，即必须要在程序启动之后才能接收到广播，因为注册的逻辑是写在onCreate()方法中的。

新建一个BootCompleteReceiver继承BroadReceiver,代码如下所示

```java
public class BootCompleteReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context, "Boot Complete",Toast.LENGTH_SHORT).show();
    }
}
```

修改AndroidManifest.xml

```xml
<application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <receiver android:name=".BootCompleteReceiver">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
            </intent-filter>
        </receiver>
</application>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
```

### 二、发送广播

#### **发送标准广播**

先定义一个MyBroadcastReceiver来接收广播

```java
public class MyBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context, "receiver in MyBroadcastReceiver",
                Toast.LENGTH_SHORT).show();
    }
}
```

注册广播

```xml
<receiver android:name=".MyBroadcastReceiver">
            <intent-filter>
                <action android:name="droidroid.com.sendbroadcast.MY_BROADCAST"/>
            </intent-filter>
</receiver>
```

为button添加点击事件以发送广播

```java
public class MainActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button = (Button) findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent("droidroid.com.sendbroadcast.MY_BROADCAST");
                sendBroadcast(intent);
            }
        });
    }
}
```

#### **发送有序广播**

在发送标准广播的基础上，新建一个新的类AnotherBroadcastReceiver

```java
public class AnotherBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context,"received in AnotherBroadcastReceiver",Toast.LENGTH_SHORT).show();
    }
}
```

注册广播接收器

```xml
<receiver android:name=".AnotherBroadcastReceiver">
            <intent-filter>
                <action android:name="droidroid.com.sendbroadcast.MY_BROADCAST"/>
            </intent-filter>
</receiver>
```
修改MainActivity

```java
public class MainActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button = (Button) findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent("droidroid.com.sendbroadcast.MY_BROADCAST");
                //发送有序广播
                sendOrderedBroadcast(intent,null);
            }
        });
    }
}
```

对MyBroadcastReceiver设置优先级

```xml
<receiver android:name=".MyBroadcastReceiver">
            <intent-filter android:priority="100">
                <action android:name="droidroid.com.sendbroadcast.MY_BROADCAST"/>
            </intent-filter>
</receiver>
```

最后在设置在MyBroadcastReceiver拦截广播

```java
@Override
public void onReceive(Context context, Intent intent) {
    Toast.makeText(context, "receiver in MyBroadcastReceiver", Toast.LENGTH_SHORT).show();
        abortBroadcast();
}
```

#### **发送本地广播**

>上面所介绍的是系统全局广播，即发出的广播可以被其他任何应用程序接收到，并且我们也可以接收来自其他应用的广播。因此容易引发安全性的问题

引入广播正是为了解决这个问题

本地广播用法并不复杂，主要使用一个LocalBroadcastManager来对广播进行管理，并提供了发送广播和注册广播接收器的方法。

```java
public class MainActivity extends Activity {

    private IntentFilter intentFilter;

    private LocalReceiver localReceiver;

    private LocalBroadcastManager localBroadcastManager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //获取实例
        localBroadcastManager = LocalBroadcastManager.getInstance(this);
        Button button = (Button) findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent("droidroid.com.localbroadcast.LOCAL_BROADCAST");
                localBroadcastManager.sendBroadcast(intent);
            }
        });
        intentFilter = new IntentFilter();
        intentFilter.addAction("droidroid.com.localbroadcast.LOCAL_BROADCAST");
        localReceiver = new LocalReceiver();
        localBroadcastManager.registerReceiver(localReceiver,intentFilter);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        localBroadcastManager.unregisterReceiver(localReceiver);
    }

    class LocalReceiver extends BroadcastReceiver{
        @Override
        public void onReceive(Context context, Intent intent) {
            Toast.makeText(context,"received local broacast",Toast.LENGTH_SHORT).show();
        }
    }
}
```

#### **实践——实现强制下线功能**

login.xml

```xml
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent" android:layout_height="match_parent"
    android:stretchColumns="1">
    <TableRow>

        <TextView android:layout_height="wrap_content"
            android:text="Account:"/>
        <EditText android:id="@+id/account"
            android:layout_height="wrap_content"
            android:hint="Input your account"/>
    </TableRow>
    <TableRow>
        <TextView
            android:layout_height="wrap_content"
            android:text="Password:"/>
        <EditText
            android:id="@+id/password"
            android:layout_height="wrap_content"
            android:inputType="textPassword"/>
    </TableRow>
    <TableRow>
        <Button
            android:id="@+id/login"
            android:layout_height="wrap_content"
            android:layout_span="2"
            android:text="Login"/>
    </TableRow>
</TableLayout>
```

activity_main.xml

```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/force_offline"
        android:text="Send force offline broadcast"/>
</LinearLayout>
```

LoginActivity.java

```java
public class LoginActivity extends BaseActivity {
    private EditText accountEdit;

    private EditText passwordEdit;

    private Button login;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.login);
        accountEdit = (EditText) findViewById(R.id.account);
        passwordEdit = (EditText) findViewById(R.id.password);
        login = (Button) findViewById(R.id.login);
        login.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String account = accountEdit.getText().toString();
                String password = passwordEdit.getText().toString();
                if (account.equals("admin") && password.equals("123456")) {
                    Intent intent = new Intent(LoginActivity.this,
                            MainActivity.class);
                    startActivity(intent);
                    finish();
                } else {
                    Toast.makeText(LoginActivity.this, "account or password is invalid",
                            Toast.LENGTH_SHORT).show();
                }
            }
        });
    }
}
```

MainActivity.java

```java
public class MainActivity extends BaseActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button forceOffline = (Button) findViewById(R.id.force_offline);

        forceOffline.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent("droidroid.com.broadcastbestpractice.FORCE_OFFLINE");
                sendBroadcast(intent);
            }
        });
    }

}
```

ForceOfflineReceiver.java

```java
public class MainActivity extends BaseActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button forceOffline = (Button) findViewById(R.id.force_offline);

        forceOffline.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent("droidroid.com.broadcastbestpractice.FORCE_OFFLINE");
                sendBroadcast(intent);
            }
        });
    }

}
```

BaseActivity.java

```java
public class BaseActivity extends Activity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ActivityCollector.addActivity(this);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        ActivityCollector.removeActivity(this);
    }
}
```

ActivityCollector.java

```java
public class ActivityCollector {

    public static List<Activity> activities = new ArrayList<Activity>();

    public static void addActivity(Activity activity){
        activities.add(activity);
    }

    public static void removeActivity(Activity activity){
        activities.remove(activity);
    }

    public static void finishAll(){
        for(Activity activity : activities){
            if(!activity.isFinishing()){
                activity.finish();
            }
        }
    }
}
```

AndroidManifest.xml

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="droidroid.com.broadcastbestpractice" >

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity android:name=".LoginActivity"
            android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
        <activity
            android:name=".MainActivity"
            >
        </activity>
        <receiver android:name=".ForceOfflineReceiver">
            <intent-filter>
                <action android:name="droidroid.com.broadcastbestpractice.FORCE_OFFLINE"/>
            </intent-filter>
        </receiver>
    </application>
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
</manifest>
```



