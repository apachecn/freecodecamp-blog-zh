# 如何用 Rust 实现朴素贝叶斯分类器

> 原文：<https://www.freecodecamp.org/news/implement-naive-bayes-with-rust/>

我想提高我的锈技能，也帮助你磨练你的技能。所以我决定写一系列关于 Rust 编程语言的文章。

通过用铁锈制造东西，我们将在这个过程中了解到广泛的技术概念。在这一期中，我们将学习如何用 Rust 实现朴素贝叶斯分类器。

在本文中，您可能会遇到一些不熟悉的术语或概念。不要气馁。如果你有时间的话，看看这些，但是不管怎样，这篇文章的主要思想不会让你迷失。

## 什么是朴素贝叶斯分类器？

朴素贝叶斯分类器是一种基于贝叶斯定理的机器学习算法。[贝叶斯定理](https://greenteapress.com/wp/think-bayes/)给了我们一种在给定一些数据的情况下更新假设(H)的概率的方法。

用数学方法表示，我们有:

\[P(H | D)= \ frac { P(D | H)P(H)} { P(D)} \]

其中\(P(H|D) =\)给定\(D\)的概率。

如果我们积累更多的数据，我们可以相应地更新\(P(H|D)\)。

朴素贝叶斯模型基于一个大的假设:一个数据点是否存在于数据集中与该数据集中已经存在的数据无关。也就是说，每一段数据不传达任何其他数据点的信息。

我们不认为这种假设是正确的，因为它是站不住脚的。但它仍然是有用的，允许我们创建工作得相当好的高效分类器( [source](https://probml.github.io/pml-book/book0.html) )。

我们将把对朴素贝叶斯的描述留在这里。还有更多的可以说，但这篇文章的要点是实践生锈。

如果您想了解有关该算法的更多信息，这里有一些资源:

*   对于 Josh Starmer 的这段视频，我说不出多少好话。
*   Joel Grus 在他的巨著*从头开始的数据科学*中写了关于朴素贝叶斯的一章，这是这个实现的主要灵感。
*   如果数学符号是你的事，[试试*统计学习的要素*](https://hastie.su.domains/Papers/ESLII.pdf) 的 6.6.3 节。
*   这里有一篇关于算法工作原理的有用文章。

朴素贝叶斯分类器的典型应用是垃圾邮件分类器。这就是我们要建造的。你可以在这里找到所有的代码:[https://github.com/josht-jpg/shaking-off-the-rust](https://github.com/josht-jpg/shaking-off-the-rust)

我们将从创建一个新的货物库开始。

```
cargo new naive_bayes --lib
cd naive_bayes 
```

现在让我们深入研究一下。

## 铁锈的符号化

我们的分类器将接收一条消息作为输入，并返回垃圾邮件或非垃圾邮件的分类。

为了处理给定的信息，我们需要*对其进行*标记。我们的标记化表示将是一组小写单词，其中顺序和重复条目被忽略。拉斯特的 **`[std::collections::HashSet](https://doc.rust-lang.org/std/collections/struct.HashSet.html)`** 结构是实现这一点的绝佳方式。

我们将要编写的函数将要求使用 [regex](https://docs.rs/regex/latest/regex/) crate。确保在您的`Cargo.toml`文件中包含此依赖关系:

```
[dependencies]
regex = "^1.5.4" 
```

这里是`tokenize`函数:

```
// lib.rs

// We'll need HashMap later
use std::collections::{HashMap, HashSet};

extern crate regex;
use regex::Regex;

pub fn tokenize(lower_case_text: &str) -> HashSet<&str> {
    Regex::new(r"[a-z0-9']+")
        .unwrap()
        .find_iter(lower_case_text)
        .map(|mat| mat.as_str())
        .collect()
}
```

该函数使用正则表达式匹配所有数字和小写字母。每当我们遇到不同类型的符号(通常是空格或标点符号)时，我们会拆分输入，并将自上次拆分以来遇到的所有数字和字母组合在一起(您可以[在 Rust】中阅读有关 regex 的更多信息)。也就是说，我们在识别和隔离输入文本中的单词。](https://rust-lang-nursery.github.io/rust-cookbook/text/regex.html)

### 一些便利的结构

使用一个`struct`来表示一个消息将会很有帮助。这个`struct`将包含一个用于消息文本的*字符串片段*，以及一个布尔值来指示该消息是否是垃圾邮件:

```
pub struct Message<'a> {
    pub text: &'a str,
    pub is_spam: bool,
}
```

`'a`是一个寿命参数注释。如果你对生命周期不熟悉，并且想了解它们，我推荐阅读 Rust 编程语言书的[第 10.3 节。](https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html)

一个`struct`也可以用来表示我们的分类器。在创建`struct`之前，我们需要稍微偏离一下拉普拉斯平滑。

### 什么是拉普拉斯平滑？

假设——在我们的训练数据中——单词 *fubar* 出现在一些非垃圾消息中，但没有出现在任何垃圾消息中。然后，朴素贝叶斯分类器将为任何包含单词 *fubar* ( [source](https://www.youtube.com/watch?v=nt63k3bfXS0) )的邮件分配一个垃圾邮件概率 **0** 。

除非我们谈论的是我在网上约会的成功，否则仅仅因为一件事还没有发生就给它分配一个 **0** 的概率是不明智的。

输入拉普拉斯平滑。这是将\(\alpha\)加到每个令牌的观察次数上的技术([源](https://www.youtube.com/watch?v=nt63k3bfXS0))。让我们从数学上来看:如果没有拉普拉斯平滑，在垃圾邮件中看到单词\(w\)的概率是:

\[P(w | S)= \ frac { number \ of \ spam \ messages \ containing \ w } { total \ number \ of \ spam } \]

使用拉普拉斯平滑，它是:

\[P(w | S)= \ frac {(a+number \ of \ spam \ messages \ containing \ w)} {(2a+total \ number \ of \ spam)} \]

回到我们的分类器`struct`:

```
pub struct NaiveBayesClassifier {
    pub alpha: f64,
    pub tokens: HashSet<String>,
    pub token_ham_counts: HashMap<String, i32>,
    pub token_spam_counts: HashMap<String, i32>,
    pub spam_messages_count: i32,
    pub ham_messages_count: i32,
} 
```

`NaiveBayesClassifier`的实现模块将围绕一个`train`方法和一个`predict`方法。

## 如何训练我们的分类器

`train`方法将接收`Message`的一部分，并遍历每个`Message`，执行以下操作:

*   检查该消息是否是垃圾邮件，并相应地更新`spam_messages_count`或`ham_messages_count`。我们将为此创建助手函数`increment_message_classifications_count`。
*   用我们的`tokenize`函数标记消息的内容。
*   遍历消息中的每个令牌，并:
*   将令牌插入`tokens`到`HashSet`，然后更新`token_spam_counts`或`token_ham_counts`。我们将为此创建助手函数`increment_token_count`。

这里是我们的`train`方法的伪代码。如果你喜欢，在看我下面的实现之前，试着把伪代码转换成 Rust。不要犹豫，请将您的实现发送给我，我很乐意看到它！

```
implementation block for NaiveBayesClassifier {

	train(self, messages) {
		for each message in messages {
			self.increment_message_classifications_count(message)

			lowercase_text = to_lowercase(message.text)
			for each token in tokenize(lowercase_text) {
				self.tokens.insert(tokens)
				self.increment_token_count(token, message.is_spam)
			}			
		}
	}

	increment_message_classifications_count(self, message) {
		if message.is_spam {
			self.spam_messages_count = self.spam_messages_count + 1
		} else {
			self.ham_messages_count = self.ham_messages_count + 1
		}
	}

	increment_token_count(&mut self, token, is_spam) {
		if token is not a key of self.token_spam_counts {
			insert record with key=token and value=0 into self.token_spam_counts
		}

		if token is not a key of self.token_ham_counts {
			insert record with key=token and value=0 into self.token_ham_counts
		}

		if is_spam {
			self.token_spam_counts[token] = self.token_spam_counts[token] + 1
		} else {
			self.token_ham_counts[token] = self.token_ham_counts[token] + 1
		}
	}

} 
```

这是 Rust 的实现:

```
impl NaiveBayesClassifier {
    pub fn train(&mut self, messages: &[Message]) {
        for message in messages.iter() {
            self.increment_message_classifications_count(message);
            for token in tokenize(&message.text.to_lowercase()) {
                self.tokens.insert(token.to_string());
                self.increment_token_count(token, message.is_spam)
            }
        }
    }

    fn increment_message_classifications_count(&mut self, message: &Message) {
        if message.is_spam {
            self.spam_messages_count += 1;
        } else {
            self.ham_messages_count += 1;
        }
    }

    fn increment_token_count(&mut self, token: &str, is_spam: bool) {
        if !self.token_spam_counts.contains_key(token) {
            self.token_spam_counts.insert(token.to_string(), 0);
        }

        if !self.token_ham_counts.contains_key(token) {
            self.token_ham_counts.insert(token.to_string(), 0);
        }

        if is_spam {
            self.increment_spam_count(token);
        } else {
            self.increment_ham_count(token);
        }
    }

    fn increment_spam_count(&mut self, token: &str) {
        *self.token_spam_counts.get_mut(token).unwrap() += 1;
    }

    fn increment_ham_count(&mut self, token: &str) {
        *self.token_ham_counts.get_mut(token).unwrap() += 1;
    }
} 
```

注意在`HashMap`中增加一个值是非常麻烦的。Rust 程序员新手很难理解

`*self.token_spam_counts.get_mut(token).unwrap() += 1`

正在做。

为了使代码更加清晰，我创建了`increment_spam_count`和`increment_ham_count`函数。但我对此并不满意——感觉还是很麻烦。如果您有更好的方法建议，请联系我。

## 如何用我们的分类器进行预测

`predict`方法将获取一个*字符串片段*，并返回模型计算出的垃圾邮件概率。

我们将创建两个助手函数`probabilities_of_message`和`probabilites_of_token`来为`predict`完成繁重的工作。

`probabilities_of_message`返回\(P(Message|Spam)\)和\(P(Message|ham)\)。

`probabilities_of_token`返回\(P(Token|Spam)\)和\(P(Token|ham)\)。

计算输入消息是垃圾消息的概率包括将每个单词在垃圾消息中出现的概率相乘。

由于概率是介于 0 和 1 之间的浮点数，将许多概率相乘会导致**下溢** ( [源](https://learning.oreilly.com/library/view/data-science-from/9781492041122/))。这是当一个操作产生的数字小于计算机能够准确存储的数字时(这里的[见](https://www.techopedia.com/definition/712/underflow)和[见](https://www.amazon.ca/Numerical-Analysis-Richard-Burden/dp/1305253663/ref=asc_df_1305253663/?tag=googleshopc0c-20&linkCode=df0&hvadid=293014842916&hvpos=&hvnetw=g&hvrand=9862733826869340686&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9001551&hvtargid=pla-450666638521&psc=1))。因此，我们将使用对数和指数将任务转化为一系列加法:

\[\prod_{i=0}^{n}p_i = exp(\sum_{i=0}^{n}日志(p_i)) \]

这是因为对于任何实数\(a\)和\(b\ ),

\[ab = exp(log(ab))= exp(log(a)+log(b))\]

我将再次从 predict 方法的伪代码开始:

```
implementation block for NaiveBayesCalssifier {
	/*...*/

	predict(self, text) {
		lower_case_text = to_lowercase(text)
		message_tokens = tokenize(text)
		(prob_if_spam, prob_if_ham) = self.probabilities_of_message(message_tokens)
		return prob_if_spam / (prob_if_spam + prob_if_ham)
	}

	probabilities_of_message(self, message_tokens) {
		log_prob_if_spam = 0
		log_prob_if_ham = 0

		for each token in self.tokens {
			(prob_if_spam, prob_if_ham) = self.probabilites_of_token(token)

			if message_tokens contains token {
				log_prob_if_spam = log_prob_if_spam + ln(prob_if_spam)
				log_prob_if_ham = log_prob_if_ham + ln(prob_if_ham)
			} else {
				log_prob_if_spam = log_prob_if_spam + ln(1 - prob_if_spam)
				log_prob_if_ham = log_prob_if_ham + ln(1 - prob_if_ham)
			}
		}

		prob_if_spam = exp(log_prob_if_spam)
		prob_if_ham = exp(log_prob_if_ham)

		return (prob_if_spam, prob_if_ham)
	}

	probabilites_of_token(self, token) {
		prob_of_token_spam = (self.token_spam_counts[token] + self.alpha) 
						/ (self.spam_messages_count + 2 * self.alpha)

		prob_of_token_ham = (self.token_ham_counts[token] + self.alpha) 
						/ (self.ham_messages_count + 2 * self.alpha)

		return (prob_of_token_spam, prob_of_token_ham)
	}

} 
```

这是 Rust 代码:

```
impl NaiveBayesClassifier {

		/*...*/

	pub fn predict(&self, text: &str) -> f64 {
        let lower_case_text = text.to_lowercase();
        let message_tokens = tokenize(&lower_case_text);
        let (prob_if_spam, prob_if_ham) = self.probabilities_of_message(message_tokens);

        return prob_if_spam / (prob_if_spam + prob_if_ham);
    }

    fn probabilities_of_message(&self, message_tokens: HashSet<&str>) -> (f64, f64) {
        let mut log_prob_if_spam = 0.;
        let mut log_prob_if_ham = 0.;

        for token in self.tokens.iter() {
            let (prob_if_spam, prob_if_ham) = self.probabilites_of_token(&token);

            if message_tokens.contains(token.as_str()) {
                log_prob_if_spam += prob_if_spam.ln();
                log_prob_if_ham += prob_if_ham.ln();
            } else {
                log_prob_if_spam += (1\. - prob_if_spam).ln();
                log_prob_if_ham += (1\. - prob_if_ham).ln();
            }
        }

        let prob_if_spam = log_prob_if_spam.exp();
        let prob_if_ham = log_prob_if_ham.exp();

        return (prob_if_spam, prob_if_ham);
    }

    fn probabilites_of_token(&self, token: &str) -> (f64, f64) {
        let prob_of_token_spam = (self.token_spam_counts[token] as f64 + self.alpha)
            / (self.spam_messages_count as f64 + 2\. * self.alpha);

        let prob_of_token_ham = (self.token_ham_counts[token] as f64 + self.alpha)
            / (self.ham_messages_count as f64 + 2\. * self.alpha);

        return (prob_of_token_spam, prob_of_token_ham);
    }
} 
```

## 如何测试我们的分类器

让我们测试一下我们的模型。下面的测试手动通过朴素贝叶斯，然后检查我们的模型给出相同的结果。

您可能会发现浏览测试的逻辑是值得的，或者您可能只想将代码粘贴到 lib.rs 文件的底部，以检查您的代码是否工作。

```
// ...lib.rs

pub fn new_classifier(alpha: f64) -> NaiveBayesClassifier {
    return NaiveBayesClassifier {
        alpha,
        tokens: HashSet::new(),
        token_ham_counts: HashMap::new(),
        token_spam_counts: HashMap::new(),
        spam_messages_count: 0,
        ham_messages_count: 0,
    };
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn naive_bayes() {
        let train_messages = [
            Message {
                text: "Free Bitcoin viagra XXX christmas deals 😻😻😻",
                is_spam: true,
            },
            Message {
                text: "My dear Granddaughter, please explain Bitcoin over Christmas dinner",
                is_spam: false,
            },
            Message {
                text: "Here in my garage...",
                is_spam: true,
            },
        ];

        let alpha = 1.;
        let num_spam_messages = 2.;
        let num_ham_messages = 1.;

        let mut model = new_classifier(alpha);
        model.train(&train_messages);

        let mut expected_tokens: HashSet<String> = HashSet::new();
        for message in train_messages.iter() {
            for token in tokenize(&message.text.to_lowercase()) {
                expected_tokens.insert(token.to_string());
            }
        }

        let input_text = "Bitcoin crypto academy Christmas deals";

        let probs_if_spam = [
            1\. - (1\. + alpha) / (num_spam_messages + 2\. * alpha), // "Free"  (not present)
            (1\. + alpha) / (num_spam_messages + 2\. * alpha),      // "Bitcoin"  (present)
            1\. - (1\. + alpha) / (num_spam_messages + 2\. * alpha), // "viagra"  (not present)
            1\. - (1\. + alpha) / (num_spam_messages + 2\. * alpha), // "XXX"  (not present)
            (1\. + alpha) / (num_spam_messages + 2\. * alpha),      // "christmas"  (present)
            (1\. + alpha) / (num_spam_messages + 2\. * alpha),      // "deals"  (present)
            1\. - (1\. + alpha) / (num_spam_messages + 2\. * alpha), // "my"  (not present)
            1\. - (0\. + alpha) / (num_spam_messages + 2\. * alpha), // "dear"  (not present)
            1\. - (0\. + alpha) / (num_spam_messages + 2\. * alpha), // "granddaughter"  (not present)
            1\. - (0\. + alpha) / (num_spam_messages + 2\. * alpha), // "please"  (not present)
            1\. - (0\. + alpha) / (num_spam_messages + 2\. * alpha), // "explain"  (not present)
            1\. - (0\. + alpha) / (num_spam_messages + 2\. * alpha), // "over"  (not present)
            1\. - (0\. + alpha) / (num_spam_messages + 2\. * alpha), // "dinner"  (not present)
            1\. - (1\. + alpha) / (num_spam_messages + 2\. * alpha), // "here"  (not present)
            1\. - (1\. + alpha) / (num_spam_messages + 2\. * alpha), // "in"  (not present)
            1\. - (1\. + alpha) / (num_spam_messages + 2\. * alpha), // "garage"  (not present)
        ];

        let probs_if_ham = [
            1\. - (0\. + alpha) / (num_ham_messages + 2\. * alpha), // "Free"  (not present)
            (1\. + alpha) / (num_ham_messages + 2\. * alpha),      // "Bitcoin"  (present)
            1\. - (0\. + alpha) / (num_ham_messages + 2\. * alpha), // "viagra"  (not present)
            1\. - (0\. + alpha) / (num_ham_messages + 2\. * alpha), // "XXX"  (not present)
            (1\. + alpha) / (num_ham_messages + 2\. * alpha),      // "christmas"  (present)
            (0\. + alpha) / (num_ham_messages + 2\. * alpha),      // "deals"  (present)
            1\. - (1\. + alpha) / (num_ham_messages + 2\. * alpha), // "my"  (not present)
            1\. - (1\. + alpha) / (num_ham_messages + 2\. * alpha), // "dear"  (not present)
            1\. - (1\. + alpha) / (num_ham_messages + 2\. * alpha), // "granddaughter"  (not present)
            1\. - (1\. + alpha) / (num_ham_messages + 2\. * alpha), // "please"  (not present)
            1\. - (1\. + alpha) / (num_ham_messages + 2\. * alpha), // "explain"  (not present)
            1\. - (1\. + alpha) / (num_ham_messages + 2\. * alpha), // "over"  (not present)
            1\. - (1\. + alpha) / (num_ham_messages + 2\. * alpha), // "dinner"  (not present)
            1\. - (0\. + alpha) / (num_ham_messages + 2\. * alpha), // "here"  (not present)
            1\. - (0\. + alpha) / (num_ham_messages + 2\. * alpha), // "in"  (not present)
            1\. - (0\. + alpha) / (num_ham_messages + 2\. * alpha), // "garage"  (not present)
        ];

        let p_if_spam_log: f64 = probs_if_spam.iter().map(|p| p.ln()).sum();
        let p_if_spam = p_if_spam_log.exp();

        let p_if_ham_log: f64 = probs_if_ham.iter().map(|p| p.ln()).sum();
        let p_if_ham = p_if_ham_log.exp();

        // P(message | spam) / (P(messge | spam) + P(message | ham)) rounds to 0.97
        assert!((model.predict(input_text) - p_if_spam / (p_if_spam + p_if_ham)).abs() < 0.000001);
    }
}
```

现在运行`cargo test`。如果您认为这是正确的，那么做得好，您已经在 Rust 中实现了一个朴素贝叶斯分类器！

谢谢你和我一起编码，朋友们。如果您有任何问题或建议，请随时联系我们。

### 参考

1.  Grus，J. (2019 年)。*数据科学从零开始:Python 的基本原理，第二版。*奥莱利媒体。
2.  唐尼(2021)。*思考贝叶斯:Python 中的贝叶斯统计，第二版。*奥莱利媒体。
3.  墨菲，K. (2012 年)。*机器学习:概率视角。*麻省理工学院出版社。
4.  Dhinakaran，V. (2017)。*铁锈食谱。* Packt。
5.  [Ng，A. (2018)。斯坦福 CS229:第五讲- GDA &朴素贝叶斯。](https://www.youtube.com/watch?v=nt63k3bfXS0)
6.  Burden，R. Faires，J. Burden，A. (2015 年)。*数值分析，第 10 版。*布鲁克斯科尔。
7.  [*下溢。*科技百科。](https://www.techopedia.com/definition/712/underflow)