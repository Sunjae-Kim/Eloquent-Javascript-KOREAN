# Chapter 2 : PROGRAM STRUCTURE

## EXPRESSION AND STATEMENTS

값을 만들어내는 코드를 **표현식\(expression\)** 이라고 부른다.

표현식이 하나의 문단이라고 한다면 JS 의 **statement** 는 한개의 문장에 속한다.

이러한 statement들로 이루어진게 하나의 프로그램이다.

```text
1;
!false;
```

> 가장 단순한 형태의 statement 는 expression 뒤에 \( ; \) 이 오는 형식인다. 위와 같은 예시가 하나의 프로그램이라고 할 수 있다.

프로그램 내부적으로 변화를 주는 statement들이 있다. 이 변화를 **side effects** 라고 부른다.

어떤 상황에서는 \( ; \) 을 statement 의 마지막에 붙이지 않아도 문제가 발생하지 않는다. 하지만 \( ; \) 을 붙이지 않았을 때 새로운 statement 에 영향을 끼칠 수 있는 일도 있으며 예기치 못한 에러를 발생 시킬 수 있는 여지가 있기 때문에 항상 statement 의 마지막에는 \( ; \) 을 붙여주도록 하자.

## BINDINGS

### 변수 선언하기

JS는 생성한 값을 잡아두고 저장하기 위해 **binding** 또는 **variable \(변수\)** 라는 것을 제공한다.

```text
let caught = 5 * 5;
```

> `caught` 라는 변수를 선언하였고 `5*5` 의 결과값을 부여했다.

위의 `let` 은 JS 의 `keyword` 이며 binding 을 정의할 것이라는것을 나타낸다. binding 뒤에 단어를 적어서 이름을 부여하고 `=` 연산자를 통해 바로 값을 부여한다.

변수를 정의하고 난 뒤로는 해당 변수를 하나의 expression으로서 사용할 수 있게 된다.

```text
let ten = 10;
console.log(ten * ten);
// → 100
```

### 변수 재할당

변수는 한번 선언이 되면 그 값이 영원히 묶이는것이 아니다. `=` 연산자를 통해서 언제나 기존의 값을 끊고 새로운 값을 할당할 수 있다.

```text
let mood = "light";
console.log(mood);
// → light
mood = "dark";
console.log(mood);
// → dark
```

변수를 하나의 '박스' 보다는 하나의 '촉수'라고 생각하여 값을 묶고 있다고 생각하는게 더 올바른 표현이다. 변수는 값을 담고있지 않으며 묶고있다고 생각하면 된다. \(두개의 변수가 한개의 값을 잡을 수 있다.\)

```text
let luigisDebt = 140;
luigisDebt = luigisDebt - 35;
console.log(luigisDebt);
// → 105
```

### 상수

`const` 와 `var` 라는 키워드 역시 `let` 과 함께 binding을 생성하는 용도로 사용한다.

```text
var name = "Ayda";
const greeting = "Hello ";
console.log(greeting + name);
// → Hello Ayda
```

`var` 은 'variable' 의 줄임말이며 "pre-2015 JavaScript" 에서 사용되었다. `let` 과 거의 동일하게 사용될 수 있으며 더 정확한 차이점은 다음 챕터에서 다루도록 하겠다.

`const` 는 'constant' 의 줄임말로 말 그대로 변하지 않는 상수를 선언할 때 사용한다. 주로 이름을 부여하거나 변하지 않는 값을 binding 할 때 쓴다.

## BINDING NAMES

거의 모든 단어가 binding의 이름이 될수 있다. 하지만 몇가지 규칙이 있으며 함께 살펴보도록 하겠다.

1. 숫자로 시작할 수 없다.
2. \( $ \) 혹은 \( \_ \) 을 제외한 다른 특수문자는 사용될 수 없다.
3. `let` 과 같은 JS 키워드나 예약어는 사용될 수 없다.

   Binding 이름으로 사용될 수 없는 목록 :

   ```text
   break case catch class const continue debugger default
   delete do else enum export extends false finally for
   function if implements import interface in instanceof let
   new package private protected public return static super
   switch this throw true try typeof var void while with yield
   ```

   > 전부 기억할 필요는 없으며 위의 이름으로 binding 이름을 설정하려고 했을 시 예기치못한 문법 오류가 발생하여 알게될 것이다.

## THE ENVIRONMENT

주어진 시간대에 존재하는 binding 의 모음과 그 값들을 **environment\(환경\)** 이라고 부른다.

프로그램이 시작 됐을 때 environment는 비어있지 않으며 language standard나 각종 시스템과 상호작용하기 위한 binding들이 기본으로 존재하게 된다.

하나의 예시로 브라우저에서 사용자의 마우스나 키보드를 읽을 수 있는 함수들이 존재하는 것 처럼 말이다.

## FUNCTIONS

environment 에 있는 많은 value 들의 타입은 **function\(함수\)** 이다. 함수는 프로그램이 하나의 value 에 잘 싸여진 형태라고 보면 된다. 이러한 value 들을 프로그램을 **실행** 시킨다.

아래는 `propmpt` 라는 binding이 가지고 있는 function을 실행한 예시이며 사용자를 통해 입력받을 수 있는 박스를 보여준다.

```text
prompt("Enter passcode");
```

![](https://eloquentjavascript.net/img/prompt.png)

함수를 실행시키는 행동을 **invoking**, **calling** 혹은 **applying** 한다고 한다.

### arguments

함수 binding 뒤에 괄호 `()` 를 붙여주는 것으로 해당 함수를 실행할 수 있다. 괄호 안의 값들은 함수 내부의 프로그램들에게 주어진다. 예를들면 `prompt("Enter passcode");` 의 부분에서 따옴표 안의 내용이 함수에 주어져서 박스안에서 텍스트로 사용이 된다. 이러한 값들을 **arguments\(매개변수\)** 라고 부른다. 함수마다 가질수 있는 매개변수의 개수나 타입은 다를 수 있다.

## THE CONSOLE.LOG FUNCTION

여러 예시들에 `console.log` 라는 함수를 출력값을 표현하는데 사용하였다. 거의 모든 JS 시스템 \(모든 현대 웹 브라우저 와 Node.js 포함\) 에서 제공되는 함수이며 매개변수를 텍스트로 출력을 해준다. 브라우저에서는 JS 콘솔에 출력이 되며 이는 기본값으로 보여지지 않게 숨겨져있다. 브라우저의 관리자 도구를 통해서 출력이 되는것을 확인할 수 있다.

Binding 이름에는 `.` 을 포함할 수 없지만 `console.log` 는 일반적으로 선언했던 binding 이 아니기 때문에 가능한 일이다. `console` 이라는 binding 에 있는 `log` 라는 property를 가져온 형태인데 Chapter 4 에서 더 자세히 알아보도록 하겠다.

## RETURN VALUES

`propmt()` 를 통해서 dialog box 를 보여주거나 텍스트를 입력하는 일은 하나의 **side effect** 이다. 이런 side effect 를 생산하기 때문에 많은 함수들은 유용하게 사용이 된다. 함수들은 값들을 만들어내기도 하며 어떤 상황에서는 side effect 가 굳이 없어도 유용하게 사용이 된다.

예를 들면 `Math.max` 라는 함수는 2개의 매개변수를 받아 둘 중에서 더 큰 숫자를 준다.

```text
console.log(Math.max(2, 4));
// → 4
```

함수가 값을 생성하는 것을 값을 **return\(반환\)** 한다고 표현한다. 모든 값을 생성하는 것을 JS 에서는 expression 이라고 하며 더 큰 표현식에서 함수호출을 사용할 수 있다는 뜻이 된다.

아래의 `Math.min` 이라는 `Math.max` 와 반대로 작동하는 함수를 더하기 expression의 일부로 사용해보겠다.

```text
console.log(Math.min(2, 4) + 100);
// → 102
```

다음 Chapter에서 어떻게 함수를 만들고 사용하는지 다루도록 하겠다.

## CONTROL FLOW

Program 에서 1개 이상의 statement 가 존재하게 될 때 statement 들은 위에서 아래 순서로 실행되게 될 것이다. 아래의 예시를 확인해보자.

```text
let theNumber = Number(prompt("Pick a number"));
console.log("Your number is the square root of " +
            theNumber * theNumber);
```

> 첫번째 statement에서 숫자를 입력받은 다음 두번째로 해당 숫자를 묶어둔 binding을 사용하여 숫자의 제곱을 출력해주는 프로그램이다.

`Number` 라는 함수는 타입을 숫자로 변환해준 뒤 반환하는 함수이다. `prompt` 함수로 입력받은 값은 string 타입으로 반환이 되기 때문에 곱하기 연산을 위해서는 숫자로 변환해줄 필요가 있기 때문이다. 비슷한 함수들로 `String` 과 `Boolean` 등이 있으며 모두 해당 타입으로 변환시켜주는 함수이다.

Straight-line control flow 를 표현한 간단한 도식을 한번 그려보겠다.

![](https://eloquentjavascript.net/img/controlflow-straight.svg)

## CONDITIONAL EXECUTION

모든 프로그램들의 흐름이 한방향으로 이어지지는 않는다. 흐름을 분기해야하는 상황이 생길 수 있으며 상황에 맞게 프로그램을 분기해야 하는데 이를 **conditional execution\(조건문\)** 이라고 한다.

![](https://eloquentjavascript.net/img/controlflow-if.svg)

### if

조건문은 JS에서 `if` keyword 를 통해서 생성한다. 한개의 조건을 통과했을때만 실행이 되는 간단한 예시를 예시를 살펴보자.

```text
let theNumber = Number(prompt("Pick a number"));
if (!Number.isNaN(theNumber)) {
  console.log("Your number is the square root of " +
              theNumber * theNumber);
}
```

> 입력받은 정보가 숫자일 때에만 해당 숫자를 제곱하여 출력하는 함수를 만들어 보았다. 만약 숫자가 아닌 'parrot' 등과 같은 문자를 입력했을 때는 아무런 출력도 보여지지 않을 것이다.

`if` 는 괄호안에 주어진 Boolean expression 에 따라서 statement 를 실행하거나 실행하지 않고 통과한다.

`Number.isNaN` 함수는 `NaN` 값이 들어왔을 때만 `true` 를 반환하는 JS 의 기본 함수이다. 첫번째 statement 의 `Number` 함수를 통해서 생성된 binding 의 값이 `NaN` 이 아닐때에만 실행한다는 조건문이 되겠다.

`if` 문 다음에 나오는 statement 는 중괄호 `{}` 를 통해 묶여져있다. 중괄호는 여러개의 statement를 하나의 statement 로 묶을 때 사용되며 이를 **block** 이라고 표현한다. 위의 예시의 경우에는 `if` 뒤에는 이미 1개의 statement 만 존재하기 때문에 중괄호를 생략해도 상관이 없다. 아래와 같은 예시처럼 사용할 수 있는것이다.

```text
if (1 + 1 == 2) console.log("It's true");
// → It's true
```

### else

`if` 조건에 부합하지 않았을 때의 코드로 분기해보자. 이 상황은 위 도식에서의 두번째 화살표로 표현이 되겠다. `else` keyword 를 통해서 분기할 수 있으며 `if` 와 함께 두가지 상황으로 나누는 예시를 보자.

```text
let theNumber = Number(prompt("Pick a number"));
if (!Number.isNaN(theNumber)) {
  console.log("Your number is the square root of " +
              theNumber * theNumber);
} else {
  console.log("Hey. Why didn't you give me a number?");
}
```

만약 2개 이상의 조건을 두고 싶다면 여러개의 `if else` 조합을 사용할 수도 있다 :

```text
let num = Number(prompt("Pick a number"));
​
if (num < 10) {
  console.log("Small");
} else if (num < 100) {
  console.log("Medium");
} else {
  console.log("Large");
}
```

> 숫자가 10 보다 작은지 먼저 확인할 것이며 그렇다면 'Small' 을 출력하고 종료할 것이다.
>
> 그렇지 않다면 두번째 제어문으로 들어가 숫자가 100 보다 작은지 확인하고 그렇다면 'Medium' 을 출력하고 종료할 것이다.
>
> 그렇지 않다면 모든 조건에 부합하지 않기 때문에 'Large' 를 출력하고 종료할 것이다.

위 코드의 schema 는 아래와 같이 표현할 수 있겠다.

![](https://eloquentjavascript.net/img/controlflow-nested-if.svg)

## WHILE AND DO LOOPS

0과 12 사이의 모든 짝수를 출력하는 프로그램을 작성해보자 :

```text
console.log(0);
console.log(2);
console.log(4);
console.log(6);
console.log(8);
console.log(10);
console.log(12);
```

잘 작동할 것이다. 하지만 프로그래밍을 하는 이유는 일을 덜 하기 위해서지 더 많은 일을 하기 위해서가 아니다. 만약 1000 까지 숫자의 모든 짝수를 구해야 한다면 위의 방법으로는 쉽지않을 것이다. 지금 필요한 것은 특정 코드를 여러번 호출하는 것이며 이를 **loop\(반복문\)** 이라고 부른다.

![](https://eloquentjavascript.net/img/controlflow-loop.svg)

반복문을 작성하게 되면 프로그램을 어느 지점으로 되돌아가게한 뒤 프로그램을 다시 반복시키는 작업이 가능하게 된다. 만약 이 작업을 counting 되는 binding과 함께 사용한다면 아래의 예시와 같이 사용할 수 있을것이다.

```text
let number = 0;
while (number <= 12) {
  console.log(number);
  number = number + 2;
}
// → 0
// → 2
//   … etcetera
```

`while` keyword 를 통해서 반복문을 작성할 수 있으며 `()` 안의 expression 을 조건으로 받는데 `if` 뒤에 나오는 조건과 같은 방법으로 사용할 수 있다. 괄호 안의 expression 이 `true` 로 변환될 수 있는 값이라면 반복이 지속된다.

위의 `number` 라는 binding 을 통해 프로그램의 진행정도를 추적할 수 있다. 반복이 진행될 때마다 `number` 는 이전 값에서 2를 더한 값을 얻게 되며 반복이 시작될 때 12보다 적은지 비교하게 된다. 만약 12보다 커지게 되면 프로그램은 종료하게 된다.

다음은 2^10 을 구하는 다른 예시를 한 번 살펴보자 :

```text
let result = 1;
let counter = 0;
while (counter < 10) {
  result = result * 2;
  counter = counter + 1;
}
console.log(result);
// → 1024
```

> 결과 값 및 counter 2개의 binding 을 사용하였다. 위의 예시에서는 0 부터 시작하여 카운팅을 시작하였는데 왜 이렇게 하였는지 Chapter 4 에서 더 살펴보도록 하겠다.

`do` 반복문은 `while` 반복문이랑 거의 동일하다고 보면 된다. 유일한 차이점이 있다면 `do` 반복문은 조건에 상관없이 최소한 한번은 무조건 statement 가 실행이 된다는 것이다.

```text
let yourName;
do {
  yourName = prompt("Who are you?");
} while (!yourName);
console.log(yourName);
```

> 반복문 종료 조건인 `yourName` 변수가 뒤에 있기 때문에 최소 1회는 무조건 이름을 입력받게 만드는 프로그램이다. 만약 아무것도 입력 받지 않게 되면 `""` 이 반환이 되는데 이는 `true` 로 변환되지 않기 때문에 이름을 입력받을 때 까지 계속해서 반복이 진행 될 것이다.

## INDENTING CODE

지금까지 작성한 예시들에서 큰 statement 안에 존재하는 statement 들 앞에는 항상 공백을 붙였다. 공백이 요구되는건 아니며 실제로 컴퓨터가 프로그램을 읽을 때 공백이 존재하지 않아도 잘 작동한다. 심지어 행 바꿈 역시 선택적인 사항이다. 코드를 작성할 때 1줄의 아주 긴 코드를 작성해도 상관은 없다.

하지만 이러한 block 내부의 indentation 은 코드의 구조를 쉽게 눈에 띄게하기 위함이다. Block 안에서 또 새로운 block 이 생성되었을 때 시작과 끝을 구분하지 못하는 상황이 발생 할 수 있다. Indentation 을 적합하게 사용하면 보여지는 program 과 block 내부의 모습을 일치하게 된다.

새로운 block 뒤에 공백 2개를 선호하는 사람과 4개를 선호하는 사람이 있으며 `\t` 문자를 선호하는 사람도 있다. 중요한 것은 모든 새로운 block 뒤에는 동일한 크기의 공백이 생겨야 한다는 것이다.

```text
if (false != true) {
  console.log("That makes sense.");
  if (1 < 2) {
    console.log("No surprise there.");
  }
}
```

## FOR LOOPS

많은 반복문 들은 위에서 사용한 `while` 반복문과 동일한 형태를 띈다. 먼저 반복문을 지속 추적하는 `counter` binding 을 생성하고 `while` 문을 통해서 `counter` 를 확인하며 조건에 도달할 때 까지 반복을 지속하는 것이다.

이런 패턴은 굉장히 흔하기 때문에 JS 와 다른 비슷한 언어들에서는 더 짧고 간편하게 사용할 수 있는 `for` 반복문을 제공한다. 대부분에 상황에서 `while` 반복문보다 짧고 간결하게 사용할 수 있다.

```text
for (let number = 0; number <= 12; number = number + 2) {
  console.log(number);
}
// → 0
// → 2
//   … etcetera
```

> `while` 반복문에서 봤던 예시와 완벽하게 동일하다. 차이점은 모든 반복을 위한 statement 를 `for` 문 뒤에 묶어서 선언한 것이다.

`for` 문 뒤의 괄호에는 반드시 2개의 \( ; \) 가 존재해야한다. 괄호 안에는 3개의 파트가 존재하며 각각 :

1. 반복문을 생성한다. \(대부분 binding 을 선언한다.\)
2. 반복문이 계속 진행이 될지 판단하는 조건 expression 을 작성한다.
3. 반복이 진행되고 난 뒤 반복문의 업데이트를 진행한다.

아래에서 2^10 의 값을 구하는 반복문을 `for` 문으로 구현해보겠다.

```text
let result = 1;
for (let counter = 0; counter < 10; counter = counter + 1) {
  result = result * 2;
}
console.log(result);
// → 1024
```

## BREAKING OUT OF A LOOP

`false` 값이 생겨야만 반복문이 끝날 수 있는것은 아니다. `break` 라는 특별한 statement 로 즉시 반복문에서 빠져나갈 수 있는 방법이 있다. 아래의 예시를 살펴보자 :

```text
for (let current = 20; ; current = current + 1) {
  if (current % 7 == 0) {
    console.log(current);
    break;
  }
}
// → 21
```

> `current` 가 20 보다 같거나 크고 7로 나누어 질 수 있는 조건이 통과만 된다면 'break' 하는 프로그램이다. `%` 연산자를 사용하면 해당 숫자가 다른 숫자를 통해 나누어 떨어질 수 있는지 쉽게 확인할 수 있다.

해당 프로그램의 경우에는 `for` 반복문의 2번째 파트가 비어있기 때문에 `break` 가 실행되지 않는다면 까지는 무한으로 실행이 될 것이다. 무한루프는 대부분의 경우에서 문제가 된다.

`continue` 키워드는 `break` 와 비슷하게 반복문에 영향을 주는 keyword 이다. `continue` 가 실행되면 해당 반복을 중단한 뒤 loop body 로 돌아가 다음 반복을 진행하게 된다.

## UPDATING BINDINGS SUCCINCTLY

특히 반복문에서 자주 볼 수 있는 형태로 하나의 binding 이 해당 binding 의 이전 값을 기반으로 새로운 값이 할당되어야 하는 경우가 종종 있다.

```text
counter = counter + 1;
```

JS 에는 위 코드의 shortcut 이 존재한다.

```text
counter += 1;
```

비슷하게 다른 연산자들도 위와 같은 연산이 가능하다.

```text
result *= 2;
counter -= 1;
```

이러한 방식으로 위에서 본 counting 예시를 다시 작성해보겠다.

```text
for (let number = 0; number <= 12; number += 2) {
  console.log(number);
}
```

단순하게 1 증감은 아래의 코드로 더 짧게 사용할 수 있다.

```text
counter++;
counter--;
```

## DISPATCHING ON A VALUE WITH SWITCH

아래와 같은 코드가 보기드문 코드는 아닐것이다 :

```text
if (x == "value1") action1();
else if (x == "value2") action2();
else if (x == "value3") action3();
else defaultAction();
```

`switch` 라고 불리는 더 직접적으로 조건에 부합하는 값에 접근하기 위해 만들어진 구조가 존재한다. 하지만 이를 위한 JS 의 문법은 보기가 조금 힘들며 오히려 `if` 문이 보기가 더 나을수도 있다.

```text
switch (prompt("What is the weather like?")) {
  case "rainy":
    console.log("Remember to bring an umbrella.");
    break;
  case "sunny":
    console.log("Dress lightly.");
  case "cloudy":
    console.log("Go outside.");
    break;
  default:
    console.log("Unknown weather type!");
    break;
}
```

`switch` 뒤의 괄호 안의 값이 `case` 뒤에 나오는 값이라면 해당 block 을 수행한다. 만약 모든 조건에 부합하지 않는다면 `default` 뒤의 block 을 수행한다. `break` 가 발견되지 않으면 바로 다음 라인으로 넘어갈 수 있기 때문에 의도적으로 그렇게 설계한 것이 아니라면 `break` 를 block 끝에 꼭 넣어주도록 하자.

## CAPITALIZATION

Binding 의 이름에는 공백이 허용되지 않으나 많은 경우 여러개의 단어를 합쳐서 binding 의 이름을 짓는 경우가 많다. 이름을 짓는 방법은 아래와 같으며 단순히 코드를 작성하는 사람의 선택에 달렸다 :

```text
fuzzylittleturtle
fuzzy_little_turtle
FuzzyLittleTurtle
fuzzyLittleTurtle
```

JS 의 기본 함수 및 대부분의 JS 프로그래머들은 모두 마지막 방법을 따르고 있다. 일종의 convention 이며 한개의 naming style 로 통일되어 있지 않으면 코드를 읽기가 쉽지 않다.

어떤 상황에서는 첫번째 글자가 대문자일 때도 있다 \( 예를 들면 `Number` 함수 \). 이는 해당 함수를 생성자로서 표시한다는 뜻인데 생성자가 무엇인지에 대해서는 Chapter 6 에서 다시 다루도록 하겠다.

## COMMENTS

코드 그 자체로는 프로그램이 전달하고자 하는 모든 정보를 사람이 완벽하게 이해할 수 없을 수 있다. 일부 상황에서는 어떤 생각들을 프로그램의 일부로서 포함시키고 싶을 때도 있다. 이를 위한게 **comment\(주석\)** 이다.

주석은 텍스트들로 이루어져 있으나 프로그램이 실행될 때 완벽하게 무시되는 부분이다. 한 행에 주석을 달아보도록 하자.

```text
let accountBalance = calculateBalance(account);
// It's a green hollow where a river sings
accountBalance.adjust();
// Madly catching white tatters in the grass.
let report = new Report();
// Where the sun on the proud mountain rings:
addToReport(accountBalance, report);
// It's a little valley, foaming like light in a glass.
```

> \( // \) 뒤에 텍스트를 달면 되는 형식이다.

여러 라인을 아우르는 주석을 작성해보도록 하자.

```text
/*
  I first found this number scrawled on the back of an old notebook.
  Since then, it has often dropped by, showing up in phone numbers
  and the serial numbers of products that I've bought. It obviously
  likes me, so I've decided to keep it.
*/
const myNumber = 11213;
```

> `/* */` 내부에서 작성하면 되며 행 바꿈을 포함한 모든 문자를 주석처리한다.

