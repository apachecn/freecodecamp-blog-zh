# 《我的世界》·福吉:如何下载、安装和使用福吉

> 原文：<https://www.freecodecamp.org/news/minecraft-forge-how-to-download-install-and-use-forge/>

如果你正在阅读这篇文章，你可能已经知道《我的世界》。我们使用伪造操纵游戏《我的世界》，使它做我们想要的。这可以是任何东西，从新的酷生物到游戏中全新的系统。

Forge 是一个修改 API。《我的世界》·福吉(或简称福吉)是我们的代码和《我的世界》本身之间的一层。

我们不能直接要求《我的世界》添加项目和做特别酷的事情。这就是为什么我们需要一个 API(应用编程接口)来处理我们的逻辑，并让《我的世界》识别它。

## 听起来很酷！我如何开始？

*   你将需要 JDK (Java 开发工具包),它是一套库、工具和运行环境来制作和运行 Java 程序。
*   一个可以从他们的官方网站购买的《我的世界》账户。([https://minecraft.net/en-us/store/](https://minecraft.net/en-us/store/)
*   IDE(推荐使用 Eclipse 或 IntelliJ 进行《我的世界》开发)

安装/获得这些软件后，在[https://files.minecraftforge.net/](https://files.minecraftforge.net/)下载你想要的 Forge 版本。

**提示**:悬停在信息按钮上并按下直接下载，以避免 Adfly 病毒！

一旦你下载了这个压缩文件，你就可以解压了。这样做，并将所有 Forge 文件放入 cd (cmd/command)目录中。运行`gradlew setupDecompWorkspace`。

接下来是挑选你的 IDE(集成开发环境)。

*   月食？`gradlew eclipse`。
*   IntelliJ？在 IntelliJ 安装程序中导入 build.gradle 文件。

## 好了，现在怎么办？我如何添加花哨的新项目？(基本模式设置)

沉住气。事情远不止如此。你将不得不纹理项目，当然，添加代码等等！在这篇文章中，我们将只看一些简单的样本代码，我也使用我自己的 mods。在这里！

` @Mod。eventbus subscriber @ Mod(modid = Version。MOD *ID，name = Version。MOD* 名称，版本=版本。版本)公共类模式{

```
public static ModMetadata metadata;

public static File baseDir;
public static Configuration config;

@SidedProxy(clientSide="com.ciphry.client.ClientProxy", serverSide="com.ciphry.common.CommonProxy")
public static CommonProxy proxy;

@Mod.EventHandler
public void preInit(FMLPreInitializationEvent event) {
    proxy.preInit(event);

    baseDir = new File(event.getModConfigurationDirectory(), MOD_ID);
    config = new Configuration(event.getSuggestedConfigurationFile());

    if (!baseDir.exists())
        baseDir.mkdir();
}

@Mod.EventHandler
public void init(FMLInitializationEvent event) {
    proxy.init(event);

}

@Mod.EventHandler
public void postInit(FMLPostInitializationEvent event) {
    proxy.postInit(event);
}
```

请随意使用这个代码。只要确保你编辑，例如，代理字符串和更多。这应该给你一个基本的 mod 类的基本概述。