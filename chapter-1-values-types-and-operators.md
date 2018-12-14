# Chapter 1 : VALUES, TYPES, AND OPERATORS

컴퓨터의 세상에서는 모든것이 데이터로 이루어져있으며 데이터가 아닌것은 존재할 수가 없다. 모든 데이터들은 아주 긴 `bits` 의 시퀀스로 저장이 된다. **`Bits`** 는 2개의 값으로 구분이 된다. \(일반적으로 0과 1로 구분이 된다. \)

**숫자 13을 `bits`로 표현하는 방법** :

```text
   0   0   0   0   1   1   0   1
 128  64  32  16   8   4   2   1
```

> 이진수의 각각 8, 4, 1 자리에 숫자 1로 표시한 부분을 다 더하여 숫자 13을 만든다.

## VALUES

평균적인 컴퓨터의 메모리는 300억 bits 규모의 데이터 저장소가 있으며 하드디스크에는 이에 몇배에 해당한다. 이런 방대한 양의 비트로 데이터 손실 없이 작업하기 위해서 데이터들을 `chunk` 라고하는 정보의 덩어리들로 나누게 된다.

JS에서는 이러한 `chunk` 들을 **`values`** 라고 부른다. 모든 `value` 들은 bits로 이루어져 있으며 저마다의 타입과 역할이 있다.

## NUMBERS

숫자의 형태로 이루어진 value 이다. 아래의 형태로 쓰인다.

```text
13
```

JS에서는 숫자값에 64 bits를 사용한다. 64개의 비트 패턴으로 숫자를 구현해야하기 때문에 제한적이라고 느낄수 있다. 하지만 이는 2^64 만큼의 다른 숫자를 만들수 있다는 뜻인데 18 뒤에 0이 18개가 붙는 숫자이다.

2^64 만큼의 정수를 표현할수 있는것은 아니며 일부는 소수점을 표현하는데 쓰이기 때문에 실제 표현할 수 있는 정수의 양은 최대 9조 정도가 된다.

9조까지의 숫자 계산은 언제나 정확성이 보장된다. 하지만 π 나 64비트로 다 표현할 수 없는 숫자의 계산은 정확도가 보장이되지 않는다.

### ARITHMETIC

`+` , `*` 와 같은 문자를 operators 라고 부른다. 숫자간의 연산을 하게하며 대표적인 **binary operator** 이다.

```text
100 + 4 * 11
```

> 연산의 순서는 수학과 동일하며 괄호 '\( \)' 를 통해서 우선순위의 변경이 가능하다.

사칙연산 외에도 프로그래밍에 자주 쓰이는 나머지를 구하는 연산자가 있는데 `%` 기호이다.

```text
314 % 100 = 14
144 % 12 = 0
```

> 위처럼 나누고 난 뒤의 나머지만 계산하여 산출하게 된다.

### SPECIAL NUMBERS

JS에서는 일반 숫자들처럼 행동하지 않는 특별한 숫자 값들이 존재한다.

```text
Infinity - Infinity = NaN
Infinity - 1 = Infinity
0 / 0 = NaN
```

> 모두 숫자타입이다. `Infinity`는 말 그대로 무한대를 의미하며 `NaN` 의 뜻은 'not a number' 이다. 논리적으로 숫자로서 계산될 수 없는 값들이 `NaN` 이라고 출력이 된다.

## STRING

String 타입은 text, 문자열을 나타낸다. 아래와 같은 형태로 표기된다.

```text
`Down on the sea`
"Lie on the ocean"
'Float on the ocean'
```

> 관례적으로 JS에서 일반 문자열은 \( ' \) 로 표기한다.

String 타입 역시 내부적으로는 bits들의 나열로 이루어져 있으며 JS는 Unicode 표준을 따른다. JS는 문자열 타입에 16 bits 를 사용한다. 이는 2^16 만큼의 다른 문자를 표현할 수 있다는 말인데 실제로 Unicode에는 거의 2배가량 많은 양의 문자들이 존재한다. 일부 이모티콘과 같은 문자는 JS에서 두개의 "character positions" 를 차지하게 되는데 이는 Chapter 5 에서 더 알아보도록 하겠다.

### **Backslash \( \ \)**

거의 모든것이 따옴표 사이에 존재할 수 있으나 일부 문자들은 존재할 수 없게 된다. 그 예시로 'Newlines' \( enter를 눌렀을 때 얻게되는 문자 \) 의 경우에는 \( \` \) 내부에 존재할 때만 온전이 사용할 수 있게 된다.

```text
`Hello
World`
```

이러한 문자들을 온전히 \( ", ' \) 사이에서 사용가능하게 하는 문자가 backslash \( \ \) 이다. backslash를 포함해서 backslash 뒤에 오는 문자는 특별한 의미가 있으며 이를 문자를 **escaping** 한다고 한다.

| Character | Meaning |
| :--- | :--- |
| \n | Newline |
| \t | Tab |
| \\ | \( \ \) Character |
| \" | \( " \) Character |
| ... | ... |

“_A newline character is written like "\n"._” 이라는 문장을 표현하기 위해서 아래와 같이 쓰게 된다.

```text
"A newline character is written like \"\\n\"."
```

### **String Concatenation**

String 타입은 숫자와 같이 나누기나 곱하기, 빼기 연산이 되지 않지만 `+` 문자를 통한 연산은 가능하다. 논리적으로 '더하기' 연산이 되는것은 아니며 문자열을 붙여주게 된다. concatenate 를 아래와 같이 표현할 수 있게된다.

```text
"con" + "cat" + "e" + "nate"
```

### **Backtick \( \` \)**

Backtick 으로 감싸진 문자열은 backtick안의 `${ }` 내부에서 문자열화 되는것을 피해갈 수 있는데 이를 'template literals' 라고 부른다. 내부에서 변수 호출 및 연산이 가능하게 되며 결과값이 문자열 형태로 반환이 된다.

```text
`half of 100 is ${100 / 2}`
```

> “_half of 100 is 50_” 이라는 값이 산출된다.

## UNARY OPERATORS

모든 연산자가 기호로 되어있는것은 아니다. 단어의 형태로 되어있는 연산자도 있으며 예를 들면 `typeof` 연산자가 되겠다.

```javascript
console.log(typeof 4.5)
// → number
console.log(typeof "x")
// → string
```

`console.log` 라는 함수를 통해서 값들을 출력시킬수 있으며 자세한 내용은 다음챕터에서 알아보겠다.

`+` 와 `*` 기호와같이 반드시 2개 이상의 값이 필요한 연산자를 **binary operator** 라고 부르며 `-` 와 `typeof` 같이 1개의 값 만으로 연산이 가능한 연산자를 **unary operator** 라고 부른다.

## BOOLEAN VALUES

`true` 와 `false` 값, 단 두개의 가능성만 가지는 타입이다.

### Comparison

boolean 타입의 값을 반환하는 코드를 작성해보자 :

```javascript
console.log(3 > 2)
// → true
console.log(3 < 2)
// → false
```

String 타입도 `<` 연산자를 통한 연산이 가능하다.

```javascript
console.log("Aardvark" < "Zoroaster")
// → true
console.log("Z" < "a")
// → true
```

> 알파벳 순서가 연산의 기준이 되며 소문자가 항상 대문자보다 크다.

JS에서 유일하게 1개의 값이 자신과 동일하지 않는다.

```javascript
console.log(NaN == NaN)
// → false
```

> `NaN` 은 nonsensical 한 연산의 결과에서 반환이 되는 값으로 다른 어떠한 nonsensical 한 결과와도 동일하지 않는다.

### Logical Operators

`&&` binary operator 를 통해 'AND' 연산이 가능하다.

```javascript
console.log(true && false)
// → false
console.log(true && true)
// → true
```

`||` binary operator 를 통해 'OR' 연산이 가능하다.

```javascript
console.log(false || true)
// → true
console.log(false || false)
// → false
```

`!` 은 unary operator 이며 'NOT' 연산을 하며 값의 반대값을 반환한다.

```javascript
console.log(!true)
// → false
console.log(!false)
// → true
```

### Precedence

논리 연산자와 다른 연산자를 함께 사용할 경우에는 굳이 괄호를 통해서 우선순위를 지정해줄 필요가 없다. `||` 와 같은 논리 연산자는 가장 낮은 우선순위를 가지게 되며 그다음으로는 `>` 와 그리고는 `+` 와 같은 순서로 연산이 된다. 아래 예시를 보자.

```javascript
1 + 1 == 2 && 10 * 10 > 50
```

### Ternary Operator

삼항연산자,  'conditional operator' 라고도 불린다. 아래와 같이 사용할 수 있다.

```javascript
console.log(true ? 1 : 2);
// → 1
console.log(false ? 1 : 2);
// → 2
```

> 첫번째 값이 `true` 이면 `:` 이전값을 반환하며 `false` 이면 `:` 이후의 값을 반환하게 된다.

## EMPTY VALUES

JS에서는 2개의 특별한 값이 있다. `undefined` 와 `null` 이다. 둘 다 'meaningful' 한 값이 없음을 표시할 때 사용이된다. 값이 없음 이라는 값으로 존재하며 아무런 정보도 가지고 있지 않는다.

두 값 모두 동일한 뜻을 가지고 있기 때문에 상호 호환이 가능하며 거의 대부분의 상황에서 동일하게 쓰인다.

```javascript
console.log(null == undefined);
// → true
console.log(null == 0);
// → false
```

주로 값을 잘못 불러오거나 의도치 않게 없는 값을 불러왔을 때 `undefined` 가 반환이 되며 의도적으로 없는 값을 변수에 할당할 때 `null` 을 할당하는 경우가 많음으로 굳이 두 값의 차이를 따지자면 의도적으로 하였느냐 아니냐가 될 수도 있겠다.

## AUTOMATIC TYPE CONVERSION

서로 같은 타입이 아닌 값들을 연산할 때 JS만의 특이한 방식이 있다. 이를 '강제 형변환' 이라고 부르며 아래가 그 대표적인 방식들이다.

```javascript
console.log(8 * null)
// → 0
console.log("5" - 1)
// → 4
console.log("5" + 1)
// → 51
console.log("five" * 2)
// → NaN
console.log(false == 0)
// → true
```

`0 == false` 와 `"" == false` 의 값은 `true` 이다. 이는 JS의 '강제 형변환' 이 발생했기 때문인데 이를 방지하기 위해서 `===` 혹은 `!==` 연산자를 사용한다. 

```javascript
console.log("" == false)
// → true
console.log("" === false)
// → false
```

강제 형변환을 예방할 수 있기 때문에 `===` 와 `!==` 연산을 사용하는것을 추천한다.

## SHORT-CIRCUITING OF LOGICAL OPERATORS

주로 왼쪽값이 없으면 오른쪽 값을 반환시킬 때 많이 사용된다. 바로 예시를 보자.

```javascript
console.log(null || "user")
// → user
console.log("Agnes" || "user")
// → Agnes
console.log(0 || -1)
// → -1
console.log("" || "!?")
// → !?
```

> 0 이나 "" 와 같이 false 와 대체되는 값들도 false로 적용이된다.

아래와 같은 상황이 발생하지 않게 유의해서 쓰도록 하자.

```javascript
console.log(true || X)
// → true
console.log((false && X) || Y)
// → Y
```

> 첫번째의 경우 `X` 가 어떤 값이던 간에 논리 연산자에 의해 반드시 `true` 가 반환이 되며, 두번째의 경우는 `X` 가 `false` 와 함께 쓰여 그 의미가 없어지게 된다.

