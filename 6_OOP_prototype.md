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
