# 使用 Python 的特殊方法使您的代码更加“Python 化”

> 原文：<https://www.freecodecamp.org/news/make-your-code-more-pythonic-using-pythons-special-methods-b348f915852e/>

马可·马森齐奥

# 使用 Python 的特殊方法使您的代码更加“Python 化”

![XAYBrQOTrtYv6JQJlUIHRbkau6mWfHS0o3P7](img/a54277a0f918ff5600db5b3adff88415.png)

在他出色的《流畅的 Python》一书中，卢西亚诺·拉马尔霍谈到了 Python 的“数据模型”他给出了一些优秀的例子，说明该语言如何通过明智地使用定义良好的 API 来实现内部一致性。特别是，他讨论了 Python 的“[特殊方法](https://docs.python.org/3/reference/datamodel.html#special-method-names)”如何实现简洁且可读性强的优雅解决方案的构建。

虽然你可以在网上找到无数关于如何实现*迭代*特殊方法( *__iter__()* 和 friends)的例子，但在这里我想展示一个如何使用两个鲜为人知的特殊方法的例子: *__del__()* 和 *__call__()* 。

对于那些熟悉 C++的人来说，它们实现了两种非常熟悉的模式:析构函数*和函数对象*(也就是说，*操作符()*)。**

### *实现一个自毁密钥*

*![lPGK0cKKFRjq4A--onWrbnrNLis8-6JaYNty](img/1af5ddc29ce16d2555f6527b44d67ecd.png)

Credit: [ke dickinson](https://www.flickr.com/photos/ke-dickinson/7159943526/in/photolist-bUGzLb-JXhz-8MMTUA-o8fGSq-9tdgxB-)* 

*假设我们想要设计一个*加密密钥*，它将依次用*主密钥*加密，并且它的“明文”值将仅用于“传输中”加密和解密我们的数据，否则将仅被加密存储。*

*您可能希望这样做的原因有很多，但最常见的是当要加密的数据非常大且加密非常耗时时。如果*主密钥*被泄露，我们可以撤销它，然后用新的主密钥重新加密加密密钥——这一切都不会导致与解密和重新加密可能价值数 TB 的数据相关的时间损失。*

*事实上，重新加密加密密钥的计算成本可能非常低，因此可以定期进行，以频繁的时间间隔(可能每周一次)轮换*主密钥*以减少攻击面。*

*如果我们使用 [OpenSSL](http://openssl.org) 命令行工具来完成所有的加密和解密任务，我们需要将加密密钥作为“明文”临时存储在一个文件中，我们将使用 shred Linux 工具安全地销毁它。*

*注意，我们在这里使用术语“明文”来表示内容被解密，而不是指纯文本格式。密钥仍然是二进制数据，但是如果被攻击者截获，那么**不会**受到加密保护。*

*然而，仅仅实现对粉碎实用程序的调用作为我们加密算法的最后一步，不足以确保在所有可能的代码路径执行下执行。可能会出现错误、引发异常、正常终止(Control-c)或程序突然终止。*

*防范各种可能性不仅令人厌倦，而且容易出错。相反，我们可以让 Python 解释器来为我们做艰苦的工作，并确保当对象被垃圾收集时某些动作总是被执行。*

*注意，这里展示的技术不适用于 SIGKILL 情况(又名 kill -9)，对于 SIGKILL 情况，您需要使用更高级的技术(信号处理程序)。*

*其思想是创建一个实现 *__del__()* 特殊方法的类，当没有对该对象的进一步引用，并且该对象正在被垃圾收集时，该方法保证总是被**调用。发生这种情况的确切时间取决于实现，但是在常见的 Python 解释器中，它似乎几乎是瞬间发生的。***

*以下是在运行 El Capitan 和 Python 2.7 的 MacOS 笔记本电脑上发生的情况:*

```
*`$ pythonPython 2.7.10 (default, Oct 23 2015, 19:19:21)[GCC 4.2.1 Compatible Apple LLVM 7.0.0 (clang-700.0.59.5)] on darwin>>> class Foo():...     def __del__(self):...         print("I'm gone, goodbye!")...>>> foo = Foo()>>> bar = foo>>> foo = None>>> bar = 99I'm gone, goodbye!>>> another = Foo()>>> ^DI'm gone, goodbye!# 使用 Python 的特殊方法使您的代码更加“Python 化”

> 原文：<https://www.freecodecamp.org/news/make-your-code-more-pythonic-using-pythons-special-methods-b348f915852e/>

马可·马森齐奥

# 使用 Python 的特殊方法使您的代码更加“Python 化”

![XAYBrQOTrtYv6JQJlUIHRbkau6mWfHS0o3P7](img/a54277a0f918ff5600db5b3adff88415.png)

在他出色的《流畅的 Python》一书中，卢西亚诺·拉马尔霍谈到了 Python 的“数据模型”他给出了一些优秀的例子，说明该语言如何通过明智地使用定义良好的 API 来实现内部一致性。特别是，他讨论了 Python 的“[特殊方法](https://docs.python.org/3/reference/datamodel.html#special-method-names)”如何实现简洁且可读性强的优雅解决方案的构建。

虽然你可以在网上找到无数关于如何实现*迭代*特殊方法( *__iter__()* 和 friends)的例子，但在这里我想展示一个如何使用两个鲜为人知的特殊方法的例子: *__del__()* 和 *__call__()* 。

对于那些熟悉 C++的人来说，它们实现了两种非常熟悉的模式:析构函数*和函数对象*(也就是说，*操作符()*)。**

### *实现一个自毁密钥*

*![lPGK0cKKFRjq4A--onWrbnrNLis8-6JaYNty](img/1af5ddc29ce16d2555f6527b44d67ecd.png)

Credit: [ke dickinson](https://www.flickr.com/photos/ke-dickinson/7159943526/in/photolist-bUGzLb-JXhz-8MMTUA-o8fGSq-9tdgxB-)* 

*假设我们想要设计一个*加密密钥*，它将依次用*主密钥*加密，并且它的“明文”值将仅用于“传输中”加密和解密我们的数据，否则将仅被加密存储。*

*您可能希望这样做的原因有很多，但最常见的是当要加密的数据非常大且加密非常耗时时。如果*主密钥*被泄露，我们可以撤销它，然后用新的主密钥重新加密加密密钥——这一切都不会导致与解密和重新加密可能价值数 TB 的数据相关的时间损失。*

*事实上，重新加密加密密钥的计算成本可能非常低，因此可以定期进行，以频繁的时间间隔(可能每周一次)轮换*主密钥*以减少攻击面。*

*如果我们使用 [OpenSSL](http://openssl.org) 命令行工具来完成所有的加密和解密任务，我们需要将加密密钥作为“明文”临时存储在一个文件中，我们将使用 shred Linux 工具安全地销毁它。*

*注意，我们在这里使用术语“明文”来表示内容被解密，而不是指纯文本格式。密钥仍然是二进制数据，但是如果被攻击者截获，那么**不会**受到加密保护。*

*然而，仅仅实现对粉碎实用程序的调用作为我们加密算法的最后一步，不足以确保在所有可能的代码路径执行下执行。可能会出现错误、引发异常、正常终止(Control-c)或程序突然终止。*

*防范各种可能性不仅令人厌倦，而且容易出错。相反，我们可以让 Python 解释器来为我们做艰苦的工作，并确保当对象被垃圾收集时某些动作总是被执行。*

*注意，这里展示的技术不适用于 SIGKILL 情况(又名 kill -9)，对于 SIGKILL 情况，您需要使用更高级的技术(信号处理程序)。*

*其思想是创建一个实现 *__del__()* 特殊方法的类，当没有对该对象的进一步引用，并且该对象正在被垃圾收集时，该方法保证总是被**调用。发生这种情况的确切时间取决于实现，但是在常见的 Python 解释器中，它似乎几乎是瞬间发生的。***

*以下是在运行 El Capitan 和 Python 2.7 的 MacOS 笔记本电脑上发生的情况:*

*
```

*如您所见，当不再有对“析构函数”方法的引用时( *foo* )或者当解释器退出时( *bar* )，该方法将被调用。*

*下面的代码片段展示了我们如何实现我们的“自加密”密钥。我称之为 *SelfDestructKey* 是因为它真正的特点是*在退出时销毁*加密密钥的明文版本。*

```
*`class SelfDestructKey(object):    """A self-destructing key: it will shred its contents when it gets deleted.        This key also encrypts itself with the given key before writing itself out to a file.    """     def __init__(self, encrypted_key, keypair):        """Creates an encryption key, using the given keypair to encrypt/decrypt it.         The plaintext version of this key is kept in a temporary file that will be securely        destroyed upon this object becoming garbage collected.         :param encrypted_key the encrypted version of this key is kept in this file: if it            does not exist, it will be created when this key is saved        :param keypair a tuple containing the (private, public) key pair that will be used to            decrypt and encrypt (respectively) this key.        :type keypair collections.namedtuple (Keypair)        """        self._plaintext = mkstemp()[1]        self.encrypted = encrypted_key        self.key_pair = keypair        if not os.path.exists(encrypted_key):            openssl('rand', '32', '-out', self._plaintext)        else:            with open(self._plaintext, 'w') as self_decrypted:                openssl('rsautl', '-decrypt', '-inkey', keypair.private, _in=encrypted_key,                        _out=self_decrypted)     def __str__(self):        return self._plaintext     def __del__(self):        try:            if not os.path.exists(self.encrypted):                self._save()            shred(self._plaintext)        except ErrorReturnCode as rcode:            raise RuntimeError(                "Either we could not save encrypted or not shred the plaintext passphrase "                "in file {plain} to file {enc}.  You will have to securely delete the plaintext "                "version using something like `shred -uz {plain}".format(                    plain=self._plaintext, enc=self.encrypted))     def _save(self):        """ Encrypts the contents of the key and writes it out to disk.         :param dest: the full path of the file that will hold the encrypted contents of this key.        :param key: the name of the file that holds an encryption key (the PUBLIC part of a key pair).        :return: None        """        if not os.path.exists(self.key_pair.public):            raise RuntimeError("Encryption key file '%s' not found" % self.key_pair.public)        with open(self._plaintext, 'rb') as selfkey:            openssl('rsautl', '-encrypt', '-pubin', '-inkey', self.key_pair.public,                    _in=selfkey, _out=self.encrypted)`*
```

*另外，请注意我是如何实现 *__str__()* 方法的，这样我就可以通过调用以下命令获得包含明文密钥的文件名:*

```
*`passphrase = SelfDestructKey(secret_file, keypair=keys) encryptor = FileEncryptor(    secret_keyfile=str(passphrase),    plain_file=plaintext,    dest_dir=enc_cfg.out)`*
```

*请注意，这是代码的简化版本。完整的代码可以从 [filecrypt](https://github.com/massenz/filecrypt) Github 仓库获得，在[的博客文章](https://codetrips.com/2016/07/13/filecrypt-openssl-file-encryption)中有更全面的解释。*

*我们可以轻松地实现 *__str__()* 方法来返回加密密钥的实际内容。*

*尽管如此，如果您查看使用加密密钥的代码，我们根本不需要调用 *_save()* 方法或直接调用 shred 实用程序。当密码超出范围或脚本终止(正常或异常)时，解释器会处理这些问题。*

### *用可调用对象实现命令模式*

*![HeGyL0Ptmx1urLNbVe6DGXC8gtDcxDvhx6MQ](img/c1f17ad4f82be3ba450e5f40fa48f4e2.png)

Credit: [Defence-Imagery via Pixabay](https://pixabay.com/en/users/Defence-Imagery-11/)* 

*Python 有一个叫做 *callable、*的概念，它本质上是“可以像函数一样被调用的东西。”这遵循了*鸭子打字*的方法:“如果它看起来像一只鸭子，而且叫起来像一只鸭子，那么它就是一只鸭子。”在*可调用*的情况下，“如果它看起来像函数，并且可以像函数一样被调用，那么它**就是**函数。”*

*要使一个类对象表现得像一个*可调用的，*我们需要做的就是定义一个 *__call__()* 方法，然后像任何其他“普通”的类方法一样实现它。*

*假设我们想要实现一个“命令运行器”脚本，它(类似于 git)可以获取一组子命令并执行它们。一种方法是在我们的 CommandRunner 类中使用[命令模式](http://amzn.to/29QSxB5):*

```
*`class CommandRunner(object):    """Implements the Command pattern, with the help of the       __call__() special method."""     def __init__(self, config):        """Initiailize the Runner with the configuration            from parsing the command line.            :param config the command-line arguments, as parsed                 by ``argparse``           :type config Namespace        """        self._config = config     def __call__(self):        method = self._config.cmd        if hasattr(self, method):            callable_meth = self.__getattribute__(method)            if callable_meth:                callable_meth()        else:            raise RuntimeError('Unexpected command "{}"; not found'.format(method))     def run(self):        # Do something with the files        pass     def build(self):        # Call an external method that takes a list of files        build(self._config.files)     def diff(self):        """Will compute the diff between the two files passed in"""        if self._config.files and len(self._config.files) == 2:            file_a, file_b = tuple(self._config.files)            diff_files(file_a, file_b)        else:            raise RuntimeError("Not enough arguments for diff: "                               "2 expected, {} found".format(                len(self._config.files) if self._config.files                                         else 'none'))     def diff_all(self):        # This will take a variable number of files and         # will diff them all        pass`*
```

*这里的配置初始化参数是由 [argparse](https://docs.python.org/3/library/argparse.html) 库返回的名称空间对象:*

```
*`def parse_command():    """ Parse command line arguments and returns a config object`*
```

```
 *`:return: the configured options    :rtype: Namespace or None    """    parser = argparse.ArgumentParser()     # Removed the `help` argument for better readability;    # always include that to help your user, when they invoke your     # script with the `--help` flag.    parser.add_argument('--host', default='localhost')    parser.add_argument('-p', '--port', type=int, default=8080,)    parser.add_argument('--workdir', default=default_wkdir)     parser.add_argument('cmd', default='run', choices=[        'run', 'build', 'diff', 'diff_all'])    parser.add_argument('files', nargs=argparse.REMAINDER")    return parser.parse_args()`*
```

*要调用这个脚本，我们可以使用如下代码:*

```
*`$ ./main.py run my_file.py`*
```

*或者:*

```
*`$ ./main.py diff file_1.md another_file.md`*
```

*值得指出的是，我们如何使用另一种特殊方法( *__getattribute__()* )和 *hasattr()* 方法来防止错误，这是上述 Python 的*数据模型*的一部分:*

```
*`if hasattr(self, method):    callable_meth = self.__getattribute__(method)`*
```

*请注意，我们可以使用 *__getattr__()* 特殊方法来定义当试图访问不存在的属性时类的行为，但是在这种情况下，在调用时这样做可能更容易。*

*假设我们告诉 *argparse* 在解析 *cmd* 参数时，将可能的值限制为*选择*的值，我们就可以保证永远不会得到“未知”的命令。然而， *CommandRunner* 类不需要知道这一点，它可以用于我们没有这种保证的其他情况。更别说我们离某个非常莫名其妙的 bug 只有一个错别字了，如果我们没有在 *__call__()* 做足功课的话。*

*为了实现所有这些，我们只需要实现一个简单的 *__main__* 代码片段:*

```
*`if __name__ == '__main__':     cfg = parse_command()     try: runner = CommandRunner(cfg)         runner() # Looks like a function, let's use it like one.     except Exception as ex:         logging.error("Could not execute command `{}`: {}".format(            cfg.cmd, ex))         exit(1)`*
```

*请注意我们是如何调用 runner 的，就好像它是一个方法一样。这将依次执行 *__call__()* 方法，并运行所需的命令。*

*我们真诚地希望每个人都同意，这是一个看起来比怪物更愉快的代码，例如:*

```
*`# DON'T DO THIS AT HOME# Please avoid castle-of-ifs, they are just plain ugly.if cfg.cmd == "build":    # do something to buildelif cfg.cmd == "run":    # do something to runelif cfg.cmd == "diff":    # do something to diffelif cfg.cmd == "diff_all":    # do something to diff_allelse:    print("Unknown command", cfg.cmd)`*
```

*了解 Python 的“特殊方法”将使您的代码在不同情况下更容易阅读和重用。这也将使你的代码更加“pythonicic 化”,并能被其他 python 爱好者立即识别，从而使你的意图更容易理解和推理。*

**最初发布于[codetrips.com](https://codetrips.com/2016/07/22/python-magic-methods/)2016 年 7 月 22 日。**