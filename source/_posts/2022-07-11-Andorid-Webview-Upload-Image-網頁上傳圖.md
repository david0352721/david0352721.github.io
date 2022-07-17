---
title: Andorid Webview Upload Image 網頁上傳圖
date: 2022-07-11 21:50:54
tags: Android, WebView, Kotlin
---

---
最近遇到了網站要包進來App內，有個頁面有上傳照片的需求，就來紀錄一下自己解決的寫法。

### 既然是WebView，一些設定還是要先做好的

先在Manifest上增加網路的設定

```Java
<user-permission android:name="android.permission.INTERNET">
```

在Android 9(API LEVEL 28)以上，如果使用 **http://** 開頭的網址需要多設定一行在Application

```Java
<application>
    ...
    android:usesCleartextTraffic="true"
    ...
</application>
```

網路設定好了，再來是WebView的初始化及Settings設定

Webview Layout的部分就自行設定，我就把它設定成全畫面

```Java
<WebView
    android:id="@+id/mainWebView"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

findViewById

```kotlin
val mainWebView = findViewById(R.id.mainWebView)
```

WebView.Settings

```kotlin
val settings = mainWebView.settings
settings.javaScriptEnabled = true //能使用Javascript
settings.domStorageEnabled = true
settings.allowFileAccess = true
settings.allowContentAccess = true
settings.cacheMode = WebSettings.LOAD_DEFAULT
settings.setSupportZoom(true)
settings.useWideViewPort = true
settings.builtInZoomControls = false
settings.setSupportMultipleWindows(true)
settings.javaScriptCanOpenWindowsAutomatically = true
```


### 觸發機制

webview內的chromclient有upload file時會觸發的function 在裡面實作

### 使用Dialog選出要拍照或是從相簿選取

dialog實作 設定items 選取function

### 先從相簿選取開始

使用registerActivityForRrsult  
launch 直接返回Uri 放回upload file  3

### 再來是拍照以及擷取相片的部分 使用同Uri

intent 拍照 
contentValue 存照片
第一次 resultActivityForResult
接回來 Uri 使用crop
使用第二次 resultActivityForResult
取回擷取完的Uri 放回upload file

### 結尾

第一篇記錄文章，現在開始希望把學到一些小知識存起來，以後突然忘記也可以回來翻找。
