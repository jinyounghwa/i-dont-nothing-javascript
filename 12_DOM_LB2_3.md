##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
이 장에서 다루는 내용  
+ DOM 레벨 2와 3에서 변경된 점
+ 스타일 조작에 쓰이는 DOM API
+ DOM이동과 범위

DOM모듈에 대한 정리는 다음과 같다.  
+ DOM코어 - 레벨 1 코어를 바탕으로 노드에 매서드와 프로퍼티를 추가한다.
+ DOM뷰 - 스타일 정보를 바탕으로 여러가지 문서 뷰를 정의한다.
+ DOM이벤트 - 이벤트에 기반한 DOM문서의 상호작용 방법을 정의한다.
+ DOM스타일 - 프로그램을 통해 CSS스타일 정보에 접근하고 변경하는 방법을 정의한다.
+ DOM이동과범위 - DOM문서를 이동하고 특정 범위를 선택하는 새 인터페이스를 정의한다.
+ DOM HTML - 레벨 1 HTML에 기반하여 새 인터페이스와 프로퍼티, 매서드를 추가한다.

DOM변경점  
브라우저의 DOM모듈 지원은 다음과 같이 확인 할 수 있다.  
```javascript
var supportsDOM2Core = document.implemention.hasfeature("Core", "2.0");
var supportsDOM3Core = document.implemention.hasfeature("Core", "3.0");
var supportsDOM2HTML = document.implemention.hasfeature("HTML", "2.0");
var supportsDOM2Views = document.implemention.hasfeature("Views", "2.0");
var supportsDOM2XML = document.implemention.hasfeature("XML", "2.0");
```

요소 스타일에 접근  
DOM요소에 대한 참조가 유효하기만 하면 언제든지 자바스크립트로 스타일을 설정 할 수 있다. 다음의 예제를 확인하자.  
```javascript
var myDiv = document.getElementById("myDiv");

//배경색 지정
myDiv.style.backgroundColor = "red";

//크기를 바꾼다.
myDiv.style.width = "100px";
myDiv.style.height = "200px";

//테두리를 지정한다.
myDiv.style.border = "1px solid black";
```
이런식으로 요소와 스타일을 바꾸면 화면에 보이는 모습도 자동으로 업데이트 된다.  
style속성에 명시한 스타일은 style객체에서도 사용할 수 있다. 다음 HTML코드를 보자
```
<div id="myDiv" style="background-Color : blue; width:10px; height:25px"><div>
```
이 요소의 style속성에 저장된 정보는 다음 코드로 알 수 있다.  
```javascript
alert(myDiv.style.backgroundColor); //blue
alert(myDiv.style.width); //10px
alert(myDiv.style.height); //25px
```
요소에 style 속성이 없으면 style객체에 기본 값이 포함되지만 이것만으로 요소의 스타일의 정보를 알 수 없다.  

DOM스타일 프로퍼티와 매서드  
다음과 가은 프로퍼티와 매서드가 있다.  
+ cssText - style속성의 CSS코드에 접근한다.
+ length - 요소의 적용된 CSS 프로퍼티 갯수
+ parentRule - CSS정보를 표현하는 객체이다.
+ getPropertyCssValue(propertyName) - 주어진 값을 표현하는 CSSValue객체를 반환한다.
+ item(색인) - 주어진 위치에 있는 CSS프로퍼티 이름을 반환한다ㅏ.
+ removeProperty(propertyName) - 주어진 프로퍼티를 제거한다.

cssText프로퍼티는 다음과 같이 사용한다.
```javascript
myDiv.style.cssText = "width:25px; height:100px; background-Color:green;"
alert(myDiv.style.cssText);
```

length프로퍼티는 item()매서드와 함께 사용하여 요소에 정의된 CSS프로퍼티를 순회하도록 디자인되었다. 이둘을 이용하면 style객체를 마치 컬렉션 처럼 쓸 수 있으며 다음 예제처럼 item()대신 대괄호 표기법을 사용하여 주어진 위치의 CSS프로퍼티 이름을 알 수 도 있다.  
```javascript
for (var i=0, len=myDiv.style.length; i<len; i++){
  alert(myDiv.style[i]); //또는 myDiv.style.item()
}
```
대괄호 표기법이나 item()어느 것으로도 CSS프로퍼티 이름("background-Color"이 아니라 "backgroundColor")을 알 수 있다 다음 예제처럼 이 프로퍼티 이름을 getPropertyValue()에 넘겨 프로퍼티 값을 알 수 있다.  
```javascript
var prop, value, i, len;
for(i=0, len=Div.styel.length; i < len; i++) {
  prop = myDiv.style[i];
  value = myDiv.style.getPropertyValue(prop);
  alert(prop + " : " + value);
}
```
getPropertyValue()매서드는 CSS프로퍼티 값을 항상 문자열 형태로 반환한다. 정보가 더 필요할 때는 getPropertyCssValue()에서 반환하는 CSSValue객체를 이용한다. 이 프로퍼티는 cssText와 cssVlaueType두 가지 프로퍼티를 가진다. cssVlaueType프로퍼티는 숫자형 상수이다. 0은 상속된 값, 1은 원시 값, 2는 목록, 3은 커스텀 값을 나타낸다.  
```javascript
var prop, value, i, len;
for (i=0, len=myDiv.style.length; i < len; i++){
  prop = myDiv.style[i];
  value = myDiv.style.getPropertyCssValue(prop);
  alert(prop + " : " + value.cssText + "("( + value.cssVlaueType + ")"));
}
```
현실적으로는 getPropertyValue()가 더 유용하다.  
