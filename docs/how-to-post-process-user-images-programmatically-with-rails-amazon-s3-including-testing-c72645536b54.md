# 如何用 Rails & Amazon S3 以编程方式对用户图像进行后处理(包括测试)

> 原文：<https://www.freecodecamp.org/news/how-to-post-process-user-images-programmatically-with-rails-amazon-s3-including-testing-c72645536b54/>

### 问题是

在我们的平台上，我们允许用户上传他们自己的图片。正如你所想象的，这会导致各种各样的图像大小、质量和格式。我们在整个平台上以各种方式展示这些图像。

在大多数情况下，我们可以通过手动设置图像标签的大小来避免大小问题。但是在一个非常重要的地方——电子邮件——某些服务器忽略我们的样式，以全尺寸显示这些图像:巨大的。

我们需要一种以编程方式重新格式化用户图像的方法。此外，由于我们无论如何都要对图像进行处理，我们希望自动旋转，调整色阶和颜色，并尽可能使它们看起来更好，更一致。

### 考虑

*   我们的图像存储在亚马逊的 S3 云存储中。幸运的是，亚马逊提供了一个相对易用的 API 来与他们的服务进行交互。
*   因为我们的图像在 S3，我认为这将是一个很好的 Lambda 函数，当用户上传照片时触发这项服务。不幸的是，我无论如何也无法在 CloudWatch 控制台(日志应该出现的地方)打印任何东西。在对着这面墙猛撞了一天之后，我决定把它带回室内。
*   我们托管在 Heroku 上，它提供了一个免费的简单的调度程序来运行任务。对于我们来说，上传后立即转换这些图像并不重要。我们可以安排一个作业，在最后 10 分钟内获取所有新内容，并对其进行转换。

### 工人

现在需要的是一个我们可以在 Heroku 允许的情况下尽可能频繁呼叫的工人(最短间隔是 10 分钟)。

#### 收集正确的用户

首先，我们将收集所有需要转换图像的用户。我们一直以特定的模式将用户图像存储在我们的 S3 存储桶中，其中包括一个`files`文件夹。我们可以只搜索个人资料图片正则表达式与`files`中的图片相匹配的用户:

```
User.where(profilePictureUrl: { '$regex': %r(\/files\/) })
```

在搜索方面，你的里程可能会有所不同:我们使用 Mongo 数据库。

当然，我们将对处理过的图像使用不同的模式。这将只挑选那些自任务上次运行以来上传了新图像的人。我们将遍历每个用户并执行以下操作。

#### 设置临时文件

我们需要一个地方来存储我们将要操作的图像数据。我们可以用一个`tmp`文件夹来实现。我们将用它来存放我们要上传到新 S3 位置的图片。我们将按照我们希望最终图像的名称来命名它。我们希望简化和标准化系统中的图像，因此我们使用唯一的用户 id 作为图像名称:

```
@temp_file_location = "./tmp/#{user.id}.png"
```

#### 获取原始图像并将其保存在本地

现在，我们将与我们的 S3 存储桶对话，并获得用户的原始、巨大、无格式的图像:

```
key = URI.parse(user.profilePictureUrl).path.gsub(%r(\A\/), '')
s3 = Aws::S3::Client.new
response = s3.get_object(bucket: ENV['AWS_BUCKET'], key: key)
```

这里的`key`代码获取我们保存为用户的`profilePictureUrl`的 URL 字符串，并删除所有不是图片结束路径的内容。

例如，`http://images.someimages.com/whatever/12345/image.png`将从代码中返回`whatever/12345/image.png`。这正是 S3 想要我们在桶里找到的图像。这里有方便的`aws-sdk`宝石为我们工作与`get_object`。

现在我们可以调用`response.body.read`来获得图像的一个 blob(blob 是正确的词，尽管它超出了我真正理解图像如何在网络上来回发送的能力范围)。我们可以在本地 tmp 文件夹中编写这个 blob:

```
File.open(@temp_file_location, 'wb') { |file| file.write(response.body.read) }
```

如果我们在这里停下来，您将会看到您实际上可以在您的 temp 文件夹中打开该文件(使用您在上面设置的名称—在我们的例子中是`<user>`)。png)。

#### 处理图像

现在我们已经从亚马逊下载了图片，我们可以对它做任何我们想做的事情！ImageMagick 是一个神奇的工具，每个人都可以免费使用。

我们使用了一个精简版的 Rails，叫做 [MiniMagick](https://github.com/minimagick/minimagick) 。这个 gem 还有一个很棒的 API，可以让事情变得非常简单。我们甚至不需要做任何特别的事情来获取图像。我们之前用来保存图像的`@temp_file_location`将很好地引起 MiniMagick 的注意:

```
image = MiniMagick::Image.new(@temp_file_location)
```

这是我们照片的设置，但是还有很多选项可以选择:

```
image.combine_options do |img|
  img.resize '300x300>'
  img.auto_orient
  img.auto_level
  img.auto_gamma
  img.sharpen '0x3'
  image.format 'png'
end
```

是在一个块中对一幅图像进行一系列操作的简便方法。退出时，图像会再次保存在原来的位置。(不能用`combine_options`的`img`进行图像格式化。)现在我们临时文件夹里的那个图像文件是各种后期处理的！

#### 上传回 S3 并另存为用户的新个人资料图片

现在我们要做的就是建立另一个到 S3 的连接并上传:

```
Aws.config.update(
  region: ENV['AWS_REGION'],
  credentials: Aws::Credentials.new(ENV['AWS_ACCESS_KEY_ID'], ENV['AWS_SECRET_ACCESS_KEY']))

s3 = Aws::S3::Resource.new
name = File.basename(@temp_file_location)
bucket = ENV['AWS_BUCKET'] + '-output'
obj = s3.bucket(bucket).object(name)
obj.upload_file(@temp_file_location, acl: 'public-read')
```

按照 Lambda 的惯例，auto tasks 将发送到一个新的 bucket，在旧 bucket 的名称后面加上“-output”，所以我坚持使用它。所有格式化的用户图像都将被转储到此桶中。因为我们用(唯一的)用户 id 来命名图片，我们确信我们永远不会用一个用户的图片覆盖另一个用户的图片。

我们在我们选择的桶中用新文件的名称创建一个新对象，然后我们`upload_file`。如果我们想让它可见而不会给我们的客户带来很多麻烦，它必须是`public-read`(你可以选择不同的安全选项)。

如果最后一行返回 true(如果上传顺利，就会返回 true)，我们就可以更新我们的用户记录:

```
new_url = "https://s3.amazonaws.com/#{ENV['AWS_BUCKET']}-output/#{File.basename(@temp_file_location)}"
user.update(profilePictureUrl: new_url)
```

就是这样！如果我们运行这个家伙，我们将自动格式化和调整系统中所有用户图像的大小。所有的原始图像都将保持原来的模式(以防出错)，所有用户的链接都将指向新的格式化图像。

### 测试

我们不可能在没有测试的情况下给 Rails 应用程序添加新功能，对吗？绝对的。这是我们的测试结果:

```
RSpec.describe Scripts::StandardizeImages, type: :service do
  let!(:user) { User.make!(:student, profilePictureUrl: 'https://s3.amazonaws.com/files/some_picture.jpg') }

  before do
    stub_request(:get, 'https://s3.amazonaws.com/files/some_picture.jpg')
      .with(
        headers: {
          'Accept' => '*/*',
          'Accept-Encoding' => 'gzip;q=1.0,deflate;q=0.6,identity;q=0.3',
          'Host' => 's3.amazonaws.com',
          'User-Agent' => 'Ruby'
        }
      )
      .to_return(status: 200, body: '', headers: {})
    allow_any_instance_of(MiniMagick::Image).to receive(:combine_options).and_return(true)
    allow_any_instance_of(Aws::S3::Object).to receive(:upload_file).and_return(true)
  end

  describe '.call' do
    it 'finds all users with non-updated profile pictures, downloads, reformats and then uploads new picture' do
      Scripts::StandardizeImages.call

      expect(user.reload.profilePictureUrl)
        .to eq "https://s3.amazonaws.com/#{ENV['AWS_BUCKET']}-output/#{user.to_param}.png"
    end
  end
end
```

如果你先看看测试本身，你会看到我们正在测试我们的用户的新的个人资料图片网址保存正确。其余的我们不太关心，因为我们实际上不希望我们的测试下载任何东西，我们可能也不想花时间让我们的测试处理图像。

当然，代码会尝试与亚马逊对话，并启动 MiniMagick。相反，我们可以截掉这些电话。以防这对你来说是新的，我将快速浏览一下这一部分。

#### 阻止通话

如果您没有在测试中模拟调用，您可能应该立即开始这样做。所需要的只是网络模拟宝石。你需要它在你的`rails_helper`里，就是这样。

当您的测试试图调用一个外部源时，您会得到这样的消息(我已经隐藏了私钥和带有…s 的东西):

```
WebMock::NetConnectNotAllowedError:
       Real HTTP connections are disabled. Unregistered request: GET https://...
You can stub this request with the following snippet:
stub_request(:get, "https://...").
         with(
           headers: {
          'Accept'=>'*/*',
          'Accept-Encoding'=>'',
          'Authorization'=>...}).
         to_return(status: 200, body: "", headers: {})
```

只要复制这一点，你就已经踏上了通往辉煌的道路。您可能需要在那个`body`中返回一些东西，这取决于您对外部 API 调用所做的事情。

我发现很难让这个存根响应返回一些我的代码会看到的图像，所以我也只是存根`MiniMagick`函数。这样做很好，因为无论如何我们在这个测试中看不到输出。您必须手动测试图像的格式是否正确。

或者，您可以在您的测试初始化器中或者可能在您的`rails_helper`上使用`Aws.config[:s3] = { stub_responses: true }`来存根所有的 S3 请求。

#### 最后一点:特拉维斯·CI

根据您决定对图像应用的选项，您可能会发现 Travis 的 ImageMagick 版本与您的版本不同。我尝试了很多方法让 Travis 用和我一样的 ImageMagick。最后，我放弃了这个电话，所以这是一个有争议的问题。但是请注意:如果您不存根那个函数，您可能会发现您的 CI 失败，因为它不能识别更新的选项(比如`intensity`)。

感谢阅读！