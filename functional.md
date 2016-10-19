```
td = function(v){
 return '<td>' + v + '</td>';
}
tr = function(v){
return '<tr>' + v + '</tr>';
}
wrapperTag = function(tag){
 return function(v){
    return '<' + tag + '>' + v + '</' + tag + '>';
 }
}

td = wrapperTag('td'), tr = wrapperTag('tr');

wrapperTag = function(tag, id){
return function(v){
   return '<' + tag + (id? ' id="' + id + '" : '') + '>' + v + '</' + tag + '>';
}
}

td('html', 'aaa');

tr('stter', 'id');

```
아래와 같은 배열이 있다고 가정하자
```
[
  ['qwre','qerqew',qwewewe'],
  ['qwre','qerqew',qwewewe'],
  ['qwre','qerqew',qwewewe'],
  ['qwre','qerqew',qwewewe']
]
```
표현하자면 다음과 같다.
```
data.reduce(function(p, c){

}, '');

data.reduce(function(p, c){
  c.reduce(function(p, c){
       return p + td(c);
   }, '');
}, '');
data.reduce(function(p, c){
 tr(c.reduce(function(p, c){
      return p + td(c);
  }, ''))
}, '');

data.reduce(function(p, c){
return p +tr(c.reduce(function(p, c){
     return p + td(c);
 }, ''))
}, '');

tbody.innerHTML =
data.reduce(function(p, c){
return p +tr(c.reduce(function(p, c){
    return p + td(c);
}, ''))
}, '');
```
추상적인 행위를 생각 해 보자
우리는 리듀싱을 할 거야
```
function(v){
try{
   v= JSON.parse(v).data;
}catch(e){
  return;
 }

tbody.innerHTML =  v.reduce(function(p, c){
return p +tr(c.reduce(function(p, c){
   return p + td(c);
}, ''))
}, '');

wrapperTag = function(tag){
 return function(v){
    return '<' + tag + '>' + v + '</' + tag + '>';
 }
}
```
```
wrapperTag = function(tag){
return function(v){
   return open(tag) + v + close(tag);
}
}


wrapperTag({tagName:'div', id:'aaa'});


<div id="a"></div>
wrapperTag(document.getElementById('a'));

wrapperTag($('.aaa'));

title = wrapperTag($('.title'));

aa.innerHTML = title(data.str);

title = wrapperTag($('.title'));
aa.innerHTML = title(paserVdom(data));

title(aa, paserVdom(data));
```
```
title(aa, data, paserVdom);

generate( aa, title, data, parser);

generate(contents, content, data, parser);

generate($('footer'), footer, data.footer, parser);

f['message']

f['_____msg']

tbody.innerHTML =  v.reduce(function(p, c){
return p +tr(c.reduce(function(p, c){
  return p + td(c);
}, ''))
}, '');
```
1. 알고리즘의 대상이 줄어든다,변수가 줄어들고 제어용 변수가 위임된다  

2. 지역변수를 없앨 수 있다, 제네릭한 알고리즘을 짠다  
```
for(){
 for(){

if(){
  if(){

for(; i< j; ){

tbody.innerHTML =  v.reduce(function(p, c){
return p +tr(c.reduce(function(p, c){
 return p + td(c);
}, ''))
}, '');

i = -2

<div>a</div>
<div>b</div>
<div>c</div>
<div>d</div>

<b></b>
<div>a</div>
<div>b</div>
<b></b>
<div>c</div>
<div>d</div>

-------------------------

<b></b>
<div>a</div>
<div>b</div>
<b></b>
<div>c</div>
<div>d</div>

b = document.getElementsByTagName('b');

Array.from(b).reduce(function(result, el){
}, [] );

Array.from(b).reduce(function(result, el){
  result.push(el.nextElementSibling);
  return result;
}, [] );

Array.from(b).map(function(el){
  return el.nextElementSibling;
});

b = Array.from(document.getElementsByTagName('b')).map(function(el){
 return el.nextElementSibling;
});

Arr.from(b,v=>v.el.nextElementSibling)

prop = function(prop){
 return function(target){
    return target[prop];
 }
};

next = prop('nextElementSibling');

b = Array.from(document.getElementsByTagName('b')).map(next);


Array.from(document.getElementsByTagName('b')).map(prop('previousElementSibling'));

Array.from(document.getElementsByTagName('div')).map(prop('innerHTML'));

domProp = function(list, mapF){
}

a = domProp(document.getElementsByTagName('div'), prop('innerHTML') );
a = domProp(document.getElementsByTagName('div'), 'innerHTML');

finder(valueFun, bestFun, coll)
best( fun, coll)
domList('div', 'innerHTML');
```
