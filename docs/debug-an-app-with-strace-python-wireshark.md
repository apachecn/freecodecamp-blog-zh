# 如何用 Strace、Python 和 Wireshark 调试应用程序

> 原文：<https://www.freecodecamp.org/news/debug-an-app-with-strace-python-wireshark/>

在这篇文章中，我将向您展示一些技术，您可以使用这些技术来解决程序运行不正常的问题。

这个列表不是通用的，取决于你在寻找什么，它可能不足以解决你的问题。但这应该是个好的开始。

在我们开始之前，您应该熟悉一些事情:

*   如何在 Linux 上运行命令
*   DNS、HTTP 和 TLS 等协议
*   像 Python 这样的脚本语言

不用太担心。我会给你足够的信息，所以你可以跟着教程。

你会学到什么？

*   strace、nslookup 和 RPM 的基本用法
*   如何使用 Python 调试器的一些有趣特性
*   如何使用 Wireshark 分析流量

## **问题:无法上传文件到 asci NEMA**

所以我用很酷的开源项目[asci NEMA](https://asciinema.org/docs/usage)为我的小型开源项目 [SuricataLog](https://pypi.org/project/SuricataLog) 录制了一个 asciicast。然后我决定与世界分享。

但与其他录音不同的是，这段录音拒绝上传:

```
[josevnz@dmaf5 SuricataLog]$ asciinema upload demo-ascii.cast 
asciinema: upload failed: <urlopen error [Errno 32] Broken pipe>
asciinema: retry later by running: asciinema upload demo-ascii.cast 
```

Asciinema 没有告诉我们太多关于错误的信息。例如:

*   该工具尝试使用什么服务器和端口来上传文件？
*   协议握手的哪一部分失败了？
*   目的地是个问题还是我这边的问题？

我们将使用一些工具来看看这里发生了什么。

## **如何用 strace 运行程序**

什么是 [strace](https://strace.io/) ？

> strace 是一个用于 Linux 的诊断、调试和指导用户空间实用程序。它用于监视和篡改进程和 Linux 内核之间的交互，包括系统调用、信号传递和进程状态的改变。系统管理员、诊断专家和故障检修人员将会发现它对于解决那些源代码不容易获得的程序的问题是非常宝贵的，因为它们不需要重新编译来跟踪它们。

当你没有一个应用程序的源代码，但是你需要知道当你调用一个程序时什么是错的时候，strace 是非常有用的。是时候看看它的实际效果了:

```
josevnz@dmaf5 SuricataLog]$ strace asciinema upload demo-ascii.cast
xecve("/usr/bin/asciinema", ["asciinema", "upload", "demo-ascii.cast"], 0x7ffdcddb1160 /* 55 vars */) = 0
brk(NULL)                               = 0x55e912d58000
arch_prctl(0x3001 /* ARCH_??? */, 0x7fff2f136480) = -1 EINVAL (Invalid argument)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=92299, ...}) = 0
mmap(NULL, 92299, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f69dd26a000
close(3)                                = 0
# 
# Commented out LOTS output
# ...
close(4)                                = 0
socket(AF_INET, SOCK_DGRAM|SOCK_CLOEXEC, IPPROTO_IP) = 4
connect(4, {sa_family=AF_INET, sin_port=htons(443), sin_addr=inet_addr("109.107.38.233")}, 16) = 0
getsockname(4, {sa_family=AF_INET, sin_port=htons(33771), sin_addr=inet_addr("192.168.1.22")}, [28 => 16]) = 0
connect(4, {sa_family=AF_UNSPEC, sa_data="\0\0\0\0\0\0\0\0\0\0\0\0\0\0"}, 16) = 0
connect(4, {sa_family=AF_INET, sin_port=htons(443), sin_addr=inet_addr("109.107.37.0")}, 16) = 0
getsockname(4, {sa_family=AF_INET, sin_port=htons(35023), sin_addr=inet_addr("192.168.1.22")}, [28 => 16]) = 0
close(4)                                = 0
socket(AF_INET, SOCK_STREAM|SOCK_CLOEXEC, IPPROTO_TCP) = 4
connect(4, {sa_family=AF_INET, sin_port=htons(443), sin_addr=inet_addr("109.107.38.233")}, 16) = 0
setsockopt(4, SOL_TCP, TCP_NODELAY, [1], 4) = 0
getsockopt(4, SOL_SOCKET, SO_TYPE, [1], [4]) = 0
getsockname(4, {sa_family=AF_INET, sin_port=htons(55682), sin_addr=inet_addr("192.168.1.22")}, [128 => 16]) = 0
ioctl(4, FIONBIO, [0])                  = 0
getpeername(4, {sa_family=AF_INET, sin_port=htons(443), sin_addr=inet_addr("109.107.38.233")}, [16]) = 0
getpid()                                = 45070
getpid()                                = 45070
getpid()                                = 45070
getpid()                                = 45070
getpid()                                = 45070
getpid()                                = 45070
write(4, "\26\3\1\2\0\1\0\1\374\3\3\327\2*\v\316GT*\262\207\235\264\317\254\37$|,V\205\362"..., 517) = 517
read(4, "\26\3\3\0z", 5)                = 5
read(4, "\2\0\0v\3\3E\217G?\335.;\212\237pn\16\257$\2\324J\324y\17\306\263\325i\264p"..., 122) = 122
read(4, "\24\3\3\0\1", 5)               = 5
read(4, "\1", 1)                        = 1
read(4, "\27\3\3\0\27", 5)              = 5
read(4, "0{}\22t9\264\265\340j\362\30\342\360\234\205\1\370\33\246\1z'", 23) = 23
read(4, "\27\3\3\17\335", 5)            = 5
read(4, "\17\5\310\261\355\271\227oUaI\366\361]\3\275q)\5{\367z\20\233\345\352k?\371\272\23\237"..., 4061) = 4061
stat("/etc/pki/tls/certs/8d33f237.0", 0x7ffd20be3620) = -1 ENOENT (No such file or directory)
read(4, "\27\3\3\1\31", 5)              = 5
read(4, "t\27\337\366G6\226Qs\273\327\314,\205\221\222Xu\233\21%\0s\340\270\224\330\t\2774\222h"..., 281) = 281
read(4, "\27\3\3\0005", 5)              = 5
read(4, "\204{\314\232\311\0P-*$\245\315\271\236c\210N\315\5\371\364\23\235\16\0350N0K\246\336\374"..., 53) = 53
write(4, "\24\3\3\0\1\1\27\3\3\0005\361\311\347\t\254m#\273\204\350\16\343\34P\320sS\211\30\232<"..., 64) = 64
ioctl(4, FIONBIO, [0])                  = 0
write(4, "\27\3\3\1\251\271\2673-\30\313\253\363\320H0\224\370Q\353(#?,\216\3\341\315|J\353\303"..., 430) = 430
write(4, "\27\3\3@\21\20\221\240\331\2737\10\244pv\312B\n\rn\272\33\336T\216\f\303\374k\177c\25"..., 16406) = 16406
write(4, "\27\3\3@\21\214\30\262\240s\216\240\354e\31\304Q\337Oy\21y\373\241g\311\224)\26\320\10{"..., 16406) = 16406
write(4, "\27\3\3@\21\36\323\240\376\276\224\35\f\10!@\36D\347\33ay\2617Hpv\4d\267y7"..., 16406) = 16406
write(4, "\27\3\3@\21\366x\264\242O2\7?\7\334\221W\24\2\f)\"@\20\375~\354\243W\32\0c"..., 16406) = 16406
write(4, "\27\3\3@\21\354\32W\36\265g\304\314\376\205\315\20\22\10c\333\342\264\330\366SS\4\217\356:V"..., 16406) = 16406
write(4, "\27\3\3@\21\1\274\35\335\271n\235e\202\202\207\221~\313\0y\210\344\312\32r\347\306x]\241C"..., 16406) = 16406
write(4, "\27\3\3@\21I\315\202\274\342\274\26\335qx\22-\226\322\320\203\231\274wLB\250\252\2\352\367\""..., 16406) = 8716
write(4, "\377\4m\341\317\376SUr\rQ\221\207\22#\262\314B7\33_v\310\271\fl\v\242\fK\v?"..., 7690) = -1 EPIPE (Broken pipe)
--- SIGPIPE {si_signo=SIGPIPE, si_code=SI_USER, si_pid=45070, si_uid=1000} ---
close(4)                                = 0
close(3)                                = 0
write(2, "\33[0;31masciinema: upload failed:"..., 76asciinema: upload failed: <urlopen error [Errno 32] Broken pipe>
) = 76
write(2, "\33[0;31masciinema: retry later by"..., 79asciinema: retry later by running: asciinema upload demo-ascii.cast
) = 79
munmap(0x7fa1aa089000, 12447744)        = 0
rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7fa1bad0fa70}, {sa_handler=0x7fa1baf551d0, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7fa1bad0fa70}, 8) = 0
munmap(0x7fa1ac649000, 593920)          = 0
exit_group(1)                           = ?
+++ exited with 1 +++ 
```

看这个套接字调用(`man 2 getpeername`):

```
getpeername(4, {sa_family=AF_INET, sin_port=htons(443), sin_addr=inet_addr("109.107.38.233")}, [16]) = 0 
```

在这下面，正如您所看到的，我们实际上正在向网站写入数据，但连接中断了:

```
write(4, "\27\3\3@\21\366x\264\242O2\7?\7\334\221W\24\2\f)\"@\20\375~\354\243W\32\0c"..., 16406) = 16406
write(4, "\27\3\3@\21\354\32W\36\265g\304\314\376\205\315\20\22\10c\333\342\264\330\366SS\4\217\356:V"..., 16406) = 16406
write(4, "\27\3\3@\21\1\274\35\335\271n\235e\202\202\207\221~\313\0y\210\344\312\32r\347\306x]\241C"..., 16406) = 16406
write(4, "\27\3\3@\21I\315\202\274\342\274\26\335qx\22-\226\322\320\203\231\274wLB\250\252\2\352\367\""..., 16406) = 8716
write(4, "\377\4m\341\317\376SUr\rQ\221\207\22#\262\314B7\33_v\310\271\fl\v\242\fK\v?"..., 7690) = -1 EPIPE (Broken pipe)
--- SIGPIPE {si_signo=SIGPIPE, si_code=SI_USER, si_pid=45070, si_uid=1000} --- 
```

那么‘109 . 107 . 38 . 233’是谁呢？：

```
[josevnz@dmaf5 SuricataLog]$ nslookup 109.107.38.233
233.38.107.109.in-addr.arpa	name = cip-109-107-38-233.gb1.brightbox.com. 
```

你可以在[关于](https://asciinema.org/about)的网页上看到，brightbox.com 为 asciinema 提供主机服务。

那么什么是错的呢？这并不是说网站关闭或无法访问。我们能进一步挖掘吗？

## **如果我只有源代码——用 Python 调试器深入研究**

```
[josevnz@dmaf5 SuricataLog]$ file /usr/bin/asciinema
/usr/bin/asciinema: Python script, ASCII text executable 
```

哦，是的，我们有！出于好奇，您打开 asciinema 脚本:

```
#!/usr/bin/python3
# EASY-INSTALL-ENTRY-SCRIPT: 'asciinema==2.0.2','console_scripts','asciinema'
import re
import sys 

# for compatibility with easy_install; see #2198
__requires__ = 'asciinema==2.0.2'

try:
    from importlib.metadata import distribution
except ImportError:
    try:
        from importlib_metadata import distribution
    except ImportError:
        from pkg_resources import load_entry_point

def importlib_load_entry_point(spec, group, name):
    dist_name, _, _ = spec.partition('==')
    matches = ( 
        entry_point
        for entry_point in distribution(dist_name).entry_points
        if entry_point.group == group and entry_point.name == name
    )   
    return next(matches).load()

globals().setdefault('load_entry_point', importlib_load_entry_point)

if __name__ == '__main__':
    sys.argv[0] = re.sub(r'(-script\.pyw?|\.exe)?

主脚本是用[简单安装](https://setuptools.pypa.io/en/latest/deprecated/easy_install.html)生成的，这意味着`asciinema.py`只是有趣代码的包装。为了找出有趣的东西在哪里，让我们通过 Python [pdb 调试器](https://www.redhat.com/sysadmin/python-debugger-pdb)运行脚本:

```
[josevnz@dmaf5 SuricataLog]$ python3 -m pdb /usr/bin/asciinema upload demo-ascii.cast 
> /usr/bin/asciinema(3)<module>()
-> import re
(Pdb) n
> /usr/bin/asciinema(4)<module>()
-> import sys
(Pdb) c
asciinema: upload failed: <urlopen error [Errno 32] Broken pipe>
asciinema: retry later by running: asciinema upload demo-ascii.cast
The program exited via sys.exit(). Exit status: 1 
```

不完全是我们需要的。程序运行，遇到异常，然后从头开始重新启动。

我们来稍微作弊一下。asciinema 是用 RPM 安装的吗(我用的是 Fedora Linux)？

```
[josevnz@dmaf5 SuricataLog]$ rpm -qif /usr/bin/asciinema
Name        : asciinema
Version     : 2.0.2
Release     : 6.fc33 
```

我们试图上传一个文件，任何看起来像上传者的东西？

```
josevnz@dmaf5 SuricataLog]$ rpm -qil asciinema|grep -i uploa
/usr/lib/python3.9/site-packages/asciinema/commands/__pycache__/upload.cpython-39.opt-1.pyc
/usr/lib/python3.9/site-packages/asciinema/commands/__pycache__/upload.cpython-39.pyc
/usr/lib/python3.9/site-packages/asciinema/commands/upload.py 
```

啊，越来越有趣了！让我们打开“upload.py”:

```
from asciinema.commands.command import Command
from asciinema.api import APIError

class UploadCommand(Command):

    def __init__(self, api, filename):
        Command.__init__(self)
        self.api = api 
        self.filename = filename

    def execute(self):
        try:
            result, warn = self.api.upload_asciicast(self.filename)

            if warn:
                self.print_warning(warn)

            self.print(result.get('message') or result['url'])

        except OSError as e:
            self.print_error("upload failed: %s" % str(e))
            return 1

        except APIError as e:
            self.print_error("upload failed: %s" % str(e))
            self.print_error("retry later by running: asciinema upload %s" % self.filename)
            return 1

        return 0 
```

让我们在 UploadCommand 中设置几个断点(我的代码副本中的第 14、26 行):

```
[josevnz@dmaf5 SuricataLog]$ python3 -m pdb /usr/bin/asciinema upload demo-ascii.cast 
> /usr/bin/asciinema(3)<module>()
-> import re
(Pdb) b /usr/lib/python3.9/site-packages/asciinema/commands/upload.py:14
Breakpoint 1 at /usr/lib/python3.9/site-packages/asciinema/commands/upload.py:14
(Pdb) c
> /usr/lib/python3.9/site-packages/asciinema/commands/upload.py(14)execute()
-> result, warn = self.api.upload_asciicast(self.filename)
(Pdb) c
> /usr/lib/python3.9/site-packages/asciinema/commands/upload.py(26)execute()
-> self.print_error("upload failed: %s" % str(e))
(Pdb) n
asciinema: upload failed: <urlopen error [Errno 32] Broken pipe>
> /usr/lib/python3.9/site-packages/asciinema/commands/upload.py(27)execute()
-> self.print_error("retry later by running: asciinema upload %s" % self.filename)
(Pdb) ll
 12  	    def execute(self):
 13  	        try:
 14 B	            result, warn = self.api.upload_asciicast(self.filename)
 15  	
 16  	            if warn:
 17  	                self.print_warning(warn)
 18  	
 19  	            self.print(result.get('message') or result['url'])
 20  	
 21  	        except OSError as e:
 22  	            self.print_error("upload failed: %s" % str(e))
 23  	            return 1
 24  	
 25  	        except APIError as e:
 26 B	            self.print_error("upload failed: %s" % str(e))
 27  ->	            self.print_error("retry later by running: asciinema upload %s" % self.filename)
 28  	            return 1
 29  	
 30  	        return 0 
```

我们有一个错误。这种类型的异常有什么有趣的吗？

```
(Pdb) source APIError
11  	class APIError(Exception):
12  	    pass
(Pdb) e.args
('<urlopen error [Errno 32] Broken pipe>',) 
```

所以没什么特别的，直接来源于`Exception`。此外，异常的参数只是错误消息。

当然，下一步是看看这个错误是否来自一个已知的库(在互联网上搜索错误)。

我是在 [GitHub](https://github.com/asciinema/asciinema/issues/335) 上发现这个问题的。进一步阅读你可以看到慷慨的 Asciicast 作者正在自掏腰包支付存储费用，因此我们所有人都可以免费享受在线存储:

> 最大大小设置为 2MB，这似乎太低了。我把它提高到 5MB。这并不多，但我是自掏腰包支付存储(S3)，所以我不能为每个用户提供千兆字节的存储。让我知道这对你是否有效。我不介意增加更多，但是现在我想在用户需求和主机成本之间找到一个好的中间点。

所以让我们确认这确实是原因:

```
[josevnz@dmaf5 SuricataLog]$ ls -lh demo-ascii.cast 
-rw-rw-r-- 1 josevnz josevnz 12M Apr 21 15:44 demo-ascii.cast 
```

到目前为止，文件太大似乎是罪魁祸首。

我仍在运行调试器，我很想看看加载了哪些 asciinema 模块。为此，切换到“*交互*模式，并使用[列表理解](https://peps.python.org/pep-0202/)和[正则表达式](https://docs.python.org/3/howto/regex.html)获得列表:

```
(Pdb) interact
*interactive*
>>> import re
>>> import sys
>>> import pprint
>>> pprint.pprint([name for name in sys.modules.keys() if re.search('asciinema', name)], indent=True)
['asciinema.asciicast.events',
 'asciinema.asciicast.v1',
 'asciinema.asciicast.v2',
 'asciinema.asciicast',
 'asciinema.term',
 'asciinema.pty',
 'asciinema',
 'asciinema.config',
 'asciinema.commands',
 'asciinema.commands.command',
 'asciinema.commands.auth',
 'asciinema.asciicast.raw',
 'asciinema.http_adapter',
 'asciinema.urllib_http_adapter',
 'asciinema.api',
 'asciinema.commands.record',
 'asciinema.player',
 'asciinema.commands.play',
 'asciinema.commands.cat',
 'asciinema.commands.upload',
 'asciinema.__main__']
>>> 
```

下面这些看起来可能包含一些线索:

*   asciinema.urllib_http_adapter
*   asci NEMA . commands . upload
*   asciinema.http _ adapter

退出调试器(或转到另一个终端)并搜索 urllib_http_adapter:

```
[josevnz@dmaf5 SuricataLog]$ find /usr/lib/python3.9/site-packages/asciinema/ -name 'urllib_http_adapter*'
/usr/lib/python3.9/site-packages/asciinema/__pycache__/urllib_http_adapter.cpython-39.opt-1.pyc
/usr/lib/python3.9/site-packages/asciinema/__pycache__/urllib_http_adapter.cpython-39.pyc
/usr/lib/python3.9/site-packages/asciinema/urllib_http_adapter.py 
```

如果您打开该文件，您会看到“post”方法是我们要进行故障诊断的方法:

```
class URLLibHttpAdapter:

    def post(self, url, fields={}, files={}, headers={}, username=None, password=None):
        content_type, body = MultipartFormdataEncoder().encode(fields, files)

        headers = headers.copy()
        headers["Content-Type"] = content_type

        if password:
            auth = "%s:%s" % (username, password)
            encoded_auth = base64.encodebytes(auth.encode('utf-8'))[:-1]
            headers["Authorization"] = b"Basic " + encoded_auth

        request = Request(url, data=body, headers=headers, method="POST")

        try:
            response = urlopen(request)
            status = response.status
            headers = self._parse_headers(response)
            body = response.read().decode('utf-8')
        except HTTPError as e:
            status = e.code
            headers = {}
            body = e.read().decode('utf-8')
        except (http.client.RemoteDisconnected, URLError) as e:
            raise HTTPConnectionError(str(e))

        return (status, headers, body) 
```

第 65 行中的一个断点将把我们带到我们需要的地方:

```
[josevnz@dmaf5 SuricataLog]$ python3 -m pdb /usr/bin/asciinema upload demo-ascii.cast 
> /usr/bin/asciinema(3)<module>()
-> import re
(Pdb) b /usr/lib/python3.9/site-packages/asciinema/urllib_http_adapter.py:65
Breakpoint 1 at /usr/lib/python3.9/site-packages/asciinema/urllib_http_adapter.py:65
(Pdb) c
> /usr/lib/python3.9/site-packages/asciinema/urllib_http_adapter.py(65)post()
-> headers = headers.copy()
(Pdb) ll
 62  	    def post(self, url, fields={}, files={}, headers={}, username=None, password=None):
 63  	        content_type, body = MultipartFormdataEncoder().encode(fields, files)
 64  	
 65 B->	        headers = headers.copy()
 66  	        headers["Content-Type"] = content_type
 67  	
 68  	        if password:
 69  	            auth = "%s:%s" % (username, password)
 70  	            encoded_auth = base64.encodebytes(auth.encode('utf-8'))[:-1]
 71  	            headers["Authorization"] = b"Basic " + encoded_auth
 72  	
 73  	        request = Request(url, data=body, headers=headers, method="POST")
 74  	
 75  	        try:
 76  	            response = urlopen(request)
 77  	            status = response.status
 78  	            headers = self._parse_headers(response)
 79  	            body = response.read().decode('utf-8')
 80  	        except HTTPError as e:
 81  	            status = e.code
 82  	            headers = {}
 83  	            body = e.read().decode('utf-8')
 84  	        except (http.client.RemoteDisconnected, URLError) as e:
 85  	            raise HTTPConnectionError(str(e))
 86  	
 87  	        return (status, headers, body)
(Pdb) args
self = <asciinema.urllib_http_adapter.URLLibHttpAdapter object at 0x7f59ed3e4640>
url = 'https://asciinema.org/api/asciicasts'
fields = {}
files = {'asciicast': ('ascii.cast', <_io.BufferedReader name='demo-ascii.cast'>)}
headers = {'User-Agent': 'asciinema/2.0.2 CPython/3.9.9 Linux/5.14.18-100.fc33.x86_64-x86_64-with-glibc2.32', 'Accept': 'application/json'}
username = 'XXXX'
password = 'XXXX0f1-1d73-43fc-XX36-c9d7ZZZAAAA' 
```

非常有趣——我们完全可以使用以下字段在没有 Python 的情况下练习上传功能(从调试器中使用`args`获得):

*   URL = '[https://asciinema.org/api/asciicasts](https://asciinema.org/api/asciicasts)
*   headers = { ' User-Agent ':' asci NEMA/2 . 0 . 2 CPython/3 . 9 . 9 Linux/5 . 14 . 18-100 . fc33 . x86 _ 64-x86 _ 64-with-glibc 2.32 '，' Accept': 'application/json'}
*   用户名= 'XXXX '
*   密码= ' xxxx0f 1-1d 73-43fc-XX36-c 9d 7 zzzaaaa '

我们会得到什么异常？我们设置了另外两个断点，并让调试器运行，直到到达它们:

```
(Pdb) b 81
Breakpoint 2 at /usr/lib/python3.9/site-packages/asciinema/urllib_http_adapter.py:81
(Pdb) b 85
Breakpoint 3 at /usr/lib/python3.9/site-packages/asciinema/urllib_http_adapter.py:85
(Pdb) c
> /usr/lib/python3.9/site-packages/asciinema/urllib_http_adapter.py(85)post()
-> raise HTTPConnectionError(str(e))
(Pdb) e
URLError(BrokenPipeError(32, 'Broken pipe')) 
```

错误类型为 [BrokenPipeError](https://docs.python.org/3/library/exceptions.html#BrokenPipeError) :

> ConnectionError 的子类，当试图在另一端已关闭的管道上写入，或试图在已关闭写入的套接字上写入时引发。对应于 errno EPIPE 和 ESHUTDOWN。

最后一件事——在将文件发送到网站之前，我们是否读取了内存中的整个文件？

```
[josevnz@dmaf5 SuricataLog]$ python3 -m pdb /usr/bin/asciinema upload demo-ascii.cast 
> /usr/bin/asciinema(3)<module>()
-> import re
(Pdb) b /usr/lib/python3.9/site-packages/asciinema/urllib_http_adapter.py:49
Breakpoint 1 at /usr/lib/python3.9/site-packages/asciinema/urllib_http_adapter.py:49
(Pdb) c
> /usr/lib/python3.9/site-packages/asciinema/urllib_http_adapter.py(49)iter()
-> yield (data, len(data))
(Pdb) len(data)
12444283 
```

12MB，对今天的计算机内存来说不算大，但也不算小。

您还记得我们之前在调试器的帮助下捕获的参数(URL、用户等等)吗？我们现在知道了足够多的信息，可以使用不同的工具( [curl](https://curl.se/) )来尝试上传文件:

```
[josevnz@dmaf5 SuricataLog]$ curl --fail --http1.1 --verbose --user $USER:$(cat ~/.config/asciinema/install-id) https://asciinema.org/api/asciicasts --form asciicast=@demo-ascii.cast
*   Trying 109.107.37.0:443...
* Connected to asciinema.org (109.107.37.0) port 443 (#0)
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*   CAfile: /etc/pki/tls/certs/ca-bundle.crt
  CApath: none
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
* TLSv1.3 (IN), TLS handshake, Server hello (2):
* TLSv1.3 (IN), TLS handshake, Encrypted Extensions (8):
* TLSv1.3 (IN), TLS handshake, Certificate (11):
* TLSv1.3 (IN), TLS handshake, CERT verify (15):
* TLSv1.3 (IN), TLS handshake, Finished (20):
* TLSv1.3 (OUT), TLS change cipher, Change cipher spec (1):
* TLSv1.3 (OUT), TLS handshake, Finished (20):
* SSL connection using TLSv1.3 / TLS_AES_128_GCM_SHA256
* ALPN, server accepted to use http/1.1
* Server certificate:
*  subject: CN=*.asciinema.org
*  start date: Mar  9 06:02:26 2022 GMT
*  expire date: Jun  7 06:02:25 2022 GMT
*  subjectAltName: host "asciinema.org" matched cert's "asciinema.org"
*  issuer: C=US; O=Let's Encrypt; CN=R3
*  SSL certificate verify ok.
* Server auth using Basic with user 'XXXX'
> POST /api/asciicasts HTTP/1.1
> Host: asciinema.org
> Authorization: Basic XXXXX=
> User-Agent: curl/7.71.1
> Accept: */*
> Content-Length: 12444495
> Content-Type: multipart/form-data; boundary=------------------------0d76dac3e1f8aed4
> Expect: 100-continue
> 
* TLSv1.3 (IN), TLS handshake, Newsession Ticket (4):
* Mark bundle as not supporting multiuse
< HTTP/1.1 100 Continue
* Mark bundle as not supporting multiuse
* The requested URL returned error: 413 Request Entity Too Large
* Closing connection 0
* TLSv1.3 (OUT), TLS alert, close notify (256):
curl: (22) The requested URL returned error: 413 Request Entity Too Large 
```

误差`413 Request Entity Too Large` [表示](https://developer.mozilla.org/en-US/docs/web/http/status/413):

> HTTP 413 负载过大响应状态代码表示请求实体大于服务器定义的限制；服务器可能会关闭连接或返回“重试后”头字段。

所以 curl 比 Python 更能告诉我们文件被拒绝的原因。

在我们的连接被切断之前，我们传输了多少数据？让我们看看是否可以使用数据包嗅探器找出答案。

## **如何使用 Wireshark 和 SSLKEYLOGFILE 检查 HTTP 流量**

您可以使用网络嗅探器，如 [Wireshark](https://wireshark.org/) 或众所周知的 [tcpdump](https://www.tcpdump.org/) 来捕获您的机器和 asciinema 网站之间的流量。

当我们使用 HTTPS 时，流量将被加密，但使用许多程序支持的一个称为' [TLS 主加密秘密](https://www.paolotagliaferri.com/overview-of-transport-layer-security-protocol-tls-1-3/)'的功能，您可以解密会话。为此，让我们在客户端上启用[功能](https://everything.curl.dev/usingcurl/tls/sslkeylogfile):

```
export SSLKEYLOGFILE=$HOME/keylogfile.txt 
```

如果支持，将使用密钥填充$SSLKEYLOGFILE 文件:

```
[josevnz@dmaf5 SuricataLog]$ export SSLKEYLOGFILE=$HOME/keylogfile.txt
[josevnz@dmaf5 SuricataLog]$ /usr/bin/asciinema upload demo-ascii.cast 
asciinema: upload failed: <urlopen error [Errno 32] Broken pipe>
asciinema: retry later by running: asciinema upload demo-ascii.cast
[josevnz@dmaf5 SuricataLog]$ ls -l $SSLKEYLOGFILE
-rw-rw-r-- 1 josevnz josevnz 832 Apr 21 21:02 /home/josevnz/keylogfile.txt

[josevnz@dmaf5 SuricataLog]$ cat /home/josevnz/keylogfile.txt

# TLS secrets log file, generated by OpenSSL / Python
SERVER_HANDSHAKE_TRAFFIC_SECRET 2987e32066d608a3de0cdd896f62801290045c2616abfaef5fac1c6986131847 4dd1a1bc1261a84886b28ee72798d89ba77d7de7051b3dcdafd548a621ed1124
EXPORTER_SECRET 2987e32066d608a3de0cdd896f62801290045c2616abfaef5fac1c6986131847 1ec8d94b7ec373a984abed25fa0dfaa6346fe67feea0516d7e2e46a666a12614
SERVER_TRAFFIC_SECRET_0 2987e32066d608a3de0cdd896f62801290045c2616abfaef5fac1c6986131847 e1d8fa6dba5eea00d4e52af0ce7e7007da0ade4c9dd9da3d9a060b55880531f1
CLIENT_HANDSHAKE_TRAFFIC_SECRET 2987e32066d608a3de0cdd896f62801290045c2616abfaef5fac1c6986131847 903bf381f927d783e72846201e87203ff130d9cf21f84cf0b923834d69c3fe76
CLIENT_TRAFFIC_SECRET_0 2987e32066d608a3de0cdd896f62801290045c2616abfaef5fac1c6986131847 495b5acf783869d74a7521e3b9c3f7bfc6dbc25e24ba95f684e96f6b2a435206
SERVER_HANDSHAKE_TRAFFIC_SECRET 82cab66e906c3cd3c58b3aeeecd66b2a12e521704d3e19e2f008550705e78e00 5a0d699640bd460530bd38148cf979e585b9a43c1bd545974561df18841fa5f4
EXPORTER_SECRET 82cab66e906c3cd3c58b3aeeecd66b2a12e521704d3e19e2f008550705e78e00 32b69cb41b8db36371e7d207a45e20d401bb05e0cd8bf492e3ace009e2845d12
SERVER_TRAFFIC_SECRET_0 82cab66e906c3cd3c58b3aeeecd66b2a12e521704d3e19e2f008550705e78e00 1f42b4392b2cc14789c4eaec4dae275c6a040ae3b11fc6bba58c90c7b80caa96
CLIENT_HANDSHAKE_TRAFFIC_SECRET 82cab66e906c3cd3c58b3aeeecd66b2a12e521704d3e19e2f008550705e78e00 bd93073bda56e559743a1f1ffc48c062089addcfc007c7defe08c28ac0ee6287
CLIENT_TRAFFIC_SECRET_0 82cab66e906c3cd3c58b3aeeecd66b2a12e521704d3e19e2f008550705e78e00 32b615c0dd25cb7b430a0cff44871e3263bd67af973e4b2f7fb19aab4df468d8
SERVER_HANDSHAKE_TRAFFIC_SECRET 68dcc859bc4edb51354a9f583e036d0b2787a337ee894e253925e273a5cd3889 a52a20827ce04dfc4ee557608ed5a0bfb6794ace0c4a1b69a1d56e5f16d8570b
EXPORTER_SECRET 68dcc859bc4edb51354a9f583e036d0b2787a337ee894e253925e273a5cd3889 8179afb8e7c7a77e35143c40a6bb62ccea2e644e48cc95b91b05f525bc59ada7
SERVER_TRAFFIC_SECRET_0 68dcc859bc4edb51354a9f583e036d0b2787a337ee894e253925e273a5cd3889 3d4abf6a9ea06395648a45428ca78c24962d8cc11440fe1d72f035ae35e61010
CLIENT_HANDSHAKE_TRAFFIC_SECRET 68dcc859bc4edb51354a9f583e036d0b2787a337ee894e253925e273a5cd3889 1d812a6c3c012a8fa4a6017ee573b47a5b361d15b861938ebca9194ecbc2a250
CLIENT_TRAFFIC_SECRET_0 68dcc859bc4edb51354a9f583e036d0b2787a337ee894e253925e273a5cd3889 6348a88dc9b6a350d72a7154140b824db80ba4f48c9e1fabcee76da8d248b041 
```

很好。下一步是捕捉流量。我们将使用带有[简单表达式](https://www.tcpdump.org/papers/ethereal-tcpdump.pdf)的 [tcpdump](https://www.tcpdump.org/) 来过滤捕获的流量:

```
[josevnz@dmaf5 temp]$ sudo tcpdump -i eno1 -v -v -v 'host asciinema.org' -w ~/temp/asciinema.org.pcap
dropped privs to tcpdump
tcpdump: listening on eno1, link-type EN10MB (Ethernet), snapshot length 262144 bytes 
```

在另一个窗口中运行 asciinema 客户端(我们将重复两次以获得更多数据):

```
[josevnz@dmaf5 SuricataLog]$ /usr/bin/asciinema upload demo-ascii.cast 
asciinema: upload failed: <urlopen error [Errno 32] Broken pipe>
asciinema: retry later by running: asciinema upload demo-ascii.cast
[josevnz@dmaf5 SuricataLog]$ 
[josevnz@dmaf5 SuricataLog]$ /usr/bin/asciinema upload demo-ascii.cast 
asciinema: upload failed: <urlopen error [Errno 104] Connection reset by peer>
asciinema: retry later by running: asciinema upload demo-ascii.cast 
```

现在终止另一个窗口中的 tcpdump 捕获:

```
tcpdump: listening on eno1, link-type EN10MB (Ethernet), snapshot length 262144 bytes
^C113 packets captured
118 packets received by filter
0 packets dropped by kernel 
```

让我们重放 pcap 文件，看看记录了什么:

```
[josevnz@dmaf5 temp]$ tcpdump -r ~/temp/asciinema.org.pcap
reading from file /home/josevnz/temp/asciinema.org.pcap, link-type EN10MB (Ethernet), snapshot length 262144
07:17:18.244941 IP dmaf5.home.59896 > cip-109-107-37-0.gb1.brightbox.com.https: Flags [S], seq 1651239781, win 64240, options [mss 1460,sackOK,TS val 3293505858 ecr 0,nop,wscale 7], length 0
07:17:18.337023 IP cip-109-107-37-0.gb1.brightbox.com.https > dmaf5.home.59896: Flags [S.], seq 2395275599, ack 1651239782, win 65160, options [mss 1460,sackOK,TS val 3934370169 ecr 3293505858,nop,wscale 7], length 0
07:17:18.337070 IP dmaf5.home.59896 > cip-109-107-37-0.gb1.brightbox.com.https: Flags [.], ack 1, win 502, options [nop,nop,TS val 3293505950 ecr 3934370169], length 0
07:17:18.337643 IP dmaf5.home.59896 > cip-109-107-37-0.gb1.brightbox.com.https: Flags [P.], seq 1:518, ack 1, win 502, options [nop,nop,TS val 3293505951 ecr 3934370169], length 517
07:17:18.429273 IP cip-109-107-37-0.gb1.brightbox.com.https > dmaf5.home.59896: Flags [.], ack 518, win 506, options [nop,nop,TS val 3934370263 ecr 3293505951], length 0
07:17:18.433850 IP cip-109-107-37-0.gb1.brightbox.com.https > dmaf5.home.59896: Flags [.], seq 1:1449, ack 518, win 506, options [nop,nop,TS val 3934370267 ecr 3293505951], length 1448
07:17:18.433863 IP dmaf5.home.59896 > cip-109-107-37-0.gb1.brightbox.com.https: Flags [.], ack 1449, win 501, options [nop,nop,TS val 3293506047 ecr 3934370267], length 0
07:17:18.433966 IP cip-109-107-37-0.gb1.brightbox.com.https > dmaf5.home.59896: Flags [P.], seq 1449:2897, ack 518, win 506, options [nop,nop,TS val 3934370267 ecr 3293505951], length 1448
07:17:18.433981 IP dmaf5.home.59896 > cip-109-107-37-0.gb1.brightbox.com.https: Flags [.], ack 2897, win 496, options [nop,nop,TS val 3293506047 ecr 3934370267], length 0
07:17:18.434089 IP cip-109-107-37-0.gb1.brightbox.com.https > dmaf5.home.59896: Flags [.], seq 2897:4345, ack 518, win 506, options [nop,nop,TS val 3934370267 ecr 3293505951], length 1448
...
07:17:30.612523 IP cip-109-107-37-0.gb1.brightbox.com.https > dmaf5.home.59898: Flags [.], ack 11148, win 501, options [nop,nop,TS val 3934382447 ecr 3293518134], length 0
07:17:30.612524 IP cip-109-107-37-0.gb1.brightbox.com.https > dmaf5.home.59898: Flags [.], ack 12596, win 501, options [nop,nop,TS val 3934382447 ecr 3293518134], length 0
07:17:30.612558 IP dmaf5.home.59898 > cip-109-107-37-0.gb1.brightbox.com.https: Flags [.], seq 35764:37212, ack 4724, win 499, options [nop,nop,TS val 3293518226 ecr 3934382447], length 1448
07:17:30.612563 IP dmaf5.home.59898 > cip-109-107-37-0.gb1.brightbox.com.https: Flags [P.], seq 37212:38660, ack 4724, win 499, options [nop,nop,TS val 3293518226 ecr 3934382447], length 1448
07:17:30.612637 IP dmaf5.home.59898 > cip-109-107-37-0.gb1.brightbox.com.https: Flags [.], seq 38660:40108, ack 4724, win 499, options [nop,nop,TS val 3293518226 ecr 3934382447], length 1448
07:17:30.612643 IP dmaf5.home.59898 > cip-109-107-37-0.gb1.brightbox.com.https: Flags [P.], seq 40108:41556, ack 4724, win 499, options [nop,nop,TS val 3293518226 ecr 3934382447], length 1448
07:17:30.613064 IP cip-109-107-37-0.gb1.brightbox.com.https > dmaf5.home.59898: Flags [P.], seq 4724:5080, ack 12596, win 501, options [nop,nop,TS val 3934382448 ecr 3293518134], length 356
07:17:30.613106 IP cip-109-107-37-0.gb1.brightbox.com.https > dmaf5.home.59898: Flags [P.], seq 5080:5104, ack 12596, win 501, options [nop,nop,TS val 3934382448 ecr 3293518134], length 24
07:17:30.614231 IP cip-109-107-37-0.gb1.brightbox.com.https > dmaf5.home.59898: Flags [R.], seq 5104, ack 12596, win 501, options [nop,nop,TS val 3934382448 ecr 3293518134], length 0 
```

是时候启动 wireshark 了。我喜欢使用图形用户界面，因为过滤功能很好，你可以更容易地浏览 PCAP 文件的内容。

![wireshark-open-pcap-file](img/e9027d92545bff728e546aaebea7a8df.png)

流量捕捉的内容:

![wireshark-traffic-dump](img/9b89f105e07cab7445413a45fd885402.png)

因此，我们在第一次收到 TLS hello 消息时，右键单击协议首选项->传输层安全性，然后单击“pre-Master-Secret log filename”:

![wireshark-tls-pre-master-key](img/d1c77744fe9a92bb78e79a995ae75e2f.png)

现在是有趣的时候了。如果你右击第一个 hello 消息并说“跟随 TLS 流”,一个新窗口将打开整个对话，直到我们重置连接，没有加密！

![wireshark-follow-tls](img/66a8a0418298ad0f11beec4d5a35d831.png)

所以在被 asciinema 服务器切断之前，我们只发送了 33 KB。多么粗鲁！:满意:

因为数据有效负载不是很大，我将在下面向您展示，请确保您注意以下几点:

1.  我更改了 Authorization: Basic 内容，因为我不想泄露我的 base64 编码的用户/密码。
2.  内容长度:12444474。asciinema 就是这样知道我们要上传的文件有多大，所以服务器拒绝了。
3.  Asciinema 使用 Nginx。
4.  你可以在最后看到关闭消息(实体太大)。

```
POST /api/asciicasts HTTP/1.1
Accept-Encoding: identity
Content-Length: 12444474
Host: asciinema.org
User-Agent: asciinema/2.0.2 CPython/3.9.9 Linux/5.14.18-100.fc33.x86_64-x86_64-with-glibc2.32
Accept: application/json
Content-Type: multipart/form-data; boundary=d5c6b2543ee94511943126c6a3c5d33a
Authorization: Basic XXXXX=
Connection: close

--d5c6b2543ee94511943126c6a3c5d33a
Content-Disposition: form-data; name="asciicast"; filename="ascii.cast"
Content-Type: application/octet-stream

{"version": 2, "width": 203, "height": 32, "timestamp": 1650568938, "env": {"SHELL": "/bin/bash", "TERM": "xterm-256color"}}
[0.191182, "o", "\u001b]777;notify;Command completed;eve_log.py --format table --timestamp '2022-02-23T18:22:24.405139+0000' test/eve.json\u001b\\\u001b]777;precmd\u001b\\\u001b]0;josevnz@dmaf5:~/SuricataLog-Logging-features-branch\u001b\\"]
[0.19215, "o", "\u001b]7;file://dmaf5/home/josevnz/SuricataLog-Logging-features-branch\u001b\\"]
[0.192399, "o", "[josevnz@dmaf5 SuricataLog-Logging-features-branch]$ "]
[1.000538, "o", "Let me show you how you can filter your Suricata alerts, displaying the results in different formats"]
[4.506902, "o", "\r\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C"]
[4.921813, "o", "\u001b[1@#"]
[5.170393, "o", "\u001b[1@ "]
[5.538486, "o", "\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C\u001b[C"]
[6.914337, "o", "\r\n"]
[6.918708, "o", "\u001b]777;notify;Command completed;# Let me show you how you can filter your Suricata alerts, displaying the results in different formats\u001b\\\u001b]777;precmd\u001b\\\u001b]0;josevnz@dmaf5:~/SuricataLog-Logging-features-branch\u001b\\"]
[6.920219, "o", "\u001b]7;file://dmaf5/home/josevnz/SuricataLog-Logging-features-branch\u001b\\"]
[6.920352, "o", "[josevnz@dmaf5 SuricataLog-Logging-features-branch]$ "]
[8.202111, "o", "1"]
[8.658197, "o", ")"]
[8.962176, "o", " "]
[10.153862, "o", "A"]
[10.409632, "o", " "]
[10.61679, "o", "n"]
[10.777002, "o", "i"]
[10.881112, "o", "c"]
[10.952884, "o", "e"]
[11.088641, "o", " "]
[11.201045, "o", "t"]
[11.466022, "o", "a"]
[11.553785, "o", "b"]
[11.818412, "o", "l"]
[11.961808, "o", "e"]
[13.51443, "o", "\r\n"]
[13.514675, "o", "bash: syntax error near unexpected token `)'\r\n"]
[13.518913, "o", "\u001b]777;notify;Command completed;1) A nice table\u001b\\\u001b]777;precmd\u001b\\\u001b]0;josevnz@dmaf5:~/SuricataLog-Logging-features-branch\u001b\\"]
[13.520551, "o", "\u001b]7;file://dmaf5/home/josevnz/SuricataLog-Logging-features-branch\u001b\\"]
[13.52072, "o", "[josevnz@dmaf5 SuricataLog-Logging-features-branch]$ "]
[22.176716, "o", "eve_log.py --format table --timestamp '2022-02-23T18:22:24.405139+0000' test/eve.jso"]
[24.202009, "o", "n"]
[26.097822, "o", "\r\n"]
[26.098024, "o", "\u001b]777;preexec\u001b\\"]
[26.312676, "o", "\u001b[?1049h\u001b[H\u001b[?1000h\u001b[?1003h\u001b[?1015h\u001b[?1006h\u001b[?25l\u001b[?1003h\r\n"]
[26.314059, "o", "\u001bP=1s\u001b\\\u001b[H\u001b[H                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                               "]
[26.314299, "o", "            \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                              "]
[26.314387, "o", "             \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                             "]
[26.314455, "o", "              \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                            "]
[26.314502, "o", "               \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                           "]
[26.31456, "o", "                \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                          "]
[26.314616, "o", "                 \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \u001bP=2s\u001b\\"]
[26.31467, "o", "\u001bP=1s\u001b\\\u001b[H\u001b[H                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                               "]
[26.314714, "o", "            \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                              "]
[26.314781, "o", "             \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                             "]
[26.314843, "o", "              \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                            "]
[26.314902, "o", "               \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                           "]
[26.314957, "o", "                \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                          "]
[26.315012, "o", "                 \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \u001bP=2s\u001b\\"]
[26.316033, "o", "\u001b[?25l"]
[26.318086, "o", "\r\u001b[2KParsing test/eve.json \u001b[38;5;237m........................................................................................................................\u001b[0m \u001b[35m  0%\u001b[0m \u001b[36m-:--:--\u001b[0m"]
[26.378123, "o", "\r\u001b[2KParsing test/eve.json \u001b[38;2;114;156;31m........................................................................................................................\u001b[0m \u001b[35m100%\u001b[0m \u001b[36m0:00:00\u001b[0m\r\n\u001b[?25h"]
[26.390312, "o", "\u001bP=1s\u001b\\\u001b[H\u001b[H                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                               "]
[26.39044, "o", "            \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                              "]
[26.390499, "o", "             \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                             "]
[26.390559, "o", "              \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                            "]
[26.390615, "o", "               \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                           "]
[26.39064, "o", "                \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                          "]
[26.390719, "o", "                 \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \u001bP=2s\u001b\\"]
[26.390868, "o", "\u001bP=1s\u001b\\\u001b[H\u001b[H                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                               "]
[26.390893, "o", "            \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                              "]
[26.390944, "o", "             \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                             "]
[26.391027, "o", "              \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                            "]
[26.391091, "o", "               \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                           "]
[26.391116, "o", "                \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                          "]
[26.391172, "o", "                 \r\n                                                                                                                                                                                                           \r\n                                                                                                                                                                                                           \u001bP=2s\u001b\\"]
[26.431391, "o", "\u001bP=1s\u001b\\\u001b[H\u001b[3m                                                                                      Suricata alerts for 2022-02-23 18:22:24.405139, logs=test/eve.json                                                   \u001b[0m\r\n.................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................\r\n...\u001b[1;35m \u001b[0m\u001b[1;35mTimestamp                      \u001b[0m\u001b[1;35m \u001b[0m...\u001b[1;35m \u001b[0m\u001b[1;35mSeverity\u001b[0m\u001b[1;35m \u001b[0m...\u001b[1;35m \u001b[0m\u001b[1;35mSignature                                           \u001b"]
[26.431543, "o", "[0m\u001b[1;35m \u001b[0m...\u001b[1;35m \u001b[0m\u001b[1;35mProtocol\u001b[0m\u001b[1;35m \u00HTTP/1.1 413 Request Entity Too Large
Content-Length: 176
Content-Type: text/html
Date: Fri, 22 Apr 2022 11:17:19 GMT
Server: nginx
Connection: close

<html>
<head><title>413 Request Entity Too Large</title></head>
<body>
<center><h1>413 Request Entity Too Large</h1></center>
<hr><center>nginx</center>
</body>
</html> 
```

# 你的下一步是什么？

因此，下次你遇到安装在你系统上的程序的问题时，你就知道该检查什么了。

我们介绍了三种方法来调查使用 HTTPS 将文件上传到远程网站的应用程序的问题:

1.  使用 *strace*
2.  如果程序是 Python 脚本，那么你很有可能自己阅读代码，并通过*调试器*运行脚本，一步一步地理解问题。这可能是最耗时的方式，但也是最有收获的方式，因为你可以了解其他优秀的开发人员是如何思考的！
3.  最后，我们捕获了我们和远程站点之间的加密流量，并分析了上传内容。通过启用某些特殊功能，我们能够解密和重放流量，从而证实了我们在之前两次交互中的发现。

这个技巧列表并不详尽，但是对于像这样的一些情况，它们会给你一个好的开始。

像往常一样，请分享您的反馈！让我们进行一次对话，这样每个人都能学到一点东西。, '', sys.argv[0])
    sys.exit(load_entry_point('asciinema==2.0.2', 'console_scripts', 'asciinema')()) 
```

主脚本是用[简单安装](https://setuptools.pypa.io/en/latest/deprecated/easy_install.html)生成的，这意味着`asciinema.py`只是有趣代码的包装。为了找出有趣的东西在哪里，让我们通过 Python [pdb 调试器](https://www.redhat.com/sysadmin/python-debugger-pdb)运行脚本:

[PRE7]

不完全是我们需要的。程序运行，遇到异常，然后从头开始重新启动。

我们来稍微作弊一下。asciinema 是用 RPM 安装的吗(我用的是 Fedora Linux)？

[PRE8]

我们试图上传一个文件，任何看起来像上传者的东西？

[PRE9]

啊，越来越有趣了！让我们打开“upload.py”:

[PRE10]

让我们在 UploadCommand 中设置几个断点(我的代码副本中的第 14、26 行):

[PRE11]

我们有一个错误。这种类型的异常有什么有趣的吗？

[PRE12]

所以没什么特别的，直接来源于`Exception`。此外，异常的参数只是错误消息。

当然，下一步是看看这个错误是否来自一个已知的库(在互联网上搜索错误)。

我是在 [GitHub](https://github.com/asciinema/asciinema/issues/335) 上发现这个问题的。进一步阅读你可以看到慷慨的 Asciicast 作者正在自掏腰包支付存储费用，因此我们所有人都可以免费享受在线存储:

> 最大大小设置为 2MB，这似乎太低了。我把它提高到 5MB。这并不多，但我是自掏腰包支付存储(S3)，所以我不能为每个用户提供千兆字节的存储。让我知道这对你是否有效。我不介意增加更多，但是现在我想在用户需求和主机成本之间找到一个好的中间点。

所以让我们确认这确实是原因:

[PRE13]

到目前为止，文件太大似乎是罪魁祸首。

我仍在运行调试器，我很想看看加载了哪些 asciinema 模块。为此，切换到“*交互*模式，并使用[列表理解](https://peps.python.org/pep-0202/)和[正则表达式](https://docs.python.org/3/howto/regex.html)获得列表:

[PRE14]

下面这些看起来可能包含一些线索:

*   asciinema.urllib_http_adapter
*   asci NEMA . commands . upload
*   asciinema.http _ adapter

退出调试器(或转到另一个终端)并搜索 urllib_http_adapter:

[PRE15]

如果您打开该文件，您会看到“post”方法是我们要进行故障诊断的方法:

[PRE16]

第 65 行中的一个断点将把我们带到我们需要的地方:

[PRE17]

非常有趣——我们完全可以使用以下字段在没有 Python 的情况下练习上传功能(从调试器中使用`args`获得):

*   URL = '[https://asciinema.org/api/asciicasts](https://asciinema.org/api/asciicasts)
*   headers = { ' User-Agent ':' asci NEMA/2 . 0 . 2 CPython/3 . 9 . 9 Linux/5 . 14 . 18-100 . fc33 . x86 _ 64-x86 _ 64-with-glibc 2.32 '，' Accept': 'application/json'}
*   用户名= 'XXXX '
*   密码= ' xxxx0f 1-1d 73-43fc-XX36-c 9d 7 zzzaaaa '

我们会得到什么异常？我们设置了另外两个断点，并让调试器运行，直到到达它们:

[PRE18]

错误类型为 [BrokenPipeError](https://docs.python.org/3/library/exceptions.html#BrokenPipeError) :

> ConnectionError 的子类，当试图在另一端已关闭的管道上写入，或试图在已关闭写入的套接字上写入时引发。对应于 errno EPIPE 和 ESHUTDOWN。

最后一件事——在将文件发送到网站之前，我们是否读取了内存中的整个文件？

[PRE19]

12MB，对今天的计算机内存来说不算大，但也不算小。

您还记得我们之前在调试器的帮助下捕获的参数(URL、用户等等)吗？我们现在知道了足够多的信息，可以使用不同的工具( [curl](https://curl.se/) )来尝试上传文件:

[PRE20]

误差`413 Request Entity Too Large` [表示](https://developer.mozilla.org/en-US/docs/web/http/status/413):

> HTTP 413 负载过大响应状态代码表示请求实体大于服务器定义的限制；服务器可能会关闭连接或返回“重试后”头字段。

所以 curl 比 Python 更能告诉我们文件被拒绝的原因。

在我们的连接被切断之前，我们传输了多少数据？让我们看看是否可以使用数据包嗅探器找出答案。

## **如何使用 Wireshark 和 SSLKEYLOGFILE 检查 HTTP 流量**

您可以使用网络嗅探器，如 [Wireshark](https://wireshark.org/) 或众所周知的 [tcpdump](https://www.tcpdump.org/) 来捕获您的机器和 asciinema 网站之间的流量。

当我们使用 HTTPS 时，流量将被加密，但使用许多程序支持的一个称为' [TLS 主加密秘密](https://www.paolotagliaferri.com/overview-of-transport-layer-security-protocol-tls-1-3/)'的功能，您可以解密会话。为此，让我们在客户端上启用[功能](https://everything.curl.dev/usingcurl/tls/sslkeylogfile):

[PRE21]

如果支持，将使用密钥填充$SSLKEYLOGFILE 文件:

[PRE22]

很好。下一步是捕捉流量。我们将使用带有[简单表达式](https://www.tcpdump.org/papers/ethereal-tcpdump.pdf)的 [tcpdump](https://www.tcpdump.org/) 来过滤捕获的流量:

[PRE23]

在另一个窗口中运行 asciinema 客户端(我们将重复两次以获得更多数据):

[PRE24]

现在终止另一个窗口中的 tcpdump 捕获:

[PRE25]

让我们重放 pcap 文件，看看记录了什么:

[PRE26]

是时候启动 wireshark 了。我喜欢使用图形用户界面，因为过滤功能很好，你可以更容易地浏览 PCAP 文件的内容。

![wireshark-open-pcap-file](img/e9027d92545bff728e546aaebea7a8df.png)

流量捕捉的内容:

![wireshark-traffic-dump](img/9b89f105e07cab7445413a45fd885402.png)

因此，我们在第一次收到 TLS hello 消息时，右键单击协议首选项->传输层安全性，然后单击“pre-Master-Secret log filename”:

![wireshark-tls-pre-master-key](img/d1c77744fe9a92bb78e79a995ae75e2f.png)

现在是有趣的时候了。如果你右击第一个 hello 消息并说“跟随 TLS 流”,一个新窗口将打开整个对话，直到我们重置连接，没有加密！

![wireshark-follow-tls](img/66a8a0418298ad0f11beec4d5a35d831.png)

所以在被 asciinema 服务器切断之前，我们只发送了 33 KB。多么粗鲁！:满意:

因为数据有效负载不是很大，我将在下面向您展示，请确保您注意以下几点:

1.  我更改了 Authorization: Basic 内容，因为我不想泄露我的 base64 编码的用户/密码。
2.  内容长度:12444474。asciinema 就是这样知道我们要上传的文件有多大，所以服务器拒绝了。
3.  Asciinema 使用 Nginx。
4.  你可以在最后看到关闭消息(实体太大)。

[PRE27]

# 你的下一步是什么？

因此，下次你遇到安装在你系统上的程序的问题时，你就知道该检查什么了。

我们介绍了三种方法来调查使用 HTTPS 将文件上传到远程网站的应用程序的问题:

1.  使用 *strace*
2.  如果程序是 Python 脚本，那么你很有可能自己阅读代码，并通过*调试器*运行脚本，一步一步地理解问题。这可能是最耗时的方式，但也是最有收获的方式，因为你可以了解其他优秀的开发人员是如何思考的！
3.  最后，我们捕获了我们和远程站点之间的加密流量，并分析了上传内容。通过启用某些特殊功能，我们能够解密和重放流量，从而证实了我们在之前两次交互中的发现。

这个技巧列表并不详尽，但是对于像这样的一些情况，它们会给你一个好的开始。

像往常一样，请分享您的反馈！让我们进行一次对话，这样每个人都能学到一点东西。