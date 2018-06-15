### 시작하며
객체지향의 4가지 특징은 **캡슐화**, **상속(확장)**, **추상화(모델링)**, **다형성** 이다. (이 내용은 어떤 자바개발서적에도 아주 자세히 설명되어 있고, 검색해도 많은 정보가 나오니 참고하자.)  
객체지향의 4가지 특징을 알았으니, 이제 이 특징들을 어떻게 코드에 녹여낼지 알아야한다. SOLID는 위 특징을 잘 사용하여 프로그램을 설계하는 방법을 제시한 5가지 원칙이다. 5가지의 원칙은 다음과 같다.  
* SRP(Single Responsibility Principle) : 단일 책임 원칙
* OCP(Open Closed Principle) : 개방 폐쇄 원칙
* LSP(Liskov Substitution Principle) : 리스코프 치환 원칙
* ISP(Interface Segregation Principle) : 인터페이스 분리 원칙
* DIP(Dependency Inversion Principle) : 의존 역전 원칙

>톱 대신 과도로도 나무를 자를 수 있지만, 많은 시간이 걸릴 것이다. 이처럼 적절한 도구가 있어도 용도에 맞게 사용하는 방법을 알아야한다.

### SRP
SRP(단일책임원칙)은 작게는 속성(클래스변수)와 메서드, 크게는 클래스는 하나의 기능을 담당(책임)해야 한다는 개념이다.   
* 속성이 여러 의미를 가져서는 안된다.
```java
class Person {
    String militaryNumber; //군번

    public static void main(String args[]) {
        Person aMan = new Person();
        Person aWoman = new Person();

        aWoman.militaryNumber = "none"; //??
    }
}
```
위 처럼 사람이라는 클래스에 군번이라는 속성이 있는경우, 여자객체를 생성하면 군번이라는 속성은 필요없는 속성이 된다. 이는 하나의 속성이 하나의 의미를 갖지 못하므로 SRP를 지키지 못하는 경우다. 이때는 남자와 여자의 클래스를 분리하고 사람이라는 상위클래스를 두어 공통으로 사용할 수 있는 속성과 그렇지 않은 속성을 각각 상위클래스와 하위클래스에 분리하여 작성해야 한다.

* 하나의 메서드에 다른여러 기능을 분기문으로 처리하면 안된다.
```java
class Person {
    String sex; //성별

    void goToilet(String sex) {
        if(sex.equals("male")) {
            //남자화장실로 간다.
        } else if(sex.equals("female")) {
            //여자화장실로 간다.
        }
    }
}
```
위 코드는 성별이라는 속성으로 화장실에 간다는 메서드 내부에서 분기를 하고 있다. 이도 마찬가지로 화장실에 간다라는 메서드가 두 개의 기능을 가지고 있기 때문에 SRP를 지키지 않고 있다. 위 코드는 아래처럼 변경될 수 있다.
```java
abstract class Person {
    abstract void goToilet();
}

class Male extends Person {
    void goToilet() {
        //남자화장실에 간다.
    }
}

class Female extends Person {
    void goToilet() {
        //여자화장실에 간다.
    }
}
```
SRP는 추상화(모델링)과정(또는 리펙토링)을 얼마나 잘 거쳤느냐에 따라 잘 되었는지 결정된다.

### OCP
OCP(개방폐쇄원칙)은 '자신의 확장에는 열려있고, 주변의 변경에 대해서는 닫혀있어야한다.'는 개념이다. 이에 대한 간단한 예로는 JDBC가 있다. JDBC 사용자는 데이터베이스의 종류가 무엇이든 Connection을 연결할 때 DB에 맞게 driver만 변경해주면 된다. [(드라이버 로딩 참고)](https://www.slipp.net/questions/276) 따라서 이 부분을 외부 설정파일로 분리하면 코드는 한줄도 수정하지 않아도 된다.   
 ```java
 //DB에 맞는 jdbc driver를 로드하면 된다.
 Class.forName("com.mysql.jdbc.Driver");
 ```
정리하면 개발자가 작성한 코드는 DB 변경에 대하여 닫혀있다. (DB가 변경되도 코드에는 영향을 주지 않는다.) 그리고 JDBC는 각 DB별 확장에 열려있다. (다양한 DB에 접근할 수 있다.)  
OCP도 마찬가지로 추상화 과정(또는 리펙토링)에서 (추상)클래스와 인터페이스로 확장될 것과 변하지 않을 것을 정확히 구분하느냐에 따라 잘 지켜졌는지 결졍된다.

### LSP
LSP(리스코프치환원칙)은 다음 조건을 만족해야 한다.
* 하위클래스 is a (kind of) 상위클래스 : 하위클래스는 상위클래스이다. (또는 종류이다.)
* 구현클래스 is able to 인터페이스 : 구현클래스는 인터페이스할 수 있다.

상속은 계층관계가 아닌 분류관계가 되어야 한다. 아빠라는 상위클래를 상속받는 아들이라는 클래스가 있다고 해보자.
```java
//아들이 태어나서 john이라는 이름을 붙였는데, 아빠라는 역할을 부여한다. (계층관계)
Father john = new Sun();

//강아지가 한마리 태어나서 choco라는 이름을 붙이고, 동물이라는 역할을 부여한다. (분류관계)
Animal choco = new Dog();
```
위 내용처럼 하위클래스는 상위클래스의 역할을 하는데 전혀 문제가 없어야 한다. 아들이 아빠의 역할을 한다는 것은 이상하지만, 강아지가 동물의 역할을 한다는 말은 이상하지 않다. 이처럼 클래스와 객체는 현실의 내용을 그대로 반영할 수 있어야 한다.

### ISP
ISP(인터페이스분리원칙)은 SRP와 같은 문제에 대한 다른 해결책이다. SRP는 클래스를 분리함으로써 하나의 클래스는 하나의 기능(책임)을 해결하는 개념이라면, ISP는 인터페이스로 분리하여 이를 해결하는 개념이다. 예를들어, 박쥐의 경우에는 동물이면서 날 수도 있고, 거꾸로 매달려서 잘 수도 있다. 이러한 특징을 동물이라는 상위클래스에 넣는다면 날지 못하는 동물은 상속받을 수 없다. 그러면 박쥐라는 클래스에 메서드로 넣자니 날 수 있는 동물은 많다. 이러한 경우에 인터페이스로 분리하여 동물이지만 날수있는, 헤엄칠수있는 등 그때그때 역할을 제한하는 개념이다.
```java
public class 날치 extends Animal implements Flyable, Swimmable {

}
```

### DIP
DIP(의존역전원칙) '구체적인 것이 추상화 된 것에 의존해야한다. 자주 변경되는 클래스에 의존하면 안된다.' 라는 개념이다. 이 의미를 간단히 코드로 보자.
```java
public class Car{
    
    void changeSeason(String season) {
        if(season.equals("winter")) {
            SnowTire newSnowTire = buySnowTire();
            changeSnowTire(newSnowTire);
        } else if(season.equals("summer")) {
            NormalTire newNormalTire = buyNormalTire();
            changeNormalTire(newNormalTire);
        }
    }

    SnowTire buySnowTire() {
        return new SnowTire();
    }

    NormalTire buyNormalTire() {
        return new NormalTire();
    }
}
```
위의 코드는 계절에 따라 타이어를 교체하는 내용이다. 겨울철에 눈이오면 차량의 타이어를 스노우타이어로 교체하기 위해 타이어를 구매하고 교체한다. 또 계절이 바뀌면 일반타이어로 교체한다. 위의 내용이 즉, 자주 변경되는 클래스(타이어)에 구체적인 것(차)이 의존하는 경우이다. 위 코드를 변경해보면
```java
public class Car{

    void changeSeason(String season) {
        Tire newTire = buyTire(season);
        changeTire(newTire);
    }

    Tire buyTire(String season) {
        if(season.equals("winter")) {
            return new SnowTire();
        } else if(season.equals("summer")) {
            return new NormalTire();
        }
    }
}
```
위의 내용은 계절이 바뀌어도 타이어를 구매하고 그 새로운 타이어로 교체한다는 것이 명확하다. 즉 어떠한 타이어로 변경되어도 자동차는 영향을 받지 않는다.  
이전에는 자동차가 스노우타이어에 의존했다면 이제는 스노우타이어와 일반타이어가 타이어라는 인터페이스에 의존한다. 즉, 의존의 방향이 역전됐다.  
상위클래스일수록, 인터페이스일수록 변하지 않을 가능성이 높다. 그래서 '상위클래스, 인터페이스를 통하여 의존해야한다'라는 개념이 DIP이다.

### 마치며
객체지향의 특징이나 SOILD는 쉽게 들어보고 워낙 자바를 배우는 초기부터 배우는 부분이기 때문에 안다고 생각할 수 있다. 하지만 직접 설명을 하거나 관련 코드를 작성해보려고 하면 막막하다. 잠깐이나마 찾아보고 정리해둬서 다시 막막해질 때 쯤 다시 봐야겠다.  
   
>**정리**  
>SRP : 클래스를 변경해야하는 이유는 하나뿐이어야 한다.  
>OCP : 자신의 확장에는 열려 있고, 주변의 변화에는 닫혀있어야 한다(코드 수정이 없어야 한다).  
>LSP : 서브 타입은 항상 슈퍼 타입으로 교체될 수 있어야 한다.  
>ISP :     
>DIP : 변하기 쉬운 것에 의존하면 안된다.  

### 참고
도서 [스프링 입문을 위한 자바 객체 지향의 원리와 이해](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788998139940&orderClick=LAG&Kc=)  
https://www.slipp.net/questions/276   
http://www.nextree.co.kr/p6960/  
http://wonwoo.ml/index.php/post/1717