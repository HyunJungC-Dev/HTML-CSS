## 변수(Variables)
+ ***어떤 특정한 값이 반복되는 경우, 변수로 정의하여 사용***할 수 있다.
+ 변수 이름 앞에는 항상 $가 붙는다.
+ 이는 사이트 전체에서 일관성을 유지할 때, 매우 유용할 수 있다.
+ 변수 선언방법
    > $변수이름: 속성값;
  
### 예시
아래와 같은 SCSS 코드에서 $가 앞에 붙은 $font-stack, $primary-color는 변수이다. 그 변수는 각각 Helvetica, sans-serif 와 #333 이라는 값을 가진다.  
이 변수를 이용해 body 안의 font 속성과 color 속성의 값을 할당할 수 있다.
    
SCSS:
```scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
    font: 100% $font-stack;
    color: $primary-color;
}
```
위 SCSS에서 정의된 내용은 SCSS 아래의 css 코드와 같이 컴파일이 된다.
CSS:   
```css
body {
    font: 100% Helvetica, sans-serif;
    color: #333;
}
```
  
## 변수 유효범위(Variable Scope)
+ 변수는 사용 가능한 유효범위가 있다.
+ 변수는 선언된 블록({}) 내에서만 유효범위를 가진다.
  
### 예시
만약 $font-stack, $primary-color 변수를 .div1{}안에서 선언했다면, .div1{}이 두 변수의 사용가능한 유효범위이다.  
   
SCSS:   
```scss
.div1 {
    $font-stack: Helvetica, sans-serif
    $primary-color: #333;
    font: 100% $font-stack;
    color: $primary-color;
}
```
따라서 해당 유효범위를 벗어난 .div2안에서 두 변수를 사용하려고 하면 Error가 발생한다.
   
SCSS:   
```scss
.div1 {
    $font-stack: Helvetica, sans-serif;
    $primary-color: #333;
    font: 100% $font-stack;
    color: $primary-color;
}

// Error
.div2 {
    font: 100% $font-stack;
    color: $primary-color;
    background: #aaa;
}
```
  
## 변수 재 할당(Variable Reassignment)
+ 이미 정의된 변수를 다른 변수의 값으로 할당하는 것을 말한다.
+ 변수에 변수를 할당하는 것이다.
  
### 예시
아래와 같이 $sea-blue, $emerald-green 이라는 색 변수에 각 색상 값을 할당하고, 그 색 변수들을 다시 $primary-color, $accent-color라는 변수에 할당하여 사용할 수 있다.
  
SCSS:   
```scss
$sea-blue: #3CB4FF
$emerald-green : #086522

$primary-color: $sea-blue
$accent-color: $emerald-green

body {
    background: $primary-color 
    color: $accent-color
}
```
   
## !global(전역 설정)
+ !global 플래그를 사용하여 변수의 유효범위를 전역(Global)로 설정할 수 있다.
   
### 예시
위에서 변수 유효범위 예시를 들 때, $font-stack과 $primary-color의 변수 유효범위가 .div1{}안이여서 .div2{}에서는 사용할 수 없었다.   
하지만 $font-stack과 $primary-color에 !global 속성을 준다면 유효범위가 전역으로 바뀌어 .div2에서도 사용할 수 있다.
   
SCSS:   
```scss
.div1 {
    $font-stack: Helvetica, sans-serif !global;
    $primary-color: #333 !global;
    font: 100% $font-stack;
    color: $primary-color;
}

// Error 안 남!
.div2 {
    font: 100% $font-stack;
    color: $primary-color;
    background: #aaa;
}
```
   
### 주의
첫번째 줄에서 $primary-color에 #ccc를 할당했는데 .div1{} 안에서 !global 플래그를 사용하여 $primary-color에 전역으로 #333을 할당하면, $primary-color가 #333 값이 되어 문제가 될 수 있다.   
즉, 변수가 적용될 때 가져오는 값은 가장 가까운 곳에서 마지막으로 선언된 값을 가져온다.
    
SCSS:   
```scss
$primary-color: #ccc;
.div1 {
    $primary-color: #333 !global;
    font: 100% $font-stack;
    color: $primary-color;
}

.div2 {
    color: $primary-color;
    background: #aaa;
}
```
   
## !default(초깃값 설정)
+ !default 플래그는 할당되지 않은 변수의 초깃값을 설정한다.
+ 즉, 할당되어 있는 변수가 있다면 변수가 기존 할당 값을 사용한다.
+ 변수와 값을 설정하겠지만, 혹시 기존 변수가 있을 경우 현재 설정하는 변수의 값은 사용하지 않겠다는 의미로 쓸 수 있다.
  예로 Bootstrap 같은 외부 Sass(SCSS) 라이브러리를 연결했을 때, 변수 이름이 같아 본인이 작성한 코드의 변수들이 덮어쓰기(overwrite)되거나, 그 반대의 경우에 문제가 된다. 

### 예시
아래에서 yellogreen 옆에 !default가 없다면, 원래 pink가 할당되어있던 $primary-color 변수의 값이 yellowgreen으로 바뀌어 background에 yellowgreen이 적용되었을 것이다.   
하지만 !default가 있다면, $primary-color에 원래 pink가 할당되어 있기 때문에 기본 pink값을 사용하게 된다.   
$primary-color가 이전에 할당된 값이 없다면 그때 !default에 의해 yellowgreen이 할당된다.
   
SCSS:      
```scss
$primary-color: pink;
.div1 {
    $primary-color: yellowgreen !default;
    background: $primary-color;
}
```
   
## #{} (문자보간)
+ #{}를 이용해서 코드의 어디에든 변수 값을 넣을 수 있다.
+ 즉, 사이에 변수가 들어갈 수 있는 공간을 만드는 것으로 볼 수 있다.
   
### 예시
Sass의 내장 함수인 문자에서 따옴표를 제거하는 unquote함수로 $family에 Droid+Sans라는 문자열을 할당한다.   
그리고 @import 시 url의 맨 뒤에 #{}를 사용해서 $family의 변수값을 넣어줄 수 있다.
   
SCSS:   
```scss
$family: unquote("Droid+Sans");
@import url("http:/fonts.googlepis.com/css?family=#{$family}");
```
   
그러면, css로 컴파일되었을 때, #{$family}위치에 Droid+Sans 값이 들어가게 된다.
```css
@import url("http:/fonts.googlepis.com/css?family=Droid+Sans")
```
   
## 가져오기(Import)
+ @import로 외부에서 가져온 Sass 파일은 모든 단일 CSS 출력 파일로 ***병합***된다.
+ CSS에서 사용되는 @import와 혼동하지 않게 주의가 필요하다.
   
### CSS에서 사용되는 @import 규칙
> @import url("가져오려고하는 css 파일의 경로");
   
### Sass에서 사용되는 @import 규칙
url을 사용할 필요가 없다.
> @import "경로";

### 예외 상황
Sass에서 @import는 기본적으로 Sass 파일을 가져오는데,   
CSS의 @import 규칙으로 컴파일 되는 예외 상황이 있다.
+ 파일 확장자가 .css 일 때
    > @import "style.css"
+ 파일 이름이 http://로 시작하는 경우
    > @import "http://style.com/style;
+ url()이 붙었을 경우
    > @import url(style);
+ @import를 사용하여 미디어 쿼리를 가져오는 경우
    > @import "sytle" screen;

## 여러 파일 가져오기
+ 하나의 @import로 여러 파일을 가져올 수 있다.
+ 각 파일 이름은 ,(쉼표)로 구분한다.
   
### 예시
,(쉼표)로 두 파일을 구분하여 import하는데  
아래와 같이 확장자가 없는 건 뒤에 .scss 혹은 .sass 가 붙어 있는 것을 의미한다.   
.sass는 ;(세미콜론)을 사용하지 않으므로 아래 두 파일은 .scss 파일을 의미함을 알 수 있다.
SCSS:   
```scss
@import "header", "footer";
```
   
## 파일 분할(Partials)
+ 프로젝트 규모가 커지면 header.scss 혹은 side-menu.scss 등과 같이 각 기능과 부분으로 나눠 유지보수가 쉽도록 관리하기도 한다.
+ 이처럼 파일이 점점 많아지게 되면, 모든 파일이 컴파일 시 각각의 .css 파일로 나눠져 저장된다면 관리나 성능 차원에서 문제가 될 수 있다.
+ 그래서 Sass는 Partials 기능을 제공한다.
+ _header.scss와 같이 ***파일 이름 앞에 _(언더바)를 붙여 @import로 가져오면 컴파일 시 .css 파일로 컴파일하지 않는 기능이다.***  
   
### 예시
header.scss, side-menu.scss, main.scss가 scss라는 폴더에 있는 상황일 때,   
아래와 같이 main.scss에서 header.scss와 side-menu.scss를 @import로 가져온다.
```scss
// main.scss에서
@import "header", "side-menu";
```
그리고 이 파일들을 css/ 디렉토리에 node-sass로 컴파일을 진행하면,   
scss/에 있던 header.scss, side-menu.scss, main.scss 파일들이   
css/안에 header.css, side-menu.css, main.css로 각각 컴파일된다.
     
이 문제는 @import하는 파일이름 앞에 _를 붙여 해결할 수 있다.
scss/ 폴더 안에 있는 scss 파일들 중 @import로 불러올 파일들인 header.scss와 side-menu.scss를 _header.scss, _side-menu.scss로 변경한다.   
   
그리고 똑같이 @import로 가져오면
```scss
// main.scss에서
@import "header", "side-menu";
```
main.css 안에 main.scss, _header.scss, _side-menu.scss가 모두 합쳐져 컴파일되어 css/ 폴더안엔 main.css 파일만 존재하게 된다.
  
***이처럼 _를 붙일 경우 별도의 파일로 컴파일 하지 않고 합쳐서 사용된다.***
> Webpack이나 Parcel, Gulp 같은 일반적인 빌드툴에서는 Partials 기능을 사용할 필요없이 설정된 값에 따라 빌드되는데, ***그래도 되도록 _를 사용할 것을 권장한다.***

## 연산(Operations)
+ Sass는 기본적인 연산 기능을 지원한다.
   
### 산술 연산자
+ &#43; 더하기
+ &#45; 빼기
+ &#42; 곱하기
    > 주의: 하나 이상의 값이 반드시 숫자(Number)여야 한다.
    > 10px * 10px 같이 단위가 붙어 있는 두 숫자끼리 곱하면 Error이다.
    > 10px * 10 = 100px 과 같이 사용해야 한다.
+ &#47; 나누기
    > 주의: 오른쪽 값이 반드시 숫자(Number)여야 한다.
    > 10px / 2px 같이 단위가 붙어 있는 숫자로 나누면 Error이다.
    > 10px / 2 = 5px 과 같이 사용해야 한다.
+ &#37; 나머지
   
### 비교연산자
아래의 연산자를 사용해서 비교한 값이 참이면 True가 나오고, 거짓이면 False가 나온다.
+ == 동등하다
+ != 동등하지 않다.
+ &#60; 대소비교/ 왼쪽이 오른쪽보다 작다.
+ &#62; 대소비교/ 왼쪽이 오른쪽보다 크다.
+ &#60;= 대소 및 동등비교/ 왼쪽이 오른쪽보다 같거나 작다.
+ &#62;= 대소 및 동등비교/ 왼쪽이 오른쪽보다 같거나 크다.
   
### 논리(불린, Boolean) 연산자
+ and 그리고 
    왼쪽과 오른쪽의 조건을 모두 만족해야한다.
+ or 또는
    왼쪽 혹은 오른쪽의 조건 중 하나라도 만족하면 된다.
+ not 부정 
    조건의 결과를 True면 False로 False면 True로 뒤집는다.

### 숫자(Numbers)
#### 상대적 단위 연산
상대적 단위인 %, em, vw 등의 연산의 경우 순수 CSS 함수인 ***CSS calc()***로 연산해야 한다.
   
```css
width: 50% - 20px // 단위 모순 에러(Incompatible units error)
width: calc(50% - 20px) // 연산 가능
```
#### 나누기 연산의 주의사항
CSS에서 속성값의 숫자를 분리하는 방법으로 /를 허용하기 때문에 /가 나누기 연산으로 사용되지 않을 수 있다.
   
SCSS:   
```scss
div{
    width: 20px + 20px; // 더하기
    height: 40px - 10px; // 빼기
    font-size: 10px * 2; // 곱하기
    margin: 30px / 2; //나누기
}
```
위에서 모든 속성에 / 나누기 연상을 사용하여 값을 할당했는데   
margin의 경우 여러 속성값을 받는 속성이라 30px과 2를 각 속성값의 숫자를 분리하는 방법으로 이해하여 컴파일 된 것을 볼 수 있다.
   
CSS:   
```css
div{
    width: 40px; /* 계산 잘 됨.*/
    height: 30px; /* 계산 잘 됨.*/
    font-size: 20px; /* 계산 잘 됨.*/
    margin: 30px / 2; /* margin의 각 속성값으로 들어가버린다.*/

}
```

### /를 나누기 연산 기능으로 사용하기 위한 조건
+ 값 또는 그 일부가 변수에 저장되거나 함수에 반환되는 경우
    > $x: 100px;
    > width: $x / 2
+ 값이 ()로 묶여있는 경우
    > height: (100px / 2);
+ 값이 다른 산술 표현식의 일부로 사용되는 경우
    > font-size: 10px + 12px / 3;

### 문자(Strings)
+ 문자 연산에는 &#43; 가 사용된다.
+ 문자 연산의 결과는 첫번째 피연산자를 기준으로 한다.
+ 첫번째 피연산자에 따옴표가 붙어있다면 연산 결과를 따옴표로 묶는다.
    > content: "Hello" + World; 면
    > content: "Hello World" 로 컴파일 된다.
+ 반대로 첫번째 피연산자에 따옴표가 붙어있지 않다면 연산 결과로 따옴표를 처리하지 않는다.
    > flex-flow: row + "-reverse" + " " + wrap 이면
    > flex-flow: row-reverse wrap; 으로 컴파인된다.


## 참고자료
+ [sass-lang 사이트](https://sass-lang.com/guide)