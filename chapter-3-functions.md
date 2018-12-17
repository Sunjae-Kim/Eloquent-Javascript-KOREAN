# Chapter 3 :FUNCTIONS

## DEFININF A FUNCTION

Value 가 함수인 일반적인 binding 을 함수라고 칭한다. 아래의 예시를 살펴보자 :

```text
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

```text
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

```text
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

```text
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

```text
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

```text
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

```text
function square(x) {
  return x * x;
}
```

이것이 **Function declaration\(함수선언\)** 이다. Statement 가 `square` 라는 바인딩을 정의하고 이를 함수라고 가르킨다. 보다 쉽게 코드를 작성할 수 있으며 이렇게 선언되는 함수 뒤에는 \( ; \) 이 필요하지 않는다.

이러한 방식으로 함수정의에는 한가지 특이한 점이 있다.

```text
console.log("The future says:", future());
​
function future() {
  return "You'll never have flying cars";
}
```

함수가 정의되는 시점이 호출되는 시점보다 이후에 있어도 동작한다. 함수선언은 일반적인 위-아래 순서의 흐름이 아니다. 이렇게 선언되는 함수는 개념적으로 가장 처음의 scope 로 이동이 되며 해당 scope 에서의 모든 코드에서 사용될 수 있다. 이는 코드를 사용하기 전에 모든 기능을 미리 정의하지 않아도 되기 때문에 종종 유용하게 사용된다.

## ARROW FUNCTIONS

세번째 함수를 호출하는 방법을 살펴보겠다. 기존에 호출했던 방식들과는 많이 다른 형태를 띈다. `function` keyword 대신에 \( =&gt; \) 라는 문자가 사용된다.

```text
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

```text
const square1 = (x) => { return x * x; };
const square2 = x => x * x;
```

Arrow function 이 매개변수가 없다면 빈 괄호 `()` 뒤에 화살표가 온다.

```text
const horn = () => {
  console.log("Toot");
};
```

하나의 언어가 arrow function과 `function` expression 두가지 모두를 가지고 있어야 하는 깊은 이유는 없다. 일부 작은 디테일한 차이점을 Chapter 6 에서 다룰것이며 거의 같은 일을 한다고 보면 된다. 2015년에 화살표 기능이 주로 함수표현식을 간결하고 짧게 쓰기 위해서 추가되었다. Chapter 5 에서 많이 사용될 것이다.

