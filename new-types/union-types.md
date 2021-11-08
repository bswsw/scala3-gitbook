# 공용체 타입 (Union Types)

공용체 타입(_union type_) `A | B`는 `A`, `B` 타입의 모든 값(_value_)을 가지는 타입이다.

```scala
case class UserName(name: String)
case class Password(hash: Hash)

def help(id: UserName | Password) =
  val user = id match
    case UserName(name) => lookupName(name)
    case Password(hash) => lookupPassword(hash)
  ...
```

공용체 타입은 교집합 타입(_intersection type_)과 마찬가지로 교환 법칙(_commutative_)이 성립하기 때문에 `A | B`는 `B | A`와 같다.

컴파일러는 공용체 타입을 명시한 표현식에만 공용체 타입으로 할당 한다. 아래 예제를 통해 확인할 수 있다.

```scala
scala> val password = Password(123)
val password: Password = Password(123)

scala> val name = UserName("Eve")
val name: UserName = UserName("Eve")

scala> if true then name else password
val res2: Object = UserName(Eve)

scala> val either: Password | UserName = if true then name else password
val either: Password | UserName = UserName(Eve)
```

`res2`의 타입은 `Object & Product` 타입이며 `UserName`과 `Password`의 부모 클래스이다.

하지만 `Password | UserName` 타입은 `res2`의 부모 클래스가 아니기 때문에 `res2`를 `help` 함수의 인자로 사용할 수 없다.

`either`의 타입은 명시적으로 `Password | UserName` 타입으로 명시하였기 때문에 `help` 함수의 인자로 사용할 수 있다.

[More details](https://docs.scala-lang.org/scala3/reference/new-types/union-types-spec.html)
