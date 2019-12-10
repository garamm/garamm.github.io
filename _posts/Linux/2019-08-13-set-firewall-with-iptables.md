---
layout: post
title: "[Linux] iptables로 방화벽 설정하기"
categories: Linux
---

---
1) 특정 포트가 외부에서 접속할 수 있도록 열기
```
iptables -I INPUT 1 -p tcp --dport 12345 -j ACCEPT
```
 : 외부에서 들어오는(INBOUND) TCP포트 12345의 연결을 받아들인다는 규칙을 방화벽 1번 방화벽 규칙으로 추가
** 옵션
-I: 새로운 규칙을 추가한다.
-p: 패킷의 프로토콜을 명시한다.
-j: 규칙에 해당되는 패킷을 어떻게 처리할지를 정한다.

---
2) 추가한 설정 조회
iptables -L -v
** 옵션
-L: 규칙을 출력
-v: 자세히

---
3) 추가한 설정 삭제
3-1) 규칙 번호로 삭제
iptables -D INPUT 1
3-2) 추가한 규칙으로 삭제
```
iptables -D INPUT -p tcp --dport 12345 -j ACCEPT
```

[출처](https://server-engineer.tistory.com/418)


---
iptables에 등록된 조건 확인하기
```
vi /etc/sysconfig/iptables
```

---
리눅스 방화벽 설정 방법

1) 모든 설정 초기화
```
iptables -F
```

2) INPUT 체인에 로컬호스트 인터페이스에 들어오는 모든 패킷을 허용 추가
```
iptables -A INPUT -i lo -j ACCEPT 
iptables -A INPUT -s 192.168.0.1 -j ACCEPT
```

3) 서버 자체 아이피 허가
```
iptables -A INPUT -s 1.217.12.345 -j ACCEPT
iptables -A INPUT -p tcp --dport 80022 -j ACCEPT
iptables -A INPUT -p tcp --dport 3306 -j ACCEPT 
```

4) INPUT 체인에 state 모듈과 매치되는 연결상태가 ESTABLISHED, RELATED인 패킷에 대해 허용 추가
: INPUT 체인에 접속에 속하는 패킷(응답 패킷을 가진것)과 기존의 접속 부분은 아니지만 연관성을 가진 패킷 (ICMP 에러나 ftp데이터 접속을 형성하는 패킷)을 허용하는 규칙
```
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

5) 특정아이피 접근 가능하도록 허가
```
iptables -A INPUT -s 123.456.789.100 -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT
iptables -A OUTPUT -d 192.168.0.1 -j ACCEPT
iptables -A OUTPUT -d 1.217.12.345 -j ACCEPT
```

6) 모든 아이피로부터의 접근 차단 : itables는 먼저 설정한 규칙이 우선순위 되므로 처음에 허가할 IP를 설정한 후 다른 모든 IP를 막아준다.
```
iptables -A INPUT -j DROP
```

7) 서비스 재시작
```
service iptables save
service iptables reload
service iptables stop
service iptables start
```
