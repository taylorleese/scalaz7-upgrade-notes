# Scalaz 7 Upgrade Notes

## Dependencies
* Dependencies are more compartmentalized

```scala
"org.scalaz" %% "scalaz-core" % scalazVersion
"org.scalaz" %% "scalaz-concurrent" % scalazVersion
"org.scalaz" %% "scalaz-iteratee" % scalazVersion
"org.scalaz" %% "scalaz-effect" % scalazVersion
"org.scalaz" %% "scalaz-iterv" % scalazVersion
"org.scalaz" %% "scalaz-typelevel" % scalazVersion
"org.scalaz" %% "scalaz-xml" % scalazVersion
```

## Package Structure
* Generic (note: valid but it's better to restrict imports to prevent conflicts with other libraries)

```scala
import scalaz._
import Scalaz._
```

* Syntax imports provide implicits to work with type classes and functions

* Syntax imports (note: there is also [```scalaz.syntax.all._```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/syntax/Syntax.scala#L115))  

```scala
import scalaz.syntax.xyz._ // e.g. import scalaz.syntax.applicative._
```

* Standard library syntax imports (note: there is also [```scalaz.syntax.std.all._```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/syntax/std/package.scala#L18)) 

```scala
import scalaz.syntax.std.xyz._ // e.g. import scalaz.syntax.std.list._
```

## ```Validation``` and ```\/```
* Is not a monad (see the Scalaz thread [here](https://groups.google.com/forum/#!msg/scalaz/IWuHC0nlVws/syRUkXJklWIJ))
* Use [```EitherT```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/EitherT.scala) rather than ```ValidationT```
* [```\/```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/Either.scala) is Scalaz either
* [```Validation.disjunction```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/Validation.scala#L312-L317) converts a validation to a Scalaz either
* ```Validation.either``` => [```Validation.toEither```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/Validation.scala#L191-L196) for Scala either
* [```.left```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/Either.scala#L382-L384) and [```.right```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/Either.scala#L386-L388) similar to [```.success```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/Validation.scala#L444-L446) and [```.fail```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/Validation.scala#L448-L450)
* [```\/.fromTryCatch```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/Either.scala#L394-L399) and [```Validation.fromTryCatch```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/Validation.scala#L452-L457)
* Validation ```|||``` => [```valueOr```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/Validation.scala#L206-L211) (```|||``` still exists but the meaning is different)
* ```ValidationNEL``` => [```ValidationNel```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/package.scala#L203)

## ```Zero```
* No longer exists
* Use [```Monoid```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/Monoid.scala)

```scala
// Example from Newman
type RawBody = Array[Byte]
implicit val RawBodyMonoid: Monoid[RawBody] = Monoid.instance(_ ++ _, Array[Byte]())
```

* No "default" ```Monoid[Boolean]```
* There is [```Monoid[Boolean @@ Disjunction]```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/std/AnyVal.scala#L68-L87) and [```Monoid[Boolean @@ Conjunction]```](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/std/AnyVal.scala#L89-L108)
* Easier to use ```Option(xyz) | false``` rather than ```~Option(xyz)``` for simple cases

## ```MA```
* No longer exists
* ```<|*|>``` => ```tuple``` in [ApplySyntax](https://github.com/scalaz/scalaz/blob/scalaz-seven/core/src/main/scala/scalaz/syntax/ApplySyntax.scala#L10)

## ```IO```
* ```scalaz.effects``` => ```scalaz.effect```
* Included in the effect dependency

```scala
"org.scalaz" %% "scalaz-effect" % scalazVersion
```

* unsafePerformIO => [```unsafePerformIO()```](https://github.com/scalaz/scalaz/blob/scalaz-seven/effect/src/main/scala/scalaz/effect/IO.scala#L23)

## lift-json-scalaz
* Use [lift-json-scalaz7](https://github.com/lift/framework/pull/1424)
* Lift version 2.5-RC4+

```scala
"net.liftweb" %% "lift-json-scalaz7" % liftVersion
```

## Upgrade Examples
* [Newman](https://github.com/stackmob/newman/pull/27)
* [Scaliak](https://github.com/stackmob/scaliak/pull/18)
* [Scalamachine](https://github.com/stackmob/scalamachine/pull/23)



