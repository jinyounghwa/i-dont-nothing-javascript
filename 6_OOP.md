##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
+ 이 장에서 다루는 내용
+ 객체 프로퍼티의 이해
+ 객체의 이해와 생성
+ 상속의 이해  

객체지향 언어는 일반적으로 클래스를 통해 같은 프로퍼티와 매서드를 가지는 객체를 여러 개 만든다는 특징이 있다. ECMAscript 5까지는 클래스라는 개념이 없으며 이에 따라 ECMAscript객체는 다른 클래스 기반 언어와 다르다.  
ECMA-262는 객체를 "프로퍼티의 순서 없는 컬렉션이며 각 프로퍼티는 원시 값이나 객체, 함수르르 포함한다." 라고 정의한다. 엄밀히 말하면 이 표현은 객체가 특별한 순서가 없는 값의 배열이라는 의미이다. 각 프로퍼티와 매서드는 이름으로 구별하며 값에 대응한다. 이런이유로 ECMAscript객체를 해시 테이블이라고 생각하면 이해하기 쉽다. 다시말해 객체는 이름-값 쌍의 그룹이며 각 값은 데이터나 함수가 될 수 있다.   
모든 객체는 참조타입을 바탕으로 생성되는데 바탕이 되는 타입은 네이티브 타입 일 수 있고, 개발자가 정의한 타입일 수도있다.  

객체에 대한 이해  
이전 장에서 설명했듯 객체를 만드는 가장 단순한 방법은 다음과 같이 Object의 인스턴스를 만들고 여기에 프로퍼티와 매서드를 추가하는 방법이다.  
<pre>
var person = new Object();
person.name = "NiCholas";
person.age = 29;
person.job = "Software Engineer";

person.sayName = function() {
  alert(this.name);
}
</pre>
위의 코드는 person이라는 객체를 만드는데, person객체에는 name,age,job 세가지 프로퍼티가 있고, sayName()이란 매서드가 있다. sayName()매서드는 this.name즉 person.name값을 표시한다. 초기 자바스크립트 개발자들은 객체를 생성할 때 이런 패턴을 자주 사용하였다. 몇년이 지나면서 객체를 만들 때는 객체 리터럴 패턴이 더 많이 쓰인다. 이전 코드는 객체 리터럴 표기법을 이용해서 아래와 같이 변경 할 수 있다.  
<pre>
var person = {
  name : "Nicholas",
  age : 29,
  jon : "Software Engineer",

  sayName : function() {
    alert(this.name);
  }
};
</pre>
이 예제의 person객체는 이전 예제의 person객체와 동일하며 같은 프로퍼티 및 매서드를 가진다. 이러한 프로퍼티는 모두 자바스크립트에서 프로퍼티의 행동을 정의하는 특징에 따라 생성된다.  

프로퍼티 타입  
프로퍼티 타입에는 데이터 프로퍼티와 접근자 프로퍼티 두가지 타입이 있다.  

데이터 프로퍼티  
데이터 프로퍼티는 데이터 값에 대한 단 하나의 위치를 포함하여 이 위치에서 값을 읽고 쓴다. 데이터 프로퍼티는 그 행동을 설명하는 네가지 속성이 있다.  
+ [[Configurable]] -  해당 프로퍼티가 delete를 통해 삭제하거나, 프로퍼티의 속성을 바꾸거나, 접근자 프로퍼티로 변환 할 수 있음을 나타낸다. 이전 예제처럼 객체에서 직접 정의한 모든 프로퍼티에서 이 속성은 기본으로 true이다.  
+ [[Enumerable]] - for - in 루프에서 해당 프로퍼티를 반환함을 나타낸다. 이전 예제처럼 객체에서 직접 정의한 모든 프로퍼티에서 이 속성은 기본적으로 true이다.  
+ [[Writeable]] - 프로퍼티의 값을 바꿀 수 있음을 나타낸다. 이전 예제처럼 객체에서 정의한 모든 프로퍼티에서 이 속성은 기본적으로 true이다.
+ [[Value]] - 프로퍼티의 실제 데이터 값을 포함한다. 프로퍼티의 값을 읽는 위치이며 새로운 값을 쓰는 위치이다. 이 속서으이 기본값은 undefined이다.  

<pre>
var person = {
  name : "Nicholas"
};
</pre>
위 코드에서는 name이라는 프로퍼티에 값 "Nicholas"를 할당하였다 즉 [[Value]]속성이 "Nicholas"로 지정되어 값이 바뀐다면 모두 이 위체에 저장된다는 뜻이다. 기본프로퍼티 속성을 바꾸려면 반드시 ECMAscript 5판의 Object.definePrperty()매서드를 사용해야 한다. 이 매서드는 프로퍼티를 추가하거나 수정할 객체, 프로퍼티 이름, 서술자 객체 세 가지를 매개변수로 받는다. 서술자 객체의 프로퍼티는 내부 속성 이름과 1:1로 대응 할 수 있다. 즉 Configurable,Enumerable,Writeable,Value 네 가지 프로퍼티가 있다 이들 값의 일부 또는 전부를 바꾸어서 대응하는 속성 값을 바꿀 수 있다.   
<pre>
var person = {};
Object.definePrperty(person, "name", {
  writeable : false,
  value:"Nicholas"
  });
alert(person.name) // "Nicholas"
person.name = "Greg";
alert(person.name) // "Nicholas"
</pre>
이 예제는 name이라는 읽기 전용 프로퍼티를 만들고 그 값을 "Nicholas"로 지정한다. 이 프로퍼티의 값은 바꿀 수 없으며 새로운 값을 할당하려 시도하면 스트릭트모드가 아닐 때는 무시되고 스트릭트 모드에서는 에러가 발생한다.  
[[Configurable]]속성이 false인 프로퍼티에서도 비슷한 규칙이 적용된다. 다음 코드를 보자.  
<pre>
var person = {};
Object.definePrperty(person, "name", {
  configurable : false,
  value:"Nicholas"
  });
  alert(person.name); // "Nicholas"
  delete person.name;
  alert(person.name); // "Nicholas"
</pre>
Configurable을 false로 설정하면 해당 프로퍼티를 객체에서 제거할 수 없게 된다. 해당 프로퍼티에서 delete를 호출하면 스트릭트 모드가 아닐때는 아무 효과도 없고 스트릭트 모드에는 에러가 발생한다. 또한 프로퍼티의 [[Configurable]]을 일단 false로 설정하면 다시는 수정할 수 없게 된다. definePrperty()를 호출해서 writeable외의 다른 속성을 수정하려 하면 에러가 발생한다.  
<pre>
var person = {};
Object.definePrperty(person, "name", {
  configurable : false,
  value : "Nicholas"
  });
  // 에러가 발생
  Object.definePrperty(person, "name", {
    configurable : true,
    value:"Nicholas"
    });
</pre>
즉 같은 프로퍼티에서 Object.definePrperty()를 여러 번 호출 할 수는 있지만, 일단 configurable을 false로 지정하면 제한이 생긴다. Object.definePrperty()를 호출할 때 configurable, writeable의 값을 명시하지 않는다면 기본값은 false이다. 대개는 사용할 일이 드믈겠지만, 객체를 잘 이해하려면 이 개념에 대해 알아야 한다.  

접근자 프로퍼티  
접근자 프로퍼티에는 데이터 값이 들어 있지 않고 대신 getter함수와 setter함수로 구성된다. (둘 모두 꼭 필요한것은 아니다.)접근자 프로퍼티를 읽을 때는 getter함수가 호출되며 유효한 값을 반환할 책임은 이 함수에 있다. 접근자 프로퍼티에 쓰기 작업을 할 때는 새로운 값과 함께 함수를 호출하며 이 함수가 데이터를 어떻게 사용할 지 결정한다 접근자 프로퍼티에는 네가지 속성이 있다.  
+ [[Configurable]] - 해당 프로퍼티가 delete를 통해 삭제하거나 프로퍼티의 속성을 바꿀 수 있거나, 데이터의 프로퍼티로 바꿀 수 있음을 나타낸다. 객체에서 직접 정의한 모든 프로퍼티에서 이 속성은 기본으로 true이다.
+ [[Enumerable]] - for - in 루프에서 해당 프로퍼티를 반환함을 나타낸다. 객체에서 직접 정의한 모든 프로퍼티에서 이 속성은 기본적으로 true이다.
+ [[Get]] - 프로퍼티를 읽을 때 호출할 함수다. 기본값은 undefined이다.
+ [[Set]] - 프로퍼티를 바꿀 때 호출할 함수다. 기본값은 undefined이다.  
접근자 프로퍼티를 명시적으로 정의 할 수는 없으며 반드시 Object.definePrperty()를 써야 한다.  
<pre>
var book = {
  _year:2004,
  edition:1
};
Object.definePrperty(book, "year", {
  get : function() {
    return this._year;
  },
  set : function(newValue){
    if(newValue > 2004){
      this._year = newValue;
      this.edition += newValue - 2004
    }
  }
  });
  book.year = 2005;
  alert(book.edition); // 2
</pre>
