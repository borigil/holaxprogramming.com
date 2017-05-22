---
title: 빌드 도구를 모르면 생기는 재앙들
date: 2011-12-24 15:14:40
desc: Spring과 Maven 프로젝트를 통해 시작하기
image: https://maven.apache.org/images/maven-logo-black-on-white.png
categories: devops
---

본격적으로 Spring Framework를 공부하기 위해 http://springsource.org 에서 제공되고 있는 오픈소스 프로젝트를 살펴보기 시작했다. Spring Source에서 제공하는 샘플 프로젝트를 가져와서 실제로 동작시켜 보고 싶었고 프로젝트를 살펴 보던 중 사내에서 운영하는 Spring 프로젝트의 구조와는 차이가 있다는 것을 발견하였다.

<!--more-->

## 에잇 일단 실행부터!

예상은 했지만 Download 받은 소스코드를 이클립스에 Import 한 뒤에 동일한 방식으로는 동작하지 않는다! Maven, pom.xml 처음 보는 키워드와 파일명이 자주 보이기 시작한다.

## Maven?

<img src='https://maven.apache.org/images/maven-logo-black-on-white.png' />

그 이유는 Spring 프로젝트 뿐만 아니라 Java의 대다수의 프로젝트는 Maven이라는 빌드 도구(Build Tool)를 사용하고 있었기 때문이였다.

> 현재 사내에서는 Maven과 같은 Build Tool을 사용하지 않고 이클립스 IDE에서 Java Project 또는 Dynamic Web Project를 통해 프로젝트를 구성하고 로컬에서 직접 빌드하고 있다.

#### 그렇다면 Maven은 무엇을 하는 녀석인가?

Download한 소스 코드를 Build하고 실제로 동작을 시켜보기 위해서는 그 무엇보다 Build Tool부터 알아나아가야 겠다. Maven은 Java기반의 프로젝트를 효율적으로 Build 할 수 있도록 도와주는 도구이다. 그 무엇보다 Maven과 같은 Build Tool을 처음 접하게 되면서 전환된 패러다임은 우리가 관리하는 프로젝트는 개발자 PC의 IDE에서만 빌드할 필요가 없다는 것이였다.

## 빌드 도구의 필요성을 모르면 생기는 재앙들

앞서 말한 것 처럼 Build Tool 없이 IDE를 통해서 빌드하고 운영 환경에 배포하는 프로세스는 여러가지 재앙을 가져다 준다.

- 가장 큰 재앙은 운영 환경에 배포되는 Version이 개발자의 PC에 의해서 결정되어 진다는 것이다.
- 이것을 시작으로 더욱 큰 재앙이 시작되는데 빌드 및 배포를 자동하는 것이 거의 불가능해진다.

> 이로 인해서 수동으로 진행되는 배포 과정에서 개발자의 실수들이 제품에 그대로 영향을 미칠 수 있다는 것이다.

#### Maven의 특징은?

Maven을 통해서 얻을 수 있는 가장 유리한 점은 개발자는 프로젝트에 필요한 다양한 플러그인을 효율적으로 관리하고 개발자의 IDE가 아닌 별도의 빌드 머신에서 빌드하고 결과물을 운영서버에 배포하는 과정을 자동화 할 수 있게 된다는 것이다.

<img src='https://docs.google.com/drawings/d/sXI3kLvdTi1iS2O2nA5En9g/image?w=720&h=513&rev=1009&ac=1' />

그 밖의 Maven의 특징은 아래와 같다:

1. 프로젝트를 모델링 - 다양한 목적을 가지고 있는 플러그인은 POM(Project Object Model)으로 정의된다. 프로젝트에 의존되는 라이브러리들은 pom.xml에서 의존관계를 정의한다.

2. Maven 플러그인을 통한 전역적인 재사용 - Maven은 빌드에 대한 대부분의 책임을 각 플러그인에 위임한다. 이러한 플러그인들은 Maven 저장소(Repository)에 저장되어 진다.

3. 공통 인터페이스 - Maven 이전에는 각 개발환경에 대한 빌드 프로세스를 파악하는데 시간이 꽤나 소요되었는데 빌드에 필요한 프로세스를 정의하고 이를 인터페이스로 제공함으로써 문제를 해결하였다고 한다.

#### 자 그럼, 실제로 사용해보자

Spring + Maven 프로젝트를 효율적으로 관리하기 위해서 기존의 이클립스 대신 STS IDE를 사용하기로 했다. STS는 이클립스 기반에 Maven과 이클립스의 Maven Plugin인 m2Eclipse가 포함된 통합 개발환경이라고 보면 된다.

> https://spring.io/tools

STS를 통해 새로운 Maven Project를 생성하게 되면 아래와 같은 프로젝트 구조를 가진다.

<img src='http://cfile26.uf.tistory.com/image/115B69334F0B973E31F96A' />

```
ㄴ /src/main/java : Java 소스 코드를 관리한다
ㄴ /src/main/resources: 소스 코드이외의 설정 파일과 같은 리소스
ㄴ /src/test/java : 테스트를 위한 소스 코드를 관리한다
ㄴ /src/test/resources : 테스트를 위한 환경에 필요한 리소스
- pom.xml : 프로젝트 정보, 프로젝트에 필요한 플러그인과 관계들을 관리한다
```

#### pom.xml

POM(Project Object Model)은 Maven의 근간이 되는 개념으로 현재 프로젝트의 구성과 프로젝트에서 사용하고 있는 외부 플러그인 그리고 각 플러그인들 간의 의존 관계를 pom.xml에 정의 할 수가 있다.

예시:

**my-app 프로젝트의 구조**

```xml
.
 |-- my-module
 |   `-- pom.xml
 `-- pom.xml
```

**my-app의 pom.xml**

```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1</version>
  <packaging>pom</packaging>

  <modules>
    <module>my-module</module>
  </modules>
</project>
```

이클립스의 Dynamic Web Project 구조는 웹 애플리케이션 프로젝트에 기반이 되는 Spring Framework, Database Connnection을 위한 라이브러리, Freemarker와 같은 Template 엔진 등을 사용하기 위해서는 `/WEB-INF/lib` 에 필요한 라이브러리을 개발자가 직접 복사해 사용했지만, Maven을 활용하면 pom.xml를 이용해 프로젝트에 필요한 라이브러리를 정의하고 Version도 효율적으로 관리 할 수 있다. 뿐만 아니라 pom.xml 정의한 다양한 플러그인은 Maven Repositoriy를 통해 Remote로 자동으로 Download하여 Local에서 관리 할 수 있다.

## Maven이 운영체제에서 IDE 독립적으로 작동할 수 있어야 한다.

STS IDE에서 Maven 플러그인을 통해 빌드하는 것은 아직까지 빌드라는 행위를 자유롭게 할 수 없다는 것을 말한다. 자신의 운영체제 또는 빌드 머신에 Maven을 설치하고 자유롭게 프로젝트를 빌드하기를 바란다.

- Maven Download - https://maven.apache.org/download.cgi
- Maven 설치 및 환경 변수 설정 - https://maven.apache.org/install.html

**설치 확인**

```shell
$ mvn -v
```

#### Maven Commands

Maven을 설치하고 운영체제에 환경 변수 설정이 완료가 되었다면, Command Line에서 아래와 같은 기본적인 명령어들을 살펴보는 것으로 마무리 하겠다.

```shell
$ mvn [options] [<goal(s)>] [<phase(s)>]
```

**프로젝트 생성**

```shell
$ mvn archetype:generate
```

**컴파일**

```shell
$ mvn compile
```

**테스트**

```shell
$ mvn test
```

**배포를 위한 패키징**

```shell
$ mvn package
```

## 마치며

Maven를 통해 처음으로 Build Tool를 통해서 프로젝트를 관리하는 시작을 하였고, 결과적으로 실제 오픈소스 프로젝트를 다운로드한 후 동작해 볼 수 있는 행복한 결과가 찾아오게 되었다. 뿐만 아니라 Maven이라는 도구를 통해 지금까지 빌드 도구를 몰랐을 때 겪었던 재앙들에서 벗어날수 있는 실마리를 찾은 수확이 가장 크다.

## 참고

- https://maven.apache.org/
- https://maven.apache.org/guides/introduction/introduction-to-the-pom.html