# 😱 log4j란?  
LOG for JAVA를 뜻하며 로그문의 출력을 다양한 대상으로 할 수 있도록 도와주는 도구이다.
자바 기반 로깅 유틸리티이며 디버그용 도구로 주로 사용된다.

## ☠️ log4j 사태?!  
애플, 텐센트, 아마존, 테슬라, 클라우드플레어, 스팀, 마인크래프, 구글, 링크드인, 깃헙 등 log4j를 사용하는 기업이 취약점에 노출 된 상황이다.  
RCE(Remote Code Execution) 원격 코드 실행 취약점으로 해커가 내 서버를 맘대로 사용할 수 있다는 것이다.  
```취약점은 JNDI와 LDAP를 이용한다. JNDI는 Java Naming and Directory Interface의 약자로 1990년대 후반부터 Java에 추가된 인터페이스이다.  
Java 프로그램이 디렉토리를 통해 데이터(Java 객체 형태)를 찾을 수 있도록 하는 디렉토리 서비스이다.  

JNDI는 이러한 디렉토리 서비스를 위해 다양한 인터페이스가 존재하는데 그 중 하나가 LDAP이다. 이 LDAP가 이번 취약점에 가장 중요한 포인트이다.  

Java 프로그램들은 앞서 말한 JNDI와 LDAP를 통해 Java 객체를 찾을 수 있다. 예시로 URL ldap://localhost:389/o=JNDITutorial을 접속한다면 LDAP 서버에서 JNDITutorial 객체를 찾을 수 있는 것이다.  

이러한 접근 인터페이스가 이번 사태에 치명적이게 된 이유는, Log4j에는 편리하게 사용하기 위해 ${prefix:name} 형식으로 Java 객체를 볼 수 있게 하는 문법이 존재하기 때문이다.  
예를 들어 ${java:version}은 현재 실행 중인 Java 버전을 볼 수 있게 한다.  

이런 문법은 로그가 기록될 때도 사용이 가능 했고, 결국 해커가 로그에 기록되는 곳을 찾아 ${jndi:sndi:snd://example.com/a}과 같은 값을 추가하기만 하면 취약점을 이용할 수 있는 것이다.   
이 값을 넣는 방법은 User-Agent와 같은 일반적인 HTTP 헤더일 수도 있고 여러가지 방법이 있다.  

출처 : https://namu.wiki/w/Log4j%20%EB%B3%B4%EC%95%88%20%EC%B7%A8%EC%95%BD%EC%A0%90%20%EC%82%AC%ED%83%9C  
```

현재 취약점 등급(CVSS)은 10점으로 매우 치명적인 상황이다.  

⚙️ 조치 방법 / 해결 방법 
1. log4j 버전을 확인한다. 1.x 버전이라면 괜찮다.
2. 2.0-beta-9 부터 2.14.1 사이라면 취약점이 노출 된 상황이다.
3. log4j의 버전을 올려도 상관 없는 상황이라면 최신 버전으로 업그레이드를 한다.
4. 업그레이드를 하지 못하는 상황이라면 사용중인 log4j jar 파일 안에 있는 JndiLookup.class 파일을 삭제 한다. 
zip -q -d log4j-core-*.jar org/apache/logging/log4j/core/lookup/JndiLookup.class
이후에는 당연히 JndiLookup 기능이 동작하지 않게 될 것이다.
5. 사용중인 log4j 의 버전이 2.10 부터 2.14.1 까지의 경우에만 적용이 가능한 방법이다. 
설정 파일에 아래와 같은 코드 중 하나를 넣어주면 된다.
```
log4j2.formatMsgNoLookups=true
```
또는
```
LOG4J_FORMAT_MSG_NO_LOOKUPS=true
```


출처 : https://devocean.sk.com/blog/techBoardDetail.do?ID=163523 


Log4j 보안 취약점 동작 원리 및 조치 방법

https://www.youtube.com/watch?v=kwS3twdVsko 


애플도 당했다! 최악의 보안사태 "Log4J" 설명해드림. 10분컷.  
https://www.youtube.com/watch?v=2mpwuRXefG8 


사상 최악! Log4J 해커들의 공격. 대응방법은?
