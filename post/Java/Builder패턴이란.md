### 시작하며
'자바 프로그래밍 면접, 이렇게 준비한다' 라는 책을 읽기 시작했다. 읽어야 겠다고 생각하고 산지는 좀 됐지만 아직 읽기 시작하진 않았다. 그런데 갑작스럽게 내일 면접이 잡혀서 급하게 책을 펼쳤다. 크게 주제들만 읽어가던 중 눈에 들어오는 내용이 있었다. Builder 패턴부분인데, 이전에 스프링부트 튜토리얼을 진행하면서 lombok 어노테이션(@builder)을 사용했었다. 튜토리얼을 진행할 때는 '빌더코드를 줄여주고, 빌더를 사용하면 객체를 좀 더 안전하고, 보기 좋게 생성할 수 있다.' 정도만 인지하고 썼었다. 마침 빌더패턴에 대해 자세한 내용이 나왔고, 샘플도 좋은 것 같아 직접 작성해 보고 정리해야겠다고 생각했다.  
간단하게 요약하면 빌더패턴을 사용했을 때의 장점은 다음과 같다. 
> 멤버변수 별로 메서드가 있으므로 인스턴스생성이 명확해진다.  
> 필수적인 멤버변수와 선택전인 멤버변수를 효과적으로 구분할 수 있다.

![자바 프로그래밍 면접, 이렇게 준비한다](/Users/jinyeong/Downloads/javabookcover.jpg)

### getter, setter 를 사용한 객체
```java
public class Pet {

    private Animal animal;
    private String petName;
    private String ownerName;
    private String address;
    private String telephone;
    private LocalDate dateOfBirth; //optional
    private String emailAddress; //optional

    public Animal getAnimal() {
        return animal;
    }

    public void setAnimal(Animal animal) {
        this.animal = animal;
    }

    public String getPetName() {
        return petName;
    }

    public void setPetName(String petName) {
        this.petName = petName;
    }

    public String getOwnerName() {
        return ownerName;
    }

    public void setOwnerName(String ownerName) {
        this.ownerName = ownerName;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public String getTelephone() {
        return telephone;
    }

    public void setTelephone(String telephone) {
        this.telephone = telephone;
    }

    public LocalDate getDateOfBirth() {
        return dateOfBirth;
    }

    public void setDateOfBirth(LocalDate dateOfBirth) {
        this.dateOfBirth = dateOfBirth;
    }

    public String getEmailAddress() {
        return emailAddress;
    }

    public void setEmailAddress(String emailAddress) {
        this.emailAddress = emailAddress;
    }
}
```
위 코드는 일반적으로 많이 사용하는 객체정의다. getter, setter를 사용했고, 필수 변수와 선택적인 변수는 로직으로 구분하여 인스턴스를 생성해야 한다. 이렇게 작성하다 보니, 런타임 시에 아래와 같은 오류를 자주 보곤 했다.
* 해당 데이터베이스에서 컬럼은 null을 허용하지 않는다. 
* nullPointerExeption (변수에 set을 하지않고 get해서..) 

빌더 패턴을 사용하면 위와같은 오류를 상당부분 줄일 수 있을거라 생각한다.

### Builer 패턴을 사용한 객체
빌더 패턴을 사용하여 위에서 작성한 Pet 객체를 수정해보자.
```java
public class Pet2 {

    private Animal animal;
    private String petName;
    private String ownerName;
    private String address;
    private String telephone;
    private LocalDate dateOfBirth; //optional
    private String emailAddress; //optional

    public Pet2(Animal animal, String petName, String ownerName, String address, String telephone, LocalDate dateOfBirth, String emailAddress) {
        this.animal = animal;
        this.petName = petName;
        this.ownerName = ownerName;
        this.address = address;
        this.telephone = telephone;
        this.dateOfBirth = dateOfBirth;
        this.emailAddress = emailAddress;
    }

    static class Builder {
        private Animal animal;
        private String petName;
        private String ownerName;
        private String address;
        private String telephone;
        private LocalDate dateOfBirth; //optional
        private String emailAddress; //optional

        public Builder withAnimal(Animal animal) {
            this.animal = animal;
            return this;
        }

        public Builder withPetName(String petName) {
            this.petName = petName;
            return this;
        }

        public Builder withOwnerName(String ownerName) {
            this.ownerName = ownerName;
            return this;
        }

        public Builder withAddress(String address) {
            this.address = address;
            return this;
        }

        public Builder withTelephone(String telephone) {
            this.telephone = telephone;
            return this;
        }

        public Builder withDateOfBirth(LocalDate dateOfBirth) {
            this.dateOfBirth = dateOfBirth;
            return this;
        }

        public Builder withEmailAddress(String emailAddress) {
            this.emailAddress = emailAddress;
            return this;
        }

        public Pet2 build() {
            //필수 멤버변수에 값이 할당되지 않으면 예외처리
            if(animal == null || petName == null || ownerName == null
                    || address == null || telephone == null) {
                throw new IllegalStateException("Cannot create Pet2");
            }

            return new Pet2(animal, petName, ownerName, address, telephone, dateOfBirth, emailAddress);
        }
    }
}
```
내용을 살펴보면, 클래스 내부에 Builer 클래스를 생성하고 각각 멤버변수별 메서드를 작성한다. 이는 변수에 값을 set하고 Builer 객체를 리턴한다.(이러한점 때문에 메서드를 연결하여 호출할 수 있다.) 마지막으로 build() 메소드는 필수 멤버변수의 null체크를 하고, 지금까지 set된 builder를 바탕으로 Pet의 생성자를 호출하고 인스턴스를 리턴한다.

### lombok 어노테이션을 사용한 Builder 구현
lombok을 사용하면 @builder 어노테이션 하나로 많은 코드를 줄일 수 있으니, 사용법만 간단하게 작성한다. (아직 나도 정확하게 알고 사용하고 있지 않아서..)    
먼저 build.gradle 의 dependencies 에 lombok을 추가한다. (IDE별로 lombok을 설치해야 한다고 한다. 이 내용은 [여기](http://jojoldu.tistory.com/251)를 참고한다.)
```gradle
dependencies {
    ...
    compileOnly('org.projectlombok:lombok')
    ...
}
```
그리고 코드에는 다음과 같이 생성자 위에 어노테이션을 넣어준다. (기본값을 지정하기 위해서는 @Builder.Default 를 멤버변수 위에 선언한다. [참고](https://projectlombok.org/features/Builder)) 
```java
@Builder
public class Pet3 {

    private Animal animal;
    private String petName;
    private String ownerName;
    private String address;
    private String telephone;
    private LocalDate dateOfBirth; //optional
    private String emailAddress; //optional
}
```
압도적으로 짧아진 코드를 볼 수 있다... lombok을 좀 더 공부해서 잘만 쓰면 개발속도가 엄청나게 향상할 것 같다.

### 인스턴스 생성
각각 구현한 Pet 클래스의 인스턴스 생성은 다음과 같이 한다.
```java
public static void main(String[] args) {
    //아무것도 사용하지 않고 getter, setter를 이용한 인스턴스 생성
    Pet pet = new Pet();
    pet.setAnimal(Animal.Cat);
    pet.setPetName("멍멍이");
    pet.setOwnerName("Jinyeong");
    pet.setAddress("Seoul");
    pet.setTelephone("01012341234");

    //builder를 구현한 객체의 인스턴스 생성
    Pet2.Builder builder = new Pet2.Builder();
    Pet2 pet2 = builder
            .withAnimal(Animal.Cat)
            .withPetName("멍멍이")
            .withOwnerName("Jinyeong")
            .withAddress("Seoul")
            .withTelephone("01012341234")
            .build();
    
    //lombok 어노테이션을 사용한 객체의 인스턴스 생성
    Pet3 pet3 = Pet3.builder()
            .animal(Animal.Cat)
            .petName("멍멍이")
            .ownerName("Jinyeong")
            .address("Seoul")
            .telephone("01012341234")
            .build();
}
```

### 마치며
빌더패턴을 이용했을 때 가장 큰 장점은 멤버변수의 optional과 required를 컴파일하는 시점에서 체크할 수 있다는 점인 것 같다. 또 특정 멤버변수에 접근하기 위한 메서드가 정해져 있기때문에 실수로 다른 멤버변수에 다른 값을 집어넣는 실수를 막을 수 있다. 즉, 객체를 사용하는 사람 입장에서도 멤버변수별로 명시적인 메서드가 존재하기 때문에 이해하기가 쉽다. 또한 한줄로 인스턴스 생성이 이루어 지기 때문에 좀 더 명확하다.   
빌더를 생성하면 getter, setter가 없어도 되는 것은 아니다. 분명 멤버변수에 접근하고 값을 변경해야하는 경우가 생길 수 있다. 하지만 builder를 구현하면 무분별하게 getter, setter를 생성하는 것을 막을 수 있다. 객체를 생성할 때는 builder를 이용하여 생성하자.    
추가로 책을 읽으면서 자주 정리하는 습관을 가져야 겠다. 책을 읽고 덮으면 조금만 지나도 까먹는다. 그때그때 괜찮은 내용은 정리해서 두고두고 봐야겠다.

### 참고
도서 [자바 프로그래밍 면접, 이렇게 준비한다](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788968481529&orderClick=LAG&Kc=)  
http://jojoldu.tistory.com/251  
https://projectlombok.org/features/Builder   