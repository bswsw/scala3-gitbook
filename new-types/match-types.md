# 매치 타입 (Match Types)

매치 타입은 검사 대상에 따라 오른쪽 타입 중 하나의 타입으로 매칭 된다.

```scala
type Elem[X] = X match
  case String      => Char
  case Array[t]    => t
  case Iterable[t] => t
```

위 예는 아래와 같이 타입이 정의할 수 있다.

```scala
Elem[String]      =:= Char
Elem[Array[Int]]  =:= Int
Elem[List[Float]] =:= Float
Elem[Nil.type]    =:= Nothing
```

> `=:=`는 왼쪽과 오른쪽이 상호 하위 유형이라는 의미이다.

일반적으로 매치 타입은 다음과 같은 형식을 가진다.

```scala
S match { P1 => T1 ... Pn => Tn }
```

`S1`, `T1`, ..., `Tn`은 타입이고 `P1`, ..., `Pn`은 타입 패턴이다.

패턴의 타입 변수는 평소와 같이 소문자로 시작한다.

매치 타입은 재귀적으로 타입 정의를 할 수 있다.

```scala
type LeafElem[X] = X match
  case String      => Char
  case Array[t]    => LeafElem[t]
  case Iterable[t] => LeafElem[t]
  case AnyVal      => X
```

재귀 매치 타입 정의에는 상위 바운드 지정이 가능하다.

```scala
type Concat[Xs <: Tuple, +Ys <: Tuple] <: Tuple = Xs match
  case EmptyTuple => Ys
  case x * xs     => x *: Concat[xs, Ys]
```
