# Scalaz 7 Upgrade Notes

## Dependencies
* Dependencies are split up

## Package Structure
* import scalaz._, Scalaz._ still works for some things but it's better to restrict imports to those that you need to prevent conflicts
* import scalaz.syntax.xyz._ (e.g. import scalaz.syntax.applicative._)
* import scalaz.syntax.std.xyz._ (e.g. import scalaz.syntax.std.list._)

## Validation and \/
* Is not a monad.
* Use EitherT rather than ValidationT
* \/ is Scalaz either
* Validation.disjunction converts a validation to a Scalaz either
* Validation.either => Validation.toEither for Scala either
* .left and .right similar to .success and .fail
* \/.fromTryCatch and Validation.fromTryCatch are similar validating(...) respectively
* Validation ||| => valueOr (||| still exists but the meaning is different)
* ValidationNEL => ValidationNel

## Zero
* No longer exists
* Use [Monoid](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/Monoid.scala)

```scala
// Example from Newman
type RawBody = Array[Byte]
implicit val RawBodyMonoid: Monoid[RawBody] = Monoid.instance(_ ++ _, Array[Byte]())
```

## MA
* No longer exists
* <|*|> => tuple in [ApplySyntax](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/syntax/ApplySyntax.scala#L10)

## IO
* scalaz.effects => scalaz.effect

## lift-json-scalaz
* Use [lift-json-scalaz7](https://github.com/lift/framework/pull/1424)
* Lift version 2.5-RC4+

```scala
"net.liftweb" %% "lift-json-scalaz7" % "2.5-RC4"
```

## Upgrade Examples
* [Newman](https://github.com/stackmob/newman/pull/27)
* [Scaliak](https://github.com/stackmob/scaliak/pull/18)
* [Scalamachine](https://github.com/stackmob/scalamachine/pull/23)



