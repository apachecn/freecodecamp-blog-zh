# 权威的 Rails 授权

> 原文：<https://www.freecodecamp.org/news/rails-authorization-with-pundit-a3d1afcb8fd2/>

约瑟夫冻住了

是一个 Ruby gem，它通过一个非常简单的 API 来处理授权。

请记住，授权不同于身份验证，身份验证是验证您就是您所说的那个人，而授权是验证您有执行某个操作的权限。

Pundit 完全属于授权阵营——使用另一个身份验证系统，如[device](https://github.com/plataformatec/devise)来处理身份验证。

### 你如何与权威人士合作

**步骤 1:** 创建一个`Policy`类，处理对特定类型记录的授权访问——无论是`Blog`还是`Potato`还是`User`。

**步骤 2:** 你调用内置的`authorize`函数，传入你试图授权访问的内容。

**步骤 3:** Pundit 将找到合适的`Policy`类，并调用与您授权的方法名称相匹配的`Policy`方法。如果它返回 true，您就有权限执行该操作。如果没有，它将抛出一个异常。

这很简单。特定模型的逻辑被封装到它自己的策略类中，这对于保持事物的整洁非常有用。竞争授权库[坎坎坎](https://github.com/CanCanCommunity/cancancan)有复杂权限失控的问题。

### 需要细微的调整

Pundit 的简单约定有时需要调整以支持更复杂的授权用例。

#### 从策略中访问更多信息

默认情况下，Pundit 为您的授权上下文提供了两个对象:被授权的`User`和`Record`。如果您的系统中有系统范围的角色，如`Admin`或`Moderator`，这就足够了，但是当您需要授权给更具体的上下文时，这就不够了。

假设您有一个支持`Organization`概念的系统，并且您必须支持这些组织中的不同角色。系统范围的授权不能解决问题—您不希望组织 Potato 的管理员能够对组织 Orange 做任何事情，除非他们是这两个组织的管理员。在授权这个案例时，您将需要访问 3 个项目:`User`、`Record`和`Organization`中的用户角色信息。理想的情况是能够访问记录所属的组织，但是让我们把它变得更难，说我们不能通过记录或用户访问它。

Pundit 提供了一个提供额外背景的机会。通过定义一个名为`pundit_user`的函数，这允许你改变什么被认为是`user`。如果从该函数返回一个带有授权上下文的对象，则该上下文将可用于您的策略。

`application_controller.rb`

```
class ApplicationController < ActionController::Base  include Pundit
```

```
 def pundit_user    AuthorizationContext.new(current_user, current_organization)  endend
```

`authorization_context.rb`

```
class AuthorizationContext  attr_reader :user, :organization
```

```
 def initialize(user, organization)    @user = user    @organization = organization  endend
```

`application_policy.rb`

```
class ApplicationPolicy  attr_reader :request_organization, :user, :record
```

```
 def initialize(authorization_context, record)    @user = authorization_context.user    @organization = authorization_context.organization    @record = record  end
```

```
 def index?    # Your policy has access to @user, @organization, and @record.    endend
```

您的策略现在可以访问所有三种类型的信息—您应该能够看到在需要时如何访问更多信息。

#### 覆盖约定并指定要使用的策略

Pundit 使用命名约定将您试图授权的内容与正确的策略匹配起来。大多数情况下，这样做很好，但是在某些情况下，您可能需要覆盖这个约定，例如当您想要授权一个没有关联模型的通用仪表板操作时。您可以传入符号来指定用于授权的操作或策略:

```
#Below will call DashboardPolicy#bake_potato?authorize(:dashboard, :bake_potato?)
```

如果您有一个不同名称的模型，您也可以在模型本身中覆盖`policy_class`函数:

```
class DashboardForAdmins  def self.policy_class   DashboardPolicy     # This forces Pundit to use Dashboard Policy instead of looking    # for DashboardForAdminsPolicy  endend
```

### 测试

授权是我强烈推荐的自动化测试套件之一。错误地设置它们可能是灾难性的，在我看来，这是手工测试中最乏味的事情之一。能够运行单个命令，并且知道您没有无意中更改任何授权业务规则，这是一种很棒的感觉。

Pundit 让测试授权变得非常简单。

```
def test_user_cant_destroy?  assert_raises Pundit::NotAuthorizedError do    authorize @record, :destroy?  endend
```

```
def test_user_can_show?  authorize @record, :show?end
```

总的来说，我喜欢学者。我只使用了很短一段时间，但我已经喜欢它胜过康康康——它只是感觉更容易维护和测试。

你觉得这个故事有帮助吗？请**鼓掌**以示支持！
如果你觉得它没有帮助，请用**评论**告诉我为什么！