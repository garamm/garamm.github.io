---
layout: post
title: "[Android/Java] apk 파일 만드는 방법"
categories: Android Java
---

---
신규로 생성하는 경우

1) Key Store 생성하기위해 Build>Generate Signed APK 선택
![img1](/img/2019-08-12-make-apk-file-1.png)

2) Generate Signed APK에서 [Create new] 버튼 클릭해서 Key Store 정보 입력
![img2](/img/2019-08-12-make-apk-file-2.png)

3) Choose keystore file에서 keystore가 만들어질 경로 지정 및 파일명 입력
![img3](/img/2019-08-12-make-apk-file-3.png)

4) 경로와 파일명이 지정되면 아래 정보를 입력한다. 중요한 것은 비밀번호 설정이다.비밀번호는 분실하면 확인하거나 변경할 방법이 없으므로 잘 기억한다. Validity (years)는 해당 Key Store의 사용 기간을 말한다.지정한 기간이 경과하면 Key Store를 새로 만들어야 한다.
![img4](/img/2019-08-12-make-apk-file-4.png)

5) 정보가 다 입력된 후에는 APK에 서명하면서 프로젝트를 APK로 추출할 수 있다. [Next]를 누른다.
![img5](/img/2019-08-12-make-apk-file-5.png)

6) APK 파일이 만들어지는 경로가 보여진다. 원하는 위치로 변경할 수 있는데 기본 경로를 사용했다.[Finish] 클릭
![img6](/img/2019-08-12-make-apk-file-6.png)

7) 프로젝트의 app 디렉토리 하위 경로를 보면 생성된 app-release.apk가 보인다. 이 파일을 구글 플레이에 등록할 수 있다.
![img7](/img/2019-08-12-make-apk-file-7.png)


---
이미 Key Store 파일이 만들어져 있다면 APK 파일만 추출한다.
플레이스토어에 이미 등록된 앱을 업데이트 하려면 build.gradle(Module:app)에서 versionCode, versionName을 업데이트 한 후 APK를 추출해야 한다.

1) Build>Generate Signed APK 선택하여 아래 창에서 [Choose existing] 선택
![img8](/img/2019-08-12-make-apk-file-8.png)

2) Key Store 파일 선택
![img9](/img/2019-08-12-make-apk-file-9.png)

3) 파일을 읽어오면 Key store 비밀번호를 입력하고 Key alias를 찾기위해 우측의 search 버튼을 클릭하여Key alias를 읽어오고 Key 비밀번호를 입력한다.
![img10](/img/2019-08-12-make-apk-file-10.png)

4) 기본 경로에 APK 파일을 서명해서 추출한다.
![img11](/img/2019-08-12-make-apk-file-11.png)