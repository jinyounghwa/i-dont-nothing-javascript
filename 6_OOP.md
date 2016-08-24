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
  alert(book.edition); // 2_
</pre>
이 코드에서 객체 book은 _year와 edition두 기본 프로퍼티를 가진 채로 생성된다. _year의 밑줄(_)은 이 프로퍼티는 객체의 매서드를 통해서만 접근 할 것이고 객체 외부에는 접근하지 않겠다는 의도를 나타낼 때 쓰는 표기법이다. year프로퍼티는 접근자 프로퍼티로 정의 했고 getter함수는 단순히 _year 값을 반환하며 setter함수는 올바른 판을 반환하기 위해 계산을 약간 한다. 따라서 year프로퍼티를 2005로 고치면 edition은 2가 된다. 접근자 프로퍼티는 일반적으로 이런 경우, 즉 프로퍼티의 값을 바꾸었을 때 해당 프로퍼티만 바뀌는게 아니라 부수적인 절차가 필요 할 때 사용한다. getter 와 setter는 꼭 지정해야 하는 것은 아니다. getter함수만 지정하면 해당 프로퍼티는 읽기 전용이 되고 이 프로퍼티를 수정하려는 시도는 모두 무시된다. 마찬가지로 setter만 지정된 프로퍼티만 읽으려 하면 스트릭트모드가 아닐때는 undefined를 반환하며 스트릭트 모드에서는 에러가 발생한다.  

다중 프로퍼티 정의  
객체에서는 프로퍼티 여러 개를 동시에 수정해야 할 가능성이 높으므로 ECMAscript 5판에서는 Object.definePrperty()매서드를 제공한다. 이 매서드는 서술자를 이용해 여러 프로퍼티리를 한번에 정의한다. 매개볏누는 프로퍼티를 추가하거나 수정할 객체, 그리고 프로퍼티 이름이 추가 및 수정할 프로퍼티 이름과 대응하는 두 가지 이다.  
<pre>
var book = {};
  Object.definePrperties(book, {
    _year : {
      value : 2004
    },
    edition : {
      value : 1
    },
    year:{
      get : function () {
        return this._year;
      },
      set: function(newValue) {
        if(newValue > 2004) {
          this._year = newValue;
          this.edition += newValue - 2004;
        }
      }
    }
    });
</pre>
이 코드는 book객체에 _year와 edition두 가지 데이터 프로퍼티와 접근자 프로퍼티 year를 생성한다. 결과 객체는 이전 섹션 예제에서 생성한 객체와 완전히 같다. 단 한가지 차이는 여러 프로퍼티를 동시에 생성했다는 것 뿐이다.  

프로퍼티 속성읽기  
ECMAscript 5의 Object.getOwnPropertyDescriptor()매서드를 이용해 원하는 프로퍼티의 서술자 프로퍼티를 읽을 수 있다. 이 매서즈는 읽어올 프로퍼티가 포함된 객체, 서술자를 가져올 프로퍼티 이름 두가지 매개변수를 받는다. 반환 값은 해당 프로퍼티의 성격에 따라 다른데, 접근자 프로퍼티는 configurable,Enumerable,get,set을 프로퍼티로 포함하는 객체를 반환하며 데이터 프로퍼티는 configurable,enumerable,writeable,value를 프로퍼티로 포함하는 객체를 반환한다.  
<pre>
var book = {};
Object.definePrperties(book, {
  _year : {
    value : 2003
  },
  edition : {
    value : 1
  },
  year : {
    get : function () {
      return this._year;
    },
    set : function(newValue) {
      if (newValue > 2004) {
        this._year = newValue;
        this.edition += newValue - 2004;
      }
    }
  }
  });
  var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
  alert(descriptor.value); // 2004
  alert(descriptor.configurable); //false
  alert(typeof descriptor.get) // "undefined"
  var descriptor = Object.getOwnPropertyDescriptor(book, "year");
  alert(descriptor.value); // "undefined"
  alert(descriptor.enumerable); //false
  alert(typeof descriptor.get); // "function"
</pre>

객체 생성  
Object 생성자를 이용하거나 객체 리터럴을 이용해 객체를 생성하면 객체 하나를 생성할 때는 편리하지만, 분명한 단점이 하나 있다. 같은 인터페이스를 가진 객체를 여러 개 만들 때는 중복된 코드를 많이 써야 한다는 점이다. 이 문제를 해결하기 위해 몇 가지 팩터리(factory)패턴을 사용하기 시작하였다.  

팩터리 패턴  
팩터리 패턴은 특정 객체를 생서하는 과정을 추상화 하는 것으로 특정 인터페이스 객체를 생성하는 과정을 함수로 추상화 하였다.  
<pre>
function createPerson(name, age, job) {
  var o = new Object();
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function () {
    alert(this.name);
  };
  return o;
}
var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");
</pre>
함수 createPerson()은 person객체를 만드는 데 필요한 정보를 매개변수로 받아 객체를 생성한다. 다양한 매개변수를 가지고 이 함수를 몇 변이든 호출해도 상상 프로퍼티 세 개와 매서드 한 개를 가진 객체를 반환한다. 그런데 이 팩터리 패턴을 쓰면 비슷한 객체를 여러 개 만들 때의 중복 코드는 해결 할 수 있지만 생성한 객체가 어떤 타입인지 알 수 없다는 문제는 해결디지 않는다. 자바스크립트가 진화하면서 새로운 패턴이 등장하였다.  

생성자 패턴  
ECMAscript의 생성자는 특정한 타입의 객체를 만드는 데 쓰인다. Object와 Array는 실행환경에서 자동으로 만들어 지는 네이티브 성성자이다. 반면 커스텀 생성자를 만들어서 타입의 객체에 필요한 프로퍼티와 매서드를 직접 정의 할 수도 있다.  
<pre>
function Person(name, age,job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = function() {
    alert(this.name);
  };
}
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
</pre>
이 예제에서는 Person()함수가 팩터리 함수 createPerson()의 역할을 대신한다. Person()코드는 다음 예외를 빼면 createPerson()코드와 같다는 부분을 기억해 두자  
+ 명시적으로 객체를 생성하지 않는다.
+ 프로퍼티와 메서드는 this객체에 직접적으로 할당된다.
+ return 문이 없다.  

함수 Person의 이름 첫 글자가 대문자 'P'인 점도 눈여겨 봐야 한다. 생성자 함수는 항상 대문자로 시작하고 생성자 함수가 아닌 함수는 소문자로 시작하는 표기법이 널리 쓰인다.  
Person의 새 인스턴스를 만들 때는 new연산자를 사용한다. 생성자를 이런식으로 호출하면 내부적으로는 다음과같은 과정이 이루어진다.  
<ul>
  <li>객체를 생성한다.</li>
  <li>생성자의 this값에 새 객체를 할당한다.따라서 this가 새 객체를 가리킨다.</li>
  <li>생성자 내부 코드를 실행한다.(객체에 프로퍼티를 추가한다.)</li>
  <li>새 객체를 반환한다.</li>
</ul>
앞 예제의 마지막에서 person1 과 person2 는 Person의 각각 다른 인스턴스로 채워진다. 두 객체의 constructor프로퍼티는 다음과 같이 Person을 가리킨다.  
<pre>
alert(person1.constructor == Person); //true
alert(person2.constructor == Person); //true
</pre>
constructor프로퍼티는 원래 객체의 타입을 파악하려는 의도였다 하지만 타입을 알아내는 목적으로는 instanceof연산자가 더 안전한 것으로 간주된다. 이 객체는 모두 Object의 인스턴스인 동시에 Person의 인스턴스이기도 하기 때문에 instanceof연산자는 다음과 같은 결과를 표시한다.  
<pre>
alert(person1 instanceof Object); //true
alert(person1 instanceof Person); //true
alert(person2 instanceof Object); //true
alert(person2 instanceof Person); //true
</pre>
생성자를 직접 만들면 인스턴스 타입을 쉽게 식별할 수 있는데 이는 팩터리 패턴에 비해 대단한 장점이다. 이 예제에서 person1과 person2모두 Object의 인스턴스인데 커스텀 객체는 모두 Object를 상속하기 때문이다.  

함수로서의 생성자
생성자 함수와 다른 함수의 차이는 어떻게 호출하느냐 이다. 생성자는 결국 함수일 뿐이며, 함수가 자동으로 생성자처럼 동작하게 만드는 특별한 문법 같은것은 없다. new연산자와 함께 호출한 함수는 생성자처럼 동작한다. new연산자 없이 호출한 함수는 일반적인 함수에서 예상하는 것과 똑같이 동작한다.  
<pre>
// 생성자로 사용
var person = new Person("NiCholas", 29, "Software Engineer");
person.sayName(); //"NiCholas"

// 함수로 호출
person("Greg", 27, "Doctor"); // window에 추가
window.sayName(); //"Greg"

// 다른 객체의 스코프에서 호출
var o = new Object();
Preson.call(o, "kristen", 25, "Nurse");
o.sayName() //"kristen"
</pre>
예제의 첫 번째 부분은 일반적인 생성자 파턴이고 두번째 부분은 new 연산자 없이 호출한 경우이며 프로퍼티는 매서드와 window객체에 추가된다.  

생성자의 문제점  
생성자 패러다임은 매우 유용하긴 하지만 단점도 있다. 주요 문제는 인스턴스마다 매서드가 생성된다는 점이다. 이전 예제에서 person1과 person2에는 모두 sayName()이라는 매서드가 있지만 이들 매서드는 Function 의 같은 인스턴스는 아니다. ECMAscript에서 함수는 객체이므로 함수를 정의 할 때마다 새로운 객체 인스턴스가 생성되는 것이나 마찬가지이다. 노닐적으로 생성자는 다실 다음과 같습니다.
<pre>
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = new Function("alert(this.name)"); //논리적으로 동등
}
</pre>
생성자를 이런 식으로 생각하면 Person의 각 인스턴스가 Function의 인스턴스를 가지며 이 인스턴스가 name프로퍼티를 표시한다는 점을 명확히 알 수 있다. 엄밀히 말해 함수를 이런 식으로 생성하면 스코프체인과 식별자 해석이라는 점에서는 다르지만 Function의 새 인스턴스를 만드는 방식은 똑같ㄷ.ㅏ 따라서 함수 이름이 같더라도 인스턴스가 다르면 둘은 다른 함수인데 다음코드를 보면 명확하게 알 수 있다.  
<pre>
alert(person1.sayName == person2.sayName) // false
</pre>
똑같은 일을 하는 Function인스턴스 두 개가 따로 존재한다는 점은 상식적이지 않은데, this객체를 응용하여 함수가 런타임에서야 비로소 객체에 묶이도록 할 수 있으므로 더 비효율 적이다. 다음과 같이 함수 정의를 생성자 밖으로 내보내면 이런 제한을 우회 할 수 있다.  
<pre>
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = sayName;
}
function sayName() {
  alert(this.name);
}
var person1 = new Person("NiCholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
</pre>
이 예제에서는 sayName()함수를 생성자 바깥에서 정의하였다. 생성자 내부에 서는 sayName프로퍼티에 전역 sayName()함수를 할당한다. 이제 sayName프로퍼티는 단순히 함수를 가리키는 포인터이르 뿐이므로 person1과 person2는 중복 문제를 막을 수 있지만, 일부 객체에서만 쓰이는 함수를 전역에 놓음으로서 전역스코프를 어지럽힌다는 단점이 있다. 객체에 에러 매서드가 필요하다면 전역함수를 여러 개 만들어야 하고 결국 커스텀 참조 타입 정의롤 보기좋은 코드로 묶을 수 없게 된다. 이 문제는 프로포 타입으로 해결 할 수 있다.
