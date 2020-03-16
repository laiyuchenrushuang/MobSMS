# MobSMS

### 集成总结

1. 官网  http://wiki.mob.com/sdk-sms-android-3-0-0/

2. 本人的appkey  网址： https://new.dashboard.mob.com/#/SMSSDK （账号手机 密码你懂得） 注意 这个签名文件是MD5,找百度盘Android--- 签名文件，如果不同 需要重新申请

### 注意事项

#### 1. AndroidManifest.xml 的权限申请--->注意android版本 适当调整

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
    <uses-permission android:name="android.permission.RECEIVE_SMS"/>
    <uses-permission android:name="android.permission.READ_SMS"/>
    <uses-permission android:name="android.permission.READ_CONTACTS"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
   
   
#### 2. APK的包，要是签名文件的apk,不要app debug测试

#### 3. 集成核心点

  
#####  3.1 app下的build.gradle添加：

<code>  
	
// 添加插件  
	
apply plugin: 'com.mob.sdk'  

// 在MobSDK的扩展中注册SMSSDK的相关信息    

MobSDK {  

    appKey "2e6742e634d90"  
    
    appSecret "c4554e755455202a79e5660da7a458f2"  
    
    SMSSDK {   
            //默认使用GUI，若不使用GUI，通过以下开关关闭  
            //gui false  
            //若使用GUI的自动填充验证码功能，需打开此设置  
            //autoSMS true  
           }  
       }  
</code>

#####  3.2 事件回调

		EventHandler eh=new EventHandler(){

			@Override
			public void afterEvent(int event, int result, Object data) {

			   if (result == SMSSDK.RESULT_COMPLETE) {
				//回调完成
				if (event == SMSSDK.EVENT_SUBMIT_VERIFICATION_CODE) {
          //提交验证码成功
				}else if (event == SMSSDK.EVENT_GET_VERIFICATION_CODE){
			    //获取验证码成功
				}else if (event ==SMSSDK.EVENT_GET_SUPPORTED_COUNTRIES){
          //返回支持发送验证码的国家列表
                } 
              }else{                                                                 
                 ((Throwable)data).printStackTrace(); 
          }
      } 
   };
   
 #####  3.3 注册和取消注册
 
 SMSSDK.registerEventHandler(eh); //注册短信回调
 
 SMSSDK.unregisterEventHandler(eh); //取消注册
 
 ##### 3.4 事件响应
 
 SMSSDK.getVerificationCode("86", et_phone.text.toString())  // 根据手机号码et_phone，发送验证码信息到此手机号[这个地方有几个重载的方法 注意搭配使用]
 
 SMSSDK.submitVerificationCode("86", et_phone.text.toString(), et_verify.text.toString()); // 根据手机号码和收到的验证码，发送验证结果回调，注意 这个地方有时间限制60s

#### 4. 注意混淆保护

-keep class com.mob.**{*;}  
-keep class cn.smssdk.**{*;}  
-dontwarn com.mob.**  


------------------------------------------------

感谢： https://www.jianshu.com/p/27d0907f1754

