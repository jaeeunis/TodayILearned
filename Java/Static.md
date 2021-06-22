# Static Keyword in Java

> 정적(static)은 **고정된**이란 의미를 가지고 있습니다. static 키워드를 사용하여 변수와 메소드를 만들 수 있는데 다른말로 정적필드와 정적 메소드라고 하며 정적 멤버라고 부른다. 정적 필드와 정적 메소드는 객체에 소속된 멤버가 아니라 클래스에 고정된 멤버이다. 

### 1. Static 변수

#### 1-1 객체를 생성하지 않고 static 자원에 접근이 가능하다.

자바에서 static으로 선언하면, 정확히 한개의 필드를 생성하고 모든 곳에서 공유한다. 얼만큼 많이 선언하느냐에 상관없이 한개의 static 필드에 속한다. **이 정적 필드의 값은 다른 동일한 클래스에서 모두 공유합니다.**  static변수는 JVM 의 Metaspace라고 불리는 특정 메모리에 이동됩니다.

```Java
public class User {

    public static int like = 1;
    public int likes;
    public String userName;
    
    public User(String userName){
        this.userName = userName;
        this.likes = like;
        like ++;
    }

    public static int getLike(){
        return like;
    }

    public void getResult() {
        System.out.println("회원 : " + userName);
        System.out.println("좋아요 수 :" + likes);
    }
  
      public static void main(String[] args) {
        User user1 = new User("Java");
        User user2 = new User("OOP");

        user1.getResult();
        user2.getResult();
    }
}
```

![image-20210622220407315](/Users/jaeeunlim/Library/Application Support/typora-user-images/image-20210622220407315.png)

<br>

### 2. Static 메소드

#### 2-1 클래스 객체를 생성하지 않고 사용할 수 있다.

static 키워드가 붙으면 객체가 항상 메모리를 할당해야 하는 멤버로 보고 method area에 메뫼를 할당한다. 객체에 소속된 변수가 아니라 클래스에 소속된 변수이기 때문에 클래스 혹은 클래스 메서드라고 합니다. 

#### 2-2 Static 메서드 안에서 인스턴스 변수를 사용할 수 없다.

![image-20210622220611340](/Users/jaeeunlim/Library/Application Support/typora-user-images/image-20210622220611340.png)

#### 2-3 메인 메서드는 Static 메서드!?

Java의 메인 메서드는 항상 static 메서드이다. 그래서 컴파일러는 객체를 생성하지 않고 클래스들이 생성되기 전에 수행할 수 있다. **어떠한 자바 프로그램에서도, 메인 메서드는 컴파일러가 시작하는 시작점이다.**

<br>

### 3. Static is a evil !? 

 #### 3-1 Garbage Collector 대상이 아니기 때문에 메모리 낭비가 발생할 수 있다.

프로그램이 종료되기까지 메모리에 상주하고 있어 자주 사용하지 않는 메서드가 누적된다면 GC에 수거되지 못하므로 오히려 메모리 낭비가 발생한다. 단순히 빠르다는 이유로 static 메서드 및 변수 사용은 지양해야 한다.

#### 3-2 객체지향 프로그래밍과 멀어진다.

정적 메소드는 다른 객체들과 서로 협력하지 않으며 단순히 함수 호출에 불과하다. 또한 다양하게 메소드를 재정의 할 수 없기 때문에 다형성에 어긋난다.

#### 3-3 Not Thread Safe and sensitive Hadling 

전역으로 관리하기 때문에 관리하거나 테스트가 어렵다. 스레드에서 값을 변경 하면 모든 다른 스레드에서 영향을 받기 때문에 추가적인 작업이 필요하다. 만약 공유해야 하는 필요성이 생기면 **싱글톤 객체** 를 사용하면 이점이 더 많다.

https://stackoverflow.com/questions/7026507/why-are-static-variables-considered-evil <br>
https://woowacourse.github.io/javable/post/2020-07-16-static-method/

<hr>

#### 참고

https://www.baeldung.com/java-static <br>
https://www.thoughtco.com/static-fields-2034338