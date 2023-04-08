---
marp: true
paginate: true
---

# Clean Architecture 1부 (1 ~ 15장)

## Driver: 이승준

---

# Clean Architecture?

![bg contain left](./images/architecture.png)
![bg contain](./images/architecture2.png)

`컴퓨터 시스템의 하드웨어 구조를 말합니다. 아키텍처는 컴퓨터 시스템을 구성하고 있는 하드웨어 장치인 CPU, 레지스터, 기억 장치, 입출력 장치 등과 같은 여러 가지 컴퓨터 구성 요소들에 대한 전반적인 기계적 구조와 이를 설계하는 방법`

---

# 아키텍처의 목표

`필요한 시스템을 만들고 유지보수하는 데 투입되는 인력을 최소화하는데 있다.`

![bg contain right](./images//architecture-not-good.png)
![bg contain](./images//architecture-not-good2.png)

---

# 두 가지 가치

- 행위 가치
- 구조 가치 => 변경하기 쉬워야 한다.

`프로그램을 동작하게 만들기는 그리 어려운 일이 아니다. 하지만 프로그램을 제대로 만드는 일은 전혀 다르다. 소프트웨어를 올바르게 만드는 일은 어렵다.`

---

![bg contain left](./images/developer-image.png)

일반적으로 업무 관리자는 행위 가치에 더 높은 가치를 둘 것이다.

### But!

**아키텍처의 중요성을 설득하는 일은 소프트웨어 개발팀이 마땅히 책임져야 한다.**

---

# 아키텍처는 Code로부터 시작된다. Clean Code!

![width:600px](./images/clean-code.png)

---

# 패러다임

프로그래밍을 하는 방법. `프로그래머에게서 특정 권한을 박탈(규칙 부과)함으로써 구조를 잡아준다`

- 구조적 프로그래밍
- 객체 지향 프로그래밍
- 함수형 프로그래밍

---

# 구조적 프로그래밍

`제어 흐름의 직접적인 전환에 대한 규칙을 부과`

- 제약 없는 go to문의 해로움 => 이해하기도 힘들고, 모듈을 더 작은 단위로 재귀적 분해하기 힘들게 만든다.
- 프로그램을 결합하는 3가지 방법인 순차, 분기, 반복만으로 충분히 계산 가능한 함수를 표현할 수 있다는 이론적 기반으로 탄생. (if/then/else, do/while 과 같은 단순 제어 구조 사용)

`이해도와 수정의 용이성이 높아지고, 모듈을 기능적으로 분해할 수 있게 되었다는 점에서 의의가 있다.`

![bg contain right:40%](./images/structure-programming.png)

---

# 객체 지향 프로그래밍

`제어 흐름의 간접적인 전환에 대한 규칙을 부과`

- 캡슐화
- 상속
- **다형성** => 거의 필살기
  - 의존성 역전

---

# 의존성 역전

![bg left contain](./images/dependency-inversion.png)
![bg contain](./images/dependency-inversion2.png)

- 일반적으로 소스 코드의 흐름은 제어의 흐름을 따르게 된다.
- ML1 과 I 인터페이스 사이 소스코드 의존성이 제어흐름과는 반대가 된다.

**`위 방법을 통해서 시스템의 소스 코드 의존성을 원하는 방향으로 설정할 수 있게 됨`**

## 아키텍트 관점에서도!

`고수준 정책 모듈을 저수준 세부사항 모듈로부터 독립시킬 수 있다.`

---

# 함수형 프로그래밍

`할당문에 대해 규칙을 부과. 프로그래밍의 함수를 수학적 함수처럼 적용하려함.`

- 클로저
- 순수성 (불변성)

경합조건, 교착상태, 동시 업데이트 등의 문제는 모두 가변 변수로 인해서 발생한다!
=> 순수성을 통해서 부수효과(side-effect)를 없앰!
![bg right contain](./images/race-problem.png)

---

**순수함수**

- 동일한 입력에 대해서 항상 같은 결과값
- 부수효과가 없음

```typescript
// not pure
let number = 1;

function increase(amount) {
  number += amount; // side effect
}

function now() {
  console.log(new Date());
}
```

---

```typescript
// pure
function add(a, b) {
  return a + b;
}

function changeState(state, val) {
  const clonedObject = _.clone(state);
  clonedObject.a = val;

  return clonedObject;
}
```

---

**완전한 불변성이 가능한가?**
컴퓨터 자원(저장공간, 처리능력)이 무한하다는 전제 하에 가능
![bg right contain](./images/event-sourcing.png)

---

![bg left:60% contain](./images/SOLID.jpeg)

`함수와 데이터 구조를 클래스로 배치하는 방법, 이들 클래스를 서로 결합하는 방법을 결정하는데 필요한 원칙`

---

# Single Responsibility Principle (SRP)

> 각 소프트웨어 모듈은 변경의 이유가 단 하나여야만 한다.

변경의 이유: 변경을 요청하는 한 명 이상의 집단(액터)를 의미함.

---

![bg contain](./images/SRP-example.png)
![bg contain](./images/SRP-example2.png)
![bg contain](./images/SRP-example3.png)

---

# Open-Closed Principle (OCP)

> 확장에는 열려있고, 수정에는 닫혀있어야 한다.

코드를 수정하기보다 새로운 코드를 추가하는 방식으로 설계해야 쉽게 변경할 수 있다.

---

![bg contain](./images/OCP-example.png)

---

# Liskov Substitution Principle (LSP)

> 상호 대체 가능한 구성요소를 이용해 소프트웨어 시스템을 만들 수 있으려면 이들 구성요소는 반드시 서로 치환 가능해야 한다.

- OO언어의 경우 상속을 통해서 쉽게 치환이 가능.
- OCP 가 가능하도록 한다.

---

![bg contain](./images/LSP-example.png)

---

# Interface Segeregation Principle (ISP)

> 사용하지 않는 것에 의존하지 않는다.

일반적으로 필요 이상으로 많은 것을 포함하는 모듈에 의존하는 것은 해롭다.
=> 불필요한 재컴파일, 재배포

---

![bg contain](./images/ISP-example.png)
![bg contain](./images/ISP-example2.png)

---

# Dependency Inversion Principle (DIP)

> 고수준 정책을 구현하는 코드는 저수준 세부사항을 구현하는 코드에 절대 의존해서는 안된다. 대신 세부사항이 정책에 의존해야 한다.
> 추상화는 세부 사항에 의존하지 않는다.

=> **_변화하기 쉬운 것보다 변화하지 않는 것에 의존하라!_** \*\*\*\*\*

---

![bg contain](./images/OCP-example.png)

---

# DIP 를 위한 코딩 실천 사항

> ---

- 변동성이 큰 구체 클래스를 참조하지 말라. 대신 추상 인터페이스를 참조하라.
- 변동성이 큰 구체 클래스로부터 파생(extends)하지 말라.
- 구체 함수를 오버라이드 하지 않는다. 차라리 추상 함수로 선언하고 구현체들을 분리한다.
- 고수준에서 구체적이고 변동성이 크다면 그 이름을 절대로 언급하지 마라.

---

# 컴포넌트 원칙

1. 컴포넌트?
1. 컴포넌트 응집도 원칙
   - 재사용/릴리스 등가 원칙
   - 공통 폐쇄 원칙
   - 공통 재사용 원칙
1. 컴포넌트 결합 원칙
   - 의존성 비순환 원칙
   - 안정된 의존성 원칙
   - 안정된 추상화 원칙

![bg right contain](./images/wall.jpeg)
![bg right contain](./images/home-arch.jpeg)

---

# 컴포넌트?

`시스템의 구성 요소로 배포할 수 있는 가장 작은 단위다. 따라서 독립적으로 개발 가능해야 한다.`
![bg right contain](./images/component.png)

---

# 컴포넌트 응집도

`어떤 클래스를 어떤 컴포넌트에 포함시켜야 할까?`

---

## 재사용/릴리스 등가 원칙 (REP)

> 재사용 단위는 릴리스 단위와 같다.

- 하나의 컴포넌트로 묶인 클래스, 모듈들은 반드시 **함께 릴리스할 수 있어야 한다**.

단일 컴포넌트는 응집성 높은 클래스와 모듈들로 구성되어야함. 컴포넌트를 구성하는 모든 모듈은 서로 공유하는 중요한 테마나 목적이 있어야 한다.
=> 추상적이다. 뭐 어쩌라는거지?

---

## 공통 폐쇄 원칙 (CCP)

> 동일한 이유, 동일한 시점에 변경되는 클래스를 같은 컴포넌트로 묶는다.

- SRP 원칙을 컴포넌트 관점에서 본 원칙.
- 물리적 또는 개념적으로 강하게 결합되어 항상 함께 변경되는 클래스들을 하나의 컴포넌트로 묶는다.
- 변경이 여러 컴포넌트에 분산되어 발생하기보다 하나의 단일 컴포넌트로 제한하는 것이 안정성이 높음.
- 릴리스, 재검증, 배포하는 일과 관련된 작업을 최소화 가능

---

## 공통 재사용 원칙 (CRP)

> 컴포넌트 사용자들을 필요하지 않는 것에 의존하게 강요하지 말라.
