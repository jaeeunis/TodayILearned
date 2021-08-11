# SOLID 원칙

### 1. 단일 책임 원칙 Single Responsibility 

> 객체는 **단 하나의 책임**만 가져야 한다는 원칙을 말한다. 여기에서 책임이란 해야하는 것 혹은 할 수 있는 기능으로 생각하면 된다. 이는 객체지향 프로그래밍에서 유지보수를 할 때 매우 중요하다. 만약 어떤 클래스를 변경할 때, 다른 클래스에 영향을 미친다면 서로 영향을 미치는 관계들을 고려해서 고쳐야 하므로 시간이 매우 오래 걸릴 것이다. 

<br>

### 2. 개방-폐쇄 원칙 Open-Closed Principle

> 기존의 코드를 변경하지 않으면서 (closed), 기능을 추가할 수 있도록 (open) 설계 되어야 한다는 원칙이다. "코드를 변경하지 않고 어떻게 기능을 추가한다는 것일까?" 하는 의문점을 처음 프로그래밍 공부할 때 가졌었던 것 같다. 
> **기능을 변경 또는 확장을 할 수 있지만, 그 기능을 사용하는 코드는 수정하지 않아야 한다는 것이다.**

<br>

어떤 파일을 읽어와 화면에 나타내는 기능이 있다고 가정하자. ( 이하 모든 예제코드는 <a href="http://wonwoo.ml/index.php/post/1726">머루의 개발블로그</a> 의 예제를 참고하였습니다.)
(이 예제를 통해서 인터페이스를 활용하는 것이 매우 중요하다는 것을 느꼈다. 시니어 또는 년차가 쌓인 개발자가 된다면 설계를 검토하는 경우가 종종 발생할 수 있다고 생각하는데, 이렇게 조금씩 이해했던 내용들을 적용할 수 있는 날이 왔으면 좋겠다.)

```java
public class ExelController {
	private final FileByteReader fileByteReader = new FileByteReader();
  
  public void write () {
    byte[] read = fileByteReader.read();
  }
}

public class FileByteReader {
  public bye[] read() {
    return new byte[0];
  }
}

// 만약 파일이 아닌 데이터베이스에서 읽어오는 기능으로 바뀐다면 fileByteReader 의 기능을 데이터베이스에서 읽어오는 기능으로 수정이 필요하므로 개방-폐쇄 원칙에 어긋난다.

public class ExelController {
	private final DataBaseReader fileByteReader = new DataBaseReader(); //수정에 열려있게 된다.
  
  public void write () {
    byte[] read = fileByteReader.read();
  }
}

public class DataBaseReader {
  public bye[] read() {
    return new byte[0];
  }
}

// 기능이 새로 변경되거나 추가될 때 기존의 코드에 변경을 하지 않기 위해서 ByteReader 인터페이스를 선언한다.
public interface ByteReader {
  byte[] read();
}

public class FileByteReader implements ByteReader {
  @Override
  public bye[] read() {
    return new byte[0];
  }
}

public class DataBaseReader implements ByteReader {
  @Override
  public bye[] read() {
    return new byte[0];
  }
}

public class ExelController {
  
	private final ByteReader byteReader;
  
  public ExelController (ByteReader byteReader) { // 클라이언트에서 사용하고 싶은 기능을 선택할 수 있다!
    this.byteReader = byteReader;
  }
  
  public void write () {
    byte[] read = fileByteReader.read();
  }
}
```



### 3. 리스코프 치환 원칙 Liskov Substitution principle 

> 상위 타입의 객체를 하위 타입의 객체로 치환해도 상위 타입을 사용하는 프로그램은 정상적으로 동작해야 한다. 
> 서브 타입은 언제나 기반 타입과 호환될 수 있어야 한다. 서브 타입은 기반 타입이 약속한 규약을 지켜야 한다는 것이다. 서브 클래스가 확장에 대한 인터펭스를 준수해야 함을 의미한다. 리스코프 원칙은 오픈-폐쇠 원칙을 제공하는 상속구조를 제공 합니다. 인터페이스 규약을 지키므로, 확장하는 부분에 다형성을 제공해 변화에 열려있는 프로그램을 만들 수 있도록 한다.

(계속 내용들을 보면 SOLID 원칙은 연관성이 있다는 것을 볼 수 있다. 객체지향으로 설계하기 위해서는 한가지만 고려하는 것이 아니라 생각을 넓혀서 생각해야한다는 것이다.)

<br>

### 4. 인터페이스 분리 원칙 Interface Segregation Principle 

> 인터페이스 분리 원칙은 클라이언트가 자신이 이용하지 않는 메서드에 의존하지 않아야 한다는 원칙이다. 인터페이스 분리 원칙은 큰 덩어리의 인터페이스들을 구체적이고 작은 단위들로 분리시킴으로써 클라이언트들이 꼭 필요한 메서드들만 이용할 수 있게 한다는 것이다. 

<br>

(이전에 면접을 위해서 SOLID 원칙을 공부했던 기억이 있었는데, 역시 이론적으로 공부하다 보니 코드를 어떻게 구현해야 하는 지 몰랐다! 이번에 머루의 개발자 블로그 코드들을 보고 이해했는데, 앞으로는 서적을 통해서 공부를 하는 습관을 길러봐야 겠다!)

```java
public interface Macpro {
  void display();
  void keybord();
  void touch();
}

public class MacPro2016 implements MacPro {
  @Overrid
  public void display() {
    System.out.println("MacPro2016 display");
  }
  
    @Overrid
  public void keybord() {
    System.out.println("MacPro2016 keybord");
  }
  
    @Overrid
  public void touch() {
    System.out.println("MacPro2016 touch");
  }
}

// 하지만 MacPro2015에는 터치바가 없는데, 클라이언트가 사용하지 않는 메서드에 의존하지 말아야 한다는 것이다. 인터페이스 분리 원칙에 따라 수정 해보자!

public interface Macpro {
  void display();
  void keybord();
}

pulbic interface MacProTouch {
  void touch();
}

//2015년 모델에는 터치가 들어가 있지 않기 때문에 2016년 맥북 클래스에 MacProTouch 인터페이스를 구현하면된다.
public class MacPro2015 implements MacPro {  
  @Overrid
  public void display() {
    System.out.println("MacPro2016 display");
  }
  
    @Overrid
  public void keybord() {
    System.out.println("MacPro2016 keybord");
  }
}
```



### 5. 의존 역전 원칙 Dependency Inversion Principle

> 의존관계를 맺을 때 변화하기 쉬운 것 또는 자주 변화하는 것보다는 변화하기 어려운 것, 거의 변화가 없는 것에 의존하라는 원칙이다. 객체지향 관점에서는 이와 같이 변하기 어려운 추상적인 것들을 표현하는 수단으로 추상클래스와 인터페이스가 있다. DIP를 만족하려면 어떤 클래스가 도움을 받을 때, 구체적인 클래스보다는 인터페이스나 추상클래스와 의존관계를 맺도록 설계해야 한다. DIP를 만족하는 설계는 **유연한 시스템**이 될 수 있다.

#### 참고

http://wonwoo.ml/index.php/post/1726
https://www.nextree.co.kr/p6960/