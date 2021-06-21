# JVM 구조 (Java Virtual Machine Architecture) 

> JVM은 시스템 메모리를 관리하면서 자바 기반 애플리케이션을 위해 실행 환경을 제공한다.
> 자바 프로그램이 어떤 운영체제 또는 기기 상에서도 실행될 수 있게 하고 메모리를 관리하고 최적화 한다.

### 1. 자바 프로그램의 구동 원리

![img](https://t1.daumcdn.net/cfile/tistory/99B3F53B5B7579C909)

1. `javac MyAProgram.java` : 소스코드를 작성하고 (.java 확장자의 소스 파일) 명령어를 통해 컴파일러가 바이트 코드로 변환한다. (.class 파일 생성)
2. `java MyAProgram` : 바이트 코드를 실행한다.
3. 런처 (java.exe)로 자바 가상 머신을 구동 하면 자바 가상 머신이 바이트 코드를 해석하여 프로그램 실행

![image](https://user-images.githubusercontent.com/40524837/122760241-dd717780-d2d5-11eb-8a7d-de3a5a1fc11e.png)

일반적으로 자바 프로그램의 파일들은 운영체제에서 직접 동작하는 것이 아니라 **JVM** 위에서 동작한다. 자바 프로그램은 한 번 만들기만 하면 윈도우든, 리눅스든, 어느 운영체제에서도 실행할 수 있다.  즉, 이식성이 높다고 말할 수 있다. 
하지만 **일반적으로 각 운영체제에 맞는 가상 머신을 설치해야 하므로 운영체제에 종속적**이라고 할 수 있다. 또한 자바 프로그램은 일반 프로그램보다 한 단계를 더 거치기 때문에 **상대적으로 실행속도가 느리다.** 

<br>

### 2. 자바 가상 머신의 구조 (JVM Architeture)

![JVM Architecture](https://4.bp.blogspot.com/-wPo3NfPJgTk/XA_3knE60VI/AAAAAAAAJ2o/Svuaykh7UAsjY0mhtgODYREuNNmxzMDngCLcBGAs/s640/jvm.JPG)

### 1. Class Loader subsystem

>  자바에서 소스를 작성하면 `MyProgram.java` 처럼 .java 파일이 생성된다. 소스를 자바 컴파일러가 컴파일하면 `MyProgram.class` .class(바이트코드)가 생성된다. 이렇게 생성된 클래스 파일들을 엮어서 할당받은 메모리 영역인 런타임 데이터 영역에 배치한다. 자바는 동적으로 클래스를 읽어오므로 (동적 로딩) 런타임 시점에서야 모든 코드가 JVM과 연결된다.

`동적 로딩` : Java는 동적 로딩을 지원하기 때문에 실행 시에 모든 클래스가 로딩되지 않고 필요한 시점에 클래스를 로딩하여 사용할 수 있다.

#### 1-1 Loading

> JVM 클래스들을 로드하기 위해 위임(delegation)을 한다. 부트스트랩 로더에서 클래스를 찾지 못하면, 확장 로더에서 찾고, 찾지 못하면 시스템 로더에서 마지막으로 시스템 로더에서 찾고 실패한다면 java.lang.ClassNot-FoundException 런타임 에러를 만난다.

- Bootstrap ClassLoader : **JRE(Java Runtime Environment)**의 lib 폴더에 있는 rt.jar 파일을 찾아 기본 자바 API 라이브러리를 로드하고 가장 최우선으로 로드된다. 

  `rt.jar` : 모든 기본 자바 환경을 클래스를 담고 있는 파일 (자바 런타임 표준 클래스가 모두 포함된 아카이브 파일 classes.zip 과 동일하며, 아카이브 형식과 이름만 다른것을 사용하고 있다. )

- Extention ClassLoader : jre의 폴더에 있는 ext폴더에 있는 모든 확장 코어 클래스파일들을 로드한다. JDK 확장 디렉토리 또는 

- Application ClassLoader : 어플리케이션 레벨에 있는 클래스들을 로드한다. 즉, 사용자가 직접 정의한 클래스 파일들을 로드한다. Classpath 환경 변수에 있는 클래스 파일이나 -classpath 또는 -cp 명령어 옵션이 있는 파일들을 로드한다. 

  `-classpath or -cp` :  JVM이 프로그램을 실행할 때, 클래스파일을 찾는 기준이 되는 파일 경로를 말하는 것이다. .class 파일을 찾을 때 classpath에 지정된 경로를 사용한다. (환경 변수 CLASSPATH 와 -classpath 를 사용한다. )

#### 1-2 Linking

- 검증 (verify) : 바이트코드 검증기는 생성된 자바 바이트코드가 적절한지 아닌지에 대해서 검증하며 검증 실패하면 런타임 에러를 볼 수 있다. 
- 준비 (prepare) : 모든 정적변수의 메모리가 할당되며 기본 default 값으로 할당된다. 클래스 변수들을 위한 메모리에 할당한다.
- 해석 (resolve) : 모든 심볼릭한 (명확하게 정의되지 않은) 메모리 참조를 메소드 영역에 있는 타입으로 직접 참조한다. 

#### 1-3 Initialization

이 단계에서 모든 정적 변수들이 정의 된 값으로 초기화 된다. 

<br>

### 2. RunTime Data Areas 

> 클래스 로더에서 분석된 클래스 파일의 데이터를 저장하고 실행 도중에 필요한 데이터를 저장한다. 메모리를 효율적으로 관리하기 위해 크게 5개의 영역으로 구분한다. 

#### 1. Method (Garbage Collection 대상)

가장 먼저 데이터가 저장되며, 클래스, 메서드, 클래스(static), 전연변수가 저장된다. 모든 스레드에서 공유할 수 있다. 

#### 2. Heap (Garbage Collection 대상)

런타임 시 결정되는 **참조형 데이터타입**이 저장되고, **new 연산자를 통해 생성된 객체**가 저장된다. 모든 스레드에서 공유할 수 있다. 

#### 3. Stack

컴파일 시 결정되는 **기본형 데이터타입**이 저장되는 공간이다. 지역변수, 매개변수, 리턴값, 참조변수 등이 저장된다. 메서드 호출될 때, 메모리에 FILO로 하나씩 생성 된다. 메서드 끝날 때, LIFO로 하나씩 제거된다. 

#### 4. Pc Register

JVM이 수행할 명령어의 주소를 저장하는 공간이다. 스레드가 시작될 때 마다 생성한다.

#### 5. Native Mehod

바이트 코드가 아닌, 기계어로 작성 된 코드를 실행하는 공간이다. 다른 언어로 작성된 코드를 수행하기 위한 것이다.

<br>

### 3. Execution Engine

> 실행 엔진은 바이트 코드로 된 .class 파일을 실행한다. 바이트 코드를 한줄씩 읽고, 다양한 메모리 영역에 나타난 데이터와 정보를 사용하고 명령을 실행한다. 

#### 1. Interpreter 

바이트 코드를 빠르게 해석하지만 한줄씩 수행하기 때문에 수행속도가 느리다. 왜냐하면 하나의 메소드가 여러번 호출되었을 때, 매번 해석해야 한다는 단점이 있습니다.

#### 2. JIT (Just-In-Time)

인터프리터의 효율을 증가하는 데 이용한다. 반복되는 코드가 발견되었을 때에는 반복되는 부분을 native code(원시코드)로 컴파일 합니다. 변환된 원시코드는 인터프리터의 변환과정없이 직접적으로 사용이 가능하기 때문에 이로인해 시스템의 성능이 좋아지게 된다. 

#### 3. Garbage Collector 

아무 참조가 없는 인스턴스들을 모아 제거하는 역할을 한다. `System.gc()` 메소드로 가비지 컬렉션을 실행할 수 있지만 보장되지는 않는다. (하지만 메서드를 호출하는 것은 시스템의 성능에 매우 큰 영향을 끼치므로 System.gc() 메서드는 절대로 사용하지 않는 것이 좋다.) https://d2.naver.com/helloworld/1329

<br>

### 4. Java Native Interface (JNI) & Native Method Libraries 

- Java Native Interface (JNI) : Native Method Libraries 와 상호작용하고 실행에 필요한 네이티브 라이브러리를 제공하는 인터페이스이다. C, C++ 로 만들어진 고유기능의 함수를 Java 메서드와 연결해준다.

- Native Method Libraries : Execution Engine으로 인해 요구되는 네이티브 라이브러리(C, C++)의 모임이다.

  C, C++ 을 작성하기 위한 비쥬얼 스튜디오가 필요하다. (**Native Method Libraries** are **libraries** that are written in other programming languages)



<hr>
**참고** <br>
http://adnjavainterview.blogspot.com/2017/04/java-vertual-machinejvm-architecture-in.html <br>
https://sas-study.tistory.com/262 <br>
https://gbsb.tistory.com/2 <br>
https://mygumi.tistory.com/115 <br>
https://docs.oracle.com/javase/tutorial/ext/basics/load.html
