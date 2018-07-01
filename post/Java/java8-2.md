### 시작하며
chapter1 에서 java8의 큰 특징들을 알아봤다. 계속 '뒤 chpater에서 살펴본다.'라는 말이 있어서 완벽히 이해는 안되지만 일단 쭉 읽어봤다. chapter2에서는 '동작 파라미터화 코드 전달하기' 라는 주제인데 주제부터 무슨소리인지 이해가 잘 안간다. 앞서 본 내용으로 봐서는 함수(일급객체)를 메서드의 인수로 전달하는 방법을 보는 것 같다. 차근차근 읽어보고 샘플을 작성해 보자.  
![Java8 in Action](java8inaction.jpg)

### chapter2
**동작파라미터화**(behavior parameterization) : 아직은 어떻게 실행할 것인지 결정하지 않은 코드 블록으로 나중에 프로그램에서 이 코드 블록을 호출하여 실행한다. 즉 메서드의 동작이 파라미터화 됨을 의미한다.
* 리스트의 모든 요소에 특정 동작을 수행
* 리스트 관련 작업을 끝낸 다음에 특정 동작을 수행
* 에러가 발생하면 특정 동작을 수행

사과농장에서 농장주가 수확한 사과를 분류해달라고 한다. 녹색사과만 분류해달라고 하다가, 빨간사과도 분류해달라고 하고, 나중에는 무게, 크기 등으로 다양한 방법으로 분류를 해달라고 할 수 있다. 이렇게 변하는 요건을 어떻게 하면 적절하게 필터링 할 수 있을까? 기존의 방법으로는 크게 두 가지 분류가 있다. 두 가지 방법을 샘플코드로 살펴보자.    
  
**1. 메서드 생성**  
가장 쉽게 생각할 수 있는 방법이다. 요건이 생길 때마다 계속 메서드는 추가되고, 필터메서드를 모아두는 클래스는 커진다. 이 방법은 동작 파라미터화가 아닌 값 파라미터화이다.
```java
public List<Apple> filterGreenApples(List<Apple> inventory){
	List<Apple> result = new ArrayList<>();
	for(Apple apple: inventory){
		if("green".equals(apple.getColor())){
			result.add(apple);
		}
	}
	return result;
}
public List<Apple> filterApplesByColor(List<Apple> inventory, String color){
	List<Apple> result = new ArrayList<>();
	for(Apple apple: inventory){
		if(apple.getColor().equals(color)){
			result.add(apple);
		}
	}
	return result;
}
public List<Apple> filterApplesByWeight(List<Apple> inventory, int weight){
	List<Apple> result = new ArrayList<>();
	for(Apple apple: inventory){
		if(apple.getWeight() > weight){
			result.add(apple);
		}
	}
	return result;
}
```
**test code**
```java
@Test
public void 사과_필터링() {
    AppleFilter appleFilter = new AppleFilter();

    List<Apple> inventory = Arrays.asList(
            new Apple(80,"green"),
            new Apple(155, "green"),
            new Apple(120, "red")
    );

    // [Apple{color='green', weight=80}, Apple{color='green', weight=155}]
    List<Apple> greenApples = appleFilter.filterApplesByColor(inventory, "green");
    Assert.assertThat(greenApples.get(0).getColor(), is("green"));

    // [Apple{color='red', weight=120}]
    List<Apple> redApples = appleFilter.filterApplesByColor(inventory, "red");
    Assert.assertThat(redApples.get(0).getColor(), is("red"));
}
```
**2. 인터페이스 사용(전략패턴)**  
인터페이스를 사용하는 방법이다. test라는 boolean을 리턴하는 메서드를 정의해 놓고 구현체를 늘려간다.(구현체가 계속 늘어가는 것이 걱정이면 자주사용되지 않을 필터링은 익명클래스를 사용한다.) 그러면 실제로 필터링을 하는 소스에서는 구현체만 전달하면 그 뒤의 일은 신경쓰지 않아도 된다.(test라는 메서드가 무슨 필터링을 하는지 중요하지 않고, boolean형을 리턴한다 라는 것만 알면 된다.)
```java
public interface ApplePredicate {
    boolean test(Apple a);
}
```
```java
public class AppleColorPredicate implements ApplePredicate {
    public boolean test(Apple apple){
        return "green".equals(apple.getColor());
    }
}
```
```java
public class AppleWeightPredicate implements ApplePredicate {
    public boolean test(Apple apple){
        return apple.getWeight() > 150;
    }
}
```
```java
public List<Apple> filter(List<Apple> inventory, ApplePredicate p){
	List<Apple> result = new ArrayList<>();
	for(Apple apple : inventory){
		if(p.test(apple)){
			result.add(apple);
		}
	}
	return result;
}
```
**test code**
```java
@Test
public void 사과_필터링() {
    AppleFilter appleFilter = new AppleFilter();

    List<Apple> inventory = Arrays.asList(
            new Apple(80,"green"),
            new Apple(155, "green"),
            new Apple(120, "red")
    );

    /*
     * 클래스로 감싸긴 하지만 boolean 을 리턴하는 메서드(동작)를 전달한다.
     * filter메서드의 인자로 우리가 전달한 ApplePredicate 객체에 따라 필터링의 동작이 결정된다.
     */

    // [Apple{color='green', weight=155}]
    List<Apple> heavyApples = appleFilter.filter(inventory, new AppleWeightPredicate());
    Assert.assertTrue(heavyApples.get(0).getWeight() > 150);

    // [Apple{color='red', weight=120}]
    List<Apple> redApples2 = appleFilter.filter(inventory, /*익명클래스*/new ApplePredicate() {
        public boolean test(Apple a){
            return a.getColor().equals("red");
        }
    });
    Assert.assertThat(redApples2.get(0).getColor(), is("red"));

}
```
위에서 살펴본 방법이 동작 파라미터화다. 하나의 인자로 여러가지 동작을 수행할 수 있게 된다.
* 하나의 인자 : List<Apple> filter(List<Apple> inventory, ApplePredicate p) 메서드에서 ApplePredicate  
* 여러가지 동작 : ApplePredicate의 구현체들의 test() 메서드

### 새로운 방법
기존의 방법이 아닌 드디어 java8에서 새로 나온 람다 표현식을 사용한다. (ApplePredicate 인터페이스의 test 메서드의 동작을 아래와 같이 표현하는 정도만 안다. 인터페이스에 정의 된 동작(함수)이 한개이기 때문에 사용이 가능하다. 아직 람다식이 정확히 동작하는 방법은 모른다.. )
```java
//ApplePredicate의 test()의 구현부분을 아래의 람다식으로 표현
List<Apple> result = appleFilter.filter(inventory, (Apple apple) -> "red".equals(apple.getColor()));
Assert.assertThat(result.get(0).getColor(), is("red"));
```
아래는 Apple 뿐만아니라 오렌지, 기타 자료형 등등까지 자유롭게 사용할 수 있는 아주 유연한 방법이다.
```java
public interface Predicate<T> {
    boolean test(T t);
}
```
```java
/* 
 * 선언부 제일 앞의 <T>는 그 뒤의 T, <T>가 제네릭 타입임을 명시해주는 역할이다.
 * <T>가 없으면 T를 못찾는다는 에러가 난다. (제네릭 참고)
 */
public <T> List<T> filter(List<T> list, Predicate<T> p) {
    List<T> result = new ArrayList<>();
    for(T e: list) {
        if(p.test(e)) {
            result.add(e);
        }
    }
    return result;
}
```
```java
List<Apple> redApples = filter(inventory, (Apple apple) -> "red".equals(apple.getColor()));

List<Integer> evenNumbers = filter(inventory, (Integer i) -> i % 2 == 0);
```

### 동작파라미터화(=코드전달) 샘플
**1. Comparator**  
  
Collections.sort를 대체할 수 있는 java8 특징으로 List의 sort 메서드이다.
```java
//java.util.Comparator
public interface Comparator<T> {
    public int compare(T o1, T o2);
}
```
**test code**
```java
List<Apple> inventory = Arrays.asList(
        new Apple(80,"green"),
        new Apple(155, "green"),
        new Apple(120, "red"),
        new Apple(70, "red")
);

inventory.sort(new Comparator<Apple>() {
    @Override
    public int compare(Apple o1, Apple o2) {
        return o1.getWeight().compareTo(o2.getWeight());
    }
});
Assert.assertThat(inventory.get(0).getWeight(), is(70));

//바로 위 sort에 대한 quick fix
inventory.sort((o1, o2) -> o1.getWeight().compareTo(o2.getWeight()));
Assert.assertThat(inventory.get(0).getWeight(), is(70));

//바로 위 sort에 대한 quick fix
inventory.sort(Comparator.comparing(Apple::getWeight));
Assert.assertThat(inventory.get(0).getWeight(), is(70));
```
3개의 sort가 다 같은 코드이다.(intellij quick fix에 나오길래 다 해봤다. 문법은 뒤 chapter에서..)

**2. Runnerable**  
  
스레드로 동작(나중에 실행될 코드 블록)을 전달하고 싶은 경우이다. Comparator와 마찬가지로 java.lang.Runnable 인터페이스를 이용한다.
```java
//기존
Thread t = new Thread(new Runnable() {
    public void run() {
        System.out.println("java8 in action");
    }
});

//java8
Thread t = new Thread(() -> System.out.println("java8 in action"));
```

**3. Event 처리**  
  
버튼클릭 등의 이벤트가 발생했을 때의 이벤트 처리이다.(옛날에 이클립스 RCP랑 안드로이드 프로그래밍할 때 많이 해본거 같다.) 마찬가지로 이벤트가 발생했을 때의 전달할 동작을 다음과 같이 작성한다.
```java
//기존
Button button = new Button("Send");
button.setOnAction(new EventHandler<ActionEvent>() {
    public void handle(ActionEvent event) {
        label.setText("Send");
    }
});

//java8
button.setOnAction((ActionEvent event) -> label.setText("Send"));
```

### 마치며
chapter2는 기존 java8 이전에서의 동작파라미터를 어떻게 구현했는지를 알아봤다. 기존에는 함수를 인자로 넘기려면 이렇게나 많은 코드와 귀찮은 작업이나 개념을 많이 알아야한다고 알려준다. 일단 람다를 활용한 마지막 코드만 봐도 코드가 많이 간결해 보인다. 이제 다음 chapter부터 람다, 메서드 레퍼런스, 스트림, 병렬 등등 java8의 특성을 본격적으로 알아보는 것 같다. 이번장 까지는 워밍업이다.  
  
요약 : 이전 버전은 구려, 이거봐바. 지저분하잖아. 그치? java8은 이렇게나 간단해! 봤지? 써.   

### 참고
(뜬금없는 제네릭 검색)  
https://stackoverflow.com/questions/11303694/what-is-static-t-listt-methodname-list-super-t-input  
https://stackoverflow.com/questions/15888551/how-to-interpret-public-t-t-readobjectdata-classt-type-in-java  