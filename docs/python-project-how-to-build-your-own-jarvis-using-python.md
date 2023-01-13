# Python 项目——如何用 Python 构建托尼·斯塔克的 JARVIS

> 原文：<https://www.freecodecamp.org/news/python-project-how-to-build-your-own-jarvis-using-python/>

你还记得托尼·斯塔克的虚拟私人助理 J.A.R.V.I.S 吗？如果你看过《钢铁侠》或《复仇者联盟》中的任何一部电影，我肯定你看过。

你有没有想过是否可以创建自己的私人助理？什么事？托尼·斯塔克可以帮我们。

![tony-snap2_rv5gmh](img/1784b1d98e6b9af3d73e805b85c915dd.png)

哎呀，你忘了他已经不在了吗？很遗憾他再也救不了我们了。但是，你最喜欢的编程语言 Python 可以帮你做到这一点。

是的，你没听错。我们可以用 Python 创建自己的 J.A.R.V.I.S。让我们开始吧！

## 项目设置

在编写这个项目的代码时，您会遇到各种模块和外部库。让我们来了解它们并安装它们。但是在我们安装它们之前，让我们创建一个虚拟环境并激活它。

我们将使用`virtualenv`创建一个虚拟环境。Python 现在附带了一个预安装的`virtualenv`库。因此，要创建虚拟环境，您可以使用下面的命令:

```
$ python -m venv env
```

上面的命令将创建一个名为`env`的虚拟环境。现在，我们需要使用以下命令激活环境:

```
$ . env/Scripts/activate
```

要验证环境是否已被激活，您可以在终端中看到`(env)`。现在，我们可以安装库了。

1.  **pyttsx3** : pyttsx 是一个跨平台的文本到语音库，与平台无关。使用这个库进行文本到语音转换的主要优点是它可以脱机工作。要安装此模块，请在终端中键入以下命令:

    ```
    $ pip install pyttsx3
    ```

2.  **演讲人识别** : 这让我们可以将音频转换成文本进行进一步处理。要安装此模块，请在终端中键入以下命令:

    ```
    $ pip install SpeechRecognition
    ```

3.  **pywhatkit** :这是一个易于使用的库，将帮助我们非常容易地与浏览器进行交互。要安装该模块，请在终端中运行以下命令:

    ```
    $ pip install pywhatkit
    ```

4.  **维基百科** :我们将使用它从维基百科网站获取各种信息。要安装此模块，请在终端中键入以下命令:

    ```
    $ pip install wikipedia
    ```

5.  **请求** :这是一个优雅简单的 Python that 库，可以让你极其轻松地发送 HTTP/1.1 请求。要安装该模块，请在终端中运行以下命令:

    ```
    $ pip install requests
    ```

### 。环境文件

我们需要这个文件来存储一些与项目相关的私有数据，比如 API 密钥、密码等等。现在，让我们存储用户和机器人的名称。

创建一个名为`.env`的文件，并在其中添加以下内容:

```
USER=Ashutosh
BOTNAME=JARVIS
```

为了使用来自`.env`文件的内容，我们将安装另一个名为 **python-decouple** 的模块，如下所示:

```
$ pip install python-decouple
```

点击了解更多关于 Python [中环境变量的信息。](https://iread.ga/posts/49/do-you-really-need-environment-variables-in-python)

## 如何用 Python 设置 JARVIS

在我们开始定义一些重要的功能之前，让我们先创建一个语音引擎。

```
import pyttsx3
from decouple import config

USERNAME = config('USER')
BOTNAME = config('BOTNAME')

engine = pyttsx3.init('sapi5')

# Set Rate
engine.setProperty('rate', 190)

# Set Volume
engine.setProperty('volume', 1.0)

# Set Voice (Female)
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)
```

我们来分析一下上面的脚本。首先，我们已经使用`pyttsx3`模块初始化了一个`engine`。`sapi5`是微软语音 API，帮助我们使用语音。点击了解更多信息[。](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/ee125663(v=vs.85))

接下来，我们使用`setProperty`方法设置语音引擎的`rate`和`volume`属性。

现在，我们可以使用`getProperty`方法从引擎中获取声音。`voices`将是我们系统中可用的声音列表。如果我们打印出来，我们可以看到如下:

```
[<pyttsx3.voice.Voice object at 0x000001AB9FB834F0>, <pyttsx3.voice.Voice object at 0x000001AB9FB83490>]
```

第一个是男声，另一个是女声。JARVIS 在电影中是一个男性助手，但是在本教程中，我选择使用`setProperty`方法将`voice`属性设置为女性。

注意:如果你得到一个与 PyAudio 相关的错误，在这里 从 [下载 PyAudio wheel 并安装在虚拟环境中。](https://www.lfd.uci.edu/~gohlke/pythonlibs/#pyaudio)

同样，使用来自*解耦*的`config`方法，我们从环境变量中得到`USER`和`BOTNAME`的值。

### 启用朗读功能

speak 函数将负责朗读传递给它的任何文本。让我们看看代码:

```
# Text to Speech Conversion
def speak(text):
    """Used to speak whatever text is passed to it"""

    engine.say(text)
    engine.runAndWait() 
```

在`speak()`方法中，引擎使用`say()`方法朗读传递给它的任何文本。使用`runAndWait()`方法，它在事件循环期间阻塞，并在命令队列被清除时返回。

### 启用问候功能

该函数将用于在程序运行时问候用户。根据当前时间，它向用户问候*早上好*、*下午好、*或*晚上好*。

```
from datetime import datetime

def greet_user():
    """Greets the user according to the time"""

    hour = datetime.now().hour
    if (hour >= 6) and (hour < 12):
        speak(f"Good Morning {USERNAME}")
    elif (hour >= 12) and (hour < 16):
        speak(f"Good afternoon {USERNAME}")
    elif (hour >= 16) and (hour < 19):
        speak(f"Good Evening {USERNAME}")
    speak(f"I am {BOTNAME}. How may I assist you?") 
```

首先，我们得到当前的小时，也就是说，如果当前时间是上午 11:15，小时将是 11。如果小时值在 6 到 12 之间，向用户祝早安。如果值在 12 和 16 之间，祝下午好，同样，如果值在 16 和 19 之间，祝晚上好。我们使用 speak 方法与用户对话。

### 如何接受用户输入

我们使用该函数从用户处获取命令，并使用`speech_recognition`模块识别命令。

```
import speech_recognition as sr
from random import choice
from utils import opening_text

def take_user_input():
    """Takes user input, recognizes it using Speech Recognition module and converts it into text"""

    r = sr.Recognizer()
    with sr.Microphone() as source:
        print('Listening....')
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print('Recognizing...')
        query = r.recognize_google(audio, language='en-in')
        if not 'exit' in query or 'stop' in query:
            speak(choice(opening_text))
        else:
            hour = datetime.now().hour
            if hour >= 21 and hour < 6:
                speak("Good night sir, take care!")
            else:
                speak('Have a good day sir!')
            exit()
    except Exception:
        speak('Sorry, I could not understand. Could you please say that again?')
        query = 'None'
    return query
```

我们已经将`speech_recognition`模块导入为`sr`。`speech_recognition`模块中的*识别器*类帮助我们识别音频。同一个模块有一个*麦克风*类，让我们可以访问设备的麦克风。所以用麦克风作为`source`，我们尝试使用*识别器*类中的`listen()`方法来听音频。

我们还将`pause_threshold`设置为 1，也就是说，即使我们说话时停顿一秒钟，它也不会抱怨。

接下来，使用来自*识别器*类的`recognize_google()`方法，我们尝试识别音频。`recognize_google()`方法使用**谷歌语音识别 API** 对传递给它的音频进行语音识别。

我们已经将语言设置为`en-in`，是英语印度。它返回音频的副本，它只是一个字符串。我们将它存储在一个名为`query`的变量中。

如果查询中有**退出**或**停止**字样，这意味着我们要求助手立即停止。因此，在停止之前，我们按照当前时间再次问候用户。如果时间在 21 点和 6 点之间，向用户祝*晚安*，否则，一些其他消息。

我们创建了一个`utils.py`文件，其中只有一个包含如下语句的列表:

```
opening_text = [
    "Cool, I'm on it sir.",
    "Okay sir, I'm working on it.",
    "Just a second sir.",
]
```

如果查询没有这两个词(exit 或 stop)，我们会告诉用户我们已经听到了。为此，我们将使用 random 模块中的 choice 方法从`opening_text`列表中随机选择任意语句。说完之后，我们退出程序。

在整个过程中，如果我们遇到异常，我们会向用户道歉，并将`query`设置为 None。最后，我们返回`query`。

## 如何设置离线功能

在`functions`文件夹中，创建一个名为`os_ops.py`的 Python 文件。在这个文件中，我们将创建各种与操作系统交互的函数。

```
import os
import subprocess as sp

paths = {
    'notepad': "C:\\Program Files\\Notepad++\\notepad++.exe",
    'discord': "C:\\Users\\ashut\\AppData\\Local\\Discord\\app-1.0.9003\\Discord.exe",
    'calculator': "C:\\Windows\\System32\\calc.exe"
} 
```

在上面的脚本中，我们创建了一个名为`paths`的字典，它以软件名作为键，以其路径作为值。您可以根据您的系统更改路径，并根据需要添加更多软件路径。

### 相机怎么开

我们将使用这个函数来打开我们系统中的摄像机。我们将使用`subprocess`模块来运行该命令。

```
def open_camera():
    sp.run('start microsoft.windows.camera:', shell=True)
```

### 如何打开记事本和不和谐

我们会用这些函数打开系统中的 Notepad++和 Discord。

```
def open_notepad():
    os.startfile(paths['notepad'])

def open_discord():
    os.startfile(paths['discord'])
```

### 如何打开命令提示符

我们将使用这个函数来打开系统中的命令提示符。

```
def open_cmd():
    os.system('start cmd')
```

### 如何打开计算器

我们将使用这个函数来打开我们系统上的计算器。

```
def open_calculator():
    sp.Popen(paths['calculator'])
```

## 如何设置在线功能

我们将增加几个在线功能。它们是:

1.  查找我的 IP 地址
2.  在维基百科上搜索
3.  在 YouTube 上播放视频
4.  在谷歌上搜索
5.  发送 WhatsApp 消息
6.  发送电子邮件
7.  获取最新新闻标题
8.  获取天气报告
9.  获取热门电影
10.  获得随机笑话
11.  获得随机建议

让我们在`functions`目录中创建一个名为`online_ops.py`的文件，并开始一个接一个地创建这些函数。现在，在文件中添加以下代码:

```
import requests
import wikipedia
import pywhatkit as kit
from email.message import EmailMessage
import smtplib
from decouple import config
```

在我们开始使用 API 之前，如果您不熟悉 API 以及如何使用 Python 与它们交互，请查看本教程。

### 如何添加查找我的 IP 地址功能

ipify 提供了一个简单的公共 IP 地址 API。我们只需要在这个 URL 上发出 GET 请求:【https://api64.ipify.org/?format=json[。它以如下形式返回 JSON 数据:](https://api64.ipify.org/?format=json)

```
{
  "ip": "117.214.111.199"
}
```

然后我们可以简单地从 JSON 数据中返回`ip`。所以，让我们创建这个方法:

```
def find_my_ip():
    ip_address = requests.get('https://api64.ipify.org?format=json').json()
    return ip_address["ip"]
```

### 如何在维基百科上添加搜索功能

为了在维基百科上搜索，我们将使用我们在本教程前面安装的`wikipedia`模块。

```
def search_on_wikipedia(query):
    results = wikipedia.summary(query, sentences=2)
    return results
```

在`wikipedia`模块中，我们有一个`summary()`方法，它接受一个查询作为参数。此外，我们还可以传递所需的句子数量。然后我们简单地返回结果。

### 如何添加在 YouTube 上播放视频功能

在 YouTube 上播放视频，我们使用的是 *PyWhatKit* 。我们已经将其导入为`kit`。

```
def play_on_youtube(video):
    kit.playonyt(video)
```

*PyWhatKit* 有一个`playonyt()`方法，接受一个主题作为参数。然后，它会在 YouTube 上搜索该主题，并播放最合适的视频。它在引擎盖下使用 [PyAutoGUI](https://pyautogui.readthedocs.io/en/latest/) 。

### 如何添加谷歌搜索功能

我们将再次使用 *PyWhatKit* 在谷歌上搜索。

```
def search_on_google(query):
    kit.search(query)
```

它有一个方法`search()`可以帮助我们立即在谷歌上搜索。

### 如何添加发送 WhatsApp 消息功能

我们将再次使用 *PyWhatKit* 发送 WhatsApp 消息。

```
def send_whatsapp_message(number, message):
    kit.sendwhatmsg_instantly(f"+91{number}", message)
```

我们的方法接受两个参数——电话号码`number`和`message`。然后它调用`sendwhatmsg_instantly()`方法来发送 WhatsApp 消息。确保您已经在 WhatsApp for Web 上登录了您的 WhatsApp 帐户。

### 如何添加发送电子邮件功能

对于发送电子邮件，我们将使用 Python 内置的`smtplib`模块。

```
EMAIL = config("EMAIL")
PASSWORD = config("PASSWORD")

def send_email(receiver_address, subject, message):
    try:
        email = EmailMessage()
        email['To'] = receiver_address
        email["Subject"] = subject
        email['From'] = EMAIL
        email.set_content(message)
        s = smtplib.SMTP("smtp.gmail.com", 587)
        s.starttls()
        s.login(EMAIL, PASSWORD)
        s.send_message(email)
        s.close()
        return True
    except Exception as e:
        print(e)
        return False
```

该方法接受`receiver_address`、`subject`和`message`作为参数。我们从`smtplib`模块中创建了一个 *SMTP* 类的对象。它以**主机**和**端口号**为参数。

然后，我们启动一个会话，使用电子邮件地址和密码登录，并发送电子邮件。确保在`.env`文件中添加了**电子邮件**和**密码**。

### 如何添加获取最新新闻标题功能

为了获取最新的新闻标题，我们将使用 [NewsAPI](https://newsapi.org/) 。在 NewsAPI 上注册一个免费帐户并获得 API 密钥。在`.env`文件中添加 **NEWS_API_KEY** 。

```
NEWS_API_KEY = config("NEWS_API_KEY")

def get_latest_news():
    news_headlines = []
    res = requests.get(
        f"https://newsapi.org/v2/top-headlines?country=in&apiKey={NEWS_API_KEY}&category=general").json()
    articles = res["articles"]
    for article in articles:
        news_headlines.append(article["title"])
    return news_headlines[:5]
```

在上面的方法中，我们首先创建一个名为`news_headlines`的空列表。然后，我们在 [NewsAPI 文档](https://newsapi.org/docs)中指定的 API URL 上发出 GET 请求。请求的 JSON 响应示例如下所示:

```
{
  "status": "ok",
  "totalResults": 38,
  "articles": [
    {
      "source": {
        "id": null,
        "name": "Sportskeeda"
      },
      "author": "Aniket Thakkar",
      "title": "Latest Free Fire redeem code to get Weapon loot crate today (14 October 2021) - Sportskeeda",
      "description": "Gun crates are one of the ways that players in Free Fire can obtain impressive and appealing gun skins.",
      "url": "https://www.sportskeeda.com/free-fire/latest-free-fire-redeem-code-get-weapon-loot-crate-today-14-october-2021",
      "urlToImage": "https://staticg.sportskeeda.com/editor/2021/10/d0b83-16341799119781-1920.jpg",
      "publishedAt": "2021-10-14T03:51:50Z",
      "content": null
    },
    {
      "source": {
        "id": null,
        "name": "NDTV News"
      },
      "author": null,
      "title": "BSF Gets Increased Powers In 3 Border States: What It Means - NDTV",
      "description": "Border Security Force (BSF) officers will now have the power toarrest, search, and of seizure to the extent of 50 km inside three newstates sharing international boundaries with Pakistan and Bangladesh.",
      "url": "https://www.ndtv.com/india-news/bsf-gets-increased-powers-in-3-border-states-what-it-means-2574644",
      "urlToImage": "https://c.ndtvimg.com/2021-08/eglno7qk_-bsf-recruitment-2021_625x300_10_August_21.jpg",
      "publishedAt": "2021-10-14T03:44:00Z",
      "content": "This move is quickly snowballing into a debate on state autonomy. New Delhi: Border Security Force (BSF) officers will now have the power to arrest, search, and of seizure to the extent of 50 km ins… [+4143 chars]"
    },
    {
      "source": {
        "id": "the-times-of-india",
        "name": "The Times of India"
      },
      "author": "TIMESOFINDIA.COM",
      "title": "5 health conditions that can make your joints hurt - Times of India",
      "description": "Joint pain caused by these everyday issues generally goes away on its own when you stretch yourself a little and flex your muscles.",
      "url": "https://timesofindia.indiatimes.com/life-style/health-fitness/health-news/5-health-conditions-that-can-make-your-joints-hurt/photostory/86994969.cms",
      "urlToImage": "https://static.toiimg.com/photo/86995017.cms",
      "publishedAt": "2021-10-14T03:30:00Z",
      "content": "Depression is a mental health condition, but the symptoms may manifest even on your physical health. Unexpected aches and pain in the joints that you may experience when suffering from chronic depres… [+373 chars]"
    },
    {
      "source": {
        "id": null,
        "name": "The Indian Express"
      },
      "author": "Devendra Pandey",
      "title": "Rahul Dravid likely to be interim coach for New Zealand series - The Indian Express",
      "description": "It’s learnt that a few Australian coaches expressed interest in the job, but the BCCI isn’t keen as they are focussing on an Indian for the role, before they look elsewhere.",
      "url": "https://indianexpress.com/article/sports/cricket/rahul-dravid-likely-to-be-interim-coach-for-new-zealand-series-7570990/",
      "urlToImage": "https://images.indianexpress.com/2021/05/rahul-dravid.jpg",
      "publishedAt": "2021-10-14T03:26:09Z",
      "content": "Rahul Dravid is likely to be approached by the Indian cricket board to be the interim coach for Indias home series against New Zealand. Head coach Ravi Shastri and the core of the support staff will … [+1972 chars]"
    },
    {
      "source": {
        "id": null,
        "name": "CNBCTV18"
      },
      "author": null,
      "title": "Thursday's top brokerage calls: Infosys, Wipro and more - CNBCTV18",
      "description": "Goldman Sachs has maintained its 'sell' rating on Mindtree largely due to expensive valuations, while UBS expects a muted reaction from Wipro's stock. Here are the top brokerage calls for the day:",
      "url": "https://www.cnbctv18.com/market/stocks/thursdays-top-brokerage-calls-infosys-wipro-and-more-11101072.htm",
      "urlToImage": "https://images.cnbctv18.com/wp-content/uploads/2019/03/buy-sell.jpg",
      "publishedAt": "2021-10-14T03:26:03Z",
      "content": "MiniGoldman Sachs has maintained its 'sell' rating on Mindtree largely due to expensive valuations, while UBS expects a muted reaction from Wipro's stock. Here are the top brokerage calls for the day:"
    }
  ]
} 
```

由于新闻包含在一个名为`articles`的列表中，我们创建了一个值为`res['articles']`的变量`articles`。现在我们正在迭代这个`articles`列表，并将`article["title"]`添加到 `news_headlines`列表中。然后，我们将返回该列表中的前五个新闻标题。

### 如何添加获取天气报告功能

为了获得天气预报，我们使用了 [OpenWeatherMap API](https://openweathermap.org/) 。注册一个免费帐户并获得应用程序 ID。确保在`.env`文件中添加 **OPENWEATHER_APP_ID** 。

```
OPENWEATHER_APP_ID = config("OPENWEATHER_APP_ID")

def get_weather_report(city):
    res = requests.get(
        f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={OPENWEATHER_APP_ID}&units=metric").json()
    weather = res["weather"][0]["main"]
    temperature = res["main"]["temp"]
    feels_like = res["main"]["feels_like"]
    return weather, f"{temperature}℃", f"{feels_like}℃"
```

根据 [OpenWeatherMap API](https://openweathermap.org/current) ，我们需要对上述带有城市名称的 URL 发出 GET 请求。我们将得到如下 JSON 响应:

```
{
    "coord": {
        "lon": 85,
        "lat": 24.7833
    },
    "weather": [
        {
            "id": 721,
            "main": "Haze",
            "description": "haze",
            "icon": "50d"
        }
    ],
    "base": "stations",
    "main": {
        "temp": 26.95,
        "feels_like": 26.64,
        "temp_min": 26.95,
        "temp_max": 26.95,
        "pressure": 1011,
        "humidity": 36
    },
    "visibility": 3000,
    "wind": {
        "speed": 2.57,
        "deg": 310
    },
    "clouds": {
        "all": 57
    },
    "dt": 1637227634,
    "sys": {
        "type": 1,
        "id": 9115,
        "country": "IN",
        "sunrise": 1637195904,
        "sunset": 1637235130
    },
    "timezone": 19800,
    "id": 1271439,
    "name": "Gaya",
    "cod": 200
}
```

我们只需要上面响应中的`weather`、`temperature`和`feels_like`。

### 如何添加获取热门电影功能

为了获得热门电影，我们将使用[电影数据库(TMDB)](https://www.themoviedb.org/) API。注册一个免费帐户并获得 API 密钥。在`.env`文件中添加 **TMDB_API_KEY** 。

```
TMDB_API_KEY = config("TMDB_API_KEY")

def get_trending_movies():
    trending_movies = []
    res = requests.get(
        f"https://api.themoviedb.org/3/trending/movie/day?api_key={TMDB_API_KEY}").json()
    results = res["results"]
    for r in results:
        trending_movies.append(r["original_title"])
    return trending_movies[:5]
```

正如我们为最新新闻标题所做的一样，我们正在创建`trending_movies`列表。然后，根据 TMDB API，我们发出一个 GET 请求。一个示例 JSON 响应如下所示:

```
{
  "page": 1,
  "results": [
    {
      "video": false,
      "vote_average": 7.9,
      "overview": "Shang-Chi must confront the past he thought he left behind when he is drawn into the web of the mysterious Ten Rings organization.",
      "release_date": "2021-09-01",
      "title": "Shang-Chi and the Legend of the Ten Rings",
      "adult": false,
      "backdrop_path": "/cinER0ESG0eJ49kXlExM0MEWGxW.jpg",
      "vote_count": 2917,
      "genre_ids": [28, 12, 14],
      "id": 566525,
      "original_language": "en",
      "original_title": "Shang-Chi and the Legend of the Ten Rings",
      "poster_path": "/1BIoJGKbXjdFDAqUEiA2VHqkK1Z.jpg",
      "popularity": 9559.446,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": "/dK12GIdhGP6NPGFssK2Fh265jyr.jpg",
      "genre_ids": [28, 35, 80, 53],
      "id": 512195,
      "original_language": "en",
      "original_title": "Red Notice",
      "overview": "An Interpol-issued Red Notice is a global alert to hunt and capture the world's most wanted. But when a daring heist brings together the FBI's top profiler and two rival criminals, there's no telling what will happen.",
      "poster_path": "/wdE6ewaKZHr62bLqCn7A2DiGShm.jpg",
      "release_date": "2021-11-04",
      "title": "Red Notice",
      "video": false,
      "vote_average": 6.9,
      "vote_count": 832,
      "popularity": 1990.503,
      "media_type": "movie"
    },
    {
      "genre_ids": [12, 28, 53],
      "original_language": "en",
      "original_title": "No Time to Die",
      "poster_path": "/iUgygt3fscRoKWCV1d0C7FbM9TP.jpg",
      "video": false,
      "vote_average": 7.6,
      "overview": "Bond has left active service and is enjoying a tranquil life in Jamaica. His peace is short-lived when his old friend Felix Leiter from the CIA turns up asking for help. The mission to rescue a kidnapped scientist turns out to be far more treacherous than expected, leading Bond onto the trail of a mysterious villain armed with dangerous new technology.",
      "id": 370172,
      "vote_count": 1804,
      "title": "No Time to Die",
      "adult": false,
      "backdrop_path": "/1953j0QEbtN17WFFTnJHIm6bn6I.jpg",
      "release_date": "2021-09-29",
      "popularity": 4639.439,
      "media_type": "movie"
    },
    {
      "poster_path": "/5pVJ9SuuO72IgN6i9kMwQwnhGHG.jpg",
      "video": false,
      "vote_average": 0,
      "overview": "Peter Parker is unmasked and no longer able to separate his normal life from the high-stakes of being a Super Hero. When he asks for help from Doctor Strange the stakes become even more dangerous, forcing him to discover what it truly means to be Spider-Man.",
      "release_date": "2021-12-15",
      "id": 634649,
      "adult": false,
      "backdrop_path": "/vK18znei8Uha2z7ZhZtBa40HIrm.jpg",
      "vote_count": 0,
      "genre_ids": [28, 12, 878],
      "title": "Spider-Man: No Way Home",
      "original_language": "en",
      "original_title": "Spider-Man: No Way Home",
      "popularity": 1084.815,
      "media_type": "movie"
    },
    {
      "video": false,
      "vote_average": 6.8,
      "overview": "After finding a host body in investigative reporter Eddie Brock, the alien symbiote must face a new enemy, Carnage, the alter ego of serial killer Cletus Kasady.",
      "release_date": "2021-09-30",
      "adult": false,
      "backdrop_path": "/70nxSw3mFBsGmtkvcs91PbjerwD.jpg",
      "vote_count": 1950,
      "genre_ids": [878, 28, 12],
      "id": 580489,
      "original_language": "en",
      "original_title": "Venom: Let There Be Carnage",
      "poster_path": "/rjkmN1dniUHVYAtwuV3Tji7FsDO.jpg",
      "title": "Venom: Let There Be Carnage",
      "popularity": 4527.568,
      "media_type": "movie"
    }
  ],
  "total_pages": 1000,
  "total_results": 20000
} 
```

从上面的回应来看，我们只需要电影的片名。我们得到一个列表`results`,然后遍历它得到电影标题，并把它附加到列表`trending_movies`。最后，我们返回列表的前五个元素。

### 如何添加获取随机笑话功能

要获得一个随机笑话，我们只需要在这个 URL 上发出 get 请求:[https://icanhazdadjoke.com/](https://icanhazdadjoke.com/)。

```
def get_random_joke():
    headers = {
        'Accept': 'application/json'
    }
    res = requests.get("https://icanhazdadjoke.com/", headers=headers).json()
    return res["joke"]
```

### 如何添加获取随机建议功能

为了获得一条随机建议，我们使用了[建议单 API](https://api.adviceslip.com/) 。

```
def get_random_advice():
    res = requests.get("https://api.adviceslip.com/advice").json()
    return res['slip']['advice'] 
```

## 如何创建 Main 方法

要运行这个项目，我们需要创建一个 main 方法。创建一个`main.py`文件并添加以下代码:

```
import requests
from functions.online_ops import find_my_ip, get_latest_news, get_random_advice, get_random_joke, get_trending_movies, get_weather_report, play_on_youtube, search_on_google, search_on_wikipedia, send_email, send_whatsapp_message
from functions.os_ops import open_calculator, open_camera, open_cmd, open_notepad, open_discord
from pprint import pprint

if __name__ == '__main__':
    greet_user()
    while True:
        query = take_user_input().lower()

        if 'open notepad' in query:
            open_notepad()

        elif 'open discord' in query:
            open_discord()

        elif 'open command prompt' in query or 'open cmd' in query:
            open_cmd()

        elif 'open camera' in query:
            open_camera()

        elif 'open calculator' in query:
            open_calculator()

        elif 'ip address' in query:
            ip_address = find_my_ip()
            speak(f'Your IP Address is {ip_address}.\n For your convenience, I am printing it on the screen sir.')
            print(f'Your IP Address is {ip_address}')

        elif 'wikipedia' in query:
            speak('What do you want to search on Wikipedia, sir?')
            search_query = take_user_input().lower()
            results = search_on_wikipedia(search_query)
            speak(f"According to Wikipedia, {results}")
            speak("For your convenience, I am printing it on the screen sir.")
            print(results)

        elif 'youtube' in query:
            speak('What do you want to play on Youtube, sir?')
            video = take_user_input().lower()
            play_on_youtube(video)

        elif 'search on google' in query:
            speak('What do you want to search on Google, sir?')
            query = take_user_input().lower()
            search_on_google(query)

        elif "send whatsapp message" in query:
            speak('On what number should I send the message sir? Please enter in the console: ')
            number = input("Enter the number: ")
            speak("What is the message sir?")
            message = take_user_input().lower()
            send_whatsapp_message(number, message)
            speak("I've sent the message sir.")

        elif "send an email" in query:
            speak("On what email address do I send sir? Please enter in the console: ")
            receiver_address = input("Enter email address: ")
            speak("What should be the subject sir?")
            subject = take_user_input().capitalize()
            speak("What is the message sir?")
            message = take_user_input().capitalize()
            if send_email(receiver_address, subject, message):
                speak("I've sent the email sir.")
            else:
                speak("Something went wrong while I was sending the mail. Please check the error logs sir.")

        elif 'joke' in query:
            speak(f"Hope you like this one sir")
            joke = get_random_joke()
            speak(joke)
            speak("For your convenience, I am printing it on the screen sir.")
            pprint(joke)

        elif "advice" in query:
            speak(f"Here's an advice for you, sir")
            advice = get_random_advice()
            speak(advice)
            speak("For your convenience, I am printing it on the screen sir.")
            pprint(advice)

        elif "trending movies" in query:
            speak(f"Some of the trending movies are: {get_trending_movies()}")
            speak("For your convenience, I am printing it on the screen sir.")
            print(*get_trending_movies(), sep='\n')

        elif 'news' in query:
            speak(f"I'm reading out the latest news headlines, sir")
            speak(get_latest_news())
            speak("For your convenience, I am printing it on the screen sir.")
            print(*get_latest_news(), sep='\n')

        elif 'weather' in query:
            ip_address = find_my_ip()
            city = requests.get(f"https://ipapi.co/{ip_address}/city/").text
            speak(f"Getting weather report for your city {city}")
            weather, temperature, feels_like = get_weather_report(city)
            speak(f"The current temperature is {temperature}, but it feels like {feels_like}")
            speak(f"Also, the weather report talks about {weather}")
            speak("For your convenience, I am printing it on the screen sir.")
            print(f"Description: {weather}\nTemperature: {temperature}\nFeels like: {feels_like}")
```

虽然上面的脚本看起来很长，但它非常简单，容易理解。

如果你仔细观察，我们所做的只是导入所需的模块以及在线和离线功能。然后在 main 方法中，我们做的第一件事是使用`greet_user()`函数问候用户。

接下来，我们运行一个 while 循环，使用`take_user_input()`函数不断地从用户那里获取输入。因为我们在这里有了查询字符串，所以我们可以添加 if-else 梯形来检查`query`字符串上的不同条件。

Note: For Python 3.10, you can use [Python Match Case](https://www.python.org/dev/peps/pep-0636/) instead of if-else ladder.

要运行该程序，您可以使用以下命令:

```
$ python main.py
```

## 结论

我们刚刚在 Python 的帮助下创建了自己的虚拟个人助理。如果您愿意，您可以向应用程序添加更多功能。你可以把这个项目添加到你的简历中，或者只是为了好玩！

观看演示视频，了解它的实际应用:

[https://player.vimeo.com/video/650156113?h=9adb36534c&app_id=122963](https://player.vimeo.com/video/650156113?h=9adb36534c&app_id=122963)

如需完整代码，请查看 GitHub 资源库:[https://GitHub . com/ashutoshkrris/Virtual-Personal-Assistant-using-Python](https://github.com/ashutoshkrris/Virtual-Personal-Assistant-using-Python)