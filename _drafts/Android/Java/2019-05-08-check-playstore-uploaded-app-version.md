---
title: "[Android/Java] PlayStore에 업로드 된 앱 버전 확인"
categories:
  - Android/Java
---

이 방법은 플레이스토어의 웹페이지 구조가 변경될 때 영향을 받기 때문에
서버에서 버전 관리하는 방법을 추천합니다.


Manifest에 Internet permission 추가
```xml
<uses-permission android:name="android.permission.INTERNET" />
```


MarketVersionChecker.java<
```java
import android.util.Log;
 
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;
 
 
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
 
public class MarketVersionChecker {
 
    public static String getMarketVersion(String packageName) {
        try {
            Document doc =
 Jsoup.connect("https://play.google.com/store/apps/details?id=" + packageName).get();
            Elements Version = doc.select(".htlgb").eq(7); // 신규앱은 평점이 없으므로 eq(5)로 해준다
 
            for (Element mElement : Version) {
                return mElement.text().trim();
            }
        } catch (IOException ex) {
            ex.printStackTrace();
        }
        return null;
    }
 
    public static String getMarketVersionFast(String packageName) {
        String mData = "", mVer = null;
        try {
            URL mUrl = new URL("https://play.google.com/store/apps/details?id=" + packageName);
            HttpURLConnection mConnection = (HttpURLConnection) mUrl.openConnection();
            if (mConnection == null) return null;
            mConnection.setConnectTimeout(5000);
            mConnection.setUseCaches(false);
            mConnection.setDoOutput(true);
            if (mConnection.getResponseCode() == HttpURLConnection.HTTP_OK) {
                BufferedReader mReader = new BufferedReader(new InputStreamReader(mConnection.getInputStream()));
                while (true) {
                    String line = mReader.readLine();
                    if (line == null) break;
                    mData += line;
                }
                mReader.close();
            }
            mConnection.disconnect();
        } catch (Exception ex) {
            ex.printStackTrace();
            return null;
        }
        String startToken = "<div class=\"BgcNfc\">Current Version</div><div><span class=\"htlgb\">";
        String endToken = "</span></div>";
        int index = mData.indexOf(startToken);
        if (index == -1) {
            mVer = null;
        } else {
            mVer = mData.substring(index + startToken.length(), index + startToken.length() + 100);
            mVer = mVer.substring(0, mVer.indexOf(endToken)).trim();
        }
        return mVer;
    }
}
```


MainActivity.java
```java
import android.content.DialogInterface;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.net.Uri;
import android.os.Handler;
import android.os.Message;
import android.support.v7.app.AlertDialog;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.support.v7.view.ContextThemeWrapper;
import android.util.Log;
import android.widget.Toast;
 
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.lang.ref.WeakReference;
import java.net.HttpURLConnection;
import java.net.URL;
 
public class MainActivity extends AppCompatActivity {
 
    String deviceVersion;
    String storeVersion;
    private BackgroundThread mBackgroundThread;
 
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        mBackgroundThread = new BackgroundThread();
        mBackgroundThread.start();
 
    }
 
    public class BackgroundThread extends Thread {
        @Override
        public void run() {
            storeVersion = MarketVersionChecker.getMarketVersion(getPackageName());
            try {
                deviceVersion = getPackageManager().getPackageInfo(getPackageName(), 0).versionName;
            } catch (PackageManager.NameNotFoundException e) {
                e.printStackTrace();
            }
            deviceVersionCheckHandler.sendMessage(deviceVersionCheckHandler.obtainMessage());
        }
    }
 
    private final DeviceVersionCheckHandler deviceVersionCheckHandler = new DeviceVersionCheckHandler(this);
 
    private static class DeviceVersionCheckHandler extends Handler {
        private final WeakReference<MainActivity> mainActivityWeakReference;
        public DeviceVersionCheckHandler(MainActivity mainActivity) {
            mainActivityWeakReference = new WeakReference<MainActivity>(mainActivity);
        }
 
        @Override
        public void handleMessage(Message msg) {
            MainActivity activity = mainActivityWeakReference.get();
            if(activity != null) {
                activity.handleMessage(msg);
            }
        }
    }
 
    public void handleMessage(Message msg) {
        Log.i("tag", "deviceVersion : "+deviceVersion);
        Log.i("tag", "storeVersion : "+storeVersion);
 
        if(storeVersion.compareTo(deviceVersion) > 0) {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.setTitle("업데이트")
                    .setMessage("새로운 버전이 있습니다.\n보다 나은 사용을 위해 업데이트 해 주세요.")
                    .setPositiveButton("업데이트 바로가기", new DialogInterface.OnClickListener() {
                        @Override
                        public void onClick(DialogInterface dialog, int which) {
                            Intent intent = new Intent(Intent.ACTION_VIEW);
                            intent.setData(Uri.parse("https://play.google.com/store/apps/details?id="+getPackageName()));
                            startActivity(intent);
                        }
                    });
            AlertDialog dialog = builder.create();
            dialog.show();
        } else {
            // 업데이트 불필요
        }
    }
}
```