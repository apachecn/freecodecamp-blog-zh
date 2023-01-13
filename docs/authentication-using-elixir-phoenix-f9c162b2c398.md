# 如何使用 Guardian 认证您的 Elixir/Phoenix API

> 原文：<https://www.freecodecamp.org/news/authentication-using-elixir-phoenix-f9c162b2c398/>

作者 Nirmalya Ghosh

# 如何使用 Guardian 认证您的 Elixir/Phoenix API

![EXyEeWVrz7qwqmszVluRIsd3KaU98o6yrdi7](img/f5b87fdccc46d34fb5107de4aade7006.png)

认证总是一个棘手的问题。人们倾向于在他们的应用程序中使用多种类型的身份验证。使用电子邮件地址和密码以及选项确认密码字段进行身份验证是最常见的。但是每次你看到一张注册表格，你就会觉得无聊，想到你必须输入这么多来注册！那有什么好玩的！

因此，对于这个应用程序，我将使用您的谷歌帐户进行身份验证。这很简单。你只需要点击一个按钮，给应用程序必要的权限来访问你的基本谷歌个人资料，你就设置好了！很酷，不是吗？

我们将使用 [Ueberauth](https://github.com/ueberauth/ueberauth) 、 [Ueberauth Google](https://github.com/ueberauth/ueberauth_google) 和 [Guardian](https://github.com/ueberauth/guardian) 来验证我们的用户。Ueberauth 和 Ueberauth Google 将帮助使用他们的 Google 凭据验证用户。Guardian 将帮助我们为登录用户生成一个 JSON Web 令牌。该令牌是必需的，并且需要出现在任何需要认证的路由的每个请求的报头中。

Guardian 将检查请求报头中的令牌，如果令牌有效，则用户可以使用经过身份验证的路由。我会详细解释这些事情。

如果您还没有安装 [Phoenix](http://www.phoenixframework.org) 及其必要的依赖项，您可以前往 [Phoenix Guides](http://www.phoenixframework.org/docs/installation) 进行安装并启动运行。

要开始，请将依赖项添加到我们的`mix.exs`。

```
defp deps do  [   ...      {:ueberauth, "~> 0.4"},   {:ueberauth_google, "~> 0.2"},   {:ja_serializer, "~> 0.11.2"},   {:guardian, "~> 0.14.2"}]end
```

之后，运行`mix deps.get`来获取依赖项。

您还需要将`ueberauth`和`ueberauth_google`添加到我们在`mix.exs`的应用程序中。

```
def application do  [mod: {SocialAppApi, []},   applications: [   ...      :ueberauth, :ueberauth_google]]end
```

现在，您需要将您的`ueberauth`、`ueberauth_google`和`guardian`配置添加到您的`config/config.exs`文件中。

```
# Ueberauth Config for oauthconfig :ueberauth, Ueberauth,  base_path: "/api/v1/auth",  providers: [    google: { Ueberauth.Strategy.Google, [] },    identity: { Ueberauth.Strategy.Identity, [        callback_methods: ["POST"],        uid_field: :username,        nickname_field: :username,      ] },  ]
```

```
# Ueberauth Strategy Config for Google oauthconfig :ueberauth, Ueberauth.Strategy.Google.OAuth,  client_id: System.get_env("GOOGLE_CLIENT_ID"),  client_secret: System.get_env("GOOGLE_CLIENT_SECRET"),  redirect_uri: System.get_env("GOOGLE_REDIRECT_URI")
```

```
# Guardian configurationconfig :guardian, Guardian,  allowed_algos: ["HS512"], # optional  verify_module: Guardian.JWT,  # optional  issuer: "SocialAppApi",  ttl: { 30, :days },  allowed_drift: 2000,  verify_issuer: true, # optional  secret_key: System.get_env("GUARDIAN_SECRET") || "rFtDNsugNi8jNJLOfvcN4jVyS/V7Sh+9pBtc/J30W8h4MYTcbiLYf/8CEVfdgU6/",  serializer: SocialAppApi.GuardianSerializer
```

正如你在这里看到的，我使用了`System.get_env() .` 这是一种在你的应用程序中存储凭证的方法，你不希望它成为你的代码库的一部分。您可以创建一个`.env`文件并存储所有这些凭证，如下所示:

```
export DB_NAME_PROD="social_app_api_db"export DB_PASSWORD_PROD="password"export DB_USERNAME_PROD="password"
```

在这之后，你需要做`source .env`，然后，你可以在你的应用程序中使用它们。

现在，我们需要用我们的控制器做一些事情，让用户注册或登录。

首先，创建一个新文件`web/controllers/auth_controller.ex`。

```
defmodule SocialAppApi.AuthController do  use SocialAppApi.Web, :controller  plug Ueberauth
```

```
 alias SocialAppApi.User  alias MyApp.UserQuery
```

```
 plug :scrub_params, "user" when action in [:sign_in_user]
```

```
 def request(_params) do  end
```

```
 def delete(conn, _params) do    # Sign out the user    conn    |> put_status(200)    |> Guardian.Plug.sign_out(conn)  end
```

```
 def callback(%{assigns: %{ueberauth_failure: _fails}} = conn, _params) do    # This callback is called when the user denies the app to get the data from the oauth provider    conn    |> put_status(401)    |> render(SocialAppApi.ErrorView, "401.json-api")  end
```

```
 def callback(%{assigns: %{ueberauth_auth: auth}} = conn, _params) do    case AuthUser.basic_info(auth) do      {:ok, user} ->        sign_in_user(conn, %{"user" => user})    end
```

```
 case AuthUser.basic_info(auth) do      {:ok, user} ->        conn        |> render(SocialAppApi.UserView, "show.json-api", %{data: user})      {:error} ->        conn        |> put_status(401)        |> render(SocialAppApi.ErrorView, "401.json-api")    end  end
```

```
 def sign_in_user(conn, %{"user" => user}) do    try do      # Attempt to retrieve exactly one user from the DB, whose      # email matches the one provided with the login request      user = User      |> where(email: ^user.email)      |> Repo.one!
```

```
 cond do        true ->          # Successful login          # Encode a JWT          { :ok, jwt, _ } = Guardian.encode_and_sign(user, :token)
```

```
 auth_conn = Guardian.Plug.api_sign_in(conn, user)          jwt = Guardian.Plug.current_token(auth_conn)          {:ok, claims} = Guardian.Plug.claims(auth_conn)
```

```
 auth_conn          |> put_resp_header("authorization", "Bearer #{jwt}")          |> json(%{access_token: jwt}) # Return token to the client
```

```
 false ->          # Unsuccessful login          conn          |> put_status(401)          |> render(SocialAppApi.ErrorView, "401.json-api")      end    rescue      e ->        IO.inspect e # Print error to the console for debugging
```

```
 # Successful registration        sign_up_user(conn, %{"user" => user})    end  end
```

```
 def sign_up_user(conn, %{"user" => user}) do    changeset = User.changeset %User{}, %{email: user.email,      avatar: user.avatar,      first_name: user.first_name,      last_name: user.last_name,      auth_provider: "google"}
```

```
 case Repo.insert changeset do      {:ok, user} ->        # Encode a JWT        { :ok, jwt, _ } = Guardian.encode_and_sign(user, :token)
```

```
 conn        |> put_resp_header("authorization", "Bearer #{jwt}")        |> json(%{access_token: jwt}) # Return token to the client      {:error, changeset} ->        conn        |> put_status(422)        |> render(SocialAppApi.ErrorView, "422.json-api")    end  end
```

```
 def unauthenticated(conn, params) do    conn    |> put_status(401)    |> render(SocialAppApi.ErrorView, "401.json-api")  end
```

```
 def unauthorized(conn, params) do    conn    |> put_status(403)    |> render(SocialAppApi.ErrorView, "403.json-api")  end
```

```
 def already_authenticated(conn, params) do    conn    |> put_status(200)    |> render(SocialAppApi.ErrorView, "200.json-api")  end
```

```
 def no_resource(conn, params) do    conn    |> put_status(404)    |> render(SocialAppApi.ErrorView, "404.json-api")  endend
```

这里`sign_in_user`将让用户登录并抛出一个`access_token`作为响应。`sign_up_user`将使用用户的谷歌凭证注册用户，然后抛出一个`access_token`作为响应。这个令牌是必不可少的，因为 Guardian 会在所有请求的头中检查这个`access_token`。它将检查用户当前是否在会话中。如果是，用户将可以使用所有经过身份验证的路由。否则，他将收到针对已认证路由的`401`响应。

让我们在应用程序中添加一些路线。我们的`router.ex`文件看起来像这样:

```
defmodule SocialAppApi.Router do  use SocialAppApi.Web, :router
```

```
 pipeline :api do    plug :accepts, ["json", "json-api"]    plug JaSerializer.Deserializer  end
```

```
 pipeline :api_auth do    plug :accepts, ["json", "json-api"]    plug Guardian.Plug.VerifyHeader, realm: "Bearer"    plug Guardian.Plug.LoadResource    plug JaSerializer.Deserializer  end
```

```
 scope "/api/v1", SocialAppApi do    pipe_through :api_auth
```

```
 resources "/users", UserController, except: [:new, :edit]    get "/user/current", UserController, :current, as: :current_user    delete "/logout", AuthController, :delete  end
```

```
 scope "/api/v1/auth", SocialAppApi do    pipe_through :api
```

```
 get "/:provider", AuthController, :request    get "/:provider/callback", AuthController, :callback    post "/:provider/callback", AuthController, :callback  endend
```

这里，管道`api_auth`是被认证的管道。管道`api`不是。所以，我们不用签到就可以参观`get “/:provider”, AuthController, :request`。

用以下代码创建另一个名为`web/models/auth_user.ex`的文件:

```
defmodule AuthUser do  alias Ueberauth.Auth
```

```
 def basic_info(%Auth{} = auth) do    {:ok,      %{        avatar: auth.info.image,        email: auth.info.email,        first_name: auth.info.first_name,        last_name: auth.info.last_name      }    }  endend
```

您还需要创建一个`User`模型。

```
mix phoenix.gen.json User users email:string auth_provider:string first_name:string last_name:string avatar:string
```

这将生成您所需要的模型和迁移。

您的模型看起来会像这样:

```
defmodule SocialAppApi.User do  use SocialAppApi.Web, :model
```

```
 schema "users" do    field :email, :string    field :auth_provider, :string    field :first_name, :string    field :last_name, :string    field :avatar, :string
```

```
 timestamps()  end
```

```
 def changeset(struct, params \\ %{}) do    struct    |> cast(params, [:email, :auth_provider, :first_name, :last_name, :avatar])    |> validate_required([:email, :auth_provider, :first_name, :last_name, :avatar])    |> unique_constraint(:email)  endend
```

您的迁移文件将如下所示:

```
defmodule SocialAppApi.Repo.Migrations.CreateUser do  use Ecto.Migration
```

```
 def change do    create table(:users) do      add :email, :string      add :auth_provider, :string      add :first_name, :string      add :last_name, :string      add :avatar, :string
```

```
 timestamps()    end
```

```
 # Unique email address constraint, via DB index    create index(:users, [:email], unique: true)  endend
```

现在，运行迁移。

```
mix ecto.migrate
```

此外，为我们的`user`模型创建一个`UserController`。它将包含以下代码:

```
defmodule SocialAppApi.UserController do  use SocialAppApi.Web, :controller
```

```
 alias SocialAppApi.User
```

```
 plug Guardian.Plug.EnsureAuthenticated, handler:     SocialAppApi.AuthController
```

```
 def index(conn, _params) do    users = Repo.all(User)    render(conn, "index.json-api", data: users)  end
```

```
 def current(conn, _) do    user = conn    |> Guardian.Plug.current_resource
```

```
 conn    |> render(SocialAppApi.UserView, "show.json-api", data: user)  endend
```

这在您努力工作之后想要检查认证路由是否工作的情况下是有用的。

用下面的代码在`web/views/error_view.ex`处再创建两个视图:

```
defmodule SocialAppApi.ErrorView do  use SocialAppApi.Web, :view  use JaSerializer.PhoenixView
```

```
 def render("401.json-api", _assigns) do    %{title: "Unauthorized", code: 401}    |> JaSerializer.ErrorSerializer.format  end
```

```
 def render("403.json-api", _assigns) do    %{title: "Forbidden", code: 403}    |> JaSerializer.ErrorSerializer.format  end
```

```
 def render("404.json-api", _assigns) do    %{title: "Page not found", code: 404}    |> JaSerializer.ErrorSerializer.format  end
```

```
 def render("422.json-api", _assigns) do    %{title: "Unprocessable entity", code: 422}    |> JaSerializer.ErrorSerializer.format  end
```

```
 def render("500.json-api", _assigns) do    %{title: "Internal Server Error", code: 500}    |> JaSerializer.ErrorSerializer.format  end
```

```
 # In case no render clause matches or no  # template is found, let's render it as 500  def template_not_found(_template, assigns) do    render "500.json-api", assigns  endend
```

同样，用下面的代码创建另一个视图`web/views/user_view.ex`:

```
defmodule SocialAppApi.UserView do  use SocialAppApi.Web, :view  use JaSerializer.PhoenixView
```

```
 attributes [:avatar, :email, :first_name, :last_name, :auth_provider]end
```

你已经准备好了。启动您的服务器:

```
mix phoenix.server
```

现在，进入[http://localhost:4000/API/v1/auth/google](http://localhost:4000/api/v1/auth/google)，你会被重定向到 Google 的登录页面。一旦您给了应用程序必要的权限，您将在响应中获得一个`access_token`:

```
{  access_token: "eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJVc2VyOjIiLCJleHAiOjE0ODk4NjM4MzUsImlhdCI6MTQ4NzI3MTgzNSwiaXNzIjoiU29jaWFsQXBwQXBpIiwianRpIjoiODU0NzJhODAtN2Q4Ny00MjM0LWIxNmUtODgyMTBmYWZkZDJmIiwicGVtIjp7fSwic3ViIjoiVXNlcjoyIiwidHlwIjoiYWNjZXNzIn0.L2LjpsyJAjF1r99hR11WVGcQ"}
```

现在，你可以为 Chrome 安装 [Modheader](https://chrome.google.com/webstore/detail/modheader/idgpnmonknjnojddfkpgkljpfnnfcklj?hl=en) 扩展和任何其他可以设置响应头的扩展。增加`Authorization`为`Request Header`，增加`Bearer <access_tok`为>的`access_token`。

![8s46wKgYRFAp7mdlDFIYgtInAUZzUpw3-INz](img/3145dc52bd27517a0a0587267275096f.png)

Modheader

进入[http://localhost:4000/API/v1/users](http://localhost:4000/api/v1/users)，你将能够看到一系列你已经注册的用户。也可以去[http://localhost:4000/API/v1/user/current](http://localhost:4000/api/v1/user/current)查看会话中的当前用户。

如果您从 Modheader 中删除该值并转到[http://localhost:4000/API/v1/users](http://localhost:4000/api/v1/users)，您将得到以下响应:

```
{  jsonapi: {    version: "1.0"  },  errors: [{    title: "Unauthorized",    code: 401  }]}
```

正如我前面提到的，您需要发送收到的`access_token`来查看经过认证的路由。现在，您知道了如何在 Elixir 中进行 API 认证。你可以在 [Github](https://github.com/ghoshnirmalya/social_app_api/tree/5f16f6432796f3fe372e971522dd588b5db3a421) 上对比你的代码和我的代码。

如果你有一些反馈，请在下面的评论中告诉我。