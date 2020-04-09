---
layout: post
title: "[Android/Java] Android 에러 Exception"
categories: Android Java
---

---
Error:FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:preDebugAndroidTestBuild'.
> Conflict with dependency 'com.android.support:support-annotations' in project ':app'. Resolved versions for app (26.1.0) and test app (27.1.1) differ. See https://d.android.com/r/tools/test-apk-dependency-conflicts.html for details.

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FAILED in 1s

Gradle에서 다음과 같이 바꿔줌
androidTestImplementation 'com.android.support.test:runner:1.0.1'
androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.1'


---
구글플레이스토어에 앱 등록 시 안전하지 않은 onReceivedSslError 경고
다음 코드 추가
```java
@Override
public void onReceivedSslError(WebView view, final SslErrorHandler handler, SslError error) {
  final AlertDialog.Builder builder = new AlertDialog.Builder(this);
  builder.setMessage(R.string.notification_error_ssl_cert_invalid);
  builder.setPositiveButton("continue", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int which) {
      handler.proceed();
    }
  });
  builder.setNegativeButton("cancel", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int which) {
      handler.cancel();
    }
  });
  final AlertDialog dialog = builder.create();
  dialog.show();
}
```

Multiple dex files define
중복된 라이브러리가 있는지 확인 후 제거해주면 됨



---
Play Store에 업로드 했는데 앱 검색이 안되는 경우

앱이 특정 기능을 필요로 할 때 사용자의 기기에 그 기능이 없으면 Play Store에서 자동으로 앱을 안보이게 숨기기 때문에 특정 기기에 필요 기능이 없더라도 앱을 사용할 수 있도록 다음과 같이 설정해주면 된다.
```xml
<uses-feature
  android:name="android.hardware.camera.autofocus"
  android:required="false" />
```
[참고](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features)

---
Activity has leaked window
Dialog가 뜬 상태에서 Activity가 종료되어 발생하는 에러
Dialog를 dismiss()하고 Activity를 종료하게 수정하면 에러해결


---
CustomAdapter에서 가장 외곽의 LinearLayout 클릭 이벤트 추가했을 때
java.lang.ClassCastException: java.lang.Integer cannot be cast to com.hanul.haps.telepay104pos.adapter.CustomerListAdapter$ViewHolder at com.hanul.haps.telepay104pos.adapter.CustomerListAdapter.getView(CustomerListAdapter.java:64)
또는 setTag() 에러가 발생하는 경우

```xml
<?xml version="1.0" encoding="utf-8"?> 
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal"> <!-- 여기에 아이디를 직접 추가하지 말고 -->
 
    <!-- 큰 뷰 안에 전체를 감싸는 뷰를 만든 후 이곳에 아이디 추가하고-->
    <!-- 해당 뷰에 setTag를 해주거나 이벤트를 적용한다. -->
    <LinearLayout
        android:id="@+id/listBackground"
        android:layout_width="match_parent" 
        android:layout_height="match_parent"
        android:orientation="horizontal">
        <TextView
            android:id="@+id/name"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_marginBottom="10dp"
        
    android:layout_marginLeft="5dp"
            android:layout_marginRight="5dp"
            android:layout_marginTop="10dp"
            android:layout_weight="0.8"
            android:gravity="center" />
    </LinearLayout>
</LinearLayout>
```


---

Error:Failed to complete Gradle execution.
Cause: The version of Gradle you are using (3.3) does not support the forTasks() method on BuildActionExecuter. Support for this is available in Gradle 3.5 and all later versions.


gradle-wrapper.properties 파일에서 distributionUrl을 다음과 같이 수정할 것
distributionUrl=https\://services.gradle.org/distributions/gradle-4.1-all.zip


---
Error:Unsupported method: BaseConfig.getApplicationIdSuffix(). The version of Gradle you connect to does not support that method. To resolve the problem you can change/upgrade the target version of Gradle you connect to. Alternatively, you can ignore this exception and read other information from the model. Consult IDE log for more details (Help | Show Log)

classpath 'com.android.tools.build:gradle:1.3.0'
-> classpath 'com.android.tools.build:gradle:2.3.2' 로 수정


---
![img1](/img/2019-07-22-android-error-1.png)
이미지와 같은 오류가 발생했을 때

build.gradle에서 띄어쓰기 한번 하고 다시 지운 후 Sync Now 해주면 해결


---
Error:Minimum supported Gradle version is 4.1. Current version is 2.14.1. If using the gradle wrapper, try editing the distributionUrl in D:\socket_test\AndroidServerSocket1\gradle\wrapper\gradle-

gradle-wrapper.properties를 다음과 같이 수정
```
#DATE
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-3.3-all.zip
```


---
cannot resolved symbol

가끔 Activity/Bundle 등에서 'cannot resolved symbol' 에러가 발생할 때
File -> Invalidate Caches / Restart 위 방법으로 캐시를 비워주면 됨


---
Execution failed for task ':app:transformClassesWithDexForRelease'
```java
defaultConfig {
    multiDexEnabled true // 이거 추가
}
```


---
error: illegal character: '\ufeff' ( illegal character 65279)

자바에서 유니코드 BOM을 인식하지 못해서 발생하는 오류
메모장이 아닌 다른 에디터에 해당 내용을 ctrl+c ctrl+v 했다가 java파일을 새로 만들어서 내용을 ctrl+c ctrl+v 하면 해결
BOM : 유니코드가 little endian, big endian, UTF-8 중 어떤건지 파악하기 위해 파일의 맨앞에 어떤 표시


---
Conflict with dependency 'com.android.support:support-annotations' in project ':app'. Resolved versions for app (26.1.0) and test app (27.1.1) differ. See https://d.android.com/r/tools/test-apk-dependency-conflicts.html for details.

app단의 gradle 파일에 가서 아래 부분을 
```java
androidTestImplementation 'com.android.support.test:runner:1.0.1'androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.1'
```
다음과 같이 바꿔 준다
```java
androidTestImplementation('com.android.support.test:runner:1.0.1',{
    excludegroup:'com.android.support',module:'support-annotations'
    })
    androidTestImplementation('com.android.support.test.espresso:espresso-core:3.0.1',{
    excludegroup:'com.android.support',module:'support-annotations'
    })
```


---
compileSdkVersion을 바꾸고 난 후 debug\AndroidManifest.xml에서 생기는 오류

File > Invalidate Caches / Restart > Invalidate and Restart


---
ListView에서 java.lang.IllegalStateException 에러처리

adapter를 notify 하기 전에 listView.setAdapter(adapter)를 다시 해주고 한다


---
ActivityThread: Performing stop of activity that is not resumed 에러

코드에 onResume 추가
```java
@Override
protectedvoidonResume(){
super.onResume();
}
```