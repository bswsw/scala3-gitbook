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

