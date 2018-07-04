# 20180704 JS with LikeLion

### JQuery

```javascript
>$ // jquery를 사용하기 위한 문법
ƒ ( selector, context ) {

		// The jQuery object is actually just the init constructor 'enhanced'
		// Need init if jQuery is called (just allow error to be thrown if not included)
		return new jQuery…
>$("css 선택자")
jQuery.fn.init [prevObject: jQuery.fn.init(1), context: document, selector: "css 선택자"]
context:document
length:0 // "css 선택자"라는 건 없기에 length가 0으로 나온다.
prevObject:jQuery.fn.init [document, context: document]
selector:"css 선택자"
__proto__:Object(0)
>$(".btn")
jQuery.fn.init(2) [a.btn.btn-primary, a.btn.btn-primary, prevObject: jQuery.fn.init(1), context: document, selector: ".btn"]
0:a.btn.btn-primary
1:a.btn.btn-primary
context:document
length:2
prevObject:jQuery.fn.init [document, context: document]
selector:".btn"
__proto__:Object(0)
>$("button")
jQuery.fn.init [prevObject: jQuery.fn.init(1), context: document, selector: "button"]
context:document
length:0
prevObject:jQuery.fn.init(1)
0:document
context:document
length:1
__proto__:Object(0)
selector:"button"
__proto__:Object(0)
>$("#title")
jQuery.fn.init [h1#title, context: document, selector: "#title"]
0:h1#title
context:document
length:1
selector:"#title"
__proto__:Object(0)
```

* JQuery의 기본형?

```javascript
// 1. $('.btn').이벤트명(fucntion(){}); -  요소.이벤트(핸들러)
$('.btn').'mouserover'(function(){console.log("건드리지마");});
// 2. $('').on(이벤트명,function(){}[이벤트 핸들러]);
$('.btn').on('mouseover', function() { console.log("건드리지마라");});

// 두개의 장점은 원래 자바스크립트는 요소 하나를 따로 뺴서 각자 이벤트를 줘야되는데 jquery는 요소가 배열처럼 있어도 동시에 적용된다는 것.
```

* 마우스가 버튼 위에 올라갔을 때, 버튼에 있는 `btn-primary` 클래스를 삭제하고,  `btn-danger` 클래스를 준다. 버튼에서 마우스가 내려왔을 때, 다시 `btn-danger `클래스를 삭제하고, `btn-primary `클래스를 추가한다.
  * hint : 여러개의 이벤트 등록하기, 요소에 class를 넣고 빼는` jquery function`을 찾아 보시오.

```javascript
>var btn = $('.btn')
<undefined
>btn.on('mouseenter mouserout', function(){ 
    if(btn.hasClass('btn-danger')){ //해당 클래스를 갖고있다면
        btn.removeClass('btn-danger').addClass('btn-primary');
        //그 클래스를 제거하고 다른걸 더해줘라
    } else { 
        btn.removeClass('btn-primary').addClass('btn-danger');
    }
});
<jQuery.fn.init(2) [a.btn.btn-danger, a.btn.btn-danger, prevObject: jQuery.fn.init(1), context: document, selector: ".btn"]
```

* 좀더 간단하게 ! - `toggleClass`를 사용해보자

```javascript
>var btn = $('.btn')
<undefined
>btn.on('mouseenter mouseout', function(){
    btn.toggleClass('btn-danger').toggleClass('btn-primary');
});
<jQuery.fn.init(2) [a.btn.btn-primary, a.btn.btn-primary, prevObject: jQuery.fn.init(1), context: document, selector: ".btn"]
```

* 위 이벤트의 단점은 하나의 버튼에만 마우스를 올려도 모든 버튼의 색이 변한다는것. 

  그렇다면 한개만 바꾸고 싶다면?

```javascript
>var btn = $('.btn')
<undefined
>btn.on('mouseenter mouseout', function(){
    $(this).toggleClass('btn-danger').toggleClass('btn-primary');
    //여기서 this는 이벤트가 발생한 바로 그친구를 가리킨다.
});
<jQuery.fn.init(2) [a.btn.btn-primary, a.btn.btn-primary, prevObject: jQuery.fn.init(1), context: document, selector: ".btn"]
```

* this 와 $(this)의 차이

```javascript
>btn.on('mouseenter mouseout', function(){
    $(this).toggleClass('btn-danger').toggleClass('btn-primary'); 
    console.dir(this); 
    console.dir($(this));
    
    // this 와 $(this)의 차이??
    // jquery 메소드를 쓸수 있느냐 없느냐. $(this)는 jquery메소드를 사용할 수 있다.
});

//마우스를 올리면
<a.btn.btn-primary
<VM610:2 jQuery.fn.init(1)
```

* 버튼에 마우스가 오버됐을 때, 상단에 있는 이미지의 속성에 `style`속성명과 `width: 100px;`의 속성값을 부여한다.

```javascript
>var btn = $('.btn')
>btn.on('mouseover', function(){
    $('img').attr('style', 'width: 100px;')
})
    
```

* modify text - 버튼에 마우스를 올리면 제목의 글자가 변할 수 있도록하기.

```javascript
>btn.on('mouseover', function(){
   $('.card-title').text("Don't Touch Me!!"); });

<jQuery.fn.init(2) [a.btn.btn-primary, a.btn.btn-primary, prevObject: jQuery.fn.init(1), context: document, selector: ".btn"]
```


* 버튼(요소)의 마우스가 오버(이벤트)됐을 때(이벤트 리스너), 이벤트가 발생한 버튼(`$(this)`)과 같은 수준에 있는 요소 중에서 `.card-title`의 속성을 가진 친구를 찾아(`find()`) 텍스트를 변경(`text()`)시킨다

```javascript
$(this).siblings().find("card-title").text("~")
```
* `siblings()`는 같은 수준에서 찾는게 아니라 하위에서 찾는것이기에 `card-title` 을 `find`하는 것이 안먹힌다. 그래서 `.parent()`를 사용하여,  상위요소에서 바꿀 수 있도록한다.

```javascript
btn.on('mouseover', function(){ 
    $(this).parent().find('.card-title').text('바뀌어랏!'); 
});
```

## 텍스트 변환기 (오타치는 사람 놀려보자!)

*index.html.erb*

```html
<textarea id = "input" placeholder = "변환할 텍스트를 입력해주세요."></textarea>
<button class="translate">바꿔줘</button>
<h3></h3>
```

* input에 들어있는 텍스트 중에서 `'관리'->'고나리', '확인'->'호가인', '훤하다' -> '후너하다' `의 방식으로 텍스트를 오타로 바꾸는 이벤트 핸들러 작성하기
* https://github.com/e-/Hangul.js 에서 라이브러리를 받아서 자음과 모음을 분리하고, 다시 단어로 합치는 기능을 살펴보기
* `string.split('')` : `''`안에 있는 것을 기준으로 문자열을 잘라준다 (`return type : 배열`)
* `Array.join('')`: 배열안에 있는 잘라진 문자열들을 다시 합쳐준다.
* `Array.map(function(el){})` : 배열을 순회하면서 하나의 요소 마다 `fucntion`을 실행시킨다.(`el`: 순회하는 각 요소, `return type: 새로운 배열`) 

### 코드 로직을 짜보자

1. `textarea`의 내용물을 가지고 오는 코드를 넣는다.
2. 버튼에 이번트 리스너(`click`)를 달아주고, 핸들러에서는 1번에서 작성한 코드를 넣는다.
3. 1번 코드의 결과물을 한글자 씩 분해해서, 배열로 만들어준다.
4. 분해한 문자열에서 , 4번째 요소가 있고, 2, 3번째의 요소가 모음인지 확인한다.
5. 4번의 조건을 만족하는 문자의 배열은  3, 4번째의 자리를 바꿔줍니다.
6. 결과물로 나온 배열을 문자열로 이어줍니다. (`join`을 이용)
7. 결과물을 출력해줄 요소를 찾는다. 
8. 요소에 결과물을 출력한다.

*index.html*

```javascript
<html>
<meta charset="utf-8">
<head>
  <title>오타소리</title>
  <script
  src="https://code.jquery.com/jquery-3.3.1.js"
  integrity="sha256-2Kok7MbOyxpgUVvAk/HJ2jigOSYS2auK4Pfzbm7uH60="
  crossorigin="anonymous"></script>
</head>
<body>
  <h2>누가 오타소리를 내었는가?</h2>
<textarea id = "input" placeholder = "오타 소리를 내보자."></textarea>
<br/>
<button class="translate">ㅅ..신이 내었사옵니다 즈언하</button>
<h3>용감하게 내질러 봅니다<br/></h3>
<script src="./hangul.js" type="text/javascript"></script>
<script src="./translate.js"></script>
<script type="text/javascript">
  $('.translate').on('click', function(){
    var input = $('#input').val(); //변수.val() - 인자가 있으면 빼오거나, 없으면 넣거나
    //console.log(input); //1번
    //var sp = input.split('');
    var result = translate(input);
    $('h3').text(result);// 6번
    console.log(result);
  }) //2번
</script>
</body>
</html>
```

*hangul.js*

https://github.com/e-/Hangul.js/blob/master/hangul.js 에서 참고할 것.

*translate.js*

```javascript
function translate(str){
  return str.split('').map(function(el){
    //el을 가지고 하나씩 조작합니다.
    var d = Hangul.disassemble(el); // 자음모음 분해!
    console.log(d); // 어떻게 변하는지 찍어보자
    if(d[3] && Hangul.isVowel(d[1]) && Hangul.isVowel(d[2])){
        var tmp = d[2];
        d[2] = d[3];
        d[3] = tmp;
    } //자리를 바꿔서 오타를 유발하자 ㅋ
    return Hangul.assemble(d); //자음모음을 결합하자
  }).join(''); //문자열끼리 합쳐보자
}
```



* 다른 파일에 있는 `function`을 불러오는건 쉽다.
* `Ajax` - 자바스크립트가 동작하는 코드 중간에 서버로 액션을 요청해서 불러오는 것 - 그러면 서버에서 응답해 오는 js코드에 어떠한 명령을 넣어줘서 보내준다.
  * 요청을 보낼때 뭐가 필요? 
    * `route`에 어느  `url`로 보낼지 지정하고 거기에 해당하는 액션을 만들어 놓는다.
    * 어느 메소드(액션)으로 보낼 것인지?
  * 고로 `Java Script`에서는 어디로 보내는지만 알고있으면된다. 즉 `url`, `http method`요거 두개만 알고 있다면 `AJAX`를 손쉽게 사용할 수 있다.
  * 화면 전환이 없이 서버에 요청을 보내고 응답을 받을 수 있기 때문에 많이 사용한다.

*`ajax`의 기본적인 사용법*
```javascript
$.ajax({
    url: 어느 주소로 요청을 보낼지, 
    method: 어떤 http method 요청을 보낼지,
    data: {
    	k: v 어떤 값을 함꼐 보낼지,
    	//서버에서는 params[k] => v
}
})
```



# 다대다 관계...

A, B테이블의 다대다 관계를 만들기 위해 가운데 Join 테이블을 만든다.

join 테이블을 느슨하게 두개로 나누어서 하면 ...?



### 좋아요!

1. 좋아요 버튼을 눌렀을 때,
2. 서버에 요청을 보낸다.(현재 유저가 현재 보고 있는 이 영화가 좋다고 하는 요청)
3. 서버가 할일
4. 응답이 오면 좋아요 버튼의 텍스트를 좋아요 취소로 바꾸고, `btn-info`클래스를 `btn-warning text-white` 로 바꿔준다.
