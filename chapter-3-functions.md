# Chapter 3 : FUNCTIONS

## DEFINING A FUNCTION

Value 가 함수인 일반적인 binding 을 함수라고 칭한다. 아래의 예시를 살펴보자 :

```javascript
const square = function(x) {
  return x * x;
};
​
console.log(square(12));
// → 144
```

> 위의 코드는 `squre` 라는 숫자의 제곱을 반환해주는 함수를 정의한다.

`function` 이라는 keyword 를 통해서 함수가 생성이 된다. 함수에는 **parameters\(매개변수\)** 와 **body** 가 있으며 body 에는 함수가 호출되었을 때 실행되는 statement 가 작성된다. 함수 body 는 안의 statement 가 1개 뿐이어도 언제나 중괄호 `{}` 로 감싸져 있어야 한다.

함수에는 여러개의 매개변수가 존재할 수도 있으며 존재하지 않을수도 있다 :

```javascript
const makeNoise = function() {
  console.log("Pling!");
};
​
makeNoise();
// → Pling!
​
const power = function(base, exponent) {
  let result = 1;
  for (let count = 0; count < exponent; count++) {
    result *= base;
  }
  return result;
};
​
console.log(power(2, 10));
// → 1024
```

> `makeNoise` 함수는 매개변수가 없는 반면 `power` 함수는 2개를 받는다.

`power` 와 `square` 함수와 같이 값을 반환하는 함수가 있는가 하면 `makeNoise` 와 같이 side effect 만 산출하는 함수도 존재한다. `return` statement 가 함수의 반환값을 결정하게 된다. `return` statement 뒤에 아무런 expression 도 존재하지 않을 경우 해당 함수는 `undefinded` 를 반환하게 된다. 마찬가지로 `makeNoise` 와 같이 `return` statement 가 없는 함수역시 `undefined` 를 반환한다.

매개변수는 일반적인 binding 과 동일하게 동작하지만 초기화 값은 함수 내부에서 지정되는 것이 아니라 **함수 호출자가 지정한다**.

## BINDINGS AND SCOPES

각 바인딩에는 **scope\(범위\)**가 있다. block 의 밖에서 선언된 binding 의 scope 는 전체 프로그램이다. 해당 binding 을 어디에서나 호출 가능하다는 뜻이다. 이를 **global** 이라고 부른다.

반면에 함수의 매개변수로 선언되거나 혹은 함수 body 내에서 선언되는 함수는 해당 함수 내에서만 참조할 수 있으며 이는 **local** binding 이라고 부른다. 함수가 매번 호출될 때마다 새로운 local binding 들이 생성이 되며 이는 같은 함수가 호출이 되더라도 해당 함수만의 고유 binding 으로 독립적으로 존재한다.

`let` 과 `const` 로 선언된 바인딩은 해당 바인딩이 선언된 블록에 대해서 local binding 이기 때문에, 반복문 내부에서 바인딩 하나를 생성하면 반복문 전후의 코드가 이를 볼 수 없다. 2015년 이전 JS 에서는 함수만이 새로운 scope 를 만들 수 있었기 때문에, `var` 키워드로 만든 이전 방식의 binding 은 모든 함수, 또는 함수에 없는 경우 글로벌 범위에서 볼 수 있었다.

```javascript
let x = 10;
if (true) {
  let y = 20;
  var z = 30;
  console.log(x + y + z);
  // → 60
}
// y is not visible here
console.log(x + z);
// → 40
```

scope 는 주변의 scope 를 '관찰' 할 수 있다. 만약 동일한 이름의 여러개의 binding 이 있다면 어떻게 될까? 이 경우에는 가장 안쪽에 있는 binding 을 인식하게 된다. 아래의 예시를 살펴보자.

```javascript
const halve = function(n) {
  return n / 2;
};
​
let n = 10;
console.log(halve(100));
// → 50
console.log(n);
// → 10
```

> global `n` 이 선언이 됐어도 매개변수로 받은 `n` 이 가장 안쪽의 scope 이기 때문에 `halve` 의 반환 값은 매개변수로 받은 `n` 을 계산한 값이 된다.

### NESTED SCOPE

JS 는 글로벌 바인딩과 로컬 바인딩만 구별하지 않는다. 블록과 함수는 다른 블록과 함수 안에서 생성될 수 있으며, 여러겹의 인접성이 생성될 수 있다.

아래의 예시를 살펴보자 :

```javascript
const hummus = function(factor) {
  const ingredient = function(amount, unit, name) {
    let ingredientAmount = amount * factor;
    if (ingredientAmount > 1) {
      unit += "s";
    }
    console.log(`${ingredientAmount} ${unit} ${name}`);
  };
  ingredient(1, "can", "chickpeas");
  ingredient(0.25, "cup", "tahini");
  ingredient(0.25, "cup", "lemon juice");
  ingredient(1, "clove", "garlic");
  ingredient(2, "tablespoon", "olive oil");
  ingredient(0.5, "teaspoon", "cumin");
};
```

`ingredient` 함수에서 외부에서 호출된 `hummus` 의 매개변수를 사용하는 모습을 볼 수 있다. 그러나 해당 함수의 local binding 들은 외부에서 호출된 함수에서는 볼 수 없다.

Block 안에서 볼 수 있는 binding 의 집합은 프로그램 텍스트에서 해당 블록 위치에 따라 결정된다. 각 local scope 는 이를 포함하는 모든 local scope 도 볼 수 있으며, 모든 scope 는 global scope 를 볼 수 있다. 이러한 형태로 binding 의 가시성에 접근하는것을 **lexical scoping** 이라고 한다.

## FUNCTIONS AS VALUES

함수 binding 은 일반적으로 프로그램의 특정 부분에 대한 이름 역할을 한다. 이러한 binding 은 한번 정의되면 절대 변하지 않는다.

하지만 그 둘은 다르다. 함수 값은 다른 값이 할 수 있는 모든 것을 할 수 있다. 단순히 그것을 호출만 하는 것이 아니라 임의의 expression 으로 사용할 수 있다. 함수 값을 새로운 바인딩에 저장하고 이를 함수에 대한 매개변수로서 전달하는 등의 작업이 가능하다. 마찬가지로, 함수를 유지하는 바인딩은 여전히 일반적인 바인딩이며, 상수가 아니라면 다음과 같이 새로운 값을 할당할 수 있다.

```javascript
let launchMissiles = function() {
  missileSystem.launch("now");
};
if (safeMode) {
  launchMissiles = function() {/* do nothing */};
}
```

Chapter 5 에서 함수 값을 다른 함수로 전달하여 수행할 수 있는 재밌는 일들을 살펴볼 것이다.

## DECLARATION NOTATION

함수 바인딩을 만드는 보다 짧은 방법이 있다. statement 의 시작에 `funcion` keyword 를 사용하면 조금 다르게 작동한다.

```javascript
function square(x) {
  return x * x;
}
```

이것이 **Function declaration\(함수선언\)** 이다. Statement 가 `square` 라는 바인딩을 정의하고 이를 함수라고 가르킨다. 보다 쉽게 코드를 작성할 수 있으며 이렇게 선언되는 함수 뒤에는 \( ; \) 이 필요하지 않는다.

이러한 방식으로 함수정의에는 한가지 특이한 점이 있다.

```javascript
console.log("The future says:", future());
​
function future() {
  return "You'll never have flying cars";
}
```

함수가 정의되는 시점이 호출되는 시점보다 이후에 있어도 동작한다. 함수선언은 일반적인 위-아래 순서의 흐름이 아니다. 이렇게 선언되는 함수는 개념적으로 가장 처음의 scope 로 이동이 되며 해당 scope 에서의 모든 코드에서 사용될 수 있다. 이는 코드를 사용하기 전에 모든 기능을 미리 정의하지 않아도 되기 때문에 종종 유용하게 사용된다.

## ARROW FUNCTIONS

세번째 함수를 호출하는 방법을 살펴보겠다. 기존에 호출했던 방식들과는 많이 다른 형태를 띈다. `function` keyword 대신에 \( =&gt; \) 라는 문자가 사용된다.

```javascript
const power = (base, exponent) => {
  let result = 1;
  for (let count = 0; count < exponent; count++) {
    result *= base;
  }
  return result;
};
```

화살표가 매개변수 뒤에 오고 가 다음 함수의 body 가 나타난다. 해당 매개변수가 해당 함수의 body 를 생성한다는 듯한 표현방식이다.

만약 매개변수가 1개밖에 없다면 매개변수를 감싸고 있는 괄호 `()` 생략이 가능하다. 만약 함수 body 가 single expression 이며 해당 expression 을 반환 할 것이라면 함수를 싸고 있는 `{}` 생략도 가능하다. 아래와 같은 코드로 작성할 수 있다 :

```javascript
const square1 = (x) => { return x * x; };
const square2 = x => x * x;
```

Arrow function 이 매개변수가 없다면 빈 괄호 `()` 뒤에 화살표가 온다.

```javascript
const horn = () => {
  console.log("Toot");
};
```

하나의 언어가 arrow function과 `function` expression 두가지 모두를 가지고 있어야 하는 깊은 이유는 없다. 일부 작은 디테일한 차이점을 Chapter 6 에서 다룰것이며 거의 같은 일을 한다고 보면 된다. 2015년에 화살표 기능이 주로 함수표현식을 간결하고 짧게 쓰기 위해서 추가되었다. Chapter 5 에서 많이 사용될 것이다.

## THE CALL STACK

함수들을 통한 control flow 는 다소 복잡하다. 아래의 예시를 통해서 살펴보도록 하자 :

```text
function greet(who) {
  console.log("Hello " + who);
}
greet("Harry");
console.log("Bye");
```

> `greet` 함수의 호출로 인해 control 은 2번째 라인으로 점프한다. control 은 이제 `console.log` 함수가 로 넘어가고 작업이 다 끝나면 다시 2번째 라인으로 복귀한다. `greet` 함수가 종료되면 호출이 되었던 4번째 라인으로 다시 점프하여 다시 `console.log` 로 넘어간다. 해당작업이 종료되면 프로그램은 종료된다.

control 의 흐름을 아래와 같이 표현할 수 있겠다.

```text
not in function
   in greet
        in console.log
   in greet
not in function
   in console.log
not in function
```

함수에서 return 이 일어나면 반드시 호출이 되었던 원래 자리로 돌아가야 한다. 위의 예시에서는 `console.log` 가 끝난뒤에 `greet` 함수로 복귀하는 상황도 있었고 두번째에서는 바로 프로그램을 종료했다.

컴퓨터가 이 context 를 저장하는 곳이 **call stack** 이다. 함수가 호출이 될 때마다 가장 최근 context 는 해당 stack 의 최상단에 저장이 된다. 함수가 return 되었을 때 최상단의 context 를 제거하고 해당 context 를 사용하여 프로그램을 진행시킨다.

이러한 stack 을 저장하기 위해서는 컴퓨터의 메모리가 필요하다. stack 이 너무 커지게 되면 "out of stack space" 혹은 "too much recursion" 이라는 메세지와 함께 에러가 발생된다. 컴퓨터에게 두 함수 사이에 무한반복을 일으키는 정말 어려운 질문을 던지는 예시를 살펴보도록 하자 :

```text
function chicken() {
  return egg();
}
function egg() {
  return chicken();
}
console.log(chicken() + " came first.");
// → ??
```

> 만약 컴퓨터의 메모리가 무한대라면 이 함수도 무한대로 동작할 것이다. 그렇지 않기 때문에 공간이 부족할 것이고 stack 이 터지게 될것이다.

## OPTIONAL ARGUMENTS

아래 함수는 아무런 문제 없이 작동할 것이다 :

```javascript
function square(x) { return x * x; }
console.log(square(4, true, "hedgehog"));
// → 16
```

`square` 에 1개의 매개변수를 받도록 정의했으나 함수 호출 시 3개의 매개변수를 넘겼음에도 JS 는 아무런 불만없이 이를 잘 실행한다. 여분의 매개변수는 알아서 무시하고 주어진 매개변수로만 계산하기 때문이다.

JS 는 함수에 전달하는 매개변수의 개수에 대해 굉장히 관용적이다. 너무 많이 전달하게 되면 여분의 매개변수는 무시를 하며, 부족하게 전달하게 되면 전달 받지 못한 값들에 대해서는 `undefined` 를 할당한다.

이러한 작동방식의 단점은 만약 실수로 잘못된 숫자를 함수의 매개변수로 넘길 수 있는것이며 넘긴 매개변수가 잘못됬는지 모를수도 있다는 것이다.

장점은 함수 호출 시 매개변수의 개수를 다르게 적용시켜도 된다는 점이다. 아래의 예시를 살펴보겠다 :

```javascript
function minus(a, b) {
  if (b === undefined) return -a;
  else return a - b;
}

console.log(minus(10));
// → -10
console.log(minus(10, 5));
// → 5
```

> 만약 매개변수가 1개만 넘어오면 그 숫자에 `-` unary operator 만 적용시키고 2개가 넘어오면 둘의 차이를 구하게 되는 함수이다.

함수 정의 시 매개변수 뒤에 `=` 연산자와 expression 을 선언한다면 함수 호출 시 해당 매개변수가 입력되지 않아도 `=` 뒤의 expression 이 기본값으로 대체될 것이다. 아래의 예시를 살펴보자 :

```javascript
function power(base, exponent = 2) {
  let result = 1;
  for (let count = 0; count < exponent; count++) {
    result *= base;
  }
  return result;
}

console.log(power(4));
// → 16
console.log(power(2, 6));
// → 64
```

다음 챕터에서는 함수 호출 시 입력받는 매개변수가 몇개가 되던지 전부 받아서 함수 body 에서 처리할 수 있는 방법을 다뤄보도록 하겠다. 이러한 함수는 넘어오는 매개변수가 아무리 많아도 다 핸들링 할 수 있기 때문에 굉장히 유용할 수 있으며 `console.log` 와 같은 함수가 이와 같은 함수가 되겠다 :

```javascript
console.log("C", "O", 2);
// → C O 2
```

## CLOSURE

함수를 value 로서 다루게 되면 함수를 호출할 때마다 지역변수들이 다시 생성된다. 만약 해당 함수호출이 종료가 되면 해당 지역변수들은 어떻게 되는가? 아래의 예시로 알아보자 :

```javascript
function wrapValue(n) {
  let local = n;
  return () => local;
}
​
let wrap1 = wrapValue(1);
let wrap2 = wrapValue(2);
console.log(wrap1());
// → 1
console.log(wrap2());
// → 2
```

> `wrapValue` 라는 지역변수를 생성하는 함수를 정의한다. 해당 함수는 지역변수에게 접근하여 반환할 수 있는 함수를 반환하게 된다.

이는 매번 함수가 호출될 때마다 새로운 지역변수가 해당 함수 내에 생성이 되며 다른 함수로 하여금 해당 지역변수가 영향을 받지 않는다는 좋은 예시가 되겠다.

이처럼 특정 scope의 특정한 지역변수를 참조할 수 있는 특징을 **closure** 라고 부른다. 특정 local scope 의 특정 binding 를 참조할 수 있는 함수 1개를 1개의 closure 라고 부른다. 이러한 동작은 바인딩의 lifetime 을 걱정하지 않아도 되게 할 뿐만 아니라 함수의 value 를 창의적인 방법으로 사용할 수 있게 한다.

조금의 변화를 통해 이전 예제를 임의의 양으로 곱하는 함수를 반환하는 함수를 만들 수 있다.

```javascript
function multiplier(factor) {
  return number => number * factor;
}
​
let twice = multiplier(2);
console.log(twice(5));
// → 10
```

이런 프로그램을 만들기 위해서는 많은 연습이 필요하다. 함수를 만드는데 좋은 모델은 함수의 value 가 body 에 있는 코드와 만들어진 environment 까지 모두 생각하는 것이다. 호출될 때, 함수의 body 는 생성된 환경을 보지만, 호출되는 환경을 보지 않는다.

예시에서는 `multiplier` 함수가 호출되고 `factor` 매개변수가 2로 묶이는 환경이 생성이 된다. `multiplier` 를 통해서 반환되는 함수 값은 해당 환경을 기억한다. 그리고 호출되었을 때 기억하고 있는 환경을 통해서 2와 매개변수로 받은 값을 곱하여 반환하게 된다.

## RECURSION

특정 함수가 자기 자신을 너무 자주 호출하여 stack 을 초과하여 문제를 일으키지 않는다면 자기 자신을 호출하는 것은 전혀 문제가 되지 않는다. 이렇게 함수가 자기 자신을 호출하게 되는 과정을 `recursive` 라고 부른다. 재귀는 함수들을 다른 스타일로 작성할 수 있다. 아래의 예시를 살펴보자 :

```javascript
function power(base, exponent) {
  if (exponent == 0) {
    return 1;
  } else {
    return base * power(base, exponent - 1);
  }
}
​
console.log(power(2, 3));
// → 8
```

이러한 방식은 수학자들이 거듭제곱을 정의하는 방식과 비슷하며 그 개념을 looping variant 보다 더 명확하게 설명한다. 해당 함수는 반복된 곱셈을 완료하기 위해 지속적으로 자기 자신을 여러 번 호출한다.

그러나 이렇게 구현하는 것에는 한가지 문제가 존재한다. JS 에서 재귀를 호출하는 것은 반복문을 사용하는 것 보다 거의 3배가량 느려지기 때문이다. 단순 반복문을 돌리는것이 함수를 여러번 호출하는것 보다 효율적이라는 것이다.

이러한 속도와 정석 사이의 딜레마는 굉장히 흥미로운 포인트이다. 일종의 기계친화적과 사람친화적의 차이라고 보면 되겠다. 거의 대부분의 모든 프로그램은 길고 복잡한 코드로 더 빠른 효율을 나타낼 수 있기 때문에 프로그래머는 이 사이의 적당한 밸런스를 잘 맞춰야 한다.

`power` 함수의 예시는 반복문의 예시가 더 명료하고 쉽게 다가온다. 그러나 종종 프로그래밍에서는 프로그램을 보다 쉽게 만들기 위해 효율을 어느 정도 포기하는 것이 도움이 되는 복잡한 개념을 다룬다.

효율성에 너무 집중하려다 보면 방해가되기 마련이다. 이것은 프로그램 디자인을 복잡하게 만드는 또 다른 요인이고, 이미 어려운 작업을 하고 있을 때, 신경써야하는 다른부분을 놓칠 수 있다.

그렇기 때문에 항상 정확하고 쉽게 이해할 수 있는 코드를 먼저 작성해야한다. 대부분의 코드가 상당한 시간이 걸릴 정도로 자주 실행되지 않지만 만약 이 방법의 효율이 너무 떨어진다고 생각이 된다면 나중에 다시 개선하도록 하자.

재귀함수는 언제나 반복문의 비 효율적인 대안이 되지는 않는다. 일부 문제는 재귀함수를 사용했을 때 훨씬 쉽게 해결이 될 수 있다. 대부분 이러한 문제들은 여러 갈래로 분기하여 탐색하거나 처리해야 하는 문제인데, 각 분기는 훨씬 더 많은 분기로 다시 나누어질 수 있다.

문제 하나를 생각해보자. 숫자 1부터 시작해서 반복적으로 5를 추가하거나 3을 곱하여서 무한한 수의 숫자가 생성될 수 있다. 만약 숫자가 주어지고 그 숫자를 생성하는 더하기 및 곱하기의 시퀀스를 찾는 함수를 만들어야 한다면 어떻게 작성할 수 있는가?

예를 들어보자, 숫자 13은 처음에 3으로 한번 곱해지고 5가 두번 더해진 결과값일 수 있다. 반면 15라는 숫자는 전혀 도달할 수 없다.

아래 재귀 함수로 푼 풀이를 보자 :

```javascript
function findSolution(target) {
  function find(current, history) {
    if (current == target) {
      return history;
    } else if (current > target) {
      return null;
    } else {
      return find(current + 5, `(${history} + 5)`) ||
             find(current * 3, `(${history} * 3)`);
    }
  }
  return find(1, "1");
}
​
console.log(findSolution(24));
// → (((1 * 3) + 5) * 3)
```

> 해당 프로그램은 반드시 가장 짧은 셋을 찾아야 할 필요는 없으며 조건에 만족하는 셋이 나오면 해당 셋을 반환하게 된다.

함수 내부의 `find` 함수가 실제 재귀하게 된다. 2개의 매개변수를 받는데 각각 current number 와 어떻게 해당 숫자에 도달하게 되었는지의 기록이다. 만약 해답을 찾게 된다면 어떻게 target 에 도달하게 되었는지 기록을 반환하게 된다. 만약 해답을 발견하지 못했다면 `null` 값을 반환하게 된다.

이렇게 동작하기 위해서 함수는 총 3가지 행동을 수행하게 된다.

> 1. 만약 현재 값이 타겟과 동일하다면 현재까지의 기록을 반환하게 된다.
> 2. 만약 현재값이 타겟보다 크게 된다면 더이상 더하기나 곱셈을 하더라도 타겟에 도달하지 못하기 때문에 `null` 을 반환하게 된다.
> 3. 마지막으로 만약 현재값이 타겟보다 작게 된다면 재귀를 총 두번하게 된다. 각각 5를 더하는 것과 3을 곱하는 것이다. 만약 처음 호출되었던 재귀가 `null` 이 아닌것을 반환한다면 해당 값이 반환될 것이며 그렇지 않다면 두번째 재귀가 무엇을 반환하던 간에 해당 값이 반환될 것이다.

이해를 조금 더 돕기 위해 숫자 13 이 되는 과정을 함수가 어떻게 찾아내는지 아래의 흐름에서 알아보자 :

> ```text
> find(1, "1")
>   find(6, "(1 + 5)")
>     find(11, "((1 + 5) + 5)")
>       find(16, "(((1 + 5) + 5) + 5)")
>         too big
>       find(33, "(((1 + 5) + 5) * 3)")
>         too big
>     find(18, "((1 + 5) * 3)")
>       too big
>   find(3, "(1 * 3)")
>     find(8, "((1 * 3) + 5)")
>       find(13, "(((1 * 3) + 5) + 5)")
>         found!
> ```

indentation 은 call stack 의 깊이를 나타낸 것이다. 최초 \( 1 + 5 \) 부터 시작하여 재귀를 시작한다. 재귀를 통해 목표 수보다 작거나 같은 수를 산출하는 모든 분기를 탐색할 수 있다. 목표에 맞는 수를 찾지 못했기 때문에 `null` 을 반환하고 다시 첫 번째 호출로 다시 돌아온게 된다. 거기서 `||` 연산자를 통해서 \( 1 \* 3 \) 호출이 된다. 즉, 첫 번째 재귀호출이 또 다른 재귀호출을 통해 목표 숫자를 탐색한다. 가장 안쪽의 호출은 문자열을 반환하고, 중간 호출의 각 연산자는 해당 문자열을 전달하며, 결국 답을 반환한다.

## GROWING FUNCTIONS

함수가 프로그램에 도입되는 두 가지 정도의 자연스런 방법이 있다.

첫번째 방법으로는 비슷한 코드를 여러번 반복하는 것이다. 아마 이 방법을 좋아하진 않을 것이다. 코드가 많아질수록 실수를 숨길 수 있는 공간이 더 넓어지고 코드를 이해하려고 하는 사람들은 읽을 자료가 더 많아진다는 것을 의미한다. 그래서 반복되는 기능에 이름을 짓고 함수로 선언한다.

두번째 방법으로는 먼저 사용될 기능들을 통해 걸맞는 함수이름 먼저 짓고 기능에 대한 코드를 채우는 것이다. 함수 자체를 정의하기 전에 해당 함수를 사용하는 코드를 작성할 수도 있다.

함수에 맞는 이름을 통해서 포장하고자 하는 개념이 얼마나 명확한지 잘 보여주는 것은 어려운 일이다. 예시를 한 번 살펴보자.

두개의 숫자를 출력하는 프로그램을 만들것이다. 숫자는 각각 소와 닭의 마릿수를 나타낼 것이며 이를 `Cows` 와 `Chickens` 로 지정하고 앞에 숫자 0 을 붙여서 항상 3자리 수로 유지하려고 한다.

```text
007 Cows
011 Chickens
```

함수에 총 두개의 매개변수가 입력이 될 것이며 이는 각각 소와 닭의 마릿수가 되겠다. 코딩을 시작해보자 :

```javascript
function printFarmInventory(cows, chickens) {
  let cowString = String(cows);
  while (cowString.length < 3) {
    cowString = "0" + cowString;
  }
  console.log(`${cowString} Cows`);
  let chickenString = String(chickens);
  while (chickenString.length < 3) {
    chickenString = "0" + chickenString;
  }
  console.log(`${chickenString} Chickens`);
}
printFarmInventory(7, 11);
```

`.length` 를 string expression 뒤에 붙이게 되면 해당 string expression 의 길이가 반환된다. 따라서, 루프는 최소 3글자 길이까지 숫자 문자열 앞에 0을 계속 추가한다.

프로그램이 완성이 되었다. 이 프로그램을 의뢰한 농장주에게 보내기 바로 전 농장주는 돼지까지 같이 셀 수 있는 프로그램을 요청하였다.

단순히 `Pigs` 라는 이름으로 동일한 작업을 반복하기 전에 뭔가 더 좋은 방법이 있지 않을까 생각해본다.

아래의 코드처럼 쓸 수 있을것 같다 :

```javascript
function printZeroPaddedWithLabel(number, label) {
  let numberString = String(number);
  while (numberString.length < 3) {
    numberString = "0" + numberString;
  }
  console.log(`${numberString} ${label}`);
}
​
function printFarmInventory(cows, chickens, pigs) {
  printZeroPaddedWithLabel(cows, "Cows");
  printZeroPaddedWithLabel(chickens, "Chickens");
  printZeroPaddedWithLabel(pigs, "Pigs");
}
​
printFarmInventory(7, 11, 3);
```

잘 작동한다. 하지만 `printZeroPaddedWithLabel` 라는 이름이 조금 이상하다. printing 하고 0을 padding 하고 label을 붙이는 것을 하나의 함수에 다 붙여 넣었다.

한개의 반복되는 함수에 여러개의 개념을 다 붙이지 말고 차근차근 시작해보자 :

```javascript
function zeroPad(number, width) {
  let string = String(number);
  while (string.length < width) {
    string = "0" + string;
  }
  return string;
}
​
function printFarmInventory(cows, chickens, pigs) {
  console.log(`${zeroPad(cows, 3)} Cows`);
  console.log(`${zeroPad(chickens, 3)} Chickens`);
  console.log(`${zeroPad(pigs, 3)} Pigs`);
}
​
printFarmInventory(7, 16, 3);
```

`zeroPad` 라는 명확한 함수이름은 코드를 읽으려고 하는 사람의 이해를 도울 수 있다. 뿐만 아니라 이러한 함수는 단지 해당 프로그램 뿐만 아니라 다른 특정 프로그램에서도 사용될 수 있다. 예를 들면 잘 정렬되는 숫자표를 만들 때에도 사용될 수 있을 것이다.

함수는 얼마나 똑똑하고 다재다능해야 하는가? 우리는 3글자 정도로만 패딩할 수 있는 아주 단순한 함수를 소숫자, 음숫자, 소수점 정렬, 다른 글자로 된 패딩 등과 같은 복잡한 일반화된 숫자 포맷 시스템에 이르기까지 무엇이든 쓸 수 있는 함수로 까지 만들 수 있다.

정말로 해당 기능이 필요하기 전 까지는 딱 필요한 부분만 구현하는게 좋다. 모든 기능을 아우러서 적용할 수 있는 'framework' 를 만들지 말자. 절대 사용할 일 없는 코드를 만들고 있을수도 있으며 정작 진짜로 필요한 작업을 완료하지 못할 것이다.

## FUNCTIONS AND SIDE EFFECTS

함수는 대개 side effect 를 위해 호출되거나 특정 value 를 반환하기 위해 호출되는것으로 나뉘어 질 수 있다. 물론 두가지를 다 수행하는 함수도 존재한다.

위 예시에서의 첫번째 `printZeroPaddedWithLabel` 함수는 문장을 출력하는 side effect 를 위해 호출됐었다. 두번째 `zeroPad` 함수는 값을 반환하기 위해 호출 됐었다. 두번째 함수가 첫번째보다 더 유용하게 사용되는 것은 결코 우연이 아니다. 값을 반환하는 함수는 직접적으로 side effect 를 수행하는 함수보다 더 새로운 방식으로 응용되기 쉽다.

순수한 함수\(pure function\) 은 side effect 가 없을 뿐더러 다른 코드에 대한 의존성 없이 특정 가치를 생산할 수 있다. 예를 들면 값이 바뀔 수 있는 global binding 을 사용하지 않는 것이다. 순수한 함수는 같은 매개변수로 호출 되었을 때 항상 같은 값을 생산하는 특성을 가지고 있다. 그 외에는 어떠한 작업도 하지 않는다. 이러한 함수의 호출을 해당 함수의 반환값만으로 대체할 수 있다. 이러한 순수함수가 제대로 작동하는지 확신하지 않을 때 단순이 함수를 호출하여 테스트할 수 있으며, 해당 context 에서 제대로 작동한다면 어떤 상황에서든 작동한다는 것을 알 수 있다. 반대로 nonpure function 은 테스트를 위해서 많은 재료가 필요하다.

하지만 순수함수가 아닌 함수를 사용하는것이 나쁜것은 아니며 코드에서 삭제하기 위해서 큰 노력을 할 필요는 없다. side effect 는 종종 유용하게 쓰인다. 예를 들자면 `console.log` 가 있겠다. 순수함수로는 구현할 방법이 없는 함수이다. 또한 어떤 작업은 side effect 를 사용할 때 더 효과적으로 표현할 수 있기 때문에 컴퓨팅 속도 때문에 순수함수를 사용하지 않을 수도 있다.  


