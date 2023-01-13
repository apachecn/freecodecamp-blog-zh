# 如何使用共享的示例来干燥您的 RSpec 测试

> 原文：<https://www.freecodecamp.org/news/how-to-dry-out-your-rspec-tests-using-shared-examples-d5cc5d33fd76/>

作者帕斯·莫迪

# 如何使用共享的示例来干燥您的 RSpec 测试

![suYDWqCKKdtPuVqX8rPGPsr1Mp4DE8bxzqY0](img/6eb2e9afdc86121e7d5bb1ec4c1291d2.png)

> “给我六个小时砍树，我会用前四个小时磨斧子。”—亚伯拉罕·林肯

当我几周前重构一个项目时，我花了大部分时间写规范。在为一些 API 编写了几个类似的测试用例之后，我开始考虑是否能够消除大量的重复。

所以我致力于阅读关于测试枯竭的最佳实践(不要重复)。我就是这样知道`shared examples`和`shared contexts`的。

在我的例子中，我最终使用了共享的例子。这是我目前为止从应用这些东西中学到的。

当您有多个描述类似行为的规范时，最好将多余的例子提取到`shared examples`中，并在多个规范中使用它们。

假设你有两个模型*用户*和*帖子*，一个用户可以有很多帖子。用户应该能够查看用户和帖子列表。在用户和帖子控制器中创建一个索引操作将会达到这个目的。

首先，为用户控制器编写索引操作的规范。它将负责获取用户，并为他们提供适当的布局。然后编写足够的代码来通过测试。

```
# users_controller_spec.rbdescribe "GET #index" do  before do     5.times do      FactoryGirl.create(:user)     end    get :index  end  it {  expect(subject).to respond_with(:ok) }  it {  expect(subject).to render_template(:index) }  it {  expect(assigns(:users)).to match(User.all) }end
```

```
# users_controller.rbclass UsersController < ApplicationController  ....  def index    @users = User.all  end  ....end
```

通常，任何控制器的索引操作都会根据需要从少数资源中获取和聚集数据。它还增加了分页，搜索，排序，过滤和范围。

最后，所有这些数据都使用 API 通过 HTML、JSON 或 XML 呈现给视图。为了简化我的例子，控制器的索引操作将只获取数据，然后通过视图显示它们。

posts 控制器中的索引操作也是如此:

```
describe "GET #index" do   before do     5.times do      FactoryGirl.create(:post)    end    get :index  end  it {  expect(subject).to respond_with(:ok) }  it {  expect(subject).to render_template(:index) }  it {  expect(assigns(:posts)).to match(Post.all) }end
```

```
# posts_controller.rbclass PostsController < ApplicationController  ....  def index    @posts = Post.all  end  ....end
```

为用户和 posts 控制器编写的 RSpec 测试非常相似。在这两种控制器中，我们都有:

*   响应代码—应该是“正常”
*   两个索引操作都应该呈现正确的部分或视图——在我们的例子中是`index`
*   我们想要呈现的数据，比如帖子或用户

让我们通过使用`shared examples`来干燥我们的索引动作的规格。

### 分享的例子放在哪里

我喜欢将共享示例放在*specs/support/shared _ examples*目录中，这样所有与`shared example`相关的文件都会自动加载。

你可以在这里阅读关于定位你的`shared examples`的其他常用惯例:[共享示例文档](https://www.relishapp.com/rspec/rspec-core/docs/example-groups/shared-examples)

### 如何定义共享示例

您的索引操作应该以 200 成功代码(OK)响应，并呈现您的索引模板。

```
RSpec.shared_examples "index examples" do   it { expect(subject).to respond_with(:ok) }  it { expect(subject).to render_template(:index) }end
```

除了您的`it`块——在您的钩子之前和之后——您可以添加`let`块、上下文和描述块，它们也可以在`shared examples`中定义。

我个人更喜欢分享的例子简洁明了，不要加上下文和 let 块。`shared examples`块也接受参数，我将在下面介绍。

### 如何使用共享示例

将`include_examples "index examples"`添加到您的用户中，并发布控制器规格，包括测试中的“索引示例”。

```
# users_controller_spec.rbdescribe "GET #index" do  before do     5.times do      FactoryGirl.create(:user)     end    get :index  end  include_examples "index examples"  it {  expect(assigns(:users)).to match(User.all) }end
```

```
# similarly, in posts_controller_spec.rbdescribe "GET #index" do  before do     5.times do      FactoryGirl.create(:post)     end    get :index  end  include_examples "index examples"  it {  expect(assigns(:posts)).to match(Post.all) }end
```

在这种情况下，你也可以用`it_behaves_like`或`it_should_behaves_like`来代替`include_examples`。`it_behaves_like`和`it_should_behaves_like`实际上是别名，工作方式相同，可以互换使用。但是`include_examples`和`it_behaves_like`不一样。

如官方文件所述:

*   `include_examples` —包括当前上下文中的示例
*   `it_behaves_like`和`it_should_behave_like`包括嵌套上下文中的示例

#### 为什么这种区别很重要？

RSpec 的文档给出了正确的答案:

> 当您在当前上下文中多次包含参数化示例时，您可能会覆盖先前的方法定义和最后的声明。

因此，当您面临参数化示例包含与相同上下文中其他方法冲突的方法时，您可以用`it_behaves_like`方法替换`include_examples`。这将创建一个嵌套的上下文并避免这种情况。

查看用户控制器规格中的以下行，并发布控制器规格:

```
it { expect(assigns(:users)).to match(User.all) }it { expect(assigns(:posts)).to match(Post.all) }
```

现在，您的控制器规格可以通过向共享示例传递参数来进一步重构，如下所示:

```
# specs/support/shared_examples/index_examples.rb
```

```
# here assigned_resource and resource class are parameters passed to index examples block RSpec.shared_examples "index examples" do |assigned_resource, resource_class|   it { expect(subject).to respond_with(:ok) }  it { expect(subject).to render_template(:index) }  it {  expect(assigns(assigned_resource)).to match(resource_class.all)   }end
```

现在，对您的用户进行以下更改，并发布控制器规格:

```
# users_controller_spec.rbdescribe "GET #index" do  before do     ...  end  include_examples "index examples", :users, User.allend
```

```
# posts_controller_spec.rbdescribe "GET #index" do  before do     ...  end  include_examples "index examples", :posts, Post.allend
```

现在，控制器规格看起来很干净，冗余更少，更重要的是，干燥。此外，这些指标的例子可以作为设计其他控制器的指标行动的基本结构。

### 结论

通过将常见示例移动到一个单独的文件中，您可以消除重复并提高整个应用程序中控制器操作的一致性。这在设计 API 时非常有用，因为您可以使用 RSpec 测试的现有结构来设计测试并创建符合通用响应结构的 API。

大多数情况下，当我使用 API 时，我使用`shared examples`为我提供一个通用的结构来设计类似的 API。

请随意分享你是如何使用`shared examples`让你的眼镜变干的。