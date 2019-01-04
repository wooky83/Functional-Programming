# Functional Programming

## 함수형 프로그래밍이란?
- **함수형 프로그래밍은 자료 처리를 수학적 함수의 계산으로 취급하고 상태와 가변 데이터를 멀리하는 프로그래밍 패러다임의 하나이다.**
- y = f(x)

## 고계함수
- 다른 함수를 인수로 취하거나, 함수를 반환하는 함수를 고계 함수(higher-order function)이라고 부른다.
- ex) map, flatmap, filter, reduce 등

## 람다함수
- 익명함수라고도 하며 이름이 없는 함수를 뜻한다.
```swift
{
    (x: Int) -> Int in
    return x + 1
}
```
```scala
(x: Int) => x + 1
```

## 커링(currying)
- 커링이란 다중 인수 (혹은 여러 인수의 튜플)을 갖는 함수를 단일 인수를 갖는 함수들의 함수열로 바꾸는 것을 말한다.
- 커링(Currying)이라는 이름은 하스켈 커리(Haskell Curry)에서 이름을 가져온 것입니다.
1. f(x, y) = x + y 일때 h(x) = y -> f(x, y) 와 같다.
2. x에 1을 대입할때 g(y) = h(1) = y -> f(1, y) = 1 + y로 정의되고
3. y에 2를 대입할때 g(2) = f(1, 2) = 1 + 2라는 과정을 거쳐서 된다..
4. 즉 f(x, y)라는 다중인수는  h(x)와 g(y) 단일인수로 변경이 가능한 것이다. 아래는 Swift 예제이다.

```swift
func curry<X, Y, Z>(_ f:@escaping (X,Y)->Z)->(X)->(Y)->Z {
    return { X in { Y in f(X,Y) } }
}

func testCurry(a: Int, b: Int) -> Int {
    return a + b
}

let fy = curry(testCurry)(1)
let result = fy(2)  // result 3
```

## 대수적 자료형(Algebraic data type) :scream:
- 대수적 자료형(Algebraic data type)은 다른 자료형의 값을 가지는 자료형이다. 대체로 다른 자료형을 생성자로 감싸고 있다. 어떤 값도 대수적 자료형의 생성자의 인자가 될 수 있다. 반면에 다른 자료형은 생성자를 실행할 수 없으며 형식 일치(Pattern matching) 과정을 통해 생성자를 얻을 수 있다

## 참조투명성(referential transparency)
- 참조 투명성(referential transparency)은 부작용(side effect)이 없음을 표현하는 속성이다. 
- 입력 값에 대해 항상 같은 값을 돌려주는 것 외에 다른 기능이 없다. 내부적으로 관리되는 변수(상태)에 영향이 없는 것을 말한다. 
- 모든 프로그램에 대해 어떤 표현식(expression) e를 모두 그 표현식의 결과로 치환해도 프로그램에 아무 영향이 없다면 그 표현식 e는 참조에 투명하다(referentially transparent). 만약 어떤 함수 f(x)가모든 입력값 x에 대해 참조에 투명하면 그 함수 f는 순수하다.

## 타입 클래스(type class)
- TypeClass는 Haskell로부터 유래된 프로그래밍 패턴
- 타입 클래스는 어떤 행동을 정의해놓은 Interface의 일종
- 어떤 타입이 타입 클래스에 속한다면, 해당 타입은 타입 클래스가 서술하는 행동을 지원하고 수행한다는 의미

## 꼬리재귀
- 재귀함수 : 하나의 함수에서 자신을 다시 호출하여 작업을 수행하는 방식으로 주어진 문제를 푸는 방법
  - 장점 : 가독성
  - 단점 : 스택오버플로우
  ```swift
  func factorial(_ n: Int) -> Int {
    if n < 1 {return 0}
    return n + factorial(n - 1)
  }
  ```
- 꼬리재귀 : 재귀의 특별한 형태로 재귀호출로 받은 결과값을 추가로 계산하거나 처리하지 않고 그대로 Return 하는 형태
```swift
func tailFactorial(_ n: Int, _ acc: Int = 0) -> Int {
    if n < 1 {return acc}
    return tailFactorial(n - 1, acc + n)
}
```
* Swift
- 꼬리 재귀 최적화를 지원하지만, ARC에 의해 코드가 변경되면서 꼬리재귀 형태가 아닌것으로 바뀌고 따라서 꼬리 재귀 최적화의 지원을 못 받는 경우가 생길 수 있음
* Scala
- 스칼라에는 꼬리 재귀 최적화 기능을 가지고 있음
  @tailrec라고 재귀 함수 앞에 붙이면 스칼라 컴파일러에 꼬리재귀가 있으니 최적화 하라고 알려준다


## Lambda랑 Closure 차이

-  lambda : 익명 함수
-  Closure : 자신의 정의된 영역의 변수를 에워싸고(close over) 있는 것.또는, 자신이 정의된 영역의 변수에 접근할 수 있는 것. Closure를 사용하기 위해서 lambda(익명함수)가 사용됨
- 자신의 스코프 내에 있는 b라는 변수는 인자로 받은 변수이고 해당 스코프 내에 갇혀있지만, a라는 변수는 대체 어디서 와서 사용되고 있는지 알 수가 없다. 이때의 a를 자유 변수(Free variable), b를 묶인 변수(Bound variable) 라고 부른다. 아래의 람다식에서는 자유 변수와 묶인 변수를 하나씩 사용하고 있다. 람다식은 사용하는 변수의 종류에 따라 두 종류로 나눌 수 있다. 바로 닫힌 람다식(Closed expression) 과 열린 람다식(Open expression) 이다. 람다 표현식에서 사용하는 변수들이 모두 묶인 변수일 때 닫힌 람다식이라고 부른다. 그리고 람다 표현식에서 사용하는 변수들 중 하나라도 자유 변수가 있을 때 열린 람다식이라고 부른다. 자, 이제 클로저를 아주 간단하게 설명할 수 있다. 클로저는 바로 열린 람다식을 닫힌 람다식으로 만드는 것이다. 클로저의 이름이 어떻게 유래되었는지도 예상이 될 것이다. 클로저는 람다식 내의 모든 자유 변수를 스코프 내로 가져와 묶는다. 그렇기 때문에 클로저는 만들어진 환경을 기억하는 것처럼 보인다.
```swift
func adder(a: Int) -> (Int) -> Int {
    return { b -> Int in
        return a + b
    }
}

let add5 = adder(a: 5)
add5(10)
```


