# 如何在欧洲 PSD2 下实施带条带的 3DS2 以实现 SCA 合规性

> 原文：<https://www.freecodecamp.org/news/implement-3ds2-for-your-saas-using-stripe-billing-and-be-sca-compliant-for-pds2/>

# **什么是 PSD2，SCA，3DS？**

## **PSD2**

第二支付服务指令(PSD2)是 2015 年公布的欧盟指令。PSD2 的目标是保护人们在线支付时的安全，促进[开放银行](https://en.wikipedia.org/wiki/Open_banking)，并使欧洲跨境支付服务更加安全。它于 2019 年 9 月生效。

## **SCA**

强客户认证(SCA)是 PSD2 的[要求，确保在线支付通过多因素认证进行，以提高在线支付的安全性。尽管 PSD2 是在 2019 年 9 月颁布的，但 SCA 已经推迟了 18 个月，以便商家和银行有更多的时间来实施解决方案。](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=uriserv:OJ.L_.2018.069.01.0023.01.ENG&toc=OJ:L:2018:069:TOC)

## **3DS2**

3d Secure 2.0(3DS 2)是 3DS 的第二次迭代，用于驱动品牌系统，如 [Visa Secure](https://usa.visa.com/visa-everywhere/security.html) 、[万事达卡身份检查](https://www.mastercard.us/en-us/merchants/safety-security/identity-check.html)和[美国运通安全钥匙](https://network.americanexpress.com/globalnetwork/safekey/us/en/)。它旨在减少欺诈，为在线支付提供额外的安全性，并得到许多主要银行的支持。

3DS2 被认为是符合 SCA 的解决方案。如果您的企业实施了 3DS2，您将不再有被银行拒绝收费的危险。

# **SCA 对你在 SaaS 的业务有影响吗？**

![image-21](img/fe9fb9d3cb20e55bc835e02a5191ac62.png)

在以下两种情况下，SCA 被视为对所有电子商务支付有效:

*   这项业务在欧盟
*   客户的银行在欧盟

如果 SCA 适用于您，而您没有对客户的交易进行身份验证，您将面临银行拒绝收费的风险。

PSD2 第[条第 12-18 条规定了几种交易的豁免。作为一家 SaaS 公司，需要注意的最关键的例外是**第 13 条。**本文指出重复性交易不需要遵守 SCA。这意味着您只需要一个 SCA 实现来处理订阅的初始创建，而不是后续的经常性费用。](https://eba.europa.eu/documents/10180/1761863/Final+draft+RTS+on+SCA+and+CSC+under+PSD2+%28EBA-RTS-2017-02%29.pdf)

如果您有兴趣阅读其他豁免的分类以及它们如何适用于您， [Stripe 将在此](https://stripe.com/guides/strong-customer-authentication#exemptions-to-strong-customer-authentication)对每个豁免进行深入探讨。

# 即使您不在欧洲，您是否也应该做好 SCA 准备？

即使您没有受到 PSD2 或 SCA 的影响，实施 3DS2 之类的解决方案也有好处。通过实施 3DS2，您将以更加安全的方式处理客户信息，并将责任从您转移到发卡方，从而降低拒付风险。

# **如何实现 SCA 合规性？**

作为一个 SaaS，遵从 SCA 意味着所有在线支付都使用三个元素中的两个进行授权，

![image-19](img/c17621bb04675d048bfa865899d4987b.png)

正如我之前提到的，3DS2 是一个符合 SCA 的解决方案。诸如 [Servicebot](https://servicebot.io) 、PayPal 和 [Stripe Checkout](https://stripe.com/payments/checkout) 等嵌入式解决方案已经使用 3DS2，因此符合 SCA。如果您正在使用一个定制的解决方案，使用 Stripe Billing 或 Braintree 之类的东西来管理您的订阅，您将需要开发一个 3DS2 实现。

# **如何使用条带计费实现 3DS2？**

![image-22](img/25a618855fbf655a814e4d723223d10e.png)

Stripe 创建了两个新的对象，作为提供 SCA 兼容解决方案的一部分，PaymentIntent 和 SetupIntent，以便于使用 3DS2。PaymentIntent 表示向某人收费的意图，用作支付认证流程的一部分。SetupIntents 类似于 PaymentIntents，但它们表示最终从某人的卡上收费的意图。如果你的 SaaS 有免费试用版，或者提供一个免费等级，你就会使用 SetupIntents，基本上以后任何地方的信用卡都要收费。

## **使用付费内容**

如果您使用 Stripe Billing 创建订阅，则默认情况下您已经在使用 PaymentIntents。它们被创建并附加到每个新订阅的每张发票上。如果您想知道一个新的订阅是否需要 SCA，您可以检查订阅的`latest_invoice`上的`payment_intent`的状态。该对象将包含一个`requires_action`的`status`——运行下面的 NodeJS 代码来查看它的运行情况。

## 这段代码创建了一个需要 SCA 的订阅

```
const STRIPE_TEST_SECRET_KEY = "rk_test_3U9s3aPLquPOczvc4FVRQKdo00AhMZlMIE";
let stripe = require("stripe")(STRIPE_TEST_SECRET_KEY);
const sub = await stripe.subscriptions.create({ //creates a SCA-required subscription
    items: [{plan : "plan_FvnU01xoIPrg9l"}], //$300 per month plan without free trial
    customer: "cus_G0juGVZSLskx57",
    default_payment_method: "pm_1FUiR8CISNxwKLmI8uIQDdnv", //This PaymentMethod always requires SCA
    expand: ["latest_invoice.payment_intent"] //we expand the payload to show up the payment intent
});
const paymentIntent = sub.latest_invoice.payment_intent;
console.log(`Subscription Status: ${sub.status}`);
console.log(`PaymentIntent Status: ${paymentIntent.status}`)
console.log(paymentIntent.status === "requires_action" ? "SCA Required" : "No SCA Required");
console.log(sub);

```

一旦您知道您的订阅需要身份验证，您可以在浏览器上使用 PaymentIntent 的 client_secret 来启动使用 Stripe.js 的 3DS2 身份验证过程

## **使用 Stripe.js 处理带有 PaymentIntent 的 card payment**

Stripe.js 有一个方便的函数叫做 [handleCardPayment](https://stripe.com/docs/stripe-js/reference#stripe-handle-card-payment) ，它从一个支付意图中获取一个客户端秘密，并启动 3DS2 进程来验证支付。

```
await stripe.handleCardPayment('PAYMENTINTENT_SECRET');
```

您可以在这里看到这一点

[https://codepen.io/bsears/embed/preview/PooGOLg?height=300&slug-hash=PooGOLg&default-tabs=js,result&host=https://codepen.io](https://codepen.io/bsears/embed/preview/PooGOLg?height=300&slug-hash=PooGOLg&default-tabs=js,result&host=https://codepen.io)

一旦客户通过身份验证，订阅将从`incomplete`状态转移到`active`状态，客户将被成功计费。

## **SetupIntents**

作为一家 SaaS 企业，如果你要么使用免费层，要么提供免费试用，你将主要与 SetupIntents 进行交互。当有人输入信用卡时，对于这些订阅中的一个，您将在[订阅对象](https://stripe.com/docs/api/subscriptions/object#subscription_object-pending_setup_intent)上看到一个`pending_setup_intent`。SetupIntent 的`client_secret`应该被传递到前端，以便 Stripe.js 可以启动 3DS2 认证流。

## **将 Stripe.js handleCardSetup 与 SetupIntent 一起使用**

这与我们处理 PaymentIntent 的方式基本相同，只是我们调用了 handleCardSetup

```
await stripe.handleCardSetup('{SETUP_INTENT_CLIENT_SECRET}')
```

您可以在下面看到一个正在运行的 SetupIntent SCA 流。

[https://codepen.io/bsears/embed/preview/RwwGyYw?height=300&slug-hash=RwwGyYw&default-tabs=js,result&host=https://codepen.io](https://codepen.io/bsears/embed/preview/RwwGyYw?height=300&slug-hash=RwwGyYw&default-tabs=js,result&host=https://codepen.io)

身份验证完成后，客户可以在以后转到付费计划，或者在免费试用结束后从他们的卡上扣费。

# **无代码替代方案**

如果您正在寻找一个 SCA 兼容的条带计费解决方案，而不必处理 3DS2 集成开发，请查看 [Servicebot](https://servicebot.io) 。我们为使用 Stripe 的 SaaS 公司提供了一个现成的符合 SCA 的嵌入式 UI！想看看实际情况吗？检查[这个演示](https://dashboard.servicebot.io/examples/signup-embed/0)并使用测试卡`4000002760003184`(任何到期和 CVC)。

[![QJkpyHN](img/b4dee6c62cd9b68078446db1d3f82bf1.png)](https://servicebot.io)