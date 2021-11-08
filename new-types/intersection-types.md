# 교집합 타입 (Intersection Types)

`&` 연산자를 사용하여 교집합 타입 (Intersection type) 을 생성할 수 있다.

## 타입 체크 (Type checking)

`S & T` 타입은 동시에 `S`, `T` 타입을 사용할 수 있음을 표현 한다.

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

`A & B` 의 멤버는 `A`와 `B`의 모든 멤버를 포함한다.

`Resettable & Growable[String]` 타입은 `reset`, `add` 메소드 모두 가지고 있다.

`&` 연산자는 교환법칙(_commutative_)이 성립되므로 `A & B`와 `B & A`는 같은 의미이다.

만약 멤버가 `A`와 `B` 타입에 모두 존재한다면 `A & B` 는 `A`와 `B`의 교집합이다.

```scala
trait A:
  def children: List[A]

trait B:
  def children: List[B]

val x: A & B = new C
val ys: List[A & B] = x.children
```

`A & B`의 `children`은 `A`와 `B`의 `children`의 교집합이며 `List[A] & List[B]` 이다.

`List`는 공변적 (_covariant_) 이기 때문에 `List[A & B]`로 단순화 할 수 있다.

컴파일러는 어떻게 `List[A] & List[B]`을 `List[A & B]`로 어떻게 단순화 할 수 있을까?

정답은 컴파일러는 필요 없다.

`A & B`는 타입의 값에 대한 요구사항의 집합을 표현하는 타입일 뿐이기 때문이다.

값이 생성되는 시점에 상속된 모든 멤버가 정의되었는지 확인해야 한다.

따라서 `A`와 `B`를 상속하는 클래스 `C`를 정의한다면 해당 시점에 요구된 타입으로 `children` 메소드를 정의해야 한다.

```scala
class C extends A, B:
  def children: List[A & B] = ???
```

[More Detail](https://docs.scala-lang.org/scala3/reference/new-types/intersection-types-spec.html)
