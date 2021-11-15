# 타입 람다 (Type lambdas)

타입 람다를 사용하면 타입 정의 없이 더 큰 종류의 타입(_higher-kinded type_) 직접 표현할 수 있다.

```scala
[X, Y] =>> Map[Y, X]
```

위의 예는 `X`, `Y` 인자를 `Map[X, Y]`에 매핑하는 이진 타입 생성자를 정의 한다.

타입 람다의 타입 매개변수는 바운드(_bound_)를 가질 수 있지만 공변성을 지정할 수는 없다.

아래와 같은 `Fuctor`에 `Either` 타입을 부분 적용하려고 하면 컴파일 에러가 발생한다.

```scala
trait Functor[F[_]] {
  def map[A, B](fa: F[A])(f: A => B): F[B]
}

type EitherFunctor1 = Functor[Either[String, _]] // 컴파일 에러
```

이 때 타입 람다를 사용하면 부분 적용이 가능하게 정의할 수 있다.

```scala
type EitherFunctor2 = Functor[[A] =>> Either[String, A]]
```

[More Details](https://docs.scala-lang.org/scala3/reference/new-types/type-lambdas-spec.html)
