# 用这个有价值的工具改进您的蓝牙项目:第 2 部分！

> 原文：<https://www.freecodecamp.org/news/improve-your-bluetooth-project-with-this-valuable-tool-part-2/>

**这个帖子最初来自[www.jaredwolff.com。](https://www.jaredwolff.com/how-to-protocol-buffer-bluetooth-low-energy-service-part-2/)**

这是使用 Nordic NRF52 系列处理器配置您自己的蓝牙低能耗服务的第 2 部分。如果你还没看过[第一部](https://www.jaredwolff.com/how-to-define-your-own-bluetooth-low-energy-configuration-service-using-protobuf)回去看看吧。我会在这里等你..

如果你跟我到目前为止，击掌。？

让我们跳进来吧！

到目前为止，我们已经使用协议缓冲区创建了一个高效的跨平台数据结构。该协议缓冲器尤其可以用于通过蓝牙低能量服务发送这些定义的数据结构。在这一部分，我将向您展示从头创建服务的内部工作原理。

附:这个帖子很长。如果你想下载一些东西，点击这里获得一份格式精美的 PDF 文件。(额外奖励，这个系列的三个部分 PDF 都有！)

## 创建服务

![Door Peoeple](img/a0822fe4b0dad8e14acc37a08559bf30.png)

一般来说，处理蓝牙低能耗问题似乎有些力不从心。正如我在这里讨论的，有几个活动部件你需要记住。

创建新服务的最好方法是复制一个已经存在的服务！我通过以下方式做到了这一点:

1.  进入 sdk ->组件-> ble -> ble_services -> ble_bas
2.  将`ble_bas.h`复制到`include/ble`
3.  将`ble_bas.c`复制到`src/ble`

然后我将它们从`ble_bas`重命名为`ble_protobuf`以保持一致。我也在文件里做了同样的事情。(BAS 是一种电池电量服务，用于使用百分比报告电池电压或相对电量)

我还删除了所有电池测量功能，因为它们将被替换。过程的这一部分相当繁琐，容易出错。如果你不熟悉 Nordic SDK，[我强烈推荐你下载这篇文章的示例代码。](https://www.jaredwolff.com/files/protobuf/)

### 添加 UUID

通常，对于供应商定义的服务，您必须使用自己的 UUID。存在为蓝牙 SIG 保留的特定范围的 UUIDs。据推测，如果你是会员，你也可以预订自己的 UUID。这里有一篇关于堆栈溢出的帖子。

在我们的例子中，我已经定义了一个用于其他项目的 UUID。如果您转到`ble_protobuf.h`，您可以看到服务和特征的 UUIDs:

```
// UUID for the Service & Char
#define PROTOBUF_UUID_BASE   {0x72, 0x09, 0x1a, 0xb3, 0x5f, 0xff, 0x4d, 0xf6, \
                               0x80, 0x62, 0x45, 0x8d, 0x00, 0x00, 0x00, 0x00}
#define PROTOBUF_UUID_SERVICE     0xf510
#define PROTOBUF_UUID_CONFIG_CHAR (PROTOBUF_UUID_SERVICE + 1) 
```

两个`PROTOBUF_UUID_BASE` `PROTOBUF_UUID_SERVICE`都用在`ble_protobuf_init`中，最后一个用在`command_char_add`中(下面我会详细描述)。

您可以不定义基本 ID，但我强烈建议您多做一点。这样，您的应用程序将不受未来蓝牙协议更新的影响。

## 创造特色

![Open Those PICKLES!](img/f35ed08f438ffb1b75932c33f96145e9.png)

Nordic 有一种相当直接的方式来初始化每个服务中的独立特征。对于每个特性，都有一个`char_add`函数，该函数随后配置特性并将其添加到服务中。

### 用`ble_add_char_params_t`配置您的特性

Nordic 已经将 BLE 特征的配置参数放入一个逻辑(且有用的)结构中。如果你在不同的平台上开发，你可能会发现相同的设置，虽然都不在一个地方！(或者逻辑处理……？)

下面是该结构的分解:

```
typedef struct
{
    uint16_t                    uuid;
    uint8_t                     uuid_type;
    uint16_t                    max_len;
    uint16_t                    init_len;
    uint8_t *                   p_init_value;
    bool                        is_var_len;
    ble_gatt_char_props_t       char_props;
    ble_gatt_char_ext_props_t   char_ext_props;
    bool                        is_defered_read;
    bool                        is_defered_write;
    security_req_t              read_access;
    security_req_t              write_access;
    security_req_t              cccd_write_access;
    bool                        is_value_user;
    ble_add_char_user_desc_t    *p_user_descr;
    ble_gatts_char_pf_t         *p_presentation_format;
} ble_add_char_params_t; 
```

正是在这里，你告诉 BLE 堆栈，这是什么类型的特征。在这个例子中，我使用了一些参数来定义 Protobuf 命令的特征。

`uuid`定义如何访问特性的地址。如果你正在定义你自己的服务。
`max_len`是您可以通过特性发送的最大数据长度。这就是为什么在你的协议缓冲区`.options`文件中为所有可变长度参数设置`max_size`很重要。一旦你编译了你的协议缓冲区，你会得到一个`*_size`变量，很像`command.pb.h`中定义的那个。下面是它的一个片段:

```
/* Maximum encoded size of messages (where known) */
#define event_size                               67 
```

因此，当定义`ble_add_char_params_t`时，我将`max_len`设置为`event_size`:

```
add_char_params.uuid                      = PROTOBUF_UUID_CONFIG_CHAR;
add_char_params.max_len                   = event_size;
add_char_params.is_var_len                = true; 
```

同样，因为我们使用了一个*字符串*作为协议缓冲区中的一个参数，所以这个数据的大小是可变的。确保发送和接收正确数量的字节是很方便的。如果输入的数据比需要的多，协议缓冲区的解码功能将会失败。(我是吃了苦头才知道的！)

`char_props`定义特性的权限。如果您熟悉计算机上的文件系统权限，这将是您的第二天性。在这个例子中，**读**和**写**就是我们要找的。

最后，以`_access`结尾的参数决定了所使用的安全类型。在大多数情况下，`SEC_OPEN`或`SEC_JUST_WORKS`已经足够了。如果您正在处理关键数据(密码等)，您可能需要实施第二层加密或启用更高级别的安全模式。

如果你正在寻找更多关于蓝牙低能耗安全的信息，这里有一个关于这个主题的有用帖子。

### 给我那个查尔。

一旦定义了参数，就像调用`characteristic_add`函数一样简单。这将把这个新特性与相关的服务关联起来。第一个参数是服务句柄，第二个是配置参数，第三个是指向特征句柄的指针。(见下文)

```
uint32_t characteristic_add(uint16_t                   service_handle,
                            ble_add_char_params_t *    p_char_props,
                            ble_gatts_char_handles_t * p_char_handle) 
```

## 让它运行起来

在`ble_protobuf.c`内设置好一切是战斗的 90%。最后一英里需要在`main.c`的`services_init`中增加一些比特

```
 ble_protobuf_init_t       protobuf_init = {0};

    protobuf_init.evt_handler          = ble_protobuf_evt_hanlder;
    protobuf_init.bl_rd_sec            = SEC_JUST_WORKS;
    protobuf_init.bl_cccd_wr_sec       = SEC_JUST_WORKS;
    protobuf_init.bl_wr_sec            = SEC_JUST_WORKS;

    err_code = ble_protobuf_init(&m_protobuf,&protobuf_init);
    APP_ERROR_CHECK(err_code); 
```

以上允许事件被引导回主上下文。通过这种方式，你的应用程序与固件代码的核心逻辑变得更加互动。在 Nordic 的例子中，他们也把安全参数带了出来，这样它们也可以在主上下文中定义。

*附注:* `m_protobuf`是使用`ble_protobuf.h`中的宏定义的，它不仅创建了服务的静态实例，还定义了用于处理事件的回调。

```
/**@brief Macro for defining a ble_protobuf instance.
 *
 * @param   _name  Name of the instance.
 * @hideinitializer
 */
#define BLE_PROTOBUF_DEF(_name)                          \
    static ble_protobuf_t _name;                         \
    NRF_SDH_BLE_OBSERVER(_name ## _obs,             \
                         BLE_PROTOBUF_BLE_OBSERVER_PRIO, \
                         ble_protobuf_on_ble_evt,        \
                         &_name) 
```

如果您正在创建自己的服务，您必须更新事件处理程序的函数名。如果您需要调整优先级，您也可以定义/更新优先级。

## 写入此特征时会发生什么情况？

![Smoke Signal](img/d7f163cd049495a820019020a80e81b5.png)

`ble_protobuf_on_ble_evt`是蓝牙低能耗服务中处理事件的主要方式。我们最关心的是`BLE_GATTS_EVT_WRITE`事件，但是你可以触发任何你感兴趣的 GATT 事件。

是行动发生的地方。它获取写入特征的数据，并根据`event_fields`对其进行解码，然后方便地将其放入一个结构中进行额外处理，等等。如果解码出错，`pb_decode`会返回一个错误。修改后，数据被编码并可供读取。自从看了[第一部](https://www.jaredwolff.com/how-to-define-your-own-bluetooth-low-energy-configuration-service-using-protobuf)，对`pb_decode`和`pb_encode`的称呼应该看起来很熟悉！

当然，你可以让你的固件做任何你想做的事情。蓝牙能源世界是你的牡蛎。

## 最终注释

当向蓝牙低能耗示例添加新服务时，您可能需要对底层代码进行一些更改。

比如`sdk_config.h`可能需要一些改动。特别是`NRF_SDH_BLE_VS_UUID_COUNT`需要增加，这取决于有多少服务 UUIDs 可用。对于这个项目，我也使用 DFU 服务(因为它应该是所有连接项目的默认设置！！)

另一个重要方面是内存和闪存管理。BLE DFU 服务附带的默认文件可能不足以支持另一个 BLE 服务。你知道不够的唯一方法是当你编译并刷新到一个 NRF52 设备。如果设备启动时显示没有足够的内存，你就必须做出建议的改变。该错误将显示在调试控制台上，通常会显示以下消息:

```
<info> app: Setting vector table to bootloader: 0x00078000
<info> app: Setting vector table to main app: 0x00026000 
```

在这里的示例代码中了解更多关于如何设置调试控制台[的信息。](https://www.jaredwolff.com/files/protobuf/)

## 结论

在这一部分中，我向您展示了使用协议缓冲区的自定义蓝牙低能耗服务的内部工作原理。在最后一部分，我将向您展示如何加载固件、运行示例 javascript 应用程序以及测试我们新开发的协议缓冲区！