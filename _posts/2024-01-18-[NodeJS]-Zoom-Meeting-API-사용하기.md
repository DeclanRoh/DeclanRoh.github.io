---
title: "[Zoom] Meeting API 🌱"
date: 2024-01-18
categories: [zoom, api, sdk]
tags:
    [
        zoom,
        meeting,
        api,
        sdk,
        나의개발일지
    ]
---
# **Zoom**
>원격 미팅, 웨비나, 채팅, 전화, 비즈니스 용 이메일 및 캘린더를 포함한 통합 커뮤니케이션 서비스 (UCaaS) 플랫폼.

* 채팅, 그룹회의(호스트 뿐만 아니라 참여자들도 마이크, 카메라, 화면 공유를 사용할 수 있다.), 투표, 거수, 화면 공유, 원격 제어 요청도 가능.
* 다양한 PC, 스마트폰등을 지원하며 운영체제 역시 MS 윈도우즈, 맥OS, 리눅스 계열의 우분투, 페도라 등 여러가지를 지원.

## **MeetingAPI**

Spring Boot는 Spring Framework를 개선한 것이다. 대표적인 개선사항은 다음과 같다.
* **개발 환경 설정 간소화**: 스프링은 버전에 따라 동작하는 외부 라이브러리를 일일이 찾아 연동해야 하지만, 스프링 부트는 **미리 설정된 스타터 프로젝트로 외부 라이브러리를 최적화해 제공**하므로 사용자가 직접 연동할 필요가 없다.
* **웹 애플리케이션 서버를 내장**: 스프링 부트는 내부에 웹 애플리케이션 서버(WAS, Web Applicationh Server)인 톰캣을 가지고 있어 웹 서비스를 `jar`파일로 간편하게 배포할 수 있다.

🤍 나는 Spring Boot가 Spring 안에 들어있는 서버인 줄 알았는데, 서버가 내장되어 있는 Spring이었다! 사실 Spring은 설정해줘야 하는 것도 많고, 초기 세팅이 힘들었는데 그런 부분을 개선해서 나온 게 Spring Boot라는 사실을 처음 알았다.

# 스프링 부트 개발 환경 설정하기
> JDK(Java Development Kit) 설치 > IDE(Integrated Development Environment) 준비 > Spring Boot 프로젝트 생성

* JDK는 자바 코드의 번역과 실행을 담당하는 개발 도구
* IDE는 개발 생산성을 높여주는 도구

## JDK 설치
1. AdoptOpenJDK(https://adoptium.net/temurin/releases)에 접속한다.
2. 운영체제는 Windows, 아키텍처는 x64, 패키지 타입은 JDK, 버전은 17 - LTS를 선택한다. 스프링 부트 3.x부터는 JDK 17 이상만 지원한다. 이 프로젝트에서는 3.1 버전을 사용하기 때문에 17 버전을 받아준다.
3. 설치파일을 내려받아 다운로드를 진행한다.
4. 명령 프롬프트에서 `java -version`을 입력해 OpenJDK 17이 설치된 것을 확인한다.

## IntelliJ IDEA 설치
1. https://www.jetbrains.com/ko-kr/idea/download 에서 Community Edition을 다운받는다.
2. 내려받은 설치 파일을 클릭해 실행한다.
3. 인텔리제이를 실행한다. 인텔리제이 설정을 import 하라는 팝업이 뜨면 Do not import settings를 선택한다.

## 스프링 부트 프로젝트 만들기
> 스프링 부트 프로젝트를 만들고 웹 브라우저에 "헬로 월드!"를 출력한다.

### 프로젝트를 만들고 실행하기
* 스프링 부트는 Spring Initializr를 사용해 쉽게 프로젝트를 생성할 수 있다.
* Spring Initializr 페이지(https://start.spring.io)에 접속한 후 각 항목을 다음과 같이 설정한다.
![Spring Initializr Setting](/assets/img/posts/2024-01-09-1.png)

* 화면에 보이는 Spring Boot 버전은 프로젝트 생성 시기에 따라 다를 수 있다.
* Project Metadata > Artifact는 프로젝트 이름을 설정하는 부분이다. 이 부분을 원하는 이름으로 수정한다.
* 화면 오른쪽에 있는 Dependencies는 스프링 부트 프로젝트에 필요한 여러 도구를 가져오는 역할을 한다.
* ADD DEPENDENCIES... 버튼을 눌러 Spring Web, H2 Database, Mustache, Spring Data JPA 도구를 추가한다.

#### Spring Boot 버전 접미사
Spring Boot 뒤에 표기된 접미사는 소프트웨어 생명주기를 의미한다.
일반적으로 GA 또는 아무런 접미사가 없는 버전을 선택하면 된다.
* `SNAPSHOT`: 현재 테스트 단계
* `Mx(Milestone)`: 주요 기능 및 버그 수정 중인 단계(M1, M2, ...)
* `RC(Release Candidate)`: 전반적 기능과 버그가 모두 수정된 최종 배포 전 단계
* `GA(General Availability)`: 최종 배포 단계(대부분 기능과 버그들이 안정화됨)

#### 각 도구의 역할
* `Spring Web`: Spring MVC를 사용하여 RESTful을 포함한 웹 애플리케이션을 구축할 수 있게 해준다. Apache Tomcat을 기본 내장 컨테이너로 사용한다.
* `Mustache`: Template Engine으로, 웹 및 독립 실행형 환경 모두를 위한 로직 없는 템플릿이다. if 문, else 절 또는 for 루프가 없고 태그만 있다.
* `H2 Database`: 자료를 저장하기 위한 데이터베이스. 작은(2mb) 설치 공간으로 JDBC API 및 R2DBC 액세스를 지원하는 빠른 인메모리 데이터베이스를 제공한다. 브라우저 기반 콘솔 애플리케이션은 물론 임베디드 모드와 서버 모드도 지원한다.
* `Spring Data JPA`: Spring Data 및 Hibernate를 사용하여 Java Persistence API를 사용하여 SQL 저장소에 데이터를 유지한다. -> 무슨 말인지 잘 모르겠다.. 더 공부해보기!

![Spring Initializr Setting Complete](/assets/img/posts/2024-01-09-2.png)

위 상태에서 GENERATE를 눌러주면 된다.

![Forder unzip](/assets/img/posts/2024-01-09-3.png)

다운로드된 프로젝트의 압축을 풀면 다음과 같다.
이제 인텔리제이에서 [Open]을 통해 해당 폴더를 열어준다.

![IntelliJ Open Folder](/assets/img/posts/2024-01-09-4.png)

프로젝트가 build(소스 코드를 실행할 수 있는 독립적인 형태로 만듬)되고 있다.

![Folder Structure](/assets/img/posts/2024-01-09-5.png)

* `src/main/java` : 자바 코드가 저장된다.
* `src/main/resources` : 외부 파일이 저장된다.
* `src/main/java/com/example/firstproject/FristprojectApplication.java` : 메인 메서드가 존재한다.

#### IntelliJ에서 Spring Boot 버전 바꾸기
![Version Change](/assets/img/posts/2024-01-09-6.png)

`build.gradle` 파일을 열고 `id 'org.springframework.boot' version` 옆에 적혀있는 버전을 원하는 버전으로 수정한다.
옆에 나타나는 코끼리 아이콘을 클릭하면 버전이 변경된다. 

### 헬로 월드! 출력하기
FirstprojectApplciation 파일에 오른쪽 마우스를 클릭해 'Run FirstprojectApp...main()' 를 선택한다. `Ctrl+Shift+F10` 단축키를 이용할 수도 있다.

![Run completed](/assets/img/posts/2024-01-09-7.png)

실행이 완료되면 위와 같이 `Started FirstprojectApplication in 3.502 seconds...` 문구를 볼 수 있다. 이제 주소 표시줄에 localhost:8080을 입력하면 서버에 접속할 수 있다.

![Whitelabel error page](/assets/img/posts/2024-01-09-8.png)

접속하면 위와 같이 Whitelabel Error Page가 보인다. 이는 아직 웹 페이지를 만들지 않았기 때문에 보이는 화면이다.

이제 HTML 파일을 만들어 화면을 만들어 보자.

src > main > resources > static 디렉터리에서 마우스 오른쪽 버튼을 눌러 New > HTML File을 만든다. 본문에 헬로 월드!를 입력한다.

![Create HTML](/assets/img/posts/2024-01-09-9.png)

변경 사항을 서버에 반영하기 위해서는 서버를 재시작해주어야 한다.

![Restart server complete](/assets/img/posts/2024-01-09-10.png)

localhost:8080/hello.html에 접속하면 방금 만든 화면을 볼 수 있게 됐다!

# 웹 서비스의 동작 원리 이해하기
> "헬로 월드!"가 출력되기까지 서버 내부에서는 어떤 일이 일어날까?

## 클라이언트-서버 구조
웹 서비스는 **클라이언트**의 요청에 따른 **서버**의 응답으로 동작한다.

클라이언트란 **서비스를 사용하는 프로그램 또는 컴퓨터**를 말하고, 서버는 **서비스를 제공하는 프로그램 또는 컴퓨터**를 말한다.

웹 브라우저가 클라이언트로서 동작하고, 스프링 부트가 서버 역할을 수행한다.

따라서 서버가 실행되고 있지 않으면 클라이언트의 요청을 처리할 수 없는 상태가 되기 때문에 웹 브라우저를 통해 웹에 접근할 수 없다.

* `localhost` : = 127.0.0.1, 내 컴퓨터를 의미한다.
* `8080` : 포트번호. 클라이언트가 내 컴퓨터의 8080번 방에 뭔가를 요청하는 것을 의미한다. 현재 8080번 방 안에는 스프링 부트 프로젝트가 Tomcat에 담겨 수행되고 있다.
* `hello.html` : 서버에 요청하는 파일. 이렇게 파일을 직접 지정하는 경우 스프링 부트는 기본적으로 **src > main > resources > static** 디렉터리에서 파일을 찾은 후 찾은 HTML 코드를 응답으로 보낸다.


# 참고자료
* [코딩 자율학습 스프링 부트 3 자바 백엔드 개발 입문](https://www.gilbut.co.kr/book/view?bookcode=BN003778)