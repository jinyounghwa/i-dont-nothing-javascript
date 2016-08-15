##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
단항 연산자
단 하나의 값에만 적용되는 연산자를 단항 연산자 라고 부른다

증감 연산자 : ++ 을 피연산자 앞에 쓰면 피연산자에 1을 더한다.
<pre>
var age = 29;
++age;

// 아래 코드도 같은 일을 한다
var age = 29;
age = age +1;
// -1
var age = 29;--
--age;
</pre>
문장은 왼쪽에서 오른쪽으로 평가한다.
<pre>
var age = 29;
var anotherAge = --age + 2;
//변수 anotherAge는 age 에서 1을 뺀 다음 2를 더한 값으로 초기화 된다.
감소연산자를 먼저 적용하므로 age가 28로 바뀌고 여기에 2를 더해 30이 된다.
</pre>
<pre>
var num1 = 2;
var num2 = 20;
var num3 = --num1 + num2; // 21
var num4 = num1 + num2; // 21
// 여기서는 num1 에 num2를 더하기 전에 num1에서 1을 뺏으므로 21이다 num4를 계산하는 과정에서
num1은 이미 1로 바뀌었으므로 num4에는 21이 저장된다.
</pre>
증감 연산자의 규칙
+ 유효한 숫자 형태의 문자열에 적용하면 해당 문자열을 숫자로 바꾼 후 증감한다. 해당 변수는 문자열에서 숫자로 바뀐다.
+ 유효한 숫자 형태의 문자열이 아니라면 변수의 값에 NaN이 저장된다. 해당 변수는 문자열에서 숫자로 바뀐다.
+ 값이 false인 불리언에 적용하면 해당값을 0으로 바꾼 후 증감한다. 해당변수는 불리언에서 숫자로 바꾼다.
+ 값이 true인 블리언에 적용하면 해당 값을 1로 바꾼 후 증감한다. 해당 변수는 블리언에서 숫자로 바꾼다.
+ 부동소수점 숫자에 적용하면 그대로 증감한다.
+ 객체에 적용하면 해당 객체의 valueOf()매서드를 먼저 호출해서 증감을 적용할 값을 얻는다. 결과가 NaN이라면 toString()매서드를 호출 한 후 다른 규칙을 다시 적용한다. 해당변수는 객체에서 숫자로 바뀐다.  

<pre>
var s1 = "2";
var s2 = "z";
var b = false;
var f = 1.1;
var o = {
  valueOf : function() {
    return -1;
  }
};
s1++; // 2
s2++; //NaN
b++; // 1.1
f--; //-1
</pre>
단항 플러스와 단항 마이너스
단항 플러스와 단항 마이너스 연산자는 친숙한 연산자이며 ECMAscript에서도 동작한다.
<pre>
var num = 25;
num = +num // 여전히 25
</pre>
단항 플러스를 숫자가 아닌 값에 적용하면 형 변환 함수인 Number()와 마찬가지로 동작한다. 블리언 값false 와
true는 각각 0과 1로 바뀌며 문자열은 이전에 설명한 규칙대로 파싱된다. 객체에서는 valueOf() 나 toString()매서드를 호출하고
반환값에 적용된다.
<pre>
var s1 = "01";
var s2 = "1.1";
var s3 = "z";
var b = false;
var f = 1.1;
var o = {
  valueOf : function() {
    return -1;
  }
};
s1 = +s1; // 1
s2 = +s2;// 1.1
s3 = +s3; // NaN
b = +b; // 0
f = +f; // 1.1
o = +o; //-1
</pre>
숫자형 값에 단항 마이너스를 적용하면 위예제와 같이 부호를 바꾼다.
숫자가 아닌 값에 단항 마이너스를 적용하면 다음과 같이 단항 플러스와 같은 규칙을 적용하고 그 결과의 부호를 바꾼다.
<pre>
var s1 = "01";
var s2 = "1.1";
var s3 = "z";
var b = false;
var f = 1.1;
var o = {
  valueOf : function() {
    return -1;
  }
};
s1 = -s1; //1
s2 = -s2; // 1.1
s3 = -s3; // NaN
b = -b; // 0
f = -f; // 1.1
o = -o; // -1
</pre>

논리 NOT
논리 NOT연산자는 느낌표(!) 로 표시하며 ECMAscript의 모든 값에 적용될 수 있다. 논리 NOT연산자는 데이터 값에 관계없이
항상 블리언 값을 반환한다. 논리 NOT연산자는 먼저 피 연산자를 블리언 값으로 변환한 다음 그 결과를 부정하며 다음과 같이 동작한다.
+ 피연산자가 객체이면 false를 반환한다.
+ 피연산자가 빈 문자열이면 true를 반환한다.
+ 피연산자가 비어있지 않은 문자열이면 false를 반환한다.
+ 피연산자가 숫자0이면 true를 반환한다.
+ 피연산자가 0이 아닌 숫자(infinity포함)라면 false를 반환한다.
+ 피연산자가 null 이면 true를 반환한다.
+ 피연산자가 NaN이면 true를 반환한다.
+ 피연산자가 undefined이면 true를 반환한다.
<pre>
alert(!false); // true
alert(!"blue"); // false
alert(!NaN); //true
alert(!""); // true
alert(!12345); //false
</pre>

논리 NOT연산자는 값을 블리언 타입으로 바꾸는데도 유용하다. 연달아 2개를 쓰면 Boolean() 함수를 쓴 것과 같다.
<pre>
alert(!!"blue"); // true
alert(!!0); // false
alert(!!NaN); //false
alert(!!""); // false
alert(!!12345); //false
</pre>

논리 AND
논리 AND연산자는 앰퍼센드 2개(&&)이다.
<pre>
var result = true && false;
</pre>
논리 AND연산자는 다음 표에 해당하는 값을 반환한다.
<table>
<tr>
  <th>피연산자 1</th>
  <th>피연산자 2</th>
  <th>결과</th>
</tr>
<tr>
<td>true</td>
<td>true</td>
<td>true</td>
</tr>
<tr>
<td>true</td>
<td>false</td>
<td>true</td>
</tr>
<tr>
<td>false</td>
<td>true</td>
<td>false</td>
</tr>
<tr>
<td>false</td>
<td>false</td>
<td>false</td>
</tr>
</table>
논리 AND는 피연산자가 블리언 값이 아니라도 사용할 수 있다. 다만 피연산자 중 하나가 불리언이 아닐 때는 다음과 같이 동작하며
논리 AND가 불리언 값을 반환하지 않을 수도 있다.
+ 첫 번째 피연산자가 객체라면 항상 두 번째 피연산자를 반환한다.
+ 두 번째 피연산자가 객체라면 첫 번째 피연산자를 true로 평가할 수 있을 때만 두 번째 피연산자를 반환한다.
+ 두 번째 피연산자 모두 객체라면 두 번째 피연산자를 반환한다.
+ 피연산자 들 중 하나라도 null이면 null을 반환한다.
+ 피연산자 둘 중 하나라도 undefined라면 undefined를 반환한다.

첫 번째 피연산자에서 결과를 정할 수 있다면 논리 AND연산자는 두 번째 피연산자를 결코 평가하지 않는다.
논리 AND에서 첫 번째  피연산자가 false라면 두번째 피연산자의 값과는 무관하게 결과는 절대 true일 수 없다.
<pre>
var found = true;
var result = (found && someUndeclaerVariable); // 여기에서 에러 발생
alert(result); // 이 행은 실행되지 않는다.
</pre>
이 코드는 논리 AND를 평가하면서 에러를 일으키는데 변수 someUndeclaerVariable가 선언되지 않았기 때문이다.
found값이 true이므로 AND연산자는 변수 someUndeclaerVariable를 평가하며 이 단계에서 someUndeclaerVariable변수가
선언되지 않았기 때문에 에러가 발생한다.
<pre>
var found = false;
var result = (found && someUndeclaerVariable); // 에러 없음
alert(result); // 동작한다.
</pre>
결과가 반드시 false가 되면 &&의 오른쪽을 반드시 평가할 이유는 없다. 사용방법에 따라 이렇게 변형하여 사용 할 수 있다.

논리 OR
ECMAscript에서 논리 OR연산자는 다음과 같이 파이프 두개 (||)로 표현된다.
<pre>
var result = true || false;
</pre>
논리 AND와 마찬가지로 피연산자 중 하나가 아니라면 논리 OR에서 불리언 값을 반환하지 않을 수 있다.
+ 첫번째 피연산자가 객체라면 첫 번째 피연산자를 반환한다.
+ 첫번째 피연산자가 false이면 두 번째 피연산자를 반환한다.
+ 두번째 피연산자가 모두 객체라면 첫 번째 피연산자를 반환한다.
+ 두번째 피연산자가 모두 null이면 null을 반환한다.
+ 두번째 피연산자가 모두 NaN이면 Nan을 반환한다.
+ 두번째 피연산자가 모두 undefined라면 undefined를 반환한다.  

<table>
<tr>
  <th>피연산자 1</th>
  <th>피연산자 2</th>
  <th>결과</th>
</tr>
<tr>
<td>true</td>
<td>true</td>
<td>true</td>
</tr>
<tr>
<td>true</td>
<td>false</td>
<td>true</td>
</tr>
<tr>
<td>false</td>
<td>true</td>
<td>false</td>
</tr>
<tr>
<td>false</td>
<td>false</td>
<td>false</td>
</tr>
</table>

<pre>
var found = true;
var result = (found || someUndeclaerVariable) // 에러 없음
alert(result); //동작한다.
</pre>
이전과 마찬가지로 someUndeclaerVariable는 정의되지 않았지만 첫번째 변수가 true이므로 두번째 변수someUndeclaerVariable를
평가하지 않고 true를 반환한다.
<pre>
var found = false;
var result = (found || someUndeclaerVariable) // 에러 발생
alert(result); //동작하지 않음
</pre>

논리 OR연산자의 행동을 이용해서 변수에 null나 undefined가 저장되지 않게 할 수 있다.
<pre>
var myObject = preferredObject || backupObject;
</pre>
이 예제에서는 변수 myObject에 두 값 중 하나가 할당되며 preferredObject변수에는 가능하다면 사용하고 싶은값이 저장되고
backupObject에는 사용하고자 하는 값을 이용할 수 없을 때 대신 쓸 값이 들어 있다. preferredObject 가 null이 아니라면
myObject에는 preferredObject에 할당된다 null일 경우 backupObject에 할당된다.

동일 연산자
두 변수가 동일한지 판단하는 일은 프로그래밍에서 중요한 부분을 차지한다. == 연산자는 동일 연산자로 불리며 === 연산자는 일치 연산자로 불린다.
문자열이나 숫자, 불리언 값에는 두 값이 동일한지 알아보기 매우 간단하지만 객체는 복잡한 문제이기 때문에 이런 표현을 사용한다.

동일과 비동일
== 연산자와 === 연산자는 피연산자를 비교하기 전에 타입강제 하여 변환한다.
변환할 때는 다음과 같은 규칙을 따른다.
+ 피연산자가 불리언 값일 때는 숫자형 값으로 변환된다. false는 0이 되고 true는 1이 된다.
+ 피연산자가 하나가 문자열이고 다른 하나가 숫자라면 문자열을 숫자로 바꿀 수 있는지 시도한다.
+ 피연산자 중 하나가 객체이고 다른 하나가 객체가 아니라면 객체의 valueOf() 매서드를 호출해 원시 값으로 바꾼 후 이전규칙을 따른다.
비교할 때는 다음과 같은 규칙을 따른다.
+ null과 undefined는 동일하다.
+ 동일 여부를 평가 할 때 null과 undefined를 결코 다른 값으로 변환하지 않는다.
+ 피연산자 중 하나가 NaN이라면 동일연산자는 false를 반환하며 비동일 연산자는 true를 반환한다. 피연산자가 모두 NaN이라도 동일 연산자(==)는
false를 반환한다. NaN은 NaN과 같지 않다.
+ 두 피연산자가 객체라면 같은 객체인지 비교한다. 두 피연산자가 모두 같은 객체에대한 참조를 가르키면 동일 연산자는 true를 반환하며 그렇지 않다면
false를 반환한다.  

<table>
<tr>
<th>표현식</th>
<th>값</th>
</tr>
<tr>
<td>null == undefined</td>
<td>true</td>
</tr>
<tr>
<td>"NaN" == Nan</td>
<td>false</td>
</tr>
<tr>
<td>5 == NaN</td>
<td>false</td>
</tr>
<tr>
<td>NaN == NaN</td>
<td>false</td>
</tr>
<tr>
<td>NaN !== NaN</td>
<td>true</td>
</tr>
<tr>
<td>false == 0</td>
<td>true</td>
</tr>
<tr>
<td>true == 1</td>
<td>true</td>
</tr>
<tr>
<td>true == 2</td>
<td>false</td>
</tr>
<tr>
<td>undefined == 0</td>
<td>false</td>
</tr>
<tr>
<td>null == 0</td>
<td>false</td>
</tr>
<tr>
<td>"5" == 5</td>
<td>true</td>
</tr>
</table>
