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
