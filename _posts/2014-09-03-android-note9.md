---
layout: post
title: 项目中的Http的操作
category: android
description: 从Request,Cookie简述项目中的一些Http操作
keywords: android, http
---


##Request##
###一般请求###
项目中使用Volley作为统一的请求库，从服务端返回的数据统一使用json进行了编码，所以App封装了GsonRequest类来统一处理请求（部分涉及文件操作的请求除外）


```java
public class GsonRequest<T> extends Request<T> {

	private final Gson mGson = new Gson();
	private final Class<T> mClazz;
	private final Listener<T> mListener;
	private final Map<String, String> mHeaders;
	private final Map<String, String> mParams;

	public GsonRequest(String url, Class<T> clazz, Listener<T> listener, ErrorListener errorListener, Map<String, String> params) {
		this(Method.POST, url, clazz, null, listener, errorListener, params);
	}

	public GsonRequest(int method, String url, Class<T> clazz, Map<String, String> headers, Listener<T> listener, ErrorListener errorListener, Map<String, String> params) {
		super(method, url, errorListener);
		this.mClazz = clazz;
		this.mHeaders = headers;
		this.mListener = listener;
		this.mParams = params;
	}

	@Override
	protected Map<String, String> getParams() throws AuthFailureError {
		return mParams;
	}

	@Override
	public Map<String, String> getHeaders() throws AuthFailureError {
		return mHeaders;
	}

	@Override
	protected void deliverResponse(T response) {
		mListener.onResponse(response);
	}

	@Override
	protected Response<T> parseNetworkResponse(NetworkResponse response) {
		try {
			GlobalApp.getInstance().checkSessionCookie(response.headers);
			String json = new String(response.data, HttpHeaderParser.parseCharset(response.headers));
			return Response.success(mGson.fromJson(json, mClazz), HttpHeaderParser.parseCacheHeaders(response));
		} catch (UnsupportedEncodingException e) {
			return Response.error(new ParseError(e));
		} catch (JsonSyntaxException e) {
			return Response.error(new ParseError(e));
		}
	}
}
```

Gson为Google官方用来解析json的工具，通过GsonRequest将json字符串转为Android中的对象。
具体调用如下（以登录为例）：


```java
executeRequest(new GsonRequest<UniversalReturn>(Request.Method.POST, MAPI.APIS.get("NORMALLOGIN"), UniversalReturn.class, GlobalApp.getInstance().getHeader(LoginAty.this), normalLoginResponseListener(), errorListener(), dataMap));
```


其中UniversalReturn为通用返回对象类，具体的回调在normalResponseLister()中进行处理：


```java
private Response.Listener<UniversalReturn> normalLoginResponseListener() {
	return new Response.Listener<UniversalReturn>() {
		@Override
		public void onResponse(final UniversalReturn response) {
			TaskUtils.executeAsyncTask(new AsyncTask<Object, Object, Object>() {
				@Override
				protected Object doInBackground(Object... params) {
					return null;
				}

				@Override
				protected void onPostExecute(Object o) {
	
					if ("1".equals(response.ret)) {
						Message msg = Message.obtain();
						msg.what = LOGIN_SUCCESS;
						loginHandler.sendMessage(msg);
					} else {
						Message msg = Message.obtain();
						msg.what = LOGIN_ERROR;
						msg.obj = response.err_msg;
						loginHandler.sendMessage(msg);
					}
					super.onPostExecute(o);
				}
			});
		}
	};
}
```


###图片请求###
使用Afinal处理项目中网络图片的加载，对此简单封装了以下FinalBitmap的使用，定义了FinalBitmapManager用以统一处理，如下：


```java
public class FinalBitmapManager {
	public static final int MEM_CACHE_SIZE = 1024 * 1024 * ((ActivityManager) GlobalApp.getInstance().getContext().getSystemService(Context.ACTIVITY_SERVICE)).getMemoryClass() / 3;

	public static FinalBitmap mFinalBitmap = FinalBitmap.create(GlobalApp.getInstance().getApplicationContext());

	public static void LoadImage(View view, String url) {
		mFinalBitmap.display(view, url);
	}

	public static void changeLoadingImage(int resouceId) {
		mFinalBitmap.configLoadingImage(resouceId);
	}

	public static void setCacheMemory() {
		mFinalBitmap.configMemoryCacheSize(MEM_CACHE_SIZE);
	}

	public static void setThreadSizes() {
		mFinalBitmap.configBitmapLoadThreadSize(6);
	}

}
```


这里只做了最简单的封装，修改加载中图片、设置缓存大小、线程数量等。


具体调用如下：


```java
FinalBitmapManager.changeLoadingImage(R.drawable.default_advertisement);
FinalBitmapManager.LoadImage(iv, mSrc[n]);
```

####上传文件请求####
在使用Volley之后，一度想使用Volley管理上传文件的请求，但是在进行多张图片上传的时候出现了问题，后面还是采用HttpClient，使用MultipartEntity包裹需要post的数据即可，文件类型构造FileBody，字符串类型构造StringBody。


###Cookie###
####请求中添加Cookie####
+ HttpClient形式：


```java
CookieSyncManager cookieSyncManager = CookieSyncManager.createInstance(BBSDetailPageNativeAty.this);
cookieSyncManager.startSync();
CookieManager cm = CookieManager.getInstance();
String Cookiestr = cm.getCookie("http://m.ci123.com");

HttpPost httppost = new HttpPost(MAPI.APIS.get("UPLOAD_IMG"));
httppost.setHeader("Cookie", Cookiestr);
```

+ Volley形式（重写getHeaders()函数即可）：


```java
@Override
public Map<String, String> getHeaders() throws AuthFailureError {
	return mHeaders;
}
```

####接受服务器Set-Cookie####
在parseNetworkResponse(NetworkResponse response)函数中进行Set-Cookie:


```java
public final void checkSessionCookie(Map<String, String> headers) { // 解析Http请求中header
	CookieSyncManager.createInstance(getApplicationContext());
	CookieManager cookieManager = CookieManager.getInstance();
	cookieManager.setAcceptCookie(true);

	if (headers.containsKey(SET_COOKIE_KEY)) {
		String cookie = headers.get(SET_COOKIE_KEY);
		if (cookie.length() > 0) {
			CookieManager.getInstance().setCookie("http://m.ci123.com", cookie);
			CookieSyncManager.getInstance().sync();
		}
	}
}
```
