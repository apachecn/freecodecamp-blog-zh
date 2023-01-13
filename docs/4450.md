# 如何使用简单的 Ruby 脚本将数据库表导出为 CSV 格式

> 原文：<https://www.freecodecamp.org/news/export-a-database-table-to-csv-using-a-simple-ruby-script-2/>

如果您有一个 Rails 项目，并且想将一个表格导出为 CSV 格式，而不必大费周章地找到一个 gem，安装并使用它，然后在不再需要时卸载它，我有一些好消息。这里有一个简单快捷的方法可以将数据库中的特定表格导出为 CSV 文件。

这是您需要运行的[代码](https://gist.github.com/foxumon/fdb30349545944eee58c7858e6bab23c)。您可以将它作为一个 rake 任务来运行，或者以另一种方式运行。