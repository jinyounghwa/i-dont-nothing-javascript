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
+ reverse() 와 sort() 는 모두 자신을 호출한 배열에 대한 참조를 반환한다.(array.sort(compare).reverse()처럼 사용할 수 있다.)

조작 매서드
배열에 들어 있는 데이터를 조작하는 매서드도 다양하다. 예를 들어 concat() 매서드는 현재 배열 데이터를 기반으로 새로운 배열을 만듭니다. 이 매서드는 먼저 현재 배열을 복사한 다음 매서드의 매개변수를 새 배열 마지막에 추가하여 반환한다. 매개변수를 넘기지 않으면 단순히 현재 배열의 복사본을 반환한다. concat() 매서드에 매개변수로 배열을 넘기면 새 배열의 데이터를 모두 추가해서 반환한다. 매개변수가 배열이 아니면 단순히 해당 데이터를 새 배열 마지막에 추가한다. 다음 예제를 보자.  
<pre>
var colors = ["red","green","blue"];
var colors2 = colors.concat("yellow",["black","brown"]);

alert(colors); //red,green,blue
alert(colors2);//red,green,blue,yellow,black,brown
</pre>
이 예제에서 color배열은 값 세 개 로시작한다. color배열에서 concat()매서드를 호출하며 문자열 "yellow"와 "black","brown"이 들어있는 배열을 매개 변수로 념겼다.colors2에 저장된 결과는 "red","green","blue","yellow","black","brown" 이다. 원래배열 colors 는 그대로 남아 있다.  

slice()매서드는 배열에 포함된 데이터 일부를 새 배열로 만든다. slice()매서드는 매개변수를 두개 받는데, 각 매개변수는 원래 데이터에서 가져올 데이터 범위의 시작과 끝을 나타낸다.  
<pre>
var colors = ["red", "green","blue","yellow","purple"];
var colors2 = colors.slice(1);
var colors3 = colors.slice(1,4);

alert(color2); // green,blue,yellow,purple
alert(color3); // green,blue,yellow
</pre>
slice()를 호출하여 매개 변수를 넘기면 앞에 있는 1개의 배열을 제외한 나머지를 가져온다. 마찬가지로 두번째 alert에서는 첫번째와 네번째를 제외한 나머지를 가져왔다. 매개변수를 음수로 넘기면 배열길이 해당값을 더한숫자를 대신 사용한다. 5개의 배열길이에서 slice(-2,-1)은 slice(5-2,5-1)으로 인식하며 결과적으로 slice(3,4)를 호출한 결과와 같다.  

splice()매서드
+ 삭제 - splice매서드를 이용해 숫자 배열 데이터를 원하는 만큼 삭제 할 수 있다. 삭제할 데이터는 첫 번째 매개변수(인덱스)부터 두 번째 매개변수(개수)만큼이다. 예를 들어 splice(0,2)는 처음 두 개를 삭제한다.  
+ 삽입 - 매개변수를 세 개 이상 넘기면 데이터를 삽입 할 수 있다. 매개변수를 삽입할 위치에 삽입할 데이터 순서로 넘기면 된다. 예를 들어 splice(2,0,"red", "green")은 배열 매서드 2에 문자열 "red"와 "green"을 삽입한다.  
+ 대체 - 삽입과 삭제를 조합하면 운하는 데이터를 다른 데이터로 대체 할 수 있다. 삽입할 데이터와 숫자가 정확하게 일치하지 않아도 된다.   
 
<pre>
var colors = ["red", "green", "blue"];
var removed = colors.splice(0,1) // 첫 번째 데이터 제거
alert(colors); // green,blue
alert(removed); // red 하나만 남은 배열
removed = colors.splice(1,0,"yellow","orange"); // 인덱스 1에 데이터 2개추가
alert(colors); // green,yellow,orange,blue
alert(removed); // 빈 배열
removed = colors.splice(1,1,"red","purple"); // 데이터 2개블 추가하고 1개를 제거
alert(colors); // green.red,purple,orange,blue
alert(removed); //yellow하나만 남은 배열
</pre>

위치 매서드  
ECMAscript 5에서는 배열에 indexOf()와 lastIndexOf()두 위치 매서드가 추가되었다. 이들 매서드는 매개변수를 두 개 받는데 첫 번째 매개변수는 검색할 데이터이며 옵션인 두 번째 매개변수는검색을 시작할 인덱이다. 두 매서드는 모두 찾아낸 데이터의 인덱스를 반환하는데 데이터를 찾지 못할 때에는 -1을 반환한다. 데이터를 검색할 때에는 타입까지 일치하는 일치연산자(===)를 사용한다.  
<pre>
var numbers = [1,2,3,4,5,4,3,2,1];

alert(numbers.indexOf(4)); //3
alert(numbers.lastIndexOf(4)); //5

alert(numbers.indexOf(4,4)); //5
alert(number.lastIndexOf(4,4)); //3

var person = {name : "NiCholas"};
var people = [{name : "NiCholas"}];
var morePeople = [person];

alert(people.indexOf(person)); // -1
alert(morePeople.indexOf(person)); //0
</pre>

반복 매서드  
ECMAscript 5에는 배열에 다섯가지 반복매서드를 추가했다. 이들 매서드는 모두 매개변수를 두 개 받는데 하나는 각 데이터에서 실행할 함수이며 옵션인 다른하나는 함수를 실행할 스코프 객체이다. 스코브는 this값에 영향을 미친다. 콜백함수는 모두 데이터, 데이터의 인덱스, 배열 자체의세 가지 매개변수를 받는다. 콜백함수를 실행했을 때 매서드의 반환값에 어떤 영향을 미치는지는 매서드에 따라 다르다.  
+ every() - 배열의 모든 데이터에서 콜백함수를 호출하고 반환값이 전부 true이면 true를 반환한다.
+ filter() - 배열의 모든 데이터에서 콜백함수를 호출하고, 반환값이 true인 데이터를 새 배열에 저장하여 반환한다.
+ forEach() - 배열의 모든 데이터에서 콜백함수를 호출한다. 이 매서드는 반환값이 없다.
+ map() - 배열의 모든 데이터에서 콜백 함수를 호출하고 그 결과르 ㄹ새 배열에 저장하여 반환한다.
+ some() - 배열의 모든 데이터에서 콜백함수를 호출하고 반환 값 중 하나라도 true이면 true를 반환한다.  

이들 매서드는 원래 배열에 들어있던 데이터를 바꾸지는 않는다. every()매서드와 some()매서드는 원래 배열의 데이터를 특정 조건에 따라 쿼리한다는 점이 비슷하다. every()매서드가 true를 반환하려면 콜백함수가 배열의 모든 데이터에서 true를 반환해야 한다. 그렇지 않다면 false를 반혼한다. some()매서드는 반대로 콜백함수가 어떤 데이터에서든 true를 반화면 true를 반환한다.  
<pre>
var numbers = [1,2,3,4,5,4,3,2,1];
var everyResult = numbers.every(function(item, index, array){
  return (item > 2);});
alert(everyResult); // false

var SomeResult = numbers.some(function(item, index, array){
  return (item >2);
  })
alert(SomeResult);
</pre>
이 코드는 every()와 some()을 호출하면서 데이터가 2를 초과하면 true를 반환하는 콜백함수를 넘긴다. 데이터 중 2를 초과하는 것은 일부에 불과하므로 every()는 false를 반환한다. 반면 some()은 해당 true를 반환한다.  
filter()매서드는 콜백 함수의 결과에 따라 해당 데이터를 반환 배열에 포함할지 결정한다. 예를들어 2보다 큰 숫자만 반환배열에 담으려면 다음과같은 코드를 쓰면 된다.  
<pre>
var numbers = [1,2,3,4,5,4,3,2,1];
var filterResult = numbers.filter(function(item,index,array){
  return (item > 2);
  })
alert(filterResult); [3,4,5,4,3]
</pre>
콜백 함수가 3보다 큰 데이터에서만 true를 반환하므로 filter()매서드는 3,4,5,4,3이 포함된 배열을 반환한다. 이 매서드는 배열에서 특정 조건에 맞는 데이터만 쿼리하려 할 때 매우 유용하다. map()매서드 역시 배열을 반환한다. 결과 배열의 각 데이터는 원래 배열에서 해당 위치에 있던 데이터를 콜백 함수에 넘긴 결과이다. 예를들어 다음과 같이 배열의 모든 데이터에 2를 곱한 결과를 배열에 담아 반환 할 수 있다.  
<pre>
var numbers = [1,2,3,4,5,4,3,2,1];
var mapResult = numbers.map(function(item, index, array){
  return item * 2;
  })
alert(mapResult);
</pre>  
이 매서드는 원래 배열과 1:1로 대응하는 배열을 만들 때 유용하다.  
forEach()는 단순히 배열의 각 데이터에서 콜백함수를 실행한다. forEach()매서드에는 반환 값이 없으며 기본적으로 해당 배열엣 for문을 실행한 것과 마찬가지 이다.  
<pre>
var numbers = [1,2,3.4,5,4,3,2,1];
numbers.forEach(function(item, index, array){
  //코드
  });
</pre>
이들 매서드는 다양한 동작을 일정 범주로 단순화해서 배열을 쉽게 조작하도록 돕는다.  

감소 매서드  
ECMAscript 5에서는 배열의 감소 매서드 reduce()와 reduceRight()도 도입되었다. 두 매서드는 모두 배열을 순회하며 콜백 함수를 실행하고 값을 하나 만들어 반환한다. reduce()매서드는 이 작업을 배열의 첫 번째 데이터에서 시작하여 마지막까지 진행하고, reduceRight()는 반다래 배열의 마지막 데이터에서 시작하여 첫 번째까지 진행한다.  
두 매서드는 모두 매개변수를 두개 받는다. 첫 번째 매개변수는 각 데이터에서 실행할 콜백 함수고 옵션인 두 번째 매개변수는 감소 작업을 시작할 초기 값이다. reduce()와 reduceRight()의 콜백 함수가 넘겨받는 매개변수는 이전 값, 현재 값, 현재 값의 인덱스, 현재 배열 네 가지이다. 콜백 함수가 반환하는 값은 자동으로 다음 데이터에서 실행하는 콜백 함수의 첫 번째 매개변수가 된다. 콜백 함수를 호출하는 데이터는 인덱스 1 이므로 첫 번째 배열 데이터에서 첫 번째 매개변수는 인덱스 0 의 데이터이고 두 번째 매개변수는 인덱스 1의 데이터 이다. reduce()매서드는 배열의 숫자를 모두 더하는 것 같은 작업에 쓸 수 있다.  
<pre>
var vales = [1,2,3.4.5];
var sum = values.reduce(function(prev, cur, index, array){
  return prev + cur;
  });
alert(sum); //15
</pre>
콜백 함수를 실행 할 때 prev는 1이고 cur은 3 이다. 콜백 함수를 두 번째 실행할 때 prev는 3(1+2)이고 cur(인덱스2)는 3이다. 이런식으로 배열의 모든 데이터를 순회한 다음 최종결과를 반환한다. reduceRight()도 같은 방식으로 동작한다.  
<pre>
var values = [1,2,3,4,5];
var sum = values.reduceRight(function(prev, cur, index, array){
  return prev + cur;
  });
alert(sum); // 15
</pre>

Date타입
ECMAscript 의 date타입은 자바 초기버전의 java.util.Date에 기반한다. 따라서 Date타입은 날짜와 시간을 저장할 때 1970년 1월 1일 자정부터 몇 밀리초가 지났는지 나타내는 숫자를 사용한다.  
<pre>
var now = new Date();
</pre>
Date생성자에 매개변수를 넘기지 않으면 생성된 객체에는 현재 날짜와 시간이 저장된다. 특정 날짜와 시간을 저장하려면 매서드 Date.parse()와 Date.UTC()가 있다. 브라우저가 날짜 형식을 지원하는 경우는 다음과 같다.  
+ 월/일/년
+ 월이름, 일, 년
+ 요일 월이름 일 년 시:분:초 타임존
+ ISO 8601 확장형식 2001-01-52T00:00:00 이 형식은 ECMAscript 5호환 브라우저에서만 동작한다.
Data.parse()에 넘긴 문자열이 올바른 날짜 형식이 아닐 때는 NaN을 반환한다.  

RegExp타입  
ECMAscript는 RegExp타입을 통해 정규 표현식을 지원한다. 정규 표현식은 다음과 같이 펄과 비슷한 문법을 써서 쉽게 생성 할 수 있다.  
<pre>
var expression = /pattern/flags;
</pre>
패턴(pattern)부분에 정규 표현식을 나타내는 식을 쓰는 이 식에는 문자 클래스, 수량자, 그룹, 룩어헤드, 역참조 등이 포함된다. 각 정규 표현식은 플래그(flags)를 통해 해당 정규 표현식이 어떻게 동작 할 지 나타낸다 플래그에는 세가지가 있다.
