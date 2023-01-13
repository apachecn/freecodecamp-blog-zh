# 如何使用 Gource 显示你的项目时间表

> 原文：<https://www.freecodecamp.org/news/using-gource-to-show-your-project-timeline/>

第一次听说 [Gource](https://github.com/acaudwell/Gource) 是在 2013 年的[。当时我看了这个展示 Ruby on Rails 源代码演变的很酷的视频:](https://leonardofaria.net/2013/01/20/gource-uma-forma-estilosa-de-ver-logs-do-seu-controle-de-versao/)

[https://www.youtube.com/embed/r0ji8FDNTj0?feature=oembed](https://www.youtube.com/embed/r0ji8FDNTj0?feature=oembed)

在[thinkfic](https://www.thinkific.com/)[，](https://www.thinkific.com/careers/)我们(几乎)每月都有产品市政厅，我们用它来传达产品决策，并让所有产品团队(设计师、工程师、产品经理和 QA)保持一致。

我们喜欢以一些我们项目的美食视频开始主题演讲。我们是这样做的:

`gource --hide dirnames,filenames --seconds-per-day 0.1 --auto-skip-seconds 1 -1280x720 -o - | ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i - -vcodec libx264 -preset ultrafast -pix_fmt yuv420p -crf 1 -threads 0 -bf 0 gource.mp4`

Gource 的 [README](https://github.com/acaudwell/Gource/blob/master/README) 拥有你可以在视频中定制的所有选项。依赖于 [ffmpeg](https://www.ffmpeg.org/) ，我们创建了一个 mp4 文件，可以在谷歌幻灯片或 YouTube 中使用。下面是上面示例的输出:

[https://www.youtube.com/embed/hYvWaA5cCJg?feature=oembed](https://www.youtube.com/embed/hYvWaA5cCJg?feature=oembed)

*也发表在[我的博客](http://bit.ly/2FWop4N)上。如果你喜欢这些内容，请在 [Twitter](https://twitter.com/leozera) 和 [GitHub](https://github.com/leonardofaria) 上关注我。*

顺便说一下，Thinkific 正在为几个职位 [招聘](https://bit.ly/thnk-senior-full-stack-eng)[。](https://bit.ly/thnk-senior-front-end-eng)