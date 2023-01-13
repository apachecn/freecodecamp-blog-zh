# 用电子方式编写特定于操作系统的代码

> 原文：<https://www.freecodecamp.org/news/how-to-write-os-specific-code-in-electron-bf6379c62ff6/>

使用 electronic 的优势之一是——因为它是跨平台的——我们不必担心我们的应用程序将在什么操作系统上运行。

然而，有时我们需要我们的代码是特定于操作系统的，例如，如果我们将要使用命令控制台或者需要检索一些关于系统的信息。

每次我们想在给定的操作系统上拥有某些功能时，都必须编写多个*if*似乎是多余的工作。它会迅速混淆代码，使代码难以理解和分析。

为了保持代码的整洁和可读性，我们可以创建一个小助手，将 *ifs* 和任何“与操作系统相关”的逻辑一起移除。

### 实施平台

```
const os = require('os');

const platforms = {
  WINDOWS: 'WINDOWS',
  MAC: 'MAC',
  LINUX: 'LINUX',
  SUN: 'SUN',
  OPENBSD: 'OPENBSD',
  ANDROID: 'ANDROID',
  AIX: 'AIX',
};

const platformsNames = {
  win32: platforms.WINDOWS,
  darwin: platforms.MAC,
  linux: platforms.LINUX,
  sunos: platforms.SUN,
  openbsd: platforms.OPENBSD,
  android: platforms.ANDROID,
  aix: platforms.AIX,
};

const currentPlatform = platformsNames[os.platform()];

const findHandlerOrDefault = (handlerName, dictionary) => {
  const handler = dictionary[handlerName];

  if (handler) {
    return handler;
  }

  if (dictionary.default) {
    return dictionary.default;
  }

  return () => null;
};

const byOS = findHandlerOrDefault.bind(null, currentPlatform);

// usage
const whatIsHeUsing = byOS({
  [MAC]: username => `Hi ${username}! You are using Mac.`,
  [WINDOWS]: username => `Hi ${username}! You are using Windows.`,
  [LINUX]: username => `Hi ${username}! You are using Linux.`,
  default: username => `Hi ${username}! You are using something different.`,
});

console.log(whatIsHeUsing('Maciej Cieslar')); // => Hi Maciej Cieslar! You are using Mac.
```

首先，我们看到了包含所有支持的操作系统名称的*平台*对象。它只是为了方便而制造的。然后我们可以使用*平台。WINDOWS* 我们传递给 byOS 函数，而不是每次都用处理程序在我们的对象中键入*‘WINDOWS’*。

接下来，注意*平台名称*对象。按键是调用[*OS . platform()*](https://nodejs.org/api/os.html#os_os_platform)*的结果。**值是来自*平台*对象的键。我们只是将它映射到一个更加用户友好的名称。*

*比如当 *os.platform()* 返回 *win32，*我们将其映射到*平台。WINDOWS* 通过调用*platforms name[OS . platform()]*。*

*在 *currentPlatform，*中，我们保存了我们现在正在使用的平台，这样我们就可以用处理程序将它与给定的对象进行匹配。*

### *实施版本*

*人们甚至可以进一步尝试区分给定操作系统的版本，例如 Windows 7、8 和 10。*

```
*`const os = require('os');

const releaseTest = {
  [platforms.WINDOWS]: (version) => {
    const [majorVersion, minorVersion] = version.split('.');

    // Windows 10 (10,0)
    if (majorVersion === '10') {
      return releases.WIN10;
    }

    // Windows 8.1 (6,3)
    // Windows 8 (6,2)
    // Windows 7 (6,1)
    if (majorVersion === '6') {
      if (minorVersion === '3' || minorVersion === '2') {
        return releases.WIN8;
      }

      return releases.WIN7;
    }

    return releases.WIN7;
  },
  [platforms.MAC]: () => releases.ANY,
  [platforms.LINUX]: () => releases.ANY,
};

const currentRelease = releaseTest[currentPlatform](os.release());

const byRelease = findHandlerOrDefault.bind(null, currentRelease);

// usage
const whatWindowsIsHeUsing = byOS({
  [WINDOWS]: byRelease({
    [WIN7]: username => `Hi ${username}! You are using Windows 7.`,
    [WIN8]: username => `Hi ${username}! You are using Windows 8.`,
    [WIN10]: username => `Hi ${username}! You are using Windows 10.`,
  }),
});

console.log(whatWindowsIsHeUsing('Maciej Cieslar')); // => Hi Maciej Cieslar! You are using Windows 7.`* 
```

*现在我们可以使用 [*os.release()*](https://nodejs.org/api/os.html#os_os_release) 来检查系统的版本。*

*我们可以拆分得到的字符串并检查 Windows 版本。完整的列表可以在这里找到[。至于 Linux/Mac，我真的看不出它有什么用处，所以我把它留在了*版本中。任何*。](https://stackoverflow.com/a/44916050/6569856)*

*在 *whatWindowsIsHeUsing* 中，您可以看到，如果我们在 Windows 上运行应用程序，我们只会检查不同的 Windows 版本。*

*您可以在[库](https://github.com/maciejcieslar/os-specific-electron)中看到代码。*

*非常感谢您的阅读！如果你对如何编写特定于操作系统的代码有更好的想法，请在下面分享！*

*如果你有任何问题或意见，欢迎在下面的评论区提出，或者给我发[消息](https://www.mcieslar.com/contact)。*

*查看我的[社交媒体](https://www.maciejcieslar.com/about/)！*

*[加入我的简讯](http://eepurl.com/dAKhxb)！*

**最初发布于 2018 年 8 月 28 日[www.mcieslar.com](https://www.mcieslar.com/writing-os-specific-code-in-electron)。**