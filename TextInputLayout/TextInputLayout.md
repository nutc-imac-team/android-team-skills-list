# TextInputLayout
## import library
先把版本調整好
```
    compile 'com.android.support:design:25.3.1'
```
##  操作說明

##### 底下是我的布局 
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.user1.texttest.MainActivity">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:gravity="center"
        >
        <android.support.design.widget.TextInputLayout
            android:id="@+id/til_account"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            >

            <EditText
                android:id="@+id/et_account"
            android:textSize="15sp"
            android:textColor="#000000"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
                android:hint="帳號"/>

        </android.support.design.widget.TextInputLayout>


        <android.support.design.widget.TextInputLayout
            android:id="@+id/til_password"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            >

            <android.support.design.widget.TextInputEditText
                android:id="@+id/tiet_password"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:hint="密碼"
                android:inputType="textPassword"
                android:textSize="15sp"
                android:textColor="#000000"
                android:layout_width="match_parent"
                android:layout_height="wrap_content
                />

        </android.support.design.widget.TextInputLayout>
        <Button
            android:id="@+id/btn_login"
            android:layout_below="@id/btn_login"
            android:layout_width="100dp"
            android:layout_height="wrap_content"
            android:layout_marginLeft="@dimen/pd_10"
            android:layout_margin="@dimen/pd_10"
            android:text="@string/login"
            />
    </LinearLayout>

</LinearLayout>

```
布局後變成這樣 
![](https://i.imgur.com/7GsGEC8.png)
而跟Edittext不一樣的地方在於它能有些特效存在


這個特效在於TextInputLayout裡的Edittext中有一個屬性
```
android:hint="帳號"
```
點選後就變成了提示標題了!
而如果是不用TextInputLayout的話則會麻煩許多

![](https://i.imgur.com/uVdm8hF.gif)

## Activity 

##其餘屬性
```
package com.example.user1.texttest;

import android.os.Bundle;
import android.support.design.widget.TextInputLayout;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import android.text.TextUtils;

public class MainActivity extends AppCompatActivity  implements View.OnClickListener{
    private Button login;
    private TextInputLayout til_account;
    private TextInputLayout til_password;
    EditText user_account,user_password;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        login=(Button)findViewById(R.id.btn_login);
        til_account = (TextInputLayout) findViewById(R.id.til_account);
        til_password = (TextInputLayout) findViewById(R.id.til_password);
        user_account =(EditText)findViewById(R.id.et_account) ;
        user_password =(EditText)findViewById(R.id.tiet_password);

        login.setOnClickListener(this);

    }

    @Override
    public void onClick(View v) {
        if(login==v){
            String account = til_account.getEditText().getText().toString();
            String password = til_password.getEditText().getText().toString();
            til_account.setErrorEnabled(true);
            til_password.setErrorEnabled(true);
            Log.e("user",""+user_account.getText().length());
            if(user_account.getText().length()>10){
                til_account.setError("字數超過了!");
            }else{
                //验证用户名和密码
                if(user_password.getText().length()>18){
                    til_password.setError("字數超過了!");
                }
                else {
                    if(validateAccount(account)&&validatePassword(password)){
                        Toast.makeText(MainActivity.this,"登录成功",Toast.LENGTH_LONG).show();
                    }
                }
            }
        }
    }
    private boolean validateAccount(String account){
        if(TextUtils.isEmpty(account)){
            showError(til_account,"用户名不能为空");
            return false;
        }
        return true;
    }
    private boolean validatePassword(String password) {
        if (TextUtils.isEmpty(password)) {
            showError(til_password,"密码不能为空");
            return false;
        }

        if (password.length() < 6 || password.length() > 18) {
            showError(til_password,"密码长度为6-18位");
            return false;
        }

        return true;
    }
    private void showError(TextInputLayout textInputLayout,String error){
        textInputLayout.setError(error);
        textInputLayout.getEditText().setFocusable(true);
        textInputLayout.getEditText().setFocusableInTouchMode(true);
        textInputLayout.getEditText().requestFocus();
    }

}

```
加入
```
            app:counterEnabled="true"
            app:counterMaxLength="10"
```
也可以選擇在Activity內使用
```
til_account.setCounterMaxLength(10);
til_account.setCounterEnabled(true);
```
可以算出輸入的字數，而且，錯了還會提醒。
![](https://i.imgur.com/l8uZsGf.png)
![](https://i.imgur.com/zAEvj3n.png)


## 顯示錯誤訊息
TextInputLayout有更簡單的方式提供使用者顯示錯信息
```
if(user_account.getText().length()>10){
                til_account.setError("字數超過了!");
            }
```
![](https://i.imgur.com/dl6dXrW.png)
## 改變外觀

可以透過宣告 style 的方式來改變外觀
```
 <style name="input_text_style_blue" parent="Theme.AppCompat.Light.NoActionBar">
        <item name="colorAccent">#0000ff</item>
    </style>
```
加入style
```
android:theme="@style/input_text_style_blue"
```
Hint 跟輸入框底線被改變顏色了
![](https://i.imgur.com/pvTaa8L.png)

如果只想要改變Hint的顏色的話

```
<style name="input_text_style" parent="Theme.AppCompat.Light.NoActionBar">
        <item name="android:textStyle">bold</item>
        <item name="android:textColor">#00ff00</item>
        <item name="android:textSize">20sp</item>
    </style>
```
在TextInputLayout裡加入這行
```
 app:hintTextAppearance="@style/input_text_style"
```
文字就改變了!

![](https://i.imgur.com/gFM8Igd.png)

更改錯誤信息的顏色
```
<style name="input_text_style_error1" parent="Theme.AppCompat.Light.NoActionBar">
        <item name="android:textStyle">bold</item>
        <item name="android:textColor">#0000ff</item>
        <item name="android:textSize">20sp</item>
    </style>
    ```  
```
在TextInputLayout裡加入這行
```
app:errorTextAppearance="@style/input_text_error_style"
```
![](https://i.imgur.com/ljrzlMD.png)

