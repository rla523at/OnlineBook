# Procedural Programming
`절차적 프로그래밍(procedural programming, PP)`은 시스템을 독립적인 기능을 담당하는, 재사용 가능한 프로시저의 구성으로 보고 프로그래밍을 하는 패러다임이다.

프로시저는 오직 로직만을 담당하기 때문에 로직에 필요한 데이터는 별도의 모듈에 위치하게 된다. 이로 인해 프로시저는 필요한 모든 데이터 모듈에 의존성을 갖게 되며 데이터 모듈이 변경되면 프로시저도 함께 변경되어야 한다.

이처럼 PP는 의존성을 적절히 관리하지 못해 데이터 모듈의 변경이 프로시저 모듈에까지 영향을 미쳐 변경이 어려워진다.

# Object Oriented Programming
`객체지향(Object Oriented Programming, OOP)`은 시스템이 역할 또는 책임을 가진 객체들로 구성되어 있으며 시스템의 기능은 객체들의 상호작용에 의해 이루어진다고 보고 프로그래밍을 하는 패러다임이다.

객체는 각자의 역할 또는 책임을 당당하기 위해 필요한 데이터와 로직을 모두 가지고 있으며  다른 객체들은 오직 이 객체의 역할과 책임에만 의존하게 된다. 따라서 객체의 역할과 책임이 달라지지 않는다면 내부의 데이터나 로직이 변경되더라도 다른 객체에는 영향을 미치지 않는다. 

이처럼 OOP는 의존성을 적절히 관리함으로써 데이터나 로직의 변경이 다른 객체에 영향을 미치는 것을 효율적으로 억제할 수 있어 변경이 수월하다.

# 책임

# 역할

# 추상화

# 캡슐화
캡슐화는 외부에서 알 필요가 없는 부분을 감춤으로써 대상을 단순화하는 추상화의 한 종류다.

변경 가능성이 높은 구현 세부사항을 안정적인 인터페이스 뒤로 캡슐화한다.

변경 될 수 있는 어떤 것이라도 캡슐화해야 한다.

객체 내부의 데이터를 외부로 감추는 것은 '데이터 캡슐화'라고 불리는 캡슐화의 한 종류일 뿐이다

# 응집도
응집도란 모듈의 내부 요소들이 밀접하게 연관되어 있는 정도이다.

모듈의 응집도가 낮을 경우, 내부 요소마다 서로 다른 이유에 의해서 변경이 발생할 수 있다. 반면에 모듈의 응집도가 높을 경우 내부 요소들은 서로 밀접하게 연관되어 있어 공통에 이유에 의해서 변경이 발생하게 된다.

따라서, 모듈이 서로 다른 이유에 의해서 변경이 발생하는 경우 모듈의 응집도가 낮다고 하며 하나의 이유에 의해서만 변경이 발생할 때 모듈의 응집도가 높다고 한다.

또한, 모듈의 응집도가 낮을 경우 관련된 기능이 서로 다른 모듈에 존재할 수 있다. 하지만 모듈의 응집도가 높을 경우 관련된 기능들은 모두 한 모듈에 존재하게 된다.

따라서, 하나의 변경이 발생할 때 많은 모듈에 변경이 발생하는 경우 모듈의 응집도가 낮다고 하며 하나의 모듈에 변경이 발생하는 경우 모듈의 응집도가 높다고 한다.

# 서브 클래싱과 서브타이핑
서브클래싱은 다른 클래스의 코드를 목적으로 상속을 사용하는 경우를 가리킨다. 서브 클래싱은 구현 상속 또는 클래스 상속이라고 부르기도 한다.

서브타이핑은 타입 계층을 구성하기 위해 상속을 사용하는 경우를 가리킨다. 서브타이핑에서는 자식 클래스와 부모 클래스의 행동이 호환되기 때문에 자식 클래스의 인스턴스가 부모 클래스의 인스턴스를 대체할 수 있다. 서브타이핑을 인터페이스 상속이라고 부르기도 한다.

> Reference  
> {cite}`Object` Chapter 13.3

# SOLID
## S(Single Responsibility Principle; SRP)
단일 책임 원칙은 "모듈은 단 한가지의 변경 이유만 가져야 한다."는 설계 원칙이다. 즉, 높은 응집도를 추구하는 설계 원칙이다.

## O(Open-Closed Principle; OCP)
개방 폐쇄 원칙은 "확장에 대해 열려있고 수정에 대해서는 닫혀있어야 한다."는 설계 원칙이다. 즉, 기존의 코드를 수정하지 않고 어플리케이션의 기능을 확장할 수 있어야 한다는 설계 원칙이다.

기존의 코드를 수정하지 않기 위해서는 추상화된 인터페이스에 의존해야 한다. 추상화된 인터페이스는 다양한 상황에서 공통점을 반영하여 핵심적인 부분만 남아있기 때문에 문맥이 바뀌더라도 변하지 않게 되고 수정하지 않을 수 있다.

## L(Liskov Substitution Principle; LSP)
리스코프 치환 원칙은 "상속 관계로 연결된 두 클래스가 서브타이핑 관계를 만족하기 위해서는 base 타입의 인스턴스가 derive 타입의 인스턴스로 대체될 수 있어야 한다"는 설계 원칙이다. 



해당 객체를 사용하는 클라이언트는 base 타입이 derive 타입으로 변경되어도, 차이점을 인식하지 못한 채 base 타입의 퍼블릭 인터페이스를 통해 derive 클래스를 사용할 수 있어야 한다는 것이다. 

base class와 derive class는 행동 호환성을 유지해야 한다.

즉, 이는 클라이언트와 객체 사이의 계약이 존재하고, 이를 준수해야 한다는 원칙이므로 계약에 따른 설계(design by contract) 라고 표현할 수도 있다. 하위 클래스는 상위 클래스의 동작 규칙(계약)을 따라야 하는 것이다.

상속 계층을 사용하는 클라이언트 입장에서 부모 클래스와 자식 클래스의 차이점을 몰라야 한다. 이를 자식 클래스와 부모 클래스 사이의 행동 호환성이라고 부른다. 여기서 중요한 것은 행동의 호환 여부를 판단하는 기준은 클라이언트의 관점이라는 것이다.

## I(Interface segregation Principle; ISP)
인터페이스 분리 원칙이란 "클라이언트가 보기에 자신이 실제로 호출할 메서드만 보이도록 인터페이스를 분리하라"는 설계 원칙이다. 

호출하지 않는 메서드에 대한 클라이언트의 의존성을 끊고, 클라이언트가 서로에 대해 독립적이 되게 만들 수 있다.

클라이언트는 자신이 실제로 호출하는 메서드에만 의존해야 한다. 비대한 클래스의 인터데이스를 여러 개의 클라이언트에 특학된 인터페이스로 분리함으로써 성취될 수 있다.

## D(dependecy inversion Principle, DIP, 의존성 역전 원칙)
의존 역전 원칙이란 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안 되며, 저수준 모듈이 고수준 모듈에 의존해야 한다는 것이다.

의존 역전 원칙이란 결국 비즈니스와 관련된 부분이 세부 사항에는 의존하지 않는 설계 원칙을 의미한다.

의존 역전 원칙은 개방 폐쇄 원칙과 밀접한 관련이 있으며, 의존 역전 원칙이 위배되면 개방 폐쇄 원칙 역시 위배될 가능성이 높다. 또한 의존 역전 원칙에서 주의해야 하는 것이 있는데, 의존 역전 원칙에서 의존성이 역전되는 시점은 컴파일 시점이라는 것이다.

상위 정책과 하위정책의 구분??

왜 Movie가 Amount DiscountPolicy보다 상위 정책인가???

객체 사이의 협력이 존재할 떄 그 협력의 본질을 담고 있는 것이 상위 수준의 정책이다.

고수준 모듈과 저수준 모듈의 구분???

https://mangkyu.tistory.com/194

---

# 기타

## 타입 추가 vs 오퍼레이션 추가

설계는 변경과 관련된 것이다. 설계의 유용성은 변경의 방향성과 발생 빈도에 따라 결정된다. 그리고 추상 데이터 타입과 객체지향 설계의 유용성은 설계에 요구되는 변경의 압력이 '타입 추가'에 관한 것인지, 아니면 '오퍼레이션 추가'에 관한 것인지에 따라 달라진다.

타입 추가라는 변경의 압력이 더 강한 경우에는 객체지향의 손을 들어줘야 한다. 추상 데이터 타입의 경우 새로운 타입을 추가하려면 타입을 체크하는 클라이언트 코드를 일일이 찾아 수정한 후 올바르게 작동하는지 테스트해야 한다. 반면 객체지향의 경우에는 클라이언트 코드를 수정할 필요가 없다. 간단하게 새로운 클래스를 상속 계층에 추가하기만 하면 된다.

이에 반해 변경의 주된 압력이 오퍼레이션을 추가하는 것이라면 추상 데이터 타입의 승리를 선언해야 한다. 객체지향의 경우 새로운 오퍼레이션을 추가하기 위해서는 상속 계층에 속하는 모든 클래스를 한번에 수정해야 한다. 이와 달리 추상 데이터 타입의 경우에는 전체 타입에 대한 구현코드가 하나의 구현체 내에 포함돼 있기 때문에 새로운 오퍼레이션을 추가하는 작업이 상대적으로 간단하다.

새로운 타입을 빈번하게 추가해야 한다면 객체지향의 클래스 구조가 더 유용하다. 새로운 오퍼레이션을 빈번하게 추가해야 한다면 추상 데이터 타입을 선택하는 것이 현명한 판단이다. 변경의 축을 찾아라. 객체지향적인 접근법이 모든 경우에 올바른 해결 방법인 것은 아니다.`(252)`

## 추상 데이터 타입
하나의 타입으로 여러가지 타입을 나타내는 경우.

추상 데이터 타입이지 알아보는 방식은 타입을 가르키는 ENUM등이 멤버 변수로 있는지 확인해보면 된다.

## 의존성 주입과 FACTORY
추상화된 인터페이스에만 의존하기 위해서는 구체 클래스 생성과 관련된 의존성을 해결해야 한다. 이를 위해 외부에서 인스턴스를 생성한 후 이를 전달해서 해결하는 의존성 주입이 필요하다.

그리고 인스턴스를 생성하는 책임을 전담하는 별도의 객체를 FACTORY라고 부른다.

