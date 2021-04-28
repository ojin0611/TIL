# Design Pattern

[TOC]



# Singleton Pattern

싱글턴 패턴은 `인스턴스가 오직 1개만 생성`되야 하는 경우에 사용되는 패턴이다.

객체를 여러 개 생성해도 그 객체들이 1개의 인스턴스만 가리키고싶을 때 사용한다. 객체를 new 로 생성하면 항상 새로운 주소공간을 할당하기때문에 객체가 여러개 생성되지만, 메소드를 이용해 객체를 한개만 생성할 수 있도록 컨트롤해주면 하나의 객체를 공유할 수 있다.



## 특징

1. private constructor
2. static method



## Example

![image-20210220000419737](images/image-20210220000419737.png)

```java
public class Speaker2 {
	private static Speaker2 speaker; // speaker 객체를 static 변수로 공유 
	private int volume;
	
    // private constructor
	private Speaker2() {
		volume=5;
	}
	
    // static method for control object. 여기가 핵심
	public static Speaker2 getInstance() {
		if(speaker==null) { // 한번도 speaker 객체가 생성된 적이 없다면
			speaker=new Speaker2(); // speaker 객체를 생성한다.
		}
		return speaker; // 공유되는, 한번만 생성되는 speaker를 반환
	}
	
	public int getVolume() {
		return volume;
	}

	public void setVolume(int volume) {
		this.volume = volume;
	}
}

```

위 코드는 멀티쓰레드 환경에서 문제가 발생할 수 있다. `getInstance() `  블럭을 여러 쓰레드가 동시에 사용한다면 여러개의 인스턴스가 발생할 위험이 있다. 

또한 변수 volume도 다른 프로세스에서 처리할 경우 값이 일관되지 않을 수 있다. 

따라서 아래처럼 작성하는 것이 멀티쓰레드 환경에서 안전하다.

```java
public class Printer {
    private static Printer printer = new Printer();
    private static int count = 0;

    private Printer(){}

    public static Printer getInstance() {
        return printer;
    }

    public synchronized static void print(String input) {
        count++;
        System.out.println(input + "count : "+ count);
    }
}
```

정적 변수 (static)는 객체가 생성되기 전 **클래스가 메모리에 로딩될 때** 만들어져 초기화가 한 번만 실행된다. 또한 프로그램이 종료될때까지 없어지지 않고 메모리에 상주하기때문에 클래스에서 생성된 모든 객체에서 참조할 수 있다. 즉, 기존 조건문에서 체크하던 부분이 원칙적으로 제거된 것이다.

count 변수의 경우, 하나의 객체만 사용되지만 여러 쓰레드가 동시에 접근하기때문에 쓰레드마다 값이 달라질 수 있다. 따라서 `synchronized` 키워드를 통해 여러 쓰레드에서 동시에 접근하는 것을 막을 수 있다.





# MVC

JSP를 이용해 구성할 수 있는 Web Application Architecture는 크게 model1, model2로 나뉜다.

JSP가 client 요청에 대한 logic 처리와 view(response page) 처리를 모두하면 model 1이고, view(response page)에 대한 처리만 하면 model 2다.

Model 2구조는 MVC Pattern을 web 개발에 도입한 구조다.



## Model 1

![image-20210401093956989](images/image-20210401093956989.png) 

구조가 단순하고 직관적이다. 대신 JSP 코드 자체가 복잡해진다. JSP 코드에 FE/BE가 혼재한다.



## Model 2

Client 요청에 대한 처리는 servlet, Logic 처리는 java class(Service, Dao), Response page는 JSP가 담당한다. 이는 MVC pattern을 웹 개발에 도입한 구조며, 완전히 같은 형태를 보인다.



**Model** : Service, Dao, Java Beans. 

- Logic을 처리하는 모든것
- controller에서 넘어온 data를 이용해 일을 수행하고 그 결과를 다시 controller에 return한다.

**View** : JSP. 

- 모든 화면 처리 담당.
- 결과 출력을 위한 코드만 존재. java code는 없다. 

**Controller** : Servlet. 

- Client 요청을 분석해 Logic 처리를 위한 Model을 호출한다.
- return 받은 결과 data를 request, session등에 저장하고 redirect/forward 방식으로 jsp를 이용해 출력



![image-20210401100354266](images/image-20210401100354266.png) 

Service, Dao 는 1개의 객체만 존재하면 되므로 Singleton으로 만들어준다. Spring은 default가 singleton이다. 즉, 유저가 직접 Singleton 코딩하지 않아도 Service와 Dao를 싱글톤 객체로 생성해준다.



# Factory Pattern

## 생성 패턴

팩토리 패턴은 **생성 패턴**(인스턴스를 만드는 절차를 추상화하는 패턴) 중 하나다. 생성 패턴에 속하는 패턴들은 객체를 생성, 합성하는 방법이나 객체의 표현방법을 시스템과 분리해준다.

생성 패턴을 이용하면 무엇이 생성되고, 누가 생성하며, 어떻게 생성되는지, 언제 생성할 것인지 결정할 때 유연성을 확보할 수 있게 된다.



## 팩토리 패턴

팩토리 패턴은 객체를 생성하는 인터페이스는 미리 정의하되, 인스턴스를 만들 클래스의 결정은 **서브 클래스**가 내리는 패턴이다.  

- 팩토리 메소드 패턴 : 객체를 생성하기 위한 인터페이스를 정의하는데, 어떤 클래스의 인스턴스를 만들지는 서브클래스에서 결정한다.
- 추상 팩토리 패턴 : 인터페이스를 이용해 서로 연관된, 의존하는 객체를 구상클래스를 지정하지 않고도 생성할 수 있다. 팩토리 메소드 패턴이 추상 팩토리 패턴에 포함될 수 있다.

결과적으로 팩토리 패턴을 통해 new 키워드를 사용하는 부분을 서브클래스에 위임함으로서 **객체 생성을 캡슐화**하고 구상 클래스에 대한 **의존성을 줄이는** 이점을 얻을 수 있다.

> 의존성이 줄어드는 것은 의존성 뒤집기 원칙(Dependency Inversion)에 기인한다.



### 예제 1

```java
// PC, Server implements interface Computer (= subclass of Computer)
// 팩토리 메소드 클래스
public class ComputerFactory {
 	// 팩토리 메소드. return 객체
    public static Computer getComputer(String type, String ram, String hdd, String cpu){
        if("PC".equalsIgnoreCase(type)) // 대소문자 구분 X
            return new PC(ram, hdd, cpu);
        else if("Server".equalsIgnoreCase(type))
            return new Server(ram, hdd, cpu);
		
        return null;
    }
}

// Application
public class TestFactory {
    public static void main(String[] args) {
        // Computer의 Subclass로 PC, Server가 있는데 Application은 이를 모른채 인스턴스를 생성할 수 있다.
        Computer pc = ComputerFactory.getComputer("pc","2 GB","500 GB","2.4 GHz");
        Computer server = ComputerFactory.getComputer("server","16 GB","1 TB","2.9 GHz");
        System.out.println("Factory PC Config::"+pc);
        System.out.println("Factory Server Config::"+server);
    }
 
}
```

팩토리 메소드 패턴을 구현할 때 Factory class를 Singleton으로 구현해도 되고, 서브클래스를 리턴하는 static method로 구현해도 된다. 위의 예제는 `getComputer()`에 입력된 파라미터에 따라 다른 서브클래스의 인스턴스를 생성해 리턴한다.



### 예제2

```java
// SpaceSuit, HydroSuit, CombatSuit extends abstract class Suit

// Factory Class extends abstract class SuitFactory
public class TypeSuitFactory extends SuitFactory{

    @Override
    public createSuit(String type){
        Suit suit = null;

        switch(type){
            case("space"):
                suit = new SpaceSuit();
                break;
            case("hydro"):
                suit = new HydroSuit();
                break;
            default:
                suit = new CombatSuit();
        }
        return suit;
    }
}

// Application의 main method
public static void main(String[] args){
    TypeSuitFactory typeSuitFactory = new TypeSuitFactory();

    Suit suit1 = typeSuitFactory.createSuit("stealth");
    Suit suit2 = typeSuitFactory.createSuit("space");
    Suit suit1 = typeSuitFactory.createSuit("");

    System.out.println(suit1.getName());
    System.out.println(suit2.getName());
    System.out.println(suit3.getName());
}
```

메인 프로그램에서는 어떤 객체가 생성됐는지 신경쓸 필요없이 반환된 객체만 사용하면 된다. 이로 인해 Suit class에서 변경이 발생해도 메인 프로그램이 변경되는 것을 최소화할 수 있다.



### 예제3

Spring에서 음식 종류별로 배달하는 서비스를 만든다.

아래는 음식 종류와, 각 종류별 Service

```java
// 사탕, 과자, 라면, 초콜릿
public enum FoodType {
    CANDY, SNACK, NOODLE, CHOCOLATE
}

// 음식 배달의 상위 인터페이스인 FoodService
public interface FoodService {

    FoodType getFoodType();

    void deliverItem();
}

// 사탕 ServiceImpl
@Service
public class CandyServiceImpl implements FoodService {

    @Override
    public FoodType getFoodType() {
        return FoodType.CANDY;
    }

    @Override
    public void deliverItem() {
        System.out.println("사탕 배달 완료!");
    }
}

// 과자 ServiceImpl
@Service
public class SnackServiceImpl implements FoodService {

    @Override
    public FoodType getFoodType() {
        return FoodType.SNACK;
    }

    @Override
    public void deliverItem() {
        System.out.println("과자 배달 완료!");
    }
}

// 라면 ServiceImpl
@Service
public class NoodleServiceImpl implements FoodService {

    @Override
    public FoodType getFoodType() {
        return FoodType.NOODLE;
    }

    @Override
    public void deliverItem() {
        System.out.println("라면 배달 완료!");
    }
}

// 초콜릿 ServiceImpl
@Service
public class ChocolateServiceImpl implements FoodService {

    @Override
    public FoodType getFoodType() {
        return FoodType.CHOCOLATE;
    }

    @Override
    public void deliverItem() {
        System.out.println("초콜릿 배달 완료!");
    }
}
```



아래는 각 `FoodServiceImpl`들을 이용해 food type에 맞는 `FoodService` 를 return해주는 factory class와 factory method다.

```java
@Component
public class FoodServiceFactory {
    // foodService를 담고있어줄 Map
    private final Map<FoodType, FoodService> foodServices = new HashMap<>();

    // 생성자 주입으로 FoodService를 상속하고 있는 bean들을 주입받는다.
    public FoodServiceFactory(List<FoodService> foodServices) {
        // foodService를 상속받는 bean이 없을 경우 IllegalArguemntException을 던진다.
        if(CollectionUtils.isEmpty(foodServices)) {
            throw new IllegalArgumentException("존재하는 foodService가 없음");
        }

        // foodService의 구현체인 bean들을 for문을 돌리면서 key는 음식 종류의 타입, value는 해당 동일한 bean을 map에 담아준다.
        for (FoodService foodService : foodServices) {
            this.foodServices.put(foodService.getFoodType(), foodService);
        }
    }

    public FoodService getService(FoodType foodType) {
        // 인자로 넘겨준 타입에 맞는 foodService의 bean을 넘겨준다.
        return foodServices.get(foodType);
    }
}
```



`FoodOrderService` class는 foodServiceFactory를 이용해, 각 음식에 맞는 주문을 해주는 method를 가진 서비스다. 이를 Test한 코드는 아래와 같다.

```java

@Service
public class FoodOrderService {

    private final FoodServiceFactory foodServiceFactory;

    public FoodOrderService(FoodServiceFactory foodServiceFactory) {
        this.foodServiceFactory = foodServiceFactory;
    }
    public void receiveOrder(OrderContent orderContent) {
        // other receiver Logic....
		foodServiceFactory.getService(orderContent.getFoodType()).deliverItem();
    }
}

@SpringBootTest
class FoodServiceFactoryTest {

    @Autowired
    private FoodServiceFactory foodServiceFactory;

    @Test
    void 음식_타입별로_서비스_가져오기() {
        // given
        FoodType candy = FoodType.CANDY;
        FoodType chocolate = FoodType.CHOCOLATE;
        FoodType snack = FoodType.SNACK;
        FoodType noodle = FoodType.NOODLE;

        // when
        FoodService candyService = foodServiceFactory.getService(candy);
        FoodService chocolateService = foodServiceFactory.getService(chocolate);
        FoodService snackService = foodServiceFactory.getService(snack);
        FoodService noodleService = foodServiceFactory.getService(noodle);

        // then
        assertThat(candyService.getFoodType(), is(FoodType.CANDY)); // 비교
        assertThat(chocolateService.getFoodType(), is(FoodType.CHOCOLATE));
        assertThat(snackService.getFoodType(), is(FoodType.SNACK));
        assertThat(noodleService.getFoodType(), is(FoodType.NOODLE));

        // print
        candyService.deliverItem();
        chocolateService.deliverItem();
        snackService.deliverItem();
        noodleService.deliverItem();
    }
}
```





## 장점

1. 클라이언트 코드로부터 서브클래스의 인스턴스화를 제거해 서로 간 종속성을 낮추고, 결합도를 느슨하게 하며, 확장을 쉽게 한다.
2. 팩토리 패턴은 클라이언트와 구현 객체들 사이에 추상화를 제공한다.