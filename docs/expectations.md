---
id: expectations
title: Expectations (assertions)
---

Expectations are pure, composable values. This forces developers to separate the scenario of a test from the checks it performs, which generally makes for cleaner/clearer code.

The easiest way to construct them is to call the `expect` macro, which is built using the [expecty](https://github.com/eed3si9n/expecty/) library.

```scala mdoc
import weaver._
import cats.effect.IO

object MySuite2 extends SimpleIOSuite {

  pureTest("And/Or composition") {
    expect(1 != 2) and expect(2 != 1) or expect(2 != 3)
  }

  pureTest("Foldable operations") {
    val list = List(1,2,3)
    import cats.instances.list._
    forall(list)(i => expect(i > 0)) and
    exists(list)(i => expect(i == 3))
  }

  pureTest("Non macro-based expectations") {
    val condition : Boolean = false
    if (condition) success else failure("Condition failed")
  }

  simpleTest("Failing fast expectations") {
    for {
      h <- IO.pure("hello")
      _ <- expect(h.nonEmpty).failFast
    } yield success
  }

}
```
