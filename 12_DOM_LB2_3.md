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
