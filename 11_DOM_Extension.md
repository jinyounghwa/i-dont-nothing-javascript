##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
이 장에서 다루는 내용  
+ 선택자 API에 대한 이해
+ HTML5 DOM확장 사용
+ 브라우저 전용 DOM확장 사용

querySelector()매서드  
querySelector매서드는 매개변수로 CSS쿼리를 받고 패턴에 일지하는 첫 번째 자손 요소를 반환하여 일치하는 것이 없다면 null을 반환한다.  
<pre>
// body 요소를 가져온다.
var body = document.querySelector("body");

//ID가 "myDiv"인 요소를 가져온다.
var myDiv = document.querySelector("#myDiv");

//클래스가 "selector"인 요소 중 첫번째를 가져온다.
var selector = document.querySelector(".selector");

//클래스가 "button"인 이미지 중 첫 번째를 가져온다.
var img = document.body.querySelector("img.button");
</pre>

querySelector()매서드를 Document타입에서 호출하면 패턴에 맞는 요소를 문서에서 찾으며 Element타입에서 호출하면 해당 요소의 자손에만 쿼리한다. CSS쿼리는 복잡할 수도, 단순할 수도 있다. querySelector()에 넘긴 쿼리 문법에 에러가 있거나 지원하지 않는 선택자를 사용하면 에러가 발생한다.  

querySelectorAll()매서드  
querySelectorAll() 매서드는 querySelector()와 마찬가지로 CSS쿼리를 매개변수로 받되 노드 전체를 반환한다. 이 매서드는 NodeList의 정적 인스턴스를 반환한다. 다시말해 반환값은 NodeList의 프로퍼티와 매서드를 모두 가지지만 접근할 때마다 다시 쿼리하는것이 아니라 처음 접근했을 때의 상태만 반영한 채 고정한다. 이렇게 구현함으로써 NodeList객체에 결부된 성능 문제가 대부분 해결되었다. 유효한 CSS쿼리를 넘겨 querySelectorAll()을 호출하면 NodeList객체를 반환하여 포함하는 요소 숫자에는 제한이 없다. 일치하는 것이 없다면 빈 NodeList를 반환한다.  
querySelector()와 마찬가지로 querySelectorAll()역시 Document와 DocumentFragment, Element 타입에서 호출 가능하다 아래 코드를 보자.  
<pre>
// < div >에 포함된 < em >요소를 모두 가져온다.
//(getElementByTagName("em")과 비슷하다.)
var ems = document.getElementById("myDiv").querySelectorAll("em");

//클래스가 "selected"인 요소를 모두 가져온다.
var selecteds = document.querySelectorAll(".selected");

//< p > 요소에 포함된 < strong > 요소를 모두 가져온다.
var strongs = document.querySelectorAll("p strong");
</pre>
반환 값인 NodeList객체에서 item()매서드나 대괄호 표기법을 써서 원하는 요소를 가져 올 수 있다. 다음 예제를 확인하라.  
<pre>
var i,len,strong;
for(i=0, lem=strong.length; i <len; i++){
  strong = strongs[i];
  strong.className = "important";
}
</pre></pre>
querySelector()과 마찬가지로 querySelectorAll()역시 브라우저에서 지원하지 않는 선택자를 쓰거나 CSS선택자 문법이 틀리면 에러가 밸생한다.  

matchesSelector()매서드  
선택자 API레벨 2 명세에서는 Element타입에 matchesSelector()라는 매서드를 정의 했다. 이 매서드는 매개변수로 CSS선택자를 받고 요소가 그에 일치하면 true를 일치하지 않으면 false를 반환한다.  
<pre>
if (document.body.matchesSelector("body.page1")){
  //true
}
</pre>
이 매서드를 이용하면 요소에 대한 참조와 querySelector()나 querySelectorAll()이 CSS쿼리로 반환하는 요소를 쉽게 비교 할 수 있다.

요소 간 이동  
요소간 이동 API에서 추가된 새 프로퍼티는 다음과 같다.  
+ childElementCount - 자식 요소 숫자를 반환하되 텍스트 노드와 주석은 제외한다.
+ firstElementChild - 첫 번째 자식 요소를 가리킨다. firstChild를 요소에 국한했다고 생각하라.
+ lastElementChild - 마지막 자식 요소를 가리킨다. lastChild를 요소에 국한했다고 생각하라.
+ previousElementSilbling - 이전 형제 요소를 가리킨다.
+ nextElementSilbling - 다음 형제 요소를 가리킨다.

지원하는 브라우저는 이들 프로퍼티를 DOM요소 전체에 추가하며 이를 통해 텍스트 노드를 신경 쓰지 않고 DOM요소 사이를 이동 할 수 있다. 예를들어 전통적인 크로스 브라우저 코드에서는 특정 요소의 자식 요소를 순회할 때 다음과 같이 했다.  
<pre>
var i,
    len,
    child = element.firstChild;
  while(child != element.lastChild){
    if(child.nodeType ==1){//요소인지 확인
      processChild(child);
    }
    child = child.nextSibling;
  }
</pre>
요소간 이동 프로퍼티를 쓰면 다음과 같이 단순한 코드를 쓸 수 있다.  
<pre>
var i,
    len,
    child = element.firstElementChild;
  while(child != element.lastChild){
    processChild(child); //요소임을 이미 알고 있음
    child = child.nextElementSilbling;
  }
</pre>
요소 간 이동을 구현한 브라우저는 인터넷 익스폴로러 9이상 동작한다.  

HTML5  
클래스 관련 추가사항
getElementByClassName()매서드
HTML5에서 가장 인기있는 document객체와 HTML요소 전체에서 사용 가능한 getElementByClassName()매서드이다. 이 매서드는 기존의 DOM기능을 바탕으로 구현한 자바스크립트 라이브러리에서 발전한 것이며 네이티브로 구현함에 따라 성능이 크게 향상되었다. getElementByClassName()매서드는 클래스 이름 문자열을 매개변수로 받으며 해당 클래스를 모두 가진 요소의 NodeList를 반환한다.클래스 이름의 순서는 상관 없다. 다음 예제를 확인해 보자.  
<pre>
//순서에 상관 없이 클래스 이름에 "usename"과 "current"모두 있는 요소를 찾는다.
var allCurrentUsernames = document.getElementByClassName("usename current");

//myDiv의 자손중에 "selected" 클래스가 있는 요소를 모두 찾는다.
var selected = document.getElementById("myDiv").getElementByClassName("selected");
</pre>
이 매서드는 호출한 요소의 자손만 쿼리하여 반환한다. document에서 getElementByClassName()을 호출하면 항상 해당 클래스를 가진 요소 전체를 반환한다. 이 매서드는 클래스를 바탕으로 이벤트를 등록하려 할 때 유용하다. 반환값은 NodeList이므로 getElementByTagName()이나 기타 NodeList객체를 반혼하는 DOM매서드와 같은 성능문제가 있음을 염두해 둘 것.  
getElementByClassName()매서드를 구현한 브라우저는 익스 9 이상이다.  

classList프로퍼티  
클래스 이름을 조작할 때는 className프로퍼티를 이용해 클래스 이름을 추가하거나 제거,교체했다 className프로퍼티는 단순한 문자열일 뿐이므로 바꿀 때마다 문자열 전체를 염두해 두고 조작해야 했으며, 클래스 이름 중 일부는 바꿀 필요가 없을 때도 복잡한 작업이 필요했다. 예를들어 다음 HTML코드를 보자.  
<pre>
< div class="bd user disabled" > ... < /div >
</pre>
이 < div > 요소에는 세 가지 클래스가 할당되어 있다. 이들 클래스 중 하나를 제거하려면 class속성을 분할해서 필요 없는 클래스를 제거 한 다음 남은 클래스를 합쳐 클래스 문자열을 다시 생성해야 한다. 다음 예제를 확인하자.  
```
// "user" 클래스를 제거한다.
// 먼저 클래스 이름 목록을 만든다.
var className = div.className.split(/\s+/);

// 제거할 클래스 이름을 찾는다.
var pos = -1;
i,
len;
for(i=0, len=className.length; i < len; i++){
  if(className[i] == "user"){
    pos = -1;
    break;
  }  
}

//클래스 이름을 제거한다.
className.splice(i,1);

//클래스 이름을 다시 지정한다.
div.className = className.join(" ");
```
< div > 요소의 class속성에서 "user" 클래스를 제거하는 간단한 작업이 이 코드 전체가 필요하다 클래스 이름을 교체하거나 클래스 이름이 요소에 적용되었는지 확인할 때도 반드시 비슷한 알고리즘을 써야 한다. 클래스 이름을 추가할 때는 문자열을 합치기만 하면 되니 간단해 보이지만 같은 클래스가 중복 적용 되었는지 반드시 체크해야 하므로 복잡하기는 마찬가지이다. 자바스크립트 라이브러리도 대부분 이런 식으로 매서드들 구현하였다. HTML5 는 모든 요소에 classList프로퍼티를 추가하여 클래스 으림을 안전하고 단순하게 조작 할 수 있다. classList프로퍼티를 추가하여 클래스 이름을 안전하고 단순하게 조작 할 수 있다. classList프로퍼티는 DOMTokenList역시 length 프로퍼티가 있어서 포함된 데이터 개수를 알 수 있고 item()매서드나 다음 메서드가 추가 된다.  
+ add(value) - 주어진 문자열 값을 목록에 추가한다. 값이 이미 존재하면 추가하지 않는다.
+ contains(value) - 주어진 값이 목록에 존재하면 true를, 그렇지 않다면 false를 반환한다.
+ remove(value) - 주어진 문자열 값을 목록에서 제거 한다.
+ toggle(value) - 값이 목록에 존재하면 제거하고 그렇지 않으면 추가한다.  
다음과 같은 단순한 코드가 이전예제 전체를 대신한다.  
```javascript
div.classList.remove("user");
```
