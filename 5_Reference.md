##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
이 장에서 다루는 내용
객체로 작업하기  
배열 생성하고 조작하기  
자바스크립트의 데이터 타입 이해  
원시 데이터 및 래퍼로 작억하기  

참조 값(객체)이란 특정 '참조 타입'의 인스턴스 이다. ECMAscript에서 참조 타입은 데이터와 기능을 그룹으로 묶는 구조이다.
참조 타입을 '클래스'라고 부르는 사람이 잇으나 이것은 잘못된 표현이다. ECMAscript는 객체지향 언어이긴 하지만 객체 지향 프로그래밍에서 널리 쓰이는 클래스나 인스터페이스 같은 구조를 갖고 있지는 않는다. 참조 타입은 '객체 정의' 라고 불리기도 한다.  
<pre>
var person = new Object();
</pre>
이 코드는 참조타입 Object인스턴스를 생성해서 변수 person에 할당한다. 이 코드에서는 생성자는 Object()이며 기본 프로퍼티와 매서드만 가진 단순한 객체를 생성하였다.  

Object타입
Object의 인스턴스를 명시적으로 생성하는 방법은 두 가지이다. 첫번째로 다음과 같이 new연산자와 Object생성자를 함께 사용하는 방법이다.  
<pre>
var person = new Object();
person.name = "NiCholas";
person.age = 29;
</pre>
다른 방법은 '객체 리터럴 표기법'이다. 객체 리터럴 표기법은 여러가지 프로퍼티를 가진 객체를 쉽게 정의 할 수 있도록 디자인된 표기법이다. 예를 들어 다음 코드는 객체 리터럴 표기법을 써서 이전 예제와 같은 person객체를 생성한다.  
<pre>
var person = {
  name : "Nicholas",
  age : 29
};
</pre>
이 예제에서 왼쪽 중괄호({})는 객체 리터럴의 시작을 나타낸다. 왼쪽 중괄호는 여러가지 목적으로 쓰이는데도, 이 코드에서 객체 리터럴의 시작임을 분명히 알 수 있는 이유는 '표현식 컨텍스트'에 있기 때문읻. ECMAscript에서 표현식 컨텍스트란 값(표현식)이 예상되는 컨텍스트 이다. 할당연산자(=)는 다음에 값이 나올것으로 예상하므로 왼쪽 중괄호를 표현식의 시작으로 간주한다. 똑같은 중괄호라도 조건문을 시작하는 예약어 if뒤에 쓰였다면 '문장 컨텍스트'에 있는것이며 블록문장이 시작됨을 나타낸다.  
다음에는 name프로퍼티, 콜론(:) , 프로퍼티의 값 순서로 썻다. 객체리터럴에서는 쉼표를 써서 프로퍼티를 구분한다. 따라서 문자열 "Nicholas" 뒤에는 쉼표가 있지만 값 29뒤에는 쉼표가 없는데 age가 객체의 마지막 프로퍼티이기때문이다. 객체 리터럴 표기법에는 프로퍼티 이름에 문자열이나 숫자를 쓸 수 있다.  
<pre>
var person = {
  "name" : "Nicholas",
  "age" : 29,
  5 : true
};
</pre>
이 예제는 name프로퍼티, age프로퍼티, 프로퍼티 "5"를 가진 객체를 만든다. 숫자형 프로퍼티 이름은 자동으로 문자열로 바뀝니다. 객체 리터럴 표기법을 쓸 때 중괄호 안쪽을 남겨두면 생성자를 썼을때와 마찬가지로 기본 프로퍼티와 매서드만 가진 객체를 만들 수 있다.  
<pre>
var person {}; //new object()와 동일
person.name = "Nicholas";
person.age = 29;
</pre>
이 예제는 좀 이상해 보이긴 하지만 이 절의 첫 번째 예제와 똑같은 일을 한다. 객체 리터럴 표기법은 프로퍼티를 여러 개 쓸 때 가독성을 확보하는 용도로만 쓰기를 권장한다.  
객체 리터럴은 다음과 같이 함수에 옵션 매개변수를 여러 개 넘길 때 많이 사용한다.
<pre>
function displayInfo(args) {
  var output = "";
  if (typeof arg.name == "string") {
    output += "name: " + args.name + "\n";
  }
  if (typeof args.age == "number") {
    output += "Age: " + args.age + "\n";
  }
  alert(output);
}
displayInfo({
  name:"Nicholas",
  age : 29
  });
displayInfo({
  name : "Greg"
  });
</pre>
displayInfo() 함수는 arg란 이름의 매개변수 하나만 받는다. 매개변수에는 name프로퍼티나 age프로퍼티가 있을 수 있고ㅡ 둘 중 아무것도 없을 수도 있다. 이 함수는 typeof연산자를 이용해 각 프로퍼티를 테스트 한 후 그에 맞개 메시지를 생성한다. 그런다음 함수를 두번 호출했는데 그 때마다 다른 데이터를 객체 리터럴 형식으로 넘겼다. 함수를 어떻게 호출하든 정확하게 동작한다.  
위와같은 점 표기법도 유용하지만 대괄호 표기법도 사용할 수 있다.일반적으로 점 표기법을 권장한다.  
<pre>
alert(person["name"]); //Nicholas
alert(person.name); //Nicholas

// 두 표기법에는 아무런 차이가 없다. 대괄호 표기법은 다음과 같이 변수를 써서 프로퍼티에 접근 할 수 있다.

var propertyName = "name";
alert(person[propertyName]) // "Nicholas"

// 대괄호 표기법에는 문법에러를 일으키는 문자가 들어가거나 키워드 및 예약어에 해당하는 프로퍼티 이름에도 접근할 수 있다.

person["first name"] = "Nicholas";
</pre>

Array타입
배열은 두가지 타입으로 만들어 진다. 첫번째는 다음과 같이 Array생성자를 쓰는 방법이다.
<pre>
var colors = new Array();
</pre>
배열에 데이터가 몇개 들어갈 지 알고 있다면 생성자에 매개변수를 넘겨서 length프로퍼티가 자동으로 그 값에 맞게 바뀌게끔 해도 된다. 예를들어 다음 코드는 length가 20인 배열을 만든다.  
<pre>
var colors = new Array(20);
</pre>
Array생성자에 배열에 들어갈 데이터를 넘겨도 된다. 다음 코드는 문자열 값이 세 개 들어 있는 배열을 만든다.  
<pre>
var colors = new Array("red", "blue", "green");
</pre>
생성자에 값 하나만 넘겨도 배열이 만들어 지는데 이 점은 약간 혼란스럽다. 매개변수로 숫자 하나만 넘기면 그 길이의 배열이 만들어 지는데 숫자가 아닌 매개변수를 넘기면 해당 데이터 하나만 가진 배열이 생성되기 때문이다. 다음 예제를 확인하자.  
<pre>
var colors = new Array(3); // 데이터가 세 개 있는 배열을 만든다.
var names = new Array("Greg") // 문자열 "Greg"만 있는 배열을 만단다.
</pre>
Array생성자를 쓸 때는 new연산자를 생략해도 된다. 결과는 같다. 두번째 방법은 '배열 리터럴' 표기법이다. 배열리터럴은 다음과 같이 대괄호 안에 데이터를 쉼표로 구분해 쓰는 방법이다.  

<pre>
var colors = ["red", "blue", "green"]; //문자열이 세 개 있는 배열을 만든다.
var names = []; // 빈 배열을 만든다.
var values = [1,2, ]; //데이터가 두개 또는 세개 들어있는 배열을 만든다. 이렇게는 하지말 것
var options = [,,,,,,] //데이터가 5개 또는 6개 있는 배열을 만든다. 이렇게 하지말 것
</pre>
배열 값에 접근하려면 다음과 같이 대괄호 안에 있으므로 0으로 시작하는 인덱스를 넣으면 된다.  
length프로퍼티는 배열을 추가할 때 사용할 수도 있다.
<pre>
var colors = ["red", "blue", "green"];
colors[colors.length] = "black" // 인덱스 3에 데이터 추가
colors[colors.length] = "brown" // 인덱스 4에 데이터 추가
</pre>

배열감지  
배열 감지는 instanceof 연산자를 사용한다.
<pre>
if (value instanceof Array) {
  //배열일 때 실행하는 코드
}
</pre>
instanceof문제는 실행컨텍스트가 하나뿐이라고 가정하는것이고 배열을 한 프레임에서 다른 프로엠으로 전달했다면 전달한 배열은 전달받은 프레임에서 직접 생성한 배열과는 다른 생성자 함수를 가지기 깨문에 문제가 발생할 수 있다. ECMAscript 5 에서는 이 문제를 우회하기 위해 Array.isArray()매서드를 제공한다.  
<pre>
if (Array.isArray(value) ) {
  //배열일 때 실행하는 코드 22장에서 추가 설명한다.
}
</pre>

변환 매서드
객체에는 모두 toLocaleString(), toString(), valueof() 매서드가 있다. 배열에서 호출했을 때는 toString()과 valueof()매서드는 같은 값을 반환한다. 두 매서드가 반환하는 값은 쉼표로 분리된 문자열인데, 각 문자열은 배열의 해당 슬롯과 동등한 문자열, 즉 각 슬롯에서 toString()매서드를 호출한 결과와 같다.  
<pre>
var colors = ["red", "blue", "green"];
alert(colors.toString()); //red,blue,green
alert(colors.valueof()); // red,blue,green
alert(colors)//red,blue,green
</pre>
위 코드에서 toString()과 valueof()매서드는 명시적으로 배열의 각 슬롯을 쉼표로 구분한 문자열을 반환한다. 마지막 줄은 배열을 alert()에 직접 넘긴것이다. alert()는 매개변수를 문자열로 받으므로 toString()을 호출한것과 같은 결과이다.  
toLocaleString()매서드는 toString()이나 valueof()와 같은 값을 반환할 때도 있고 그렇지 않을때도 있다. 배열에서 호출할 때 각값에서 toString()이 아니라 toLocaleString()을 호출해서 문자열 값을 얻는다는 점이 다르다.(당연한 말인것 같은데 뭔소린지..)  
<pre>
var person1 = {
  toLocaleString : function () {
    return "Nicholas";
  },
  toString : function () {
    return "Nicholas";
  }
};
var person2 = {
  toLocaleString : function () {
    return "Grigorious";
  },
  toString : function () {
    return "Greg";
  }
};

var people = [person1, person2];
alert(people); // Nicholas, Greg
alert(people.toString()); // Nicholas, Greg
alert(people.toLocaleString()); // Nicholas, Grigorious
</pre>
위 예제에서는 person1과 person2 두 객체를 정의 했다. 각 객체에서 toString()매서드와 toLocaleString()매서드가 서로 다른 값을 반환하도록 정의했다 다음에는 people배열을 만들어서 두 객체를 담았다. people배열을 alert()에 넘기면 배열의 각 값에서 toString()매서드를 호출하므로  "Nicholas,Greg" 가 표시된다. 이는 다음줄에서 toString()을 명시적으로 호출 것과 같은 결과이다. join()매서드를 사용하면 다른구분자를 써서 배열을 문자열로 나타낼 수 있다.  
<pre>
var colors = ["red", "blue", "green"];
alert(colors.join(",")); // red,blue,green
alert(colors.join(||)); // red||blue||green
</pre>

스택 매서드  
배열이 다른 데이터 구조인 거처럼 동작하게 하는 매서드가 있다는 점이다. 데이터를 삭제할 때는 마지막에 추가된 데이터가 제일먼제 삭제된다. pop()와 push()매서드를 사용하여 마치 스택처름 동작 할 수 있다. push()매서드는 매개 변수 숫자에는 제한이 없으며 받은 매개변수를 그대로 배열에 추가한 후 이에 맞게 늘어난 배열 길이를 반환한다. pop()매서드는 이와 반대로 배열의 마지막 데이터를 제거하고 length프로퍼티를 그에 맞게 줄여서 반환한다.
<pre>
var colors = new Array(); // 배열생성
var count = colors.push("red","green"); // 데이터 2개 추가
alert(count); //2

count = colors.push("black"); // 다른데이터 추가
alert(count); //3

var item = color.pop(); //마지막 데이터 꺼냄
alert(item); //"black"
alert(colors.length); //2
</pre>
스택 매서드는 다음과 같이 다른 매서드와 함께 사용할 수 있다.   
<pre>
var colors = ["red", "blue"];
colors.push("brown"); //데이터 추가
colors[3] = "black"; //다른 데이터 추가
alert(colors.length);

var item = colors.pop(); // 마지막 데이터 꺼냄
alet(item); // "black"
</pre>

큐 매서드  
shift()와 push()매서드를 사용하면 배열이 큐처럼 동작하게 할 수 있다.  
<pre>
var colors = new Array();
var count = colors.push("red", "green");
alert(count);

count = colors.push("black");
alert(count);

var item = colors.shft(); //첫 번째 데이터 꺼냄
alert(item); //"red"
alert(colors.length) //2
</pre>

unshift()매서드도 있는데 이것은 shift()매서드와 반대로 동작한다. unshift()를 pop()매서드와 조합하면 큐의반대 즉 다음과 같이 배열 마지막에 데이터를 추가하고 앞에서 꺼내는 방식으로 사용 할 수 있다.   
<pre>
var colors = new Array();
var count = colors.unshift("red", "green"); // 데이터 2개 추가

count = colors.unshift("black"); // 다른 데이터 추가
alet(count) // 3

var item = colors.pop(); //첫번째 데이터 꺼냄
alert(item); //"green"
alert(colors.length); //2
</pre>

정렬 메서드  
정렬메서드는 sort()와 reverse()매서드를 사용한다. reverse()함수는 단순이 데이터 순서를 뒤집는 역할을 하고, sort()는 기본적으로 작은값이 첫 번째에 오고 가장 큰 값이 마지막에 오도록 정렬한다. 이면에서 데이터를 문자열로 변환한 후 이를 순서로 판단한다.이는 좋지 않은 방법인데 숫자로된 배열을 호출할 경우 다음과 같은 결과를 내기 때문이다.   
<pre>
var value = [1,2,3,4,5];
value.reverse();
alert(value); // 5,4,3,2,1

var values = [0,1,5,10,15];
values.sort();
alert(values); // 0,1,10,15,5
</pre>
이를 해결하기 위한 비교 함수를 작성해 보자  
<pre>
function compare(value1, value2) {
  if (value1 < value2) {
    return -1;
  }else if (value1 > value2) {
    return 1;
  }else {
    return 0;
  }
}
</pre>
이 비교함수는 대부분의 데이터 타입에서 동작하며 다음과 같이 sort() 매서드의 매개 변수로 사용 할 수 있다.  
<pre>
var values = [0,1,5,10,15];
values.sort(compare);
alert(values); // 0,1,4,10,15
</pre>
비교 함수를 sort()매서드에 매개함수로 넘기면 올바른 순서가 유지된다. 비교함수에 반환 값을 다음과 같이 바꾸면 역순으로 정렬 할 수 있다.  
<pre>
function compare(value1, value2) {
  if (value1 < value2) {
    return 1;
  }else if (value1 > value2) {
    return -1;
  }else {
    return 0;
  }
}
var values = [0,1,5,10,15];
values.sort(compare);
alert(values); // 15,10,5,1,0
</pre>

조장 매서드 
