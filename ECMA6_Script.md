
# ECMA6 
## 템플릿 문자열 
`var name = "jeonka" ;`  
이 변수를 출력하기 위해서는?  
기존 >   
```alert("나의 이름은 : "+ name +" 입니다 ."");```  
ECMA6 >  
```alert(`나의 이름은 ${name} 입니다.`)```  
즉, ECMA6 에서는 **``** 로 감싼 문자열 내부에 **${}** 를 사용하고 안에 표현식을 넣으면 표현식이 계산되어 들어간다.

##  let 키워드와 const 키워드   

구분|선언|위치|재선언
:--:|:--:|:--:|:--:
var|변수|전역|가능
let|변수|지역|불가능
const|상수|지역|불가능

```var``` 키워드는 **비동기함수** 와 같이 사용하면 안된다. 
```
for(var i = 0 ; i < 3 ; i ++){
	setTimeout(()=>{
		alert(i);
	},1000,i);
}
```
위 코드의 경우 `for`문이 3번 반복하여 전역 변수인 `i`의 값은 전역 스코프에 3으로 존재하게 된다.  
따라서 3, 3, 3 출력.  

이를 개선하려면 ```var```키워드를 ```let```으로 변경한다.(```let```의 경우 지역 스코프이기 때문에 ```setTimeout()```이 인자로 받은 ```i```의 값이 같이 변하지 않는다.)


## for in 반복문
자바스크립트에서 배열 혹은 객체 내 요소를 꺼낼 때 사용하는 반복문은 ```for in```이다.

```
var arr = [1,2,3,4];
for( let i in arr){
	alert(` ${i} 번째 값은 ${arr[i]} 이다.`);
}
```
이 때 `i`(반복변수)에는 **요소** 가 아닌 **인덱스** 가 들어간다.

## for of 반복문
```
var arr = [1,2,3,4];
for( let element of arr){
	alert( `요소는 ${element} 이다.`);
}
```
이 경우 `for in` 과는 다르게 **배열의 요소**에 직접 접근이 가능하나, 반복 횟수를 확인 할 수 없다.

----
----

## 자바스크립트의 실행 순서

```
console.log("A");
setTimeout(function(){
	console.log("B");
},0);
console.log("C");
```
위 코드의 출력 결과는 **A C B** 이다.  
그 이유는 **비동기함수**는 처리를 웹 브라우저가 처리를 하기 때문이다.  
웹 브라우저의 경우 **이벤트 큐** 를 통해 비동기 함수를 처리한다  
**이벤트 큐**내 함수 중 처리가 완료된 함수가 있더라도 **메인스택**이 비워지기 전까지 이 결과를 보내지 않는다.  
따라서 메인 스택에 있는 **A**와 **C**가 출력이 완료된 후 **B**가 출력된다.   

따라서 아래 코드의 경우 출력값이 없다.  
```
setTimeout(function(){
	alert('SetTimeOut);
},0);
while(true){};
```
이유는 메인스택의 `while(true)`가 끝나지 않기 때문에 **이벤트큐**의 값을 전달받을 수 없다.  

## 클로저의 활용
```
for( var i = 0 ; i < 3 ; i++){
	setTimeout(function(){
		console.log(i);
	},0)
}
```
이 함수의 출력값은 **3 3 3**이다.    
이는 반복문(메인스택)이 모두 끝난 후 비동기 함수가 실행되기 때문이다.  

따라서 **0,1,2** 즉 `i`의 값을 복사해두어야 한다.  
```
for(var i = 0; i < 3; i++){
	(function (closed_i_){
		setTimeout(function(){
			console.log(closed_i);
		},0);
	})(i); // 값을 보내둔다.
}
```
출력값 : **0 1 2**  

## 매개변수의 기본
가변 인자를 사용하는 방법 중 arguments 이외에 여려가지 방식이 있다.  
그 중 매개변수를 입력하지 않았을 때 **강제로 초기화하는 방식**을 설명한다.  
```
function test(a,b,c){
	b = b || 22; // b 가 undefined 일 경우 22 대입
	c = c || 33; // c 가 undefined 일 경우 33 대입 
	console.log(`a : ${a}, b : ${b}, c : ${c}`);
}

test(1,2);
```

위 코드를 조금 더 간단히 하자면  
```
function test(a,b=22,c=33){
	console.log(`a : ${a}, b : ${b}, c : ${c}`);
}
test(1,2); 
```
와 같이 수정할 수 있다.  

## 전개 연산자
전개 연산자는 **가변인자**를 만들 때 사용한다.  
먼저 예시 코드이다.  
```
function test(a, b, ...numbers){
	console.log("numbers[0] : "+numbers[0]);
	console.log("a : " + a);
	console.log("numbers : "+numbers)
}
test(1,2,3,4,5,6);
```
전개연산자의 이름을 `arguments` 라고 할 경우 기존 `arguments` 객체를 덮어버리기 때문에 다른 이름을 사용함  
출력값 >  
**numbers[0] : 3  
a : 1  
numbers : 3,4,5,6**  

전개연산자를 다음과 같이 활용할 수 있다.  
```
function test(a,b,c,d){
	console.log(`${a} ${b} ${c} ${d}`);
}
var arr = [1,2,3,4];
test(...arr);

/*
var arr1 = [12,34];
var arr2 = [56,78];
test(...arr1, ...arr2); // 이와같은 코드도 가능하다.
*/
```
출력값 : **1 2 3 4**  

## 화살표 함수

### 기본 형태 
익명함수인  `function(){}` 를 간단히 표현한 것으로  `() => {}` 와 같이 쓴다.  
그러나 화살표 함수의 경우 `this` 키워드의 의미가 다르다.  
구분|this의미
:--|:--
익명함수|함수 자체에 바인딩 되어있는 객체  
화살표함수|전역객체  

따라서 **화살표함수** 에서 쓰고자 하는 `this`의 경우 다른 변수로 **치환** 해서 사용해야 한다.  

만약 중괄호 내 **코드가 한줄** 이라면 **중괄호를 생략**하며 `return` 키워드를 **사용하지 않아도** 값을 반환한다.  

아래는 예시이다.  
```
const multiply = (a,b)=> a*b;
console.log(multiply(1,2));
console.log(multiply(3,4));
```
출력값 : **2 12**

----
----

## 옵션 객체 

함수의 매개변수로 **객체를 전달**하는 경우 이 때 매개변수를 **옵션객체** 라고 한다.  

이 객체를 초기화하기 위해서는 다음과 같이 한다.  
```
function test(options){
	options.valueA = options.valueA || 10;
	options.valueB = options.valueB || 20;

	console.log(options.valueA + ':' + options.valueB);
}

test({
	valueA : 52
});
```
출력값 > **52 : 20**

## 객체, 배열의 복사

자료형|복사형태|원본값변경시  
:--|--|:--
기본자료형(숫자, 문자열, boolean...)|깊은복사|원본만 변경
객체형 자료형(배열, 객체...)|얕은복사|원본 및 사본 둘 다 변경  

즉, 위 표를 코드로 구현하면 다음과 같다.
```
var orig = 10;
var newV = orig;

orig = 200;
console.log(`orig=${orig}, newV=${newV}`);
```
출력 > **orig=200, newV=10**  
즉, 원본만 변경된다.  

그러나 배열(객체) 의 경우는 다르다  
```
var oriArr = [1,2,3];
var newArr = oriArr;

oriArr[0] = 100;
console.log(`oriArr=${oriArr}, newArr=${newArr}`);
```
출력 > **oriArr=100,2,3, newArr=100,2,3**  
즉, 모든 값에 영향을 미친다.  
이 경우는 원본의 경우 메모리가 클 수 있기 때문에 일일히 복사를 하지 않고 해당 객체 또는 배열의 **주소만 참조(얕은복사)**하는 형식으로 복사가 되기 때문이다.  

따라서 배열(객체)를 복사할 시 **객체 내 요소를 하나하나 복사(깊은복사)**해야한다.  
```

function clone(obj){
	var output = [];
	for(var index in obj){
		output[index] = obj[index]; // 배열 및 객체의 값을 하나하나 
	}
	return output;
}
var oriArr = [1,2,3];
var newArr = clone(oriArr);
oriArr[0] = 100;
console.log(`oriArr=${oriArr}, newArr=${newArr}`);

var oldA = {
	a:'a',
	b:'b',
	toString : function(){
		for(var key in this){
			if(key != 'toString'){
				console.log(`${key}:${this[key]}`);
			}
		}
	}
};
var newA = clone(oldA);
oldA.a = 'AA';
oldA.toString();
newA.toString();
```
출력값 >   
**oriArr=100,2,3, newArr=1,2,3  
a:AA  
b:b  
a:a  
b:b**  
