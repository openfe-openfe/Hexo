---
title: Android集成微信支付功能
comments: true
date: 2016-03-31 21:00:37
update: 2016-03-31 21:00:37
categories: Android
tags: ['Android','移动支付']
---

在上一篇讲到了[Android集成支付宝支付功能](http://dkylin.com/archives/android-alipay-tutorial.html)，那么这一篇将介绍Android集成微信支付功能。

<!-- more -->

## 开发准备

[1. 微信支付-官方文档](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=8_5)

[2. 微信支付-官方Demo&SDK](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=11_1)

[3. 本教程对应的源代码](https://github.com/dkylin/MobilePaymentDemo/tree/master/Android-Wechat-Pay)

## 微信接入

想要接入微信移动支付功能，需要在[微信开放平台](https://open.weixin.qq.com/cgi-bin/index?t=home/index&lang=zh_CN)及[微信商户平台](https://pay.weixin.qq.com/index.php/home/login?return_url=%2F)注册帐号，并且申请应用，应用申请结束之后需要申请微信支付接口，这个可以在对应平台中查看操作流程，并不是很难。

在官方Demo中，并没有用到服务器端，其实在实际开发中肯定是需要将一些操作步骤放在服务器端的。本篇博客也是基于实际开发环境而作介绍的。所以有客户端代码及服务器端代码，与官方Demo有很大的不同。

### 开发前配置

**1. 导入微信支付Jar包**

将下载的Jar包`libammsdk.jar`复制到libs文件夹下，如果libs文件夹不存在则新建一个，然后右键Jar包，选择`Add to Build Path`即可。

如果开发的时候发现Jar包不起作用，那么则进入该项目的`Java Build Path`，选中`Order and Export`，将libammsdk.jar勾选即可。

**2. 修改Manifest文件**

添加相应权限：

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

在主界面中添加以下内容(不添加也可以支付，但是还是按照官方规定来比较保险)：

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW"/>
    <category android:name="android.intent.category.DEFAULT"/>
    <data android:scheme="wxd930ea5d5a258f4f" />
</intent-filter>
```

同时添加Activity如下:

```xml
<activity
    android:name="com.pay.wechat.wxapi.WXPayEntryActivity"
    android:exported="true"
    android:launchMode="singleTop" >
</activity>
```

修改之后应该是这样的：

```xml
<application
    android:allowBackup="true"
    android:icon="@drawable/ic_launcher"
    android:label="@string/app_name"
    android:theme="@style/AppTheme" >
    <activity
        android:name="com.pay.activity.MainActivity"
        android:label="@string/app_name" >
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
        <intent-filter>
            <action android:name="android.intent.action.VIEW"/>
            <category android:name="android.intent.category.DEFAULT"/>
            <data android:scheme="wxd930ea5d5a258f4f" />
        </intent-filter>
    </activity>
    <activity
        android:name="com.dgaotech.dgfw.wxapi.WXPayEntryActivity"
        android:exported="true"
        android:launchMode="singleTop" >
    </activity>
</application>
```

### 程序结构

由于我的代码分为了客户端跟服务器端，所有程序结构较官方Demo有些不同。

客户端src目录结构如下：

```
src
├── com.pay.activity
|   ├── MainActivity.java
├── com.pay.wechat
|   ├── Constans.java
|   ├── WechatPay.java
├── com.dgaotech.dgfw.wxapi
|   ├── WXPayEntryActivity.java
├── com.pay.utils
|   ├── HttpUtils.java
└── ...
```

* MainActivity.java - 程序主界面
* Constans.java - 基础配置参数
* WechatPay.java - 微信支付类
* WXPayEntryActivity.java - 支付完成的回调界面
* HttpUtils.java - Http请求的辅助类

注意：WXPayEntryActivity.java界面是支付完成(成功/失败/退出)之后的回调界面，它的包名应该是本应用在微信注册的包名，也就是本应用的包名+.wxapi。如：例子程序的包名是：`com.dgaotech.dgfw`，WXPayEntryActivity的包名应该是：`com.dgaotech.dgfw.wxapi`。

### 客户端程序

以我写的Demo为例，主界面代码如下：

```java 
public class MainActivity extends Activity implements OnClickListener{
	private Button btnWechatPay;

	Handler createOrderHandler = new Handler() {
		public void handleMessage(android.os.Message msg) {
			if (msg.what == 0) {
				String result = (String) msg.obj;
				WechatPay.pay(MainActivity.this, result);
			}
		};
	};
	
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        initView();
    }
    
    private void initView(){
    	btnWechatPay = (Button) findViewById(R.id.btn_wechat_pay);
    	btnWechatPay.setOnClickListener(this);
    }
    
	public class CreateOrderThread extends Thread {
		@Override
		public void run() {
			final String trade_no = System.currentTimeMillis()+""; // 每次交易号不一样
			final String total_fee = "0.01";
			final String subject = "测试的商品";
			String result = WechatPay.createOrder(trade_no, total_fee, subject);
			Message msg = createOrderHandler.obtainMessage();
			msg.what = 0;
			msg.obj = result;
			createOrderHandler.sendMessage(msg);
		}
	}
    @Override
	public void onClick(View view) {
		switch (view.getId()) {
		case R.id.btn_wechat_pay:
			// 使用微信进行支付
			new CreateOrderThread().start();
			break;
		default:
			break;
		}
	}
```

当点击了支付按钮之后，创建一个子线程先创建一个订单（统一下单），订单创建完成之后会返回`prepay_id`参数，有了这个参数之后才能向微信发起支付。

下面是微信支付类`WechatPay.java`的代码：

```java
public class WechatPay {
	/**
	 * 生成订单的方法
	 * 
	 * @param tradeNo 交易号
	 * @param totalFee 支付金额
	 * @param subject 详细描述
	 * @return
	 */
	public static String createOrder(String tradeNo, String totalFee, String subject) {
		String result = "";
		String URL_PREPAY = Constants.URL_PAY_CALLBACK + "/UnifiedOrderServlet";
		try {
			subject = URLEncoder.encode(subject, "UTF-8");
			String url = URL_PREPAY + "?trade_no=" + tradeNo + "&total_fee=" + totalFee + "&subject=" + subject;
			result = HttpUtils.doGet(url);
		} catch (UnsupportedEncodingException e) {
			e.printStackTrace();
		}
		return result;
	}
	/**
	 * 支付的方法
	 * @param activity
	 * @param result 服务器生成订单返回的json字符串
	 * 
	 */
	public static void pay(Activity activity, String result) {
		IWXAPI api = WXAPIFactory.createWXAPI(activity, Constants.WX_APP_ID); // 将该app注册到微信
		JSONObject jsonObject;
		try {
			jsonObject = new JSONObject(result);
			PayReq payReq = new PayReq();
			payReq.appId = Constants.WX_APP_ID;
			payReq.partnerId = Constants.WX_MCH_ID;
			payReq.prepayId = jsonObject.getString("prepayid");
			payReq.nonceStr = jsonObject.getString("noncestr");
			payReq.timeStamp = jsonObject.getString("timestamp");
			payReq.packageValue = jsonObject.getString("package");
			payReq.sign = jsonObject.getString("sign");
			api.sendReq(payReq);
		} catch (JSONException e) {
			e.printStackTrace();
		}
	}
}
```

此类有两个方法，第一个方法`createOrder()`的功能是向服务器发送统一下单的请求以获取`prepay_id`参数。第二个方法则是支付功能。

### 服务器端程序

服务器端主要功能是生成预支付订单（官方称统一下单），同时可以在服务器端配置回调接口。

统一下单的Servlet主要代码如下：

```java
public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	PaymentController controller = new PaymentController();
		
	response.setContentType("text/html;charset=UTF-8");
	response.getOutputStream().write(controller.unifiedOrder(request, response).getBytes("UTF-8"));
}
```

`PaymentController`类中有两个方法，`unifiedOrder(HttpServletRequest request,HttpServletResponse response)`方法就是生成预支付订单的方法，`getRemoteHost(HttpServletRequest request)`方法则是获取客户端IP的方法。具体代码请查看源代码。

## Demo截图

![Android-Wechat-Pay](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fmobile-payment%2Fandroid-wechat-pay.png)

## 注意事项

源代码中涉及到的敏感参数已清空，请自行替换成自己申请的应用中包含的参数。

[本教程对应源代码](https://github.com/dkylin/MobilePaymentDemo/tree/master/Android-Wechat-Pay)
