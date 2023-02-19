---
title: 'Prototype / Class'
date: '2020-10-05'
tags: ['Javascript', 'Prototype', 'Class']
draft: false
summary: 'Javascript 의 Prototype 및 Class 의 개념에 대해 이해한 내용을 정리 했습니다.'
---

![Javascript](https://upload.wikimedia.org/wikipedia/commons/6/6a/JavaScript-logo.png?20120221235433)

# 알아두면 좋은 Javascript

> 파이널 프로젝트를 진행하며 구현하고자 하는 기능들은 대부분 Ajax 를 이용해서 구현해야 했다. 그래서 자연스레 Javascript를 접할 기회가 많았는데, 이번에 Node.js 를 공부하며 느꼈다. 프로젝트에서 만난 Javascript는 우주 먼지보다 작은 존재라는 것을..

## Prototype 톺아보기

Prototype 은 Class 에 대한 문법을 공부하기 위해서 필수적으로 알아야 하는 개념이라고 생각한다. 나는 클래스 기반으로 동작하는 Java 로 프로그래밍 공부를 시작해서 Javascript의 Class 문법도 금방 이해할 수 있을 줄 알았는데 전혀 아니었다.

Javascript는 프로토타입 기반으로 동작하는 언어로써, Class 문법을 사용할 수 있지만 이는 보기에만 클래스 기반으로 움직이는 것 처럼 보이게 한다. 즉, 겉으로는 클래스 기반인 척 하지만 속으로는 프로토타입 기반으로 움직이고 있다는 말이다.**_~(🍪겉바속촉 재질?)~_**

그렇다고 Protytpe과 Class가 비슷하냐? 그 것도 아니다! 하지만 두가지 개념에 대해 충분히 헷갈릴 수 있다.
**_~(블로그 주인장께서 그랬다.)~_** 그렇기에 위의 두 개념에 대한 차이점을 이해하고 Class 문법을 쉽게 이해하기 위해서 Prototype 에 대한 개념을 알아야 하고, 이를 넘어 Prototype-Object, Prototype-properties 등 다양한 문법과 이에 대한 개념을 익혀야 한다.

### 참고하면 좋은 블로그 링크

[\[Javascript \] 프로토타입 이해하기](https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67)

[Javascript 기초 - Object prototype 이해하기](http://insanehong.kr/post/javascript-prototype/)

### _Prototype?_

Javascript에서는 Class는 없지만**_~(겉바속촉)~_** 함수와 new 를 통해 Class 처럼 표현할 수 있다.

```javascript
// Dog 라는 함수를 만들고
// 해당 함수가 eyes(눈), nose(코)라는 속성으로 구성되어 있음을 선언한다.
function Dog() {
  this.eyes = 2
  this.nose = 1
}

// 또익이와 꼬망이는 Dog 라는 함수를 통해 객체를 만들고, Dog 라는 것을 명시한다.
var 또익이 = new Dog()
var 꼬망이 = new Dog()

console.log(또익이.eyes) // 또익이의 눈은 2개이다. Dog 함수의 eyes 속성을 상속받기 때문이다.
console.log(꼬망이.nose) // 꼬망이의 코는 1개이다. Dog 함수의 nose 속성을 상속받기 때문이다.
```

위와 같이 `또익이`와 `꼬망이`는 `Dog`라는 함수를 통해 객체가 만들어지며 이는 eyes 2개, nose 1개를 공통적으로 가지고 있다. 이는 메모리에서 각 속성이 `Dog` 함수를 통해 만들어진 객체의 개수만큼 할당된다. 즉, `또익이`와 `꼬망이`같은 강아지를 1000마리를 만들면 2000개의 변수가 할당된다.

이와 같이 특정 함수를 통해 만들어진 객체가 만들어질 수록 할당해야 할 메모리가 많아지기 때문에 굉장히 비 효율적이다. 이러한 문제를 Prototype 을 통해 해결할 수 있다.

```javascript
function Dog() {}

Dog.prototype.eyes = 2
Dog.prototype.nose = 1

var 또익이 = new Dog()
var 꼬망이 = new Dog()

console.log(또익이.eyes)
console.log(꼬망이.nose)
```

위의 코드에서 `**Dog.prototype**` 는 비어있는 Object로서 메모리 어딘가에 존재하고 있으며, `Dog` 함수로 만들어진 객체 `또익이`와 `꼬망이`들은 메모리 어딘가에 존재하는 Object에 있는 모든 속성의 값을 사용할 수 있다.  
즉, `Dog` 함수에 존재하는 `eyes` 와 `nose` 속성을 메모리 어딘가에 저장하고, `또익이` 와 `꼬망이` 가 `Dog` 함수의 `eyes` 와 `nose` 속성을 공유한다는 뜻이다.

즉, 첫 번째 코드에서 `Dog` 함수는 해당 함수 내에 속성을 만들어 해당 함수로 만들어질 객체에 해당 함수의 속성을 할당했다. 반면 두 번째 코드에서 `Dog` 함수는 껍데기만 있을 뿐 속은 텅 비었다. 하지만 `**Object.prototype`\*\* 이라는 속성을 이용해 범용으로 사용 가능하도록(?) `Dog` 함수의 속성을 선언하였다.  
이로써 `Dog` 함수로 만들어진 객체가 몇 개가 되든 `**Dog.prototype**` 의 `eyes` 와 `nose` 속성을 공유할 수 있다.

### _Prototype Link? Prototype Object?_

블로그에서는 Javascript의 기초가 탄탄하지 않은 사람들이 Prototype Link와 Prototype Object에 대해 명확한 차이를 이해하기 어렵다고 말한다.**_~(== 나)~_**  
그래서 이 둘이 도대체 무엇이며 차이는 무엇인지 알아보려고 한다.

먼저 **Prototype Link** 와 **Prototype Obejct** 는 **_Prototype_** 이라고 통틀어 부른다.

**Prototype Link**는 마치 Spring Framework 의 특징 중 하나인 Dependency(주입)의 일부분 같았다.**_~(굉장히 주관적인 의견)~_** 왜냐하면 블로그에 상세히 나와 있는 그림을 보면 알겠지만 특정 함수로 만들어진 객체에 속성이 선언되지 않았는데도 \***\*proto\*\*** 라는 속성**(Prototype Property)**을 통해 참조하고 있는 함수의 속성을 가리킬 수 있기 때문이다.  
또한, 해당 이름에서도 알 수 있듯 Link 라고 대놓고 말하고 있다. (Spring Framework 를 공부할 때 Autowired 를 통해 bean 으로 등록하여 끌어다 사용하는 것을 '링크 건다' 라고 표현했던 기억이 있다.)  
이렇게 특정 객체가 참조하고 있는 함수를 **proto** 속성을 통해 속성을 가리키는 것(상위 프로토타입과 연결되어 있는 형태)을 **Prototype Chain** 이라고 한다.  
이를 또 다른 관점으로 생각해보면 DOM 과 비슷하다고 말할 수 있을 것 같다.

![https://miro.medium.com/max/1400/1*mwPfPuTeiQiGoPmcAXB-Kg.png](https://miro.medium.com/max/1400/1*mwPfPuTeiQiGoPmcAXB-Kg.png)

**Prototype Object**는 말 그대로 원시의 객체를 뜻한다. Javascript에서 모든 객체는 함수를 통해 생성되는데, Object, Function, Array 등 이러한 것들도 함수로 정의되어 있다. 이렇게 객체는 모두 함수로 생성된다는 특징을 바탕으로 함수가 정의 될 때는 두 가지 특징을 가진다.

1.  **해당 함수에 Constructor(생성자) 자격 부여**
    - 생성자 자격이 부여되면 new를 통해 새로운 객체를 만들수 있게 된다.
2.  **해당 함수의 Prototype Object 생성 및 연결**
    - 생성자 자격을 부여하게 되면서 객체가 만들어질 때 함께 생성된 함수를 가르키고 있다.

이러한 특징을 가진 **Protype Object**는 일반적인 객체로 속성을 입맛에 맞게 추가/삭제가 가능하다. 또한 **Protype Object**의 특징으로 인해 해당 함수로 만들어진 객체는 해당 함수를 참조할 수 있다.

## Class 톺아보기

위의 Prototype 개념에 대한 이해가 조금 되었다면 이제 Class 개념에 대해 이해할 차례이다.

```javascript
// Human 함수 생성
var Human = function (type) {
  this.type = type || 'human'
}

Human.isHuman = function (human) {
  return human instanceof Human
}

// Human()을 참조하여 생성될 객체에 부여할 속성 선언
Human.prototype.breathe = function () {
  alert('h-a-a-a-m')
}

// Human()를 참조받아 새로운 객체 생성
var sh = function (type, firstName, lastName) {
  Human.apply(this.arguments)
  this.firstName = firstName
  this.lastName = lastName
}

// 위에서 Human() 함수를 참조받아 만들어진 객체의 기본 속성 설정
sh.prototype = Object.create(Human.prototype) // Human() 함수의 속성을 불러와 설정한다.
sh.prototype.constructor = sh // 생성자는 sh
sh.prototype.sayName = function () {
  alert(this.firstName + ' ' + this.lastName)
} // sh 객체에 sayName 이라는 함수를 추가한다.

var oldSh = new sh('human', 'SeongHeon', 'Sim')
Human.isHuman(oldSh)
console.log(Human.isHuman(oldSh))
// 위의 코드에서 Human 생성자 함수가 있고, 그 함수를 Zero 생성자 함수가 상속한다.
// Zero 생성자 함수를 보면 상송받기 위한 코드가 상당히 난해한데, Human.apply 와 Object.create 부분이 상속받으면서 복잡해졌다.
```

위의 코드의 경우 Prototype 을 이용해 상속을 구현했다. 하지만 난생 처음 보는 코드가 난무해서 꽤나 당황했다. 위의 코드를 Class 문법을 적용하면 아래의 코드처럼 바뀌고, 코드를 확인해보면 가독성이 많이 좋아졌다는 것을 느낄 수 있다. 왜냐하면 Java의 클래스 구조와 굉장히 비슷하기 때문이다! 다만, Javascript에서 아래의 코드가 클래스를 기반으로 동작 하지는 않는 점을 참고해야 한다.

```javascript
// Class를 이용해 상속 개념 이해하기
// 클래스 선언
class Person {
  // Person 클래스의 생성자 선언
  constructor(type = 'person') {
    this.type = type
  }

  // 정적 메소드 생성 (위의 코드에서 Human.isHuman 같은 클래스 함수)
  static isPerson(person) {
    return person instanceof Person
  }

  // 숨쉬는 메소드 생성
  breathe() {
    alert('하-아-아-암')
  }
}

// Person 클래스를 상속받는 Ssh 클래스 생성
class Ssh extends Person {
  // 상속받은 Person 클래스의 생성자를 불러와 Ssh 인스턴스 변수값 설정
  constructor(type, firstName, lastName) {
    super(type)
    this.firstName = firstName
    this.lastName = lastName
  }

  // Ssh 클래스에 sayName() 함수를 만들어,
  // 해당 함수에 Person 클래스의 breathe() 함수 삽입
  sayName() {
    super.breathe()
    alert(`${this.firstName} ${this.lastName}`)
  }
}

// newSsh 객체를 Ssh 클래스를 이용해 생성
const newSsh = new Ssh('person', 'SeongHeon', 'Sim')
Person.isPerson(newSsh)
```
