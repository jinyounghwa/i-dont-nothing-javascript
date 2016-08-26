##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
상속  
상속은 객체지향 프로그래밍과 관련하여 자주 설명하는 개념이다 대부분의 객체지향 언어는 시그너처만을 상속하는 인터페이스 상속과 실제 메서드를 상속하는 구현상속 두가지 타입을 지원하지만 ECMAScriot 에는 시그니처가 없으므로 인터페이스 상속은 불가능하며 구현상속만 프로토타입 체인을 통해 지원한다.  

프로토타입 체인  
ECMA-262 는 프로토타입 체인을 ECMAScriot의 우선적 상속방법으로 설명한다. 프로토타입 체인의 기본 아이디어는 프로토타입개념을 이용해 두 가지 참조타입 사이에서 프로퍼티와 매서드를 상속한다는 것이다. 모든 생성자에는 생성자 자신을 가리키는 프로토타입 객체가 있으며 인스턴스는 프로토타입을 가리키는 내부 포인터가 있다. 그렇다면 프로토타입이 사실 다른 타입의 인스턴스라면 어쩔텐가? 이런 경우 프로토타입(A) 자체에 다른 프로토타입(B)를 가리키는 포인터가 있을 것이며 B에는 또다른 생성자를 가리키는 포인터가 있을 것이다. 프로토타입 B역시 다른 타입의 인스턴스라면 이런 패턴이 계속 연결되어 인스턴스와 프로토타입을 잇는 체인이 형성될 것이다. 프로토타입은 바로 이런 개념이다.  
<pre>
function SuperType () {
  this.property = true;
}
SuperType.prototype.getSuperValue = function () {
  return this.property;
};
function SubType() {
  this.subpropety = false;
}
// SuperType을 상속
SubType.prototype = new SuperType();
SubType.prototype.getSubValue = function () {
  return this.subpropety;
};
var instance = new SubType();
alert(instance.getSuperValue()); // true
</pre>
이 코드는 
