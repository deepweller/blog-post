### 시작하며
chapter2에서는 메서드에 동작(코드 블록)을 전달하는 방법을 봤다. 이는 동작파라미터화 라는 용어로 설명하고 있으며, 변화하는 요구사항에 효과적으로 대응하는 방법이다. java8 이전 버전에서는 어떠한 동작을 실행하기 위해서는 간단히 메서드를 만들 수 있다. 또한 클래스로 감싸서 코드 블럭을 전달하거나 인터페이스를 만들어서 동작을 파라미터로 전달할 수 있었다. `java8` 에서는 이러한 동작파라미터화를 람다식과 메서드 레퍼런스라는 새로운 문법으로 지원한다. 기존 방식보다 훨씩 직관적이고, 코드가 간결해짐을 예제로 살펴봤다. 이번 `chapter3` 에서는 람다식과 메서드 레퍼런스가 어떻게 동작하는지까지 살펴본다. 이번 chapter도 천천히 예제를 실행해보면서 진행해보자.  
![Java8 in Action](java8inaction.jpg)  

### chapter3
**람다 표현식** : 메서드로 전달할 수 있는 익명 함수를 단순화한 것으로 다음과 같은 특징을 가진다.
* 익명 : 일반 메서드와 달리 이름이 없다.
* 함수 : 메서드처럼 특정 클래스에 종속되지 않으므로 '함수' 라고 부른다. (파라미터 리스트, 바디, 반환형식, 예외리스트는 포함한다.)
* 전달 : 람다 표현식을 메서드의 파라미터로 전달하거나 변수로 저장할 수 있다.
* 간결성 : 구현할 코드가 줄어든다.

간단한 코드를 살펴보면, 아래는 사과의 무게를 비교하는 방법이다.
```java
//java8 이전
Comparator<Apple> byWeight = new Comparator<Apple>() {
    public int compare(Apple a1, Apple a2) {
        return a1.getWeight().compareTo(a2.getWeight());
    }
}
```
```java
//java8+
Comparator<Apple> byWeight = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```
하나씩 살펴보면, 다음과 같다.
1. `(Apple a1, Apple a2)` : 람다 파라미터
1. `->` : 람다의 파라미터 리스트와 바디를 구분
1. `a1.getWeight().compareTo(a2.getWeight());` : 람다 바디

아래는 몇가지 추가 샘플이다.
```java
//String 파라미터 하나를 가지고, int를 리턴한다. (return문은 함축되어 있으므로 명시하지 않아도 된다.)
(String s) -> s.length();

//Apple 형의 파라미터를 하나 가지고, boolean을 리턴한다.
(Apple a) -> a.getWeight > 150;

//int 형식의 파라미터 2개를 가지고, 리턴값은 없다. (여려행을 포함할 수 있다.)
(int x, int y) -> {
    System.out.println("Result");
    System.out.println(x+y);
}

//파라미터가 없고, int를 반환한다.
() -> 42;

//Apple 형의 파라미터를 2개를 가지고, int를 반환한다.
(Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```
이번에는 람다 문법의 잘못된 예제를 살펴보자.
```java
(Integer i) -> return "number" + i;
//return 키워드는 흐름제어문이므로 {return "number" + i;} 가 되야 한다.

(String s) -> {"str str";}
//"str str"는 구문(statement)이 아니라 표현식(expression)이다. 따라서 "str str"; 또는 {return "str str";} 가 되야 한다.
```

### 함수형 인터페이스
함수형 인터페이스는 하나의 추상 메서드를 가지는 인터페이스이다. 예로는 앞서 살펴본 `java.util.Comparator`, `java.lang.Runnable` 이 있다. 각각 `compare()`, `run()`을 추상 메서드를 하나씩 가진다. (참고로 디폴트 메서드가 여러개 있어도 추상 메서드가 단 한개만 있으면 함수형 인터페이스이다. 9장에 나온다고 한다.)  
  
람다식으로 함수형 인터페이스의 한개 있는 추상메서드를 구현하여 전달할 수 있으므로 람다식은 함수형 인터페이스의 인스턴스가 된다. 이 말을 코드로 살펴보자.
```java
Runnable r1 = () -> System.out.println("hello 1"); //람다 식을 변수(r1)에 할당한다.

Runnable r2 = new Runnable() {
    public void run() {
        System.out.println("hello 2"); //코드를 전달하기 위해 익명클래스를 사용한다. 익명클래스의 인스턴스를 변수 r2에 할당한다.
    }
}

public static void process(Runnable r) {
    r.run();
}

process(r1);
process(r2);
process(() -> System.out.println("hello 3")); //람다식을 직접 전달한다.
```
출력 결과는  
hello 1  
hello 2  
hello 3  
가 차례로 출력된다. 위 코드의 `r1` 과 마지막 `process`를 보면 람다식 자체가 `r2`의 `new Runnable()` 과 같이 인터페이스의 인스턴스가 되어 `process(Runnable r)`의 파라미터로 전달됨을 볼 수 있다. **람다식은 함수형 인터페이스의 인스턴스이며 메서드의 파라미터로 전달된다.**

> `@FunctionalInterface` : 함수형 인터페이스를 명시하는 어노테이션이다. 추상 메서드가 하나 이상이면 컴파일에러가 난다.

### java.util.function
**Predicate** (java.util.function.Predicate<T>)  
`test`라는 추상 메서드가 정의되어 있고 `boolean`을 리턴한다. 불린 표현식이 필요한 상황에서 사용한다.
```java
public static <T> List<T> filter(List<T> list, Predicate<T> p) {
    List<T> results = new ArrayList<>();
    for(T s : list) {
        if(p.test(s)) results.add(s);
    }
    return results;
}

Predicate<String> nonEmptyStringPredicate = (String s) -> !s.isEmpty();
List<String> nonEmpty = filter(listOfStrings, nonEmptyStringPredicate);
```

**Consumer** (java.util.function.Consumer<T>)  
`accept`라는 추상메서드가 정의되어 있고 T형식의 객체를 받아 특정 동작을 수행한다.
```java
public static <T> void forEach(List<T> list, Consumer<T> c) {
    for(T i : list) {
        c.accept(i);
    }
}

forEach(
    Arrays.asList(1,2,3,4,5),
    (Integer i) -> System.out.println(i)
);
```

**Function** (java.util.function.Function<T, R>)  
T를 받아서 R을 리턴하는 `apply`라는 추상메서드가 정의되어 있다. 입력을 출력으로 매핑하는 람다를 정의할 때 사용할 수 있다.
```java
public static <T, R> List<R> map(List<T>, Function<T, R> f) {
    List<R> result = new ArrayList<>();
    for(T s : list) {
        result.add(f.apply(s));
    }
    return result;
}

List<Integer> l = map(
                        Arrays.asList("lambdas", "in", "action"),
                        (String s) -> s.length()
                        );
```

#### 박싱, 언박싱, 오토박싱  
`primitive type`을 `wrapper class`로 변환하는 기능을 박싱이라 하고, 그 반대는 언박싱이다. 오토박싱은 박싱과 언박싱이 자동으로 이루어지는 기능이다.  
하지만 class는 힙에 저장되므로 메모리를 더 사용하게되고, 탐색하는 과정도 필요해진다. java8에서는 오토박싱을 피할 수 있는 함수형 인터페이스를 제공한다. 기존 함수형 인터페이스의 이름 앞에 자료형명이 붙는다.  
   
Predicate : IntPredicate, LongPredicate...  
Counsumer : IntCounsumer...  
...
  
```java
IntPredicate evenNum = (int i) -> i % 2 == 0;
evenNum.test(1000); // true (박싱 x)

Predicate<Integer> oddNum = (Integer i) -> i % 2 == 1;
oddNum.test(1000); // false (박싱 o)
```

#### 형식추론
컴파일러는 람다표현식의 컨택스트를 이용해서 관련된 함수형 인터페이스를 추론한다. 즉 함수의 정의를 알 수 있으므로 파라미터까지 추론이 가능하기 때문에 생략이 가능해진다. 상황에 따라 가독성을 위해 생략하거나 유지하면 된다.
```java
List<Apple> apples = filter(inventory, a -> "green".equals(a.getColor())); // a 에 Apple을 명시하지 않음

Comparator<Apple> c = (a1, a2) -> a1.getWeight().compareTo(a2.getWeight()); // 마찬가지로 Apple을 명시하지 않음
```

#### 람다 캡처링
람다식에 지역변수를 사용하는 것을 람다 캡처링이라 한다. 하지만 지역변수는 재할당이 되서는 안되고 `final`로 선언이 되어있거나, final과 다름없이(선언 이후 재할당이 없는) 사용되고 있는 지역변수만 할당이 가능하다.
```java
int a = 123;
Runnable r = () -> System.out.println(a); // ok

int b = 456;
Runnable r = () -> System.out.println(b); // error
b = 789;
```

#### 메서드 레퍼런스
메서드 레퍼런스는 특정 메서드만을 호출하는 람다의 축약형이다. 메서드 레퍼런스는 메서드 앞에 `::`를 붙인다. 예를 들어 `Apple::getWeigth`는 `Apple` 클래스에 정의된 `getWeight`의 메서드 레퍼런스다.  
메서드 레퍼런스는 3가지 유형이 있다.  
1. 정적 메서드 레퍼런스
1. 다양한 형식의 인스턴스 메서드 레퍼런스
1. 기존 객체의 인스턴스 메서드 레퍼런스

```java
//1
(args) -> ClassName.staticMethod(args);
ClassName::staticMethod;

//2
(arg0, rest) -> arg0.instanceMethod(rest); //arg0 : ClassName
ClassName::instanceMethod;

//3
(args) -> expr.instanceMethod(args);
expr::instanceMethod;
```

컴파일러는 메서드 레퍼런스가 주어진 함수형 인터페이스와 호환하는지 확인한다. 메서드 레퍼런스는 컨텍스트의 형식과 일치해야 한다.

> 함수 디스크립터  
> Comparator 의 경우 (T, T) -> int 가 함수 디스크립터

```java

Function<String, Integer> stringToInteger = (String s)
 -> Integer.parseInt(s);

//위 람다식은 자신의 인수를 Integer의 정적 메서드인 parseInt로 전달한다.
//이 메서드는 String을 인수로 받아 파싱한 후 Integer를 리턴한다.
Function<String, Integer> stringToInteger = Integer::parsInt;

BiPredicate<List<String>, String> contains = (list, element) -> list.contains(element);

//이 람다식은 첫 번째 인수의 contains 메서드를 호출한다.
//첫 번째 인수가 List이므로 다음과 같이 작성한다.
BiPredicate<List<String>, String> contains = List::contains;
```

#### 생성자 레퍼런스
클래스 명과 `new`키워드로 생성자 레퍼런스를 만들 수 있다.
```java
Supplier<Apple> c1 = Apple::new; //생성자 레퍼런스
Apple a1 = c1.get(); //NoArgsConstructor
//기존코드
Supplier<Apple> c1 = () -> new Apple();
```
위 코드는 생성자에 인수가 없는 경우이다. 아래는 인수가 있는 경우이다.
```java
Function<Integer, Apple> c1 = Apple::new; //생성자 레퍼런스
Apple a1 = c1.apply(110);
//기존코드
Function<Integer, Apple> c1 = (weight) -> new Apple(weight);
```
아래 코드는 위 내용을 응용하여 리스트에 있는 숫자를 무게로 가지는 Apple객체를 생성하는 코드이다.
```java
List<Integer> weights = Arrays.asList(1,2,3,4);
List<Apple> apples = map(weights, Apple:new);

public static List<Apple> map(List<Integer> list, Function<Integer, Apple> f) {
    List<Apple> result = new ArrayList<>();
    for(Integer e : list) {
        result.add(f.apply(e));
    }
    return result;
}
```
아래 코드는 위 예제를 더 응용해서 다양한 무게 + 다양한 과일을 생성하는 코드이다.
```java
static Map<String, Function<Integer, Fruit>> map = new HashMap<>();
static {
    map.put("apple", Apple::new);
    map.put("orange", Orange::new);
    //..등
}

public static Fruit createFruit(String fruit, Integer weight) {
    return map.get(fruit.toLowerCase()) //과일에 맞는Funtion 객체를 얻는다.
                                .apply(weight);
}
```

### 마치며
이번장에서는 람다식, 메서드레퍼런스, 생성자레퍼런스의 동작에 방식에 대해 자세히 봤다. 또 앞장에서 봤던 람다식과 메서드레퍼런스 샘플들을 다시 봤다. 사용법과 동장방식을 봤지만 실제 적용을 하는데는 많은 연습이 필요할 것 같다. 함수형 프로그래밍이 익숙해질 시간이 필요할 것 같다.  
이번 장은 글을 쓰다가 중간에 너무 방치해두다가 한달 가까이 지난 후에 다시 이어서 썼다. 그러다보니 중간에 맥락이 끊기는 느낌이 들 수 있다. 어쩃든 다시 꾸준히 써야겠다.

### 참고
only 책