# 교차 타입 (Intersection Types)

`&` 연산자를 사용하여 교차 타입 (Intersection type) 을 생성할 수 있다.

## 타입 체크 (Type checking)

`S & T` 타입은 동시에 `S` , `T` 타입을 사용할 수 있음을 표현 한다.

```scala
trait Resettable:
  def reset(): Unit

trait Growable[T]:
  def add(t: T): Unit
  
def fun(x: Resettable & Growable[String]) =
  x.reset()
  x.add("first")
```

`fun` 함수의 파라미터 `x` 는 `Resettable` , `Growable[String]` 둘 다 구현 되어야 한다.

교차 타입 `A & B` 의 멤버는 `A`와 `B`의 모든 메법를 포함 한다. `Resettable & Growable[String]` 타입은 `reset`, `add` 메소드 모두 가지고 있다.

`&` 연산자는 가환성(Commutative)를 가지므로 `A & B`와 `B & A`는 같은 타입이다.

만약 멤버가 `A`와 `B` 타입에 모두 존재한다면 `A & B` 는 `A`와 `B`의 교집합이다.

```scala
trait A:
  def children: List[A]

trait B:
  def children: List[B]

val x: A & B = new C
val ys: List[A & B] = x.children
```

`A & B`의 `children` 타입은 `A`와 `B`의 `childeren`의 교집합이며 `List[A] & List[B]` 이다. `List`는 공변적 (covariant) 이기 때문에 `List[A & B]`로 단순화 할 수 있다.

