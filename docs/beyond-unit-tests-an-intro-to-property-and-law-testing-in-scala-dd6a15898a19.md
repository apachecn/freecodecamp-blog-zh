# 超越单元测试:Scala 中属性和法则测试的介绍

> 原文：<https://www.freecodecamp.org/news/beyond-unit-tests-an-intro-to-property-and-law-testing-in-scala-dd6a15898a19/>

#### 让你的单元测试更上一层楼。

我使用 ScalaCheck 测试库至少有 2 年了。它允许你把你的单元测试带到下一个层次。

*   通过用随机数据生成大量测试并断言函数的属性，你可以进行 ****基于属性的测试**** 。下面描述了一个简单的代码示例。
*   你可以做 ****法则测试**** ，这甚至更强大，允许你检查你的类型的数学属性。

# 基于属性的测试

下面是我们钟爱的`User`数据类型:

```
case class User(name: String, age: Int)
```

和一个随机`User`发生器:

```
import org.scalacheck.{ Gen, Arbitrary }
import Arbitrary.arbitrary
implicit val randomUser: Arbitrary[User] = Arbitrary(for {
  randomName <- Gen.alphaStr
  randomAge  <- Gen.choose(0,80)
  } yield User(randomName, randomAge))
```

我们现在可以生成这样一个`User`:

```
scala> randomUser.arbitrary.sample
res0: Option[User] = Some(User(OtwlaaxGbmdhuorlmgvXitbmGfbgetm,22))
```

让我们在`User`上定义一些功能:

```
def isAdult: User => Boolean = _.age >= 18
def isAllowedToDrink : User => Boolean = _.age >= 21
```

让我们宣称:

****所有成年人都可以喝酒。****

我们能以某种方式证明这一点吗？这对所有用户都是正确的吗？

这就是属性测试的用处。它允许我们不写特定的单元测试。他们会在这里:

*   18 岁的人不允许喝酒
*   19 岁的人不允许喝酒
*   20 岁的人不允许喝酒

所有这些语句都可以由一个单独的属性检查来代替:

```
import org.scalacheck.Prop.forAll
val allAdultsCanDrink = forAll { u: User =>
    if(isAdult(u)) isAllowedToDrink(u) else true }
```

让我们运行它:

```
scala> allAdultsCanDrink.check()
! Falsified after 0 passed tests.
> ARG_0: User(,19)
```

对于一个 19 岁的人来说，这是意料之中的失败。

属性测试很棒，原因如下:

*   通过编写不太具体的测试来节省时间
*   找到您忘记处理的 Scala 检查生成的新用例
*   迫使你用更普遍的方式思考
*   比传统的单元测试给你更多重构的信心

# 法律测试

情况会变得更好:让我们更进一步，定义用户之间的排序:

```
import scala.math.Orderingimplicit val userOrdering: Ordering[User] = Ordering.by(_.age)
```

我们希望确保我们没有 ****忘记任何边缘情况**** ，并且我们正确地定义了我们的顺序。这个属性有一个名字，叫做 ****总序。**** 它需要持有以下属性:

*   ****总体****
*   ****反对称****
*   ****传递性****

我们能以某种方式证明这一点吗？这对所有用户都是正确的吗？

这不需要编写一个测试就可以实现！

我们使用`cats-laws`库来定义我们想要在我们定义的排序上测试的法则:

```
import cats.kernel.laws.discipline.OrderTests
import cats._
import org.scalatest.FunSuite
import org.typelevel.discipline.scalatest.Discipline
import org.scalacheck.ScalacheckShapeless._
class UserOrderSpec extends FunSuite with Discipline  {
//needed boilerplate to satisfy the dependencies of the framework
  implicit def eqUser[A: Eq]: Eq[Option[User]] = Eq.fromUniversalEquals
//convert our standard ordering to a `cats` order 
  implicit val catsUserOrder: Order[User] = Order.fromOrdering(userOrdering)

//check all mathematical properties on our ordering
  checkAll("User", OrderTests[User].order)
}
```

让我们运行它:

```
scala> new UserOrderSpec().execute()
UserOrderSpec:
- User.order.antisymmetry *** FAILED ***
  GeneratorDrivenPropertyCheckFailedException was thrown during property evaluation.
   (Discipline.scala:14)
    Falsified after 1 successful property evaluations.
    Location: (Discipline.scala:14)
    Occurred when passed generated values (
      arg0 = User(h,17),
      arg1 = User(edsb,17),
      arg2 = org.scalacheck.GenArities$Lambda$2739/1277317528@41d7b4cf
    )
    Label of failing property:
      Expected: true
  Received: false
- User.order.compare
- User.order.gt
- User.order.gteqv
- User.order.lt
- User.order.max
- User.order.min
- User.order.partialCompare
- User.order.pmax
- User.order.pmin
- User.order.reflexitivity
- User.order.reflexitivity gt
- User.order.reflexitivity lt
- User.order.symmetry
- User.order.totality
- User.order.transitivity
```

果不其然，它在反对称性定律上失败了！同龄不同名不应该是平等的。我们忘了在最初的`Ordering`中使用这个名字，所以让我们修复它并重新运行法律:

```
implicit val userOrdering: Ordering[User] = Ordering.by( u => (u.age, u.name))
scala> new UserOrderSpec().execute()
UserOrderSpec:
- User.order.antisymmetry
- User.order.compare
- User.order.gt
- User.order.gteqv
- User.order.lt
- User.order.max
- User.order.min
- User.order.partialCompare
- User.order.pmax
- User.order.pmin
- User.order.reflexitivity
- User.order.reflexitivity gt
- User.order.reflexitivity lt
- User.order.symmetry
- User.order.totality
- User.order.transitivity
```

现在已经过去了:)

如果你想知道除了`Order` s 还能测试什么，去看看这里的文档:[https://typelevel.org/cats/typeclasses/lawtesting.html](https://typelevel.org/cats/typeclasses/lawtesting.html)

# 摘要

*   属性测试比单元测试更强大。它们允许我们定义函数的属性，并使用随机数据生成器生成大量的测试。
*   法律测试将它带到了下一个层次，并使用像`Order`这样的结构的数学属性来生成属性和测试。
*   下一次当你定义一个订单，并想知道它是否定义良好时，继续运行它的法律！