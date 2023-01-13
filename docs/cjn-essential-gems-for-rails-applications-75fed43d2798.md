# Rails 应用程序的精华

> 原文：<https://www.freecodecamp.org/news/cjn-essential-gems-for-rails-applications-75fed43d2798/>

gem 位于项目文件夹中的 Gemfile 中。让我们看看一些你会想要的。

### **将 _ 分页**

**为您的应用程序添加分页功能**

#### **安装**

添加到`Gemfile`

```
gem 'will_paginate'
```

安装

```
bundle install
```

**用途**

在. html.erb 文件中，在视图中呈现页面链接:

```
<%= will_paginate @places %>
  <% @places.each do |place| %>
    <h1><%= place.name %></h1>
    <br />
  <% end %>
<%= will_paginate @places %>
```

在控制器中，您可以定义每页要显示多少个条目。在下面的例子中，每页将列出 3 个条目。

```
def index
  @places = Place.all.paginate(page: params[:page], per_page: 3)
end
```

来源:https://github.com/mislav/will_paginate

### **简单 _ 形式**

**表格变得简单！**

#### **安装**

添加到`Gemfile`

```
gem 'simple_form'
```

安装

```
bundle install
```

运行发电机

```
rails generate simple_form:install
```

**用途**

在`.html.erb`文件中:

```
<%= simple_form_for @place do |f| %>
  <%= f.input :name %>
  <%= f.input :address %>
  <%= f.input :description %>
  <br />
  <%= f.submit 'Create', class: 'btn btn-primary' %>
<% end %>
```

来源:https://github.com/plataformatec/simple_form

### **设计**

**增加用户管理**

#### **安装**

添加到`Gemfile`

```
gem 'devise'
```

安装

```
bundle install
```

运行发电机

```
rails generate devise:install
```

在*config/environments/development . Rb*配置设备

添加这一行:

```
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 } 
```

在*app/views/layouts/application . html . erb*中编辑代码

添加这一行:

```
<% if notice %>
  <p class="alert alert-success"><%= notice %></p>
<% end %>
<% if alert %>
  <p class="alert alert-danger"><%= alert %></p>
<% end %>
```

在*app/views/layouts/application . html . erb*中添加注册和登录链接

```
<p class="navbar-text pull-right">
<% if user_signed_in? %>
  Logged in as <strong><%= current_user.email %></strong>.
  <%= link_to 'Edit profile', edit_user_registration_path, :class => 'navbar-link' %> |
  <%= link_to "Logout", destroy_user_session_path, method: :delete, :class => 'navbar-link'  %>
<% else %>
  <%= link_to "Sign up", new_user_registration_path, :class => 'navbar-link'  %> |
  <%= link_to "Login", new_user_session_path, :class => 'navbar-link'  %>
<% end %>
</p>
```

如果用户未登录，则强制将其重定向到登录页面。

在*app/controllers/application _ controller . Rb*中编辑

```
before_action :authenticate_user!
```

来源:https://guides.railsgirls.com/devise

### **地理编码器**

**添加来自 Google API 的地理编码**

#### **安装**

添加至`Gemfile`

```
gem 'geocoder'
```

安装

```
bundle install
```

创建一个迁移文件，在您的终端上运行:

```
rails generate migration AddLatitudeAndLongitudeToModel latitude:float longitude:float
```

```
rake db:migrate
```

为现有地点表添加纬度和经度列的迁移文件示例:

```
class AlterPlacesAddLatAndLng < ActiveRecord::Migration[5.0]
  def change
    add_column :places, :latitude, :float
    add_column :places, :longitude, :float
  end
end
```

在你的 *`app/model/place.rb`* 中添加这几行

```
geocoded_by :address
after_validation :geocode
```

将此脚本添加到 *`show.html.erb`*

```
<script>
<% if @place.latitude.present? && @place.longitude.present? %>
  function initMap() {
  var myLatLng = {lat: <%= @place.latitude %>, lng: <%= @place.longitude %>};
  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 15,
    center: myLatLng
  });
  var marker = new google.maps.Marker({
    position: myLatLng,
    map: map,
    title: 'Hello World!'
  });
}
</script>
```

来源:https://www.rubydoc.info/gems/geocoder/1.3.7

### **费加罗报**

**在 Heroku 部署中轻松配置您的 APIs】**

#### **安装**

添加到`Gemfile`

```
gem 'figaro'
```

安装并创建一个带注释的`config/application.yml`文件，并将其添加到您的`.gitignore`中

```
bundle exec figaro install
```

在`config/application.yml`中更新您的 API 密钥

要更新 heroku 中的 API 密钥，请访问您的终端:

```
figaro heroku:set -e productionheroku restart
```

来源:https://github.com/laserlemon/figaro

### 载波波

**图像上载程序**

#### **安装**

添加到`Gemfile`

```
gem 'carrierwave'
```

安装

```
bundle install
```

在你的终端里

```
rails generate uploader Image
```

它将生成以下内容:

```
app/uploaders/image_uploader.rb
```

创建迁移文件

```
rails g migration add_image_to_courses image:string
```

运行迁移文件

```
rake db:migrate
```

在你的模型中

```
class User < ApplicationRecord  mount_uploader :image, ImageUploaderend
```

添加到`html.erb`即`app/views/instructor/courses/new.html.erb`

```
<%= f.input :image %>
```

添加到控制器`app/controllers/instructor/courses_controller.rb`

```
params.require(:course).permit(:title, :description, :cost, :image)
```

添加到`show.html.erb`，即`app/views/instructor/courses/show.html.erb`

```
<%= image_tag @course.image, class: 'img-fluid' %>
```

还要更新以下内容:

*   `app/views/courses/show.html.erb`
*   `app/views/courses/index.html.erb`
*   `app/views/instructor/courses/show.html.erb`

#### 【ImageMagick 的图像分辨率，更多关于载波的信息

载波波输入显示可以使用`MiniMagick`和`RMagick`

这里显示`app/uploaders/image_uploader.rb`

```
class ImageUploader < CarrierWave::Uploader::Base
  # Include RMagick or MiniMagick support:
  # include CarrierWave::RMagick
  # include CarrierWave::MiniMagick
```

#### **安装 ImageMagick**

我们需要更新我们开发环境的程序数据库，以确保当我们安装一个程序时，我们得到的是最新的版本。

```
$ sudo apt-get update
```

安装 ImageMagick

```
$ sudo apt-get install imagemagick
```

安装迷你地图

添加到`Gemfile`

```
gem 'mini_magick'
```

安装

```
bundle install
```

取消`app/uploaders/image_uploader.rb`中 MiniMagick 的注释

```
class ImageUploader < CarrierWave::Uploader::Base
  # Include RMagick or MiniMagick support:
  # include CarrierWave::RMagick
  include CarrierWave::MiniMagick
```

这条线使 CarrierWave 能够通过 MiniMagick gem 访问我们安装在程序上的 ImageMagick 程序。这将允许我们调整图像的大小。

解锁图像缩放功能，如`resize_to_fill`、`resize_to_fit`、`resize_and_pad`和`resize_to_limit`。

将此添加到`app/uploaders/image_uploader.rb`

```
# Process files as they are uploaded:
process resize_to_fill: [800, 350]
```

### 整合亚马逊 S3 视频上传

#### **安装**

将这一行添加到应用程序的`Gemfile`:

```
gem 'carrierwave-aws'
```

从您的 shell 运行 bundle 命令来安装它:

```
bundle install
```

#### 步骤 1:配置初始化器

通常我们会先安装我们的 gem，但是为了防止从 fog 切换到 AWS 时出现错误，我们需要先更新我们的初始化程序。

你可以在文档的用法部分阅读如何配置 carrierwave-aws gem [的细节。按照他们指定的模式，我们应该更新`config/initializers/carrierwave.rb`,如下所示:](https://github.com/sorentwo/carrierwave-aws#usage)

```
# config/initializers/carrierwave.rb
CarrierWave.configure do |config|
  config.storage    = :aws
  config.aws_bucket = ENV["AWS_BUCKET"]
  config.aws_acl    = "public-read"
  config.aws_credentials = {
      access_key_id:     ENV["AWS_ACCESS_KEY"],
      secret_access_key: ENV["AWS_SECRET_KEY"],
      region:            ENV["AWS_REGION"]
  }
end
```

现在保存文件。

#### 步骤 2:更新我们的 Gemfile

将`carrierwave-aws`宝石添加为文档中描述的[。编辑`Gemfile`,如下所示:](https://github.com/sorentwo/carrierwave-aws#installation)

```
gem 'carrierwave'gem 'mini_magick'gem 'carrierwave-aws'
```

保存文件并运行命令来安装 gem。

```
$ bundle install
```

然后重新启动服务器。

#### 步骤 3:向 application.yml 添加区域

我们需要在我们的`application.yml`文件中添加一个区域。打开您的`config/application.yml`,添加这一行来指定我们想要使用的区域:

```
# config/application.yml
AWS_ACCESS_KEY: "Your-access-key"
AWS_SECRET_KEY: "Your-secret-key"
AWS_BUCKET:     "Your-bucket"
AWS_REGION:     "us-east-1"
```

保存文件。

#### 步骤 4:更新上传程序

如果您还记得以前，我们在存储提供者内部指定了上传者使用的`:fog`。与其这样，我们需要把它切换到`:aws`。

```
# encoding: utf-8
class ImageUploader < CarrierWave::Uploader::Base
  # Include RMagick or MiniMagick support:
  # include CarrierWave::RMagick
  include CarrierWave::MiniMagick
  # Choose what kind of storage to use for this uploader:
  #storage :file
  storage :aws
  # A bunch more comments down here....
end
```

保存文件。

还有一件事我们必须做的是重新同步本地主机和 heroku。为此，我们需要运行一个简单的命令:

```
$ figaro heroku:set -e production
```

通过上传课程的图像来确保上传继续进行，并确保上传成功。

### **视频**

在`</head>`前添加 css 文件

```
<link href="http://vjs.zencdn.net/5.12.6/video-js.css" rel="stylesheet">
```

在`</body>`前添加 JavaScript 文件

```
<script src="http://vjs.zencdn.net/5.12.6/video.js"></script>
```

```
<script src="http://vjs.zencdn.net/ie8/1.1.2/videojs-ie8.min.js"></script>
```

注意:你必须把 videoJS `script`标签放在主体的底部，否则你会因为 Turbolinks 而在加载视频播放器时出现问题。

感谢阅读！希望这对你有帮助。