# 通过编写 7 个项目来学习 Julia 动手编程教程

> 原文：<https://www.freecodecamp.org/news/learn-julia-by-coding-7-projects/>

Julia 编程语言被用于许多真正有影响力和有趣的挑战，如机器学习和数据科学。

但是在你接触复杂的东西之前，有必要探索一下基础知识，为你打下坚实的基础。

在本教程中，我们将通过构建 7 个小型的 Julia 项目来复习 Julia 的一些基础知识:

*   疯狂的 Libs
*   猜数字游戏💯
*   电脑号码猜测器🤖
*   岩石🗿，纸张📃、剪刀✂️
*   密码生成器🎫
*   骰子滚动模拟器🎲
*   倒计时定时器⏱️

如果你还没有下载茱莉亚，请前往:[https://julialang.org/downloads/](https://julialang.org/downloads/)或观看此视频:

[https://www.youtube.com/embed/t67TGcf4SmM?feature=oembed](https://www.youtube.com/embed/t67TGcf4SmM?feature=oembed)

Download Julia

同样值得注意的是，如果你对 Julia 完全陌生，想要全面了解这门语言，你可以[看看这篇 freeCodeCamp 文章](https://www.freecodecamp.org/news/learn-julia-programming-language/)。

## 适合初学者的 Julia 项目

### 如何在朱莉娅·✍️中建立疯狂图书馆

在 Mad Libs 中，用户被提示输入不同类型的单词。然后，用户输入的随机单词被插入到一个句子中。这导致了一些非常古怪和有趣的结果。让我们试着用 Julia 编写一个简单的程序。

在这个问题的核心，我们想要连接(或加在一起)多个字符串，以便我们从带有一些占位符的句子到带有用户输入的句子。

在 Julia 中实现这一点的最简单方法是字符串插值:

```
julia> name = "Logan"
"Logan"

julia> new_string = "Hello, my name is $name"
"Hello, my name is Logan"
```

这里我们可以看到，我们可以使用`$name`语法将我们定义的名称变量插入到字符串中。

有很多其他方法可以做到这一点，比如使用`string`函数:

```
julia> new_string = string("Hello, my name is ", name)
"Hello, my name is Logan" 
```

但是在这种情况下，字符串插值似乎是最直接、最易读的。

既然我们知道如何设置字符串，我们需要提示用户输入。

为此，我们可以如下使用`readline`函数:

```
julia> my_name = readline()
Logan
"Logan"
```

`readline`函数接受用户的单行输入。这正是我们想要使用的。让我们将所有这些放在一个简单的例子中:

```
function play_mad_libs()

    print("Enter a verb (action): ")
    verb1 = readline()

    print("Enter an adjective (descriptive word): ")
    adj1 = readline()

    print("Enter a noun (person place or thing): ")
    noun1 = readline()

    print("Enter another noun (person place or thing): ")
    noun2 = readline()

    print("Enter a catchphrase (something like 'hands up!'): ")
    phrase1 = readline()

    base_sentence = "John $verb1 down the street one night, playing with his $adj1 $noun1\. When all of a / sudden, a $noun2 jumped out at him and said $phrase1"

    print("\n\n", base_sentence)
end

# Link to source code: https://github.com/logankilpatrick/Julia-Projects-for-Beginners/blob/main/madlibs.jl
```

Possible solution to the Mad Libs project

在这个例子中，我们学习了如何处理字符串、定义函数、使用打印语句等等！

如前所述，有很多其他的方法来做我们上面做的事情。所以如果你想了解更多关于使用字符串的知识，请点击这里查看 Julia 文档。

### 如何在 Julia 中构建一个猜数字游戏💯

在这个游戏中，我们将不得不产生一个随机数，然后猜猜它是什么。

首先，我们需要生成一个随机数。像往常一样，有许多方法可以做到这一点，但最直接的方法是按照以下步骤进行:

```
julia> rand(1:10)
4
```

`rand`函数将您想要使用的数字范围作为您将生成的数字的界限。在这种情况下，我们将范围设置为`1-10`，包括这两个数字。

为了让这个例子正常工作，我们需要讨论的另一个新主题是 while 循环。while 循环的基本结构是:

```
while some_condition is true
   do something
end
```

该循环将继续迭代，直到不再满足 while 循环的条件。您将很快看到我们如何使用它来不断提示用户输入一个数字，直到他们猜对为止。

最后，为了让我们简单一点，我们将添加一个 if 语句，告诉我们是否猜测到一个接近目标数字的数字。Julia 中 if 语句的结构是:

```
if some_condition is true
   do something
end
```

最大的区别是 if 语句只检查一次，然后就完成了。除非 if 语句在循环中，否则不会重新检查初始条件。

现在我们已经有了基本的想法，让我们看看构建数字猜测器的实际代码。在检查下面的解决方案之前，请确保您自己尝试过。编码快乐！🎉

```
# Number Guessing Game in Julia
# Source: https://github.com/logankilpatrick/10-Julia-Projects-for-Beginners

function play_number_guess_human()

    total_numbers = 25 # 

    # Generate a random number within a certain range
    target_number = rand(1:total_numbers)
    guess = 0

    # While the number has not been guessed, keep prompting for guesses
    while guess != target_number
        print("Please guess a number between 1 and $total_numbers: ")
        guess = parse(Int64, readline())
        # Convert the string value input to a number

        # If we are within +/-2 of the target, give a hint
        if abs(target_number - guess) <= 2 && target_number != guess
            print("\nYou are getting closer!\n")
        end
    end

    print("Nice job, you got it!")
end
```

Possible solution to the Guess the Number Game project

### 如何在 Julia 中建立一个计算机号码猜测器🤖

既然我们已经看到了让我们尝试并猜测计算机随机生成的东西的样子，那么让我们看看计算机是否能做得更好。

在这个游戏中，我们将选择一个数字，然后看看计算机需要多长时间来猜测这个数字。为此，我们将引入一些新概念，如随机模块和 for 循环。

我们将从思考如何让计算机不重复地猜测随机数开始。

一个简单的解决方案是使用`rand`函数，但问题是没有内置的方法来确保计算机不会多次猜测同一个数字——因为它毕竟是随机的！

我们可以通过组合`collect`函数和`shuffle`函数来解决这个问题。我们首先定义一个随机种子:

```
julia> rng = MersenneTwister(1234);
```

随机种子使得随机数生成器产生可重复的结果。接下来，我们需要定义所有可能的猜测:

```
julia> a = collect(1:50)
50-element Vector{Int64}:
1
2
3
⋮
```

我们现在需要使用`shuffle`函数使猜测变得随机:

```
julia> using Random
julia> shuffle(rng, a)
50-element Vector{Int64}:
41
23
13
49
⋮
```

既然我们已经设置了随机猜测，是时候一次遍历一个，看看这个数字是否等于用户输入的目标。

同样，在查看以下解决方案之前，请尝试一下:

```
# Computer Number Guessing Game in Julia
# Source: https://github.com/logankilpatrick/10-Julia-Projects-for-Beginners

using Random

function play_number_guess_computer()

    print("Please enter a number between 1 and 50 for the computer to try and guess: ")

    # Take in the user input and convert it to a number
    target_number = parse(Int64, readline())

    # Create an array of 50 numbers
    guess_order = collect(1:50)

    # Define our random seed
    rng = MersenneTwister(1234)

    # Shuffle the array randomly given our seed
    shuffled_guess = shuffle(rng, guess_order)

    # Loop through each guess and see if it's right
    for guess in shuffled_guess

        if guess == target_number
            print("\nThe computer cracked the code and guessed it right!")
            break # Stop the for loop if we get it right
        end

        print("\nComputer guessed: $guess")
    end
end
```

Possible solution to the Computer Number Guesser project

### 如何建造岩石🗿，纸张📃，剪刀手✂️在朱丽亚

如果你从未玩过石头、剪子、布，那你就错过了！基本要点是你试图用石头、布或剪刀击败你的对手。

在这个游戏中，石头打败剪刀，剪刀打败布，布打败石头。如果两个人做同一个，你再来一次。

在这个例子中，我们将对着电脑玩石头、布、剪刀。我们还将使用`sleep`函数引入一个短暂的延迟，就好像有人在大声朗读单词一样(如果你亲自演奏，你就会这么做)。

sleep 函数接收一个数字，代表你想睡多久(以秒为单位)。我们可以用这个函数或循环来减慢速度，就像你在这个游戏中看到的那样。

```
sleep(1) # Sleep for 1 second 
```

让我们也探索一下我在编写本教程时发现的一个函数，`Base.prompt` ，它帮助我们做我们之前使用`readline`所做的事情。

然而，在这种情况下，`prompt`会自动在行尾追加一个`:`,这样我们就可以避免打印和用户输入各占两行:

```
human_move = Base.prompt("Please enter 🗿, 📃, or ✂️") 
```

我们还需要使用一个`elseif`来使这个示例游戏工作。为了完整性，我们可以将`if`、`elseif`和`else`链接在一起。尝试将 if 条件、提示和休眠放在一起以获得期望的行为，然后查看下面的代码:

![1-406j3f0e3nN-VxRJUUtK7A](img/0e43290344df87996340c3831a0070d7.png)

Gif of playing Rock Paper Scissors in the Julia REPL

```
# Rock 🗿, Paper 📃, Scissors ✂️ Game in Julia

function play_rock_paper_scissors()
    moves = ["🗿", "📃", "✂️"]
    computer_move = moves[rand(1:3)]

    # Base.prompt is similar to readline which we used before
    human_move = Base.prompt("Please enter 🗿, 📃, or ✂️")
    # Appends a ": " to the end of the line by default

    print("Rock...")
    sleep(0.8)

    print("Paper...")
    sleep(0.8)

    print("Scissors...")
    sleep(0.8)

    print("Shoot!\n")

    if computer_move == human_move
        print("You tied, please try again")
    elseif computer_move == "🗿" && human_move == "✂️"
        print("You lose, the computer won with 🗿, please try again")
    elseif computer_move == "📃" && human_move == "🗿"
        print("You lose, the computer won with 📃, please try again")
    elseif computer_move == "✂️" && human_move == "📃"
        print("You lose, the computer won with ✂️, please try again")
    else
        print("You won, the computer lost with $computer_move, nice work!")
    end

end
```

Possible solution to the Rock Paper Scissors project

### 如何在 Julia 中构建密码生成器🎫

**警告:请勿使用此代码生成真实密码！**

在这个数据泄露层出不穷、每个网站都使用同一个密码的时代，拥有一个安全的密码非常重要。在本例中，我们将生成任意数量的长度可变的密码。

考虑到这可能需要很长时间，我们还将添加一个外部包， [ProgressBars.jl](https://github.com/cloud-oak/ProgressBars.jl) ，以直观地显示我们的 for 循环的进度。如果你以前从未添加过外部包，可以考虑[看看这篇强大的教程](https://blog.devgenius.io/the-most-underrated-feature-of-the-julia-programming-language-the-package-manager-652065f45a3a)，它讲述了为什么包管理器是 Julia 编程语言中最被低估的特性。

要添加 Julia 包，请打开 REPL 并键入`]`，然后键入`add ProgressBars`。之后，正如我们对 Random 模块所做的那样(注意我们不需要添加它，因为它是基本 Julia 的一部分)，我们可以说`using ProgressBars`来加载它。

我们将在这里介绍的主要新概念是向量/数组。在 Julia 中，我们可以将任何类型放入数组。要创建空数组，我们需要:

```
password_holder = []
```

然后添加一些东西，我们使用`push!`函数，你将在下面的例子中看到。

如前所述，我们将使用 ProgressBars 包在屏幕上显示进度。请注意，Julia 的速度非常快，它可能不会显示加载屏幕，除非您手动调用睡眠功能或输入大量密码来减慢速度。查看自述文件，了解在实践中使用它的示例。

与另一个示例一样，在分析下面的示例之前，尝试将一些代码放在一起:

```
# Generate Passwords in Julia
# Source: https://github.com/logankilpatrick/10-Julia-Projects-for-Beginners
using ProgressBars
using Random

# WARNING: Do not use this code to generate actual passwords!
function generate_passwords()
    num_passwords = parse(Int64, Base.prompt("How many passwords do you want to generate?"))
    password_length = parse(Int64, Base.prompt("How long should each password be?"))

    # Create an empty vector / array
    password_holder = []

    # Generate a progress bar to show how close we are to being done
    for i in ProgressBar(1:num_passwords)
        # Add the new password into the password holder
        push!(password_holder, randstring(password_length))
        sleep(0.2) # Manually slowdown the generation of passwords
    end

    # Only show the passwords if there are less than 100
    if length(password_holder) <= 100
        # Loop through each password one by one
        for password in password_holder
            print("\n", password)
        end
    end
end
```

Possible solution to the Password Generator project

### 如何在 Julia 中构建掷骰子模拟器🎲

骰子是一种有趣的方式来探索和发挥与 unicode 字符的随机性。

Julia 对 unicode 有惊人的支持，如果你想看到它支持的所有字符，[请访问 Julia 文档](https://docs.julialang.org/en/v1/manual/unicode-input/)。

让我们从定义一个骰子面数组开始。要访问 unicode 字符，我们可以通过键入以下内容使用朱莉娅·REPL 完成制表符:

```
julia> \dicei
```

然后是选项卡按钮。这将创建“模具面-1”的`⚀`。如果我们对 6 面骰子的所有 6 面都这样做，我们最终会得到:

```
dice_faces = ["⚀", "⚁", "⚂", "⚃", "⚄", "⚅"]
```

对于这个游戏，我们希望不断地询问用户是否想掷骰子。如果是这样，我们生成一个 1 到 6 之间的随机数，然后显示我们上面创建的数组中的骰子面。

就像我们在之前的项目中所做的一样，我们将希望如下使用`rand`函数:

```
rand(1:num_sides_dice)
```

在你检查出下面突出显示的一个可能的解决方案之前尝试一下，并且记住我们如何扩展它或者使用这个代码来编程一个像大富翁这样的更大的游戏。

```
# Code from https://github.com/logankilpatrick/Julia-Projects-for-Beginners

function rolling_dice()

    # Number of sides for dice
    num_sides_dice = 6

    # While the user wants to roll a die, continue to generate a number between 1 and the number of sides
    dice_faces = ["⚀", "⚁", "⚂", "⚃", "⚄", "⚅"]

    while true
        print("Do you want to roll a dice? (1=Yes/0=No): ")
        guess = parse(Int64, readline())
        # Convert the string value input to a number

        if guess == 1
            println("Rolling dice")
            current_side = rand(1:num_sides_dice)
            println("Dice has number $(dice_faces[current_side])")
        elseif guess == 0
            println("Exiting")
            break # Stop the while loop if the user decides to do so
        else
            println("Invalid input, please try again")
        end 
    end

end 
```

Possible solution to the Dice Simulator project

### 如何在 Julia 中建立一个倒计时器⏱️

倒计时，不管是好是坏，都是生活的一大部分。从新年前夕到一位家长令人沮丧地试图说服孩子遵守一些规则，我们经常看到并参与倒计时。

现在，我们将有机会编写一个程序(耶)。在核心部分，我们将再次使用`sleep`函数，在石头剪刀布的例子中我们有机会使用它。

快速提醒一下，`sleep`将我们希望程序暂停的秒数作为参数。

对于这个例子，我们将通过使用函数来尝试做一些 while 循环嵌套。我们希望有一个循环，继续提示用户是否要设置计时器，然后如果他们要设置计时器，我们调用一个名为`run_timer`的函数。`run_timer`函数应该提示用户输入他们希望定时器运行多长时间。

这里需要注意的是，我们还想在每次迭代中打印计时器剩余的时间。因此，如果用户输入 5，我们不能只做`sleep(5)`，因为用户在这 5 秒内看不到任何事情发生。

下面是给你启动的主要功能。如果你觉得合适，可以随意修改。使用该起始代码，然后根据上述规范定义`run_timer`功能。

请记住，有许多可能的方法来实现这一点，我们在底部包括的解决方案只是一种可能的方法。

```
# Code from: https://github.com/logankilpatrick/Julia-Projects-for-Beginners

function run_timer()
	# TODO
end

# Call the run_timer function in a loop until the user quits it
function countdown_timer()

    # While the user chooses to run the countdown timer
    while true
        print("Do you want set a countdown timer? (1=Yes/0=No): ")
        answer = parse(Int64, readline())
        # Convert the string value input to a number

        if answer == 1
            # Run the timer
            run_timer()
        elseif answer == 0
            println("Exiting...")
            break # Stop the countdown timer
        else
            println("Invalid input, please try again")
        end 
    end

end
countdown_timer()
```

试一试，记住你需要使用`parse`、`readline`、`sleep`和`println`函数来使这个函数工作。

```
# Code from: https://github.com/logankilpatrick/Julia-Projects-for-Beginners

function run_timer()
    print("Enter the amount of seconds: ")
    seconds = parse(Int64, readline())

    println("Countdown starts now with $seconds seconds remaining.")
    current_seconds = seconds

    # While the countdown timer is not finished
    while current_seconds != 0

        # Print the current countdown
        if current_seconds != seconds
            println("Seconds left: $current_seconds")
        end

        # Wait for one second
        sleep(1)
        current_seconds = current_seconds - 1
    end
    println("The countdown is over!")
end

# Call the run_timer function in a loop until the user quits it
function countdown_timer()

    # While the user chooses to run the countdown timer
    while true
        print("Do you want set a countdown timer? (1=Yes/0=No): ")
        answer = parse(Int64, readline())
        # Convert the string value input to a number

        if answer == 1
            # Run the timer
            run_timer()
        elseif answer == 0
            println("Exiting...")
            break # Stop the countdown timer
        else
            println("Invalid input, please try again")
        end 
    end

end

countdown_timer()
```

Possible solution to the Countdown Timer project

## 包扎🎁

我希望你在工作和阅读这些项目时，和我在创建它们时一样开心。

如果你想制作自己版本的这篇文章，制作一些小的 Julia 项目并与世界分享，请这样做，并在这里打开一个 PR:[https://github . com/logankilpatrick/10-Julia-Projects-for-初学者](https://github.com/logankilpatrick/10-Julia-Projects-for-Beginners)。

我可以很容易地改变回购的名称，以适应小项目的涌入。

我还会注意到，像这样的练习也是潜在地为朱莉娅做贡献的一个很好的方式。当我写这篇文章的时候，我打开了两个关于 Julia 的公关，我认为这将有助于改善开发者的体验:

*   [https://github.com/JuliaLang/julia/pull/43635](https://github.com/JuliaLang/julia/pull/43635)和
*   [https://github.com/JuliaLang/julia/pull/43640](https://github.com/JuliaLang/julia/pull/43640)。

如果你喜欢这个教程，[让我们在 Twitter 上联系一下](https://twitter.com/OfficialLoganK)。