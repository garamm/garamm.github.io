---
layout: post
title: "[Android/Java] 앱이 실행중인지 확인하기"
category: android
date: "2019-05-08"
---

```java
private boolean isAppRunning(Context context){
    ActivityManager activityManager = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
    List<RunningAppProcessInfo> procInfos = activityManager.getRunningAppProcesses();
    for(int i = 0; i < procInfos.size(); i++){
        if(procInfos.get(i).processName.equals(context.getPackageName())){
            return true;
        }
    }
    return false;
}
```