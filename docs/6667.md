# 为未来的自己编写测试将如何使你的测试更好

> 原文：<https://www.freecodecamp.org/news/how-writing-tests-for-your-future-self-will-make-your-tests-better-3311a57e07c4/>

当实践测试驱动开发(TDD)时，我们有时倾向于关注测试**一切。** 这种 100%覆盖的心态有时会让我们把事情变得过于复杂。

以前，我是负责让测试更干燥的人，因为我讨厌看到重复的代码。当时我是 Ruby 元编程的新手，我总是想通过混合重复的代码来使事情变得“更简单”,并想出一个庞然大物。典型的例子:

```
describe 'when receiving the hero details' do
  it 'should have the top level keys as methods' do
    top_level_keys = %w{id name gender level paragonLevel hardcore skills items followers stats kills progress dead last-updated}

    top_level_keys.each do |tl_key|
      @my_hero.send(tl_key).must_equal @my_hero.response[tl_key.camelize(:lower)]
    end
  end
```

好吧，那么[是在 2012 年。](https://github.com/corroded/covetous/blob/master/spec/covetous/profile/hero_spec.rb#L20-L32)作为背景，这是暴雪[暗黑 3 API](https://dev.battle.net/io-docs) 的[红宝石](https://rubygems.org/)(类似于 [npm 包](https://www.npmjs.com/))。那么我在这里测试的是什么？阅读代码后，它看起来非常简单:它说顶层的键可以是方法。因此，如果 API 返回如下内容:

```
{
  paragonLevel: 10,
  hardcore: true,
  kills: 1234
}
```

然后给定一个 hero 实例，我可以将它们作为方法调用，它应该像这样返回它们:

```
> hero = Covetous::Profile::Hero.new 'user#1234', '1234'
> hero.paragon_level # 10
> hero.kills # 1234
```

好吧，我实话实说。当我写这篇文章的时候，我正在查看我以前的开源项目，并看到了这个例子。正如我所说的，它看起来非常简单，但是在实际分析时，我意识到它比我想象的要糟糕得多。我花了 15 分钟才得到它所做的，即使规格说明说它应该做什么。在我输入上面的代码块之前，我想仔细检查一下我是否理解正确。当我这样做的时候，我写测试的方式让一切都变得混乱。为什么？

### 问题是

正如我所说的，当时我是元编程的新手，看到了使用它的机会。当时，这看起来非常聪明，但是现在我有了更多的经验，我知道在测试中这样做是一种负担而不是一种恩惠。

你看，我学到的一件事就是**测试代码就是未测试代码**。让那件事过去一段时间。

[**测试代码是未测试的代码。**](https://twitter.com/corrodedlotus/status/982741953308700672)

[**测试代码是未测试的代码。**](https://github.com/ericboehs/talk-notes/blob/master/2015-11-15-1040-rubyconf-how-to-stop-hating-your-test-suite.md#test-structre)

顺便说一下，这是两个不同的链接。这基本上意味着您的测试运行的任何代码都可能有自己的错误。您实际上没有对测试代码进行测试，所以不能保证它能工作。您可以做的唯一保证是，当您注释掉代码中的实际行时，测试会失败，而当您取消注释时，测试会通过。虽然有时，即使有这种红绿测试，你仍然可以得到假阳性。所以避免这种情况的最好方法是让你的测试**尽可能简单**和**尽可能清晰**。

回到我的测试。如果我没记错的话，我最初是一个一个地做方法的。然后我看到了一个模式，这让我觉得至少所有方法的模式都是一样的，那么为什么不让代码[变干](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)-呃？

我做了一个所有可能方法的数组，循环遍历它们，并断言调用方法应该与查看响应并获取值一样。这很简单，但让我不感兴趣的主要原因是:

```
@my_hero.send(tl_key).must_equal @my_hero.response[tl_key.camelize(:lower)]
```

在 Ruby 中，`send`调用传递的字符串作为方法。所以如果`tl_key`的值是`paragonLevel`(来自数组)，这一行基本上说:

```
@my_hero.paragonLevel.must_equal @my_hero.response['paragonLevel']
```

看，这就是我再次怀疑自己的地方。我的`README`说应该是`@my_hero.paragon_level`，但是看测试，不是。我现在该相信谁？我通过的测试，还是我的`README`？这就是为什么元编程在测试中是危险的确切原因——您永远不会真正知道您的测试是否通过，要么是因为它们是正确的，要么是您在某种程度上配置错误。和不写测试差不多！

### 做得更好

那么我该如何重写呢？从那以后，我明白了为十岁的自己写测试就足够了。意思是，十年前的我。我总是问自己:“十年后，如果没有背景，我还能理解这些吗？”如果没有，那么这意味着我要么需要在评论中写一个注释**要么**我的测试太复杂了。

让我们试着重写这个。正如我所说的，我们应该尽可能简单明了。这里有一个解决方案:

```
# Given I queried my hero against the API:
let(:my_hero) { Covetous::Profile::Hero.new 'corroded-6950', '12345678' }
it 'should have the top level keys as methods' do
  expect(my_hero.id).to eq 12345
  expect(my_hero.name).to eq 'corrodeath'
  expect(my_hero.gender).to eq 'female'
  expect(my_hero.level).to eq 70
  ...
end
```

看多露骨？当然，这是重复的，但是 10 年后，我很确定我仍然明白我的期望是什么。我不需要在大脑中“编译和解释”代码。我刚看了说明书！

此外，有了这个，我甚至不用回忆`camelize(:lower)`实际上是做什么的(坦白说:我不得不在阅读我的旧代码时查找它)。

再举个例子怎么样？假设我们有一个模型:

```
class Something < ActiveRecord::Base
  VALID_THINGS = %w(yolo swag)
  OTHER_VALID_THINGS = %w(thing another_thing)
  def valid_things_ids
    where(group: group).pluck(:id)
  end
end
```

以上只是一个虚构的例子，基于我目前公司的真实课程。我看到的规格是这样的:

```
subject(:valid_things_ids) { described_class.valid_things_ids(group) }

let(:group) { 'example' }

before do
  described_class::VALID_THINGS.each do |thing|
    FactoryGirl.create(:something, group: 'example', name: thing)
  end
end

described_class::VALID_THINGS.each do |thing|
  it "contains things with the name #{thing}" do
    the_thing = described_class.find_by_group_and_name('example', thing)
    expect(valid_things_ids).to include the_thing.id
  end
end
```

好吧。首先，这是一个正确的测试，因为给定了一些`somethings`，我们可以调用该方法，它会返回该组`somethings`的所有 id(例如`example`)。

然而，我的问题是，我们需要测试所有有效的东西吗？那`OTHER_VALID_THINGS`呢？如果我们想测试`VALID_THINGS`的所有可能值，那么我们也应该测试`OTHER_VALID_THINGS`的所有可能值。如果我们不想测试所有可能的值，那么为什么要使用`VALID_THINGS`？为什么不设计一个随机样本，证明这种方法是可行的呢？

像这样的怎么样？

```
subject(:valid_things_ids) { described_class.valid_things_ids(group) }

let(:group) { 'blurb' }

let!(:random_thing) { FactoryGirl.create(:something, group: 'blurb', id: 111) }
let!(:another_thing) { FactoryGirl.create(:something, group: 'blurb', id: 222) }
let!(:not_included) { FactoryGirl.create(:something, group: 'shrug', id: 333) }

it do
  expect(valid_things_ids).to include 111
  expect(valid_things_ids).to include 222
  expect(valid_things_ids).not_to include 333
end
```

所以在这里，我创建了 3 个`somethings`并给了它们 id。我让第三个有不同的组。现在，如果我用`blurb`作为参数运行这个方法，我可以预期它包括前两个，而不是最后一个。

几个月后阅读它，我不会对正在测试的内容感到困惑，因为它很简单，我甚至不用问为什么我只测试代码的某一部分而不是全部。

还要注意测试的明确性。我希望它包含 id`111`和`222`。通常，人们会这样测试它:

```
expect(valid_things_ids).to include random_thing.id
```

我不太喜欢这些测试，因为在这一点上它们仍然依赖于代码。如果由于某种原因 id 是`nil`，并且代码在返回`nil`的地方也有一个 bug，那么这个测试仍然会通过。虽然没有明确的 id 和期望。当然会有警告，但我想我更愿意处理这些，而不是可能的误报的不确定性。

### 包扎

从上面的两个例子中你可以看到，从长远来看，简单易懂的测试会对你有所帮助。非常明确有助于理解测试和减少错误。

请记住，如果有一半是假阳性，那么您的 100%测试覆盖率并不重要。测试的时候要时刻记住过去的自己。试着展望未来，问问自己你的测试意味着什么。