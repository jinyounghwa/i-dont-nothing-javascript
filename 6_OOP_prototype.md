##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
프로토타입 패턴
모든 함수는 prototype 프로퍼티를 가진다. 이 프로퍼티는 해당 참조 타입의 인스턴스가 가져야 할 프로퍼티와 메서드를 담고 있는 객체이다. 이 객첸느 생성자를 호출 할 때 생성되는 객체의, 문자 그대로, 프로포타입이다. 프로토타입의 프로퍼티와 메서드는 객체 인스턴스 전체에서 공유된다는 점이 프로토 타입의 장점이다. 객체 정보를 생성자에 할당하는 대신 다음과 같이 직접적으로 프로토타입에 할당 할 수 있다.  
<pre>
function Person () {
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function () {
  alert(this.name);
}
var person1 = new Person();
person1.sayName(); //"Nicholas"

var person2 = new Person();
person2.sayName(); //"Nicholas"

alert(person1.sayName == person2.sayName); //true
</pre>
여기에서 프로퍼티들과 sayName()매서드는 Person의 prototype프로퍼티에 직접 추가되었고 생성자 함수는 비워 두었다. 생성자 함수가 비어 있지만 생성자를 호출해 만든 객체에도 프로퍼티와 메서드가 존재한다. 생성자 패턴과는 달리 프로퍼티와 매서드를 모든 인스턴스에서 person1과 person2는 같은 프로퍼티 집합에 접근하며 같은 sayName()함수를 공유한다.  

프로토타입 함수는 어떻게 움직이는가  
함수가 생성될 때마다 prototype프로퍼티 역시 특정 규칙에 따라 생성된다. 기본적으로 모든 프로토타입은 자동으로 constructor프로퍼티를 갖는데 이 프로퍼티는 해당 프로토타입이 프로퍼티로서 소속된 함수를 가리킨다. 예를 들어 이전 예제에서 Person.prototype.constructor은 Person을 가리킨다 다음에는 생성자에 따라 각종 프로퍼티와 메서드가 프로로토 타입에 추가된다.  
커스텀 생성자를 정의하면 해당 프로토 타입은 단지 constructor프로퍼티만 가지며 다른 메서드는 Object에서 상속한다. 생성자를 호출해서 인스턴스를 생성 할 때마다 해당 인스턴스 내부에는 생성자의 프로토 타입을 가리키는 포인터가 생성된다. 이 포인터를 [[Prototype]]이라고 부른다. 스크립트에서 [[Prototype]]에 접근하는 표준은 없지만 크롬과 파이어폭스에는 __proto__ 라는 프로퍼티를 지원한다. 인스턴스와 직접연결되는것은 생성자의 프로토타입이지 생성자가 아님을 이해할 것.  
![Minion](https://github.com/jinyounghwa/i-dont-nothing-javascript/blob/master/image/proto61.png)  

위 그림은 Person 생성자와 Person프로토타이브 Person의 두 인스턴스 사이의 관계를 나타낸다. Person.prototype은 프로토타입 객체를 가리키지만 Person.prototype.constructor는 Person을 가리키고 있다. 프로토타입은 constructor프로퍼티를 가지고 기타 추가된 프로토타입도 가진다. Person의 인스턴스 person1과 person2는 Person.prototype을 가리키는 내부 프로퍼티를 가질 뿐 생성좌아 직접 연결되지는 않는다. 이 이슨턴스들에는 아무 프로퍼티나 매서드도 없는데 person1.sayName()이 동작한다. 이는 객체 프로퍼티를 검색하는 매커니즘 때문이다.  
[[Prototype]]은 구현 환경에 따라 접근 불가능할 수도 있지만, 객체 사이에 프로토타입 연결이 존재하는지는 isPrototypeOf()매서드를 통해 알 수 있다 요약하면, isPrototypeOf()은 다음과 같이 [[Prototype]]이 자신을 호출하는 프로토타입을 가리킬 때 true를 반환한다.  
<pre>
alert(Person.prototype.isPrototypeOf(person1)); //true
alert(Person.prototype.isPrototypeOf(person2)); //true
</pre>
ECMAscript 5판에서는 [[Prototype]]값을 반환하는 Object.getPrototypeof()라는 매서드가 추가되었다.  
<pre>
alert(Object.getPrototypeOf(person1) == Person.prototype); //true
alert(Object.getPrototypeOf(person1).name);// "Nicholas"
</pre>
코드의 첫줄은 단순히 Object.getPrototypeOf()에서 반환한 객체가 해당 객체의 프로토타입이 맞는지만 확인한다. 두번째 줄은 프로토타입의 name프로퍼티값인" Nicholas"를 가져온다. Object.getPrototypeOf()를 쓰면 객채의 프로토타입을 이용한 상속구현에 중요한 역할을 한다. 객체에서 프로퍼티를 읽으려 할 때마다 해당 프로퍼티 이름으로 찾으려고 검색한다. 검색은 객체 인스턴스 자체에서 시작한다. 인스턴스 프로퍼티 이름을 찾으면 그 값을 반환한다. 프로퍼티를 찾지 못하면 포인터를 프로토타입으로 올려서 검색을 계속한다. 프로퍼티를 프로토타입에서 찾으면 그 값을 반환한다. 따라서 person1.sayName()를 호출하면 두 단계가 발생한다. 첫단계에서 자바스크립트 엔진은 person1인스턴스에 sayName이라는 프로퍼티가 있는지 확인한다. 찾을 수 없으므로 person1프로토타입에 프로퍼티 sayName이 있는지 확인한다. 프로토타입에서 프로퍼티를 찾을 수 있으므로 프로토타입에 저장된 함수가 실행된다. person2.sayName()을 호출하면 똑같은 방식으로 검색하여 실행하고 같은 결과를 반환한다. 프로토타입은 이런 식으로 여러 객체 인스턴스에서 프로퍼티와 매서드를 공유한다.   

객체 인스턴스에서 프로토타입에 있는 값을 읽을 수는 있지만 수정은 불가능하다. 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 해당 프로퍼티는 인스턴스에 추가되며 프로토타입까지 올라가지는 않는다.  
<pre>
function Person() {
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function () {
  alert(this.name);
}

var person1 = new Person();
var person2 = new Person();

person1.name = "Greg";
alert(person1.name); // "Greg" -인스턴스 에서
alert(person2.name); // "Nicholas" - 프로토타입에서 
</pre>
