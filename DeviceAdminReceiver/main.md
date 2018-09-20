# DeviceAdminReceiver


**Development Environment**

+ windows 7

+ Android Studio 3.0.1

+ Android api 28

+ Virtul Devices Pixel 2


**Built Environment**

+ 設置DeviceAdmin

    
首先需要在應用中設置DeviceAdmin以便之後取得ComponentName。


1. 新增一個class來繼承DeviceAdminReceiverclass
    
    
直接建立在package下
![](./picture/p1.png)
```
public class MyAdmin extends DeviceAdminReceiver{}
```
2. 在manifest中註冊DeviceAdminReceiver

    
在manifest中的<application></application>之間加入內容：

```

<receiver
            android:name="com.example.user.t0828.MyAdmin"
            android:label="@string/sample_device_admin"
            android:description="@string/sample_device_admin_description"
            android:permission="android.permission.BIND_DEVICE_ADMIN">
            <meta-data
                android:name="android.app.device_admin"
                android:resource="@xml/my_admin" />
            <intent-filter>
                <action android:name="android.app.action.DEVICE_ADMIN_ENABLED" />
            </intent-filter>
        </receiver>

```

<receiver>下的name為自己的 package名 + 繼承DeviceAdminReceiver的類別名。

alt+enter在label下新增string資源。

在@string按下shift+左鍵新增description資源。


在meta-data下的resource新增xml檔XML內容：

```

<device-admin xmlns:android="http://schemas.android.com/apk/res/android">
    <uses-policies>
        <limit-password />
        <watch-login />
        <reset-password />
        <force-lock />
        <wipe-data />
    </uses-policies>
</device-admin>

```

詳細的權限內容可以在android文件中查閱，接下來便能夠開始為應用設置DeviceOwner。


* 設置DeviceOwner

    
一、進入adb指令列模式
    
    
首先要先找到自己的platform-tools資料夾，裡面會附有adb.exe檔
可以在android studio的 Tools -> Android -> SDK Manager 中的 Android SDK Location查詢到SDK路徑，在SDK下可以找到platform-tools資料夾
如果沒有可能是尚未安裝platform-tools。
![](./picture/p3.png)
接下來開啟cmd使用cd指令進入到platform-tools資料夾位置，抵達位置後便能夠透過cmd進入 Android 系統指令列模式可以先透過以下指令搜尋已連接的裝置：
    
```
    
adb devices
    
```
    
顯示連接的手機設備電腦只有連接一台裝置的情況可以直接輸入指令：

    
```
    
adb shell
    
```
    
連接模擬器與一般裝置可以輸入指令：
    
```
    
adb -d shell    //連接一般裝置
    
```
    
```
    
adb -e shell    //連接模擬器
    
```
    
輸入成功會出現$字號便能開始為應用程式設置DeviceOwner。
    
    
設置指令：
    
```
    
dpm set-device-owner com.example.user.t0828/.MyAdmin
   
```
    
com.example.user.t0828要改為app的package名稱.MyAdmin則改為app中 extends DeviceAdminReceiver 的 class。設定完成後便能夠開始執行dpm指令。
    
如果要離開shell可以直接輸入```exit```
    

**The Simplest Sample**


```
 
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        DevicePolicyManager mDevicePolicyManager = (DevicePolicyManager) getSystemService(Context.DEVICE_POLICY_SERVICE);
        ComponentName mDPM = new ComponentName(this, MyAdmin.class);
        String[] packages = {this.getPackageName()};

        if (mDevicePolicyManager.isDeviceOwnerApp(this.getPackageName())) {
            mDevicePolicyManager.setLockTaskPackages(mDPM, packages);
            DevicePolicyManager.LOCK_TASK_FEATURE_OVERVIEW);
            startLockTask();
            mDevicePolicyManager.setLockTaskFeatures(mDPM, DevicePolicyManager.LOCK_TASK_FEATURE_HOME);

        } else {
            Toast.makeText(getApplicationContext(),"Not owner", Toast.LENGTH_LONG).show();
        }

```


    
