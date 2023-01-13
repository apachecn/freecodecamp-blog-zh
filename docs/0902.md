# Python 项目——如何使用 Python 和 Rich 构建一个 Wordle 克隆

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-wordle-clone-using-python-and-rich/>

Wordle 是一个流行的游戏，你可以在六次尝试中猜出一个五个字母的单词。每次猜中后，瓷砖的颜色会改变，以显示您的猜测与单词的接近程度。

它类似于我已经向你展示过如何使用 Python 构建的[刽子手游戏](https://ireadblog.com/posts/124/hangman-game-using-python)。

在本教程中，我们将使用 Python 和 Rich(一个富文本格式库)构建我们自己的流行游戏 Wordle 的终端版本。

如果你刚刚接触到 Rich，请查看本教程开始学习。在开始跟随本文之前，请确保您已经安装了 Rich。

## 项目演示

让我们看看我们的最终游戏会是什么样子:

[https://www.youtube.com/embed/-Mzsvg7IrME?rel=0](https://www.youtube.com/embed/-Mzsvg7IrME?rel=0)

## 让我们编写我们的 Wordle 游戏

和真正的 Wordle 游戏一样，每天随机选择一个五个字母的单词。为此，我们将从五个字母的单词列表中随机选择一个单词。

因此，首先，让我们在一个单独的`words.py`文件中定义名为`word_list`的单词列表:

```
word_list = ['ABOUT', 'ABOVE', 'ABUSE', 'ACTOR', 'ACUTE', 'ADMIT', 'ADOPT', 'ADULT', 'AFTER', 'AGAIN', 'AGENT', 'AGREE', 'AHEAD', 'ALARM', 'ALBUM', 'ALERT', 'ALIKE', 'ALIVE', 'ALLOW', 'ALONE', 'ALONG', 'ALTER', 'AMONG', 'ANGER', 'ANGLE', 'ANGRY', 'APART', 'APPLE', 'APPLY', 'ARENA', 'ARGUE', 'ARISE', 'ARRAY', 'ASIDE', 'ASSET', 'AUDIO', 'AUDIT', 'AVOID', 'AWARD', 'AWARE', 'BADLY', 'BAKER', 'BASES', 'BASIC', 'BASIS', 'BEACH', 'BEGAN', 'BEGIN', 'BEGUN', 'BEING', 'BELOW', 'BENCH', 'BILLY', 'BIRTH', 'BLACK', 'BLAME', 'BLIND', 'BLOCK', 'BLOOD', 'BOARD', 'BOOST', 'BOOTH', 'BOUND', 'BRAIN', 'BRAND', 'BREAD', 'BREAK', 'BREED', 'BRIEF', 'BRING', 'BROAD', 'BROKE', 'BROWN', 'BUILD', 'BUILT', 'BUYER', 'CABLE', 'CALIF', 'CARRY', 'CATCH', 'CAUSE', 'CHAIN', 'CHAIR', 'CHART', 'CHASE', 'CHEAP', 'CHECK', 'CHEST', 'CHIEF', 'CHILD', 'CHINA', 'CHOSE', 'CIVIL', 'CLAIM', 'CLASS', 'CLEAN', 'CLEAR', 'CLICK', 'CLOCK', 'CLOSE', 'COACH', 'COAST', 'COULD', 'COUNT', 'COURT', 'COVER', 'CRAFT', 'CRASH', 'CREAM', 'CRIME', 'CROSS', 'CROWD', 'CROWN', 'CURVE', 'CYCLE', 'DAILY', 'DANCE', 'DATED', 'DEALT', 'DEATH', 'DEBUT', 'DELAY', 'DEPTH', 'DOING', 'DOUBT', 'DOZEN', 'DRAFT', 'DRAMA', 'DRAWN', 'DREAM', 'DRESS', 'DRILL', 'DRINK', 'DRIVE', 'DROVE', 'DYING', 'EAGER', 'EARLY', 'EARTH', 'EIGHT', 'ELITE', 'EMPTY', 'ENEMY', 'ENJOY', 'ENTER', 'ENTRY', 'EQUAL', 'ERROR', 'EVENT', 'EVERY', 'EXACT', 'EXIST', 'EXTRA', 'FAITH', 'FALSE', 'FAULT', 'FIBER', 'FIELD', 'FIFTH', 'FIFTY', 'FIGHT', 'FINAL', 'FIRST', 'FIXED', 'FLASH', 'FLEET', 'FLOOR', 'FLUID', 'FOCUS', 'FORCE', 'FORTH', 'FORTY', 'FORUM', 'FOUND', 'FRAME', 'FRANK', 'FRAUD', 'FRESH', 'FRONT', 'FRUIT', 'FULLY', 'FUNNY', 'GIANT', 'GIVEN', 'GLASS', 'GLOBE', 'GOING', 'GRACE', 'GRADE', 'GRAND', 'GRANT', 'GRASS', 'GREAT', 'GREEN', 'GROSS', 'GROUP', 'GROWN', 'GUARD', 'GUESS', 'GUEST', 'GUIDE', 'HAPPY', 'HARRY', 'HEART', 'HEAVY', 'HENCE', 'HENRY', 'HORSE', 'HOTEL', 'HOUSE', 'HUMAN', 'IDEAL', 'IMAGE', 'INDEX', 'INNER', 'INPUT', 'ISSUE', 'JAPAN', 'JIMMY', 'JOINT', 'JONES', 'JUDGE', 'KNOWN', 'LABEL', 'LARGE', 'LASER', 'LATER', 'LAUGH', 'LAYER', 'LEARN', 'LEASE', 'LEAST', 'LEAVE', 'LEGAL', 'LEVEL', 'LEWIS', 'LIGHT', 'LIMIT', 'LINKS', 'LIVES', 'LOCAL', 'LOGIC', 'LOOSE', 'LOWER', 'LUCKY', 'LUNCH', 'LYING', 'MAGIC', 'MAJOR', 'MAKER', 'MARCH', 'MARIA', 'MATCH', 'MAYBE', 'MAYOR', 'MEANT', 'MEDIA', 'METAL', 'MIGHT', 'MINOR', 'MINUS', 'MIXED', 'MODEL', 'MONEY', 'MONTH', 'MORAL', 'MOTOR', 'MOUNT', 'MOUSE', 'MOUTH', 'MOVIE', 'MUSIC', 'NEEDS', 'NEVER', 'NEWLY', 'NIGHT', 'NOISE', 'NORTH', 'NOTED', 'NOVEL', 'NURSE', 'OCCUR', 'OCEAN', 'OFFER', 'OFTEN', 'ORDER', 'OTHER', 'OUGHT', 'PAINT', 'PANEL', 'PAPER', 'PARTY', 'PEACE', 'PETER', 'PHASE', 'PHONE', 'PHOTO', 'PIECE', 'PILOT', 'PITCH', 'PLACE', 'PLAIN', 'PLANE', 'PLANT', 'PLATE', 'POINT', 'POUND', 'POWER', 'PRESS', 'PRICE', 'PRIDE', 'PRIME', 'PRINT', 'PRIOR', 'PRIZE', 'PROOF', 'PROUD', 'PROVE', 'QUEEN', 'QUICK', 'QUIET', 'QUITE', 'RADIO', 'RAISE', 'RANGE', 'RAPID', 'RATIO', 'REACH', 'READY', 'REFER', 'RIGHT', 'RIVAL', 'RIVER', 'ROBIN', 'ROGER', 'ROMAN', 'ROUGH', 'ROUND', 'ROUTE', 'ROYAL', 'RURAL', 'SCALE', 'SCENE', 'SCOPE', 'SCORE', 'SENSE', 'SERVE', 'SEVEN', 'SHALL', 'SHAPE', 'SHARE', 'SHARP', 'SHEET', 'SHELF', 'SHELL', 'SHIFT', 'SHIRT', 'SHOCK', 'SHOOT', 'SHORT', 'SHOWN', 'SIGHT', 'SINCE', 'SIXTH', 'SIXTY', 'SIZED', 'SKILL', 'SLEEP', 'SLIDE', 'SMALL', 'SMART', 'SMILE', 'SMITH', 'SMOKE', 'SOLID', 'SOLVE', 'SORRY', 'SOUND', 'SOUTH', 'SPACE', 'SPARE', 'SPEAK', 'SPEED', 'SPEND', 'SPENT', 'SPLIT', 'SPOKE', 'SPORT', 'STAFF', 'STAGE', 'STAKE', 'STAND', 'START', 'STATE', 'STEAM', 'STEEL', 'STICK', 'STILL', 'STOCK', 'STONE', 'STOOD', 'STORE', 'STORM', 'STORY', 'STRIP', 'STUCK', 'STUDY', 'STUFF', 'STYLE', 'SUGAR', 'SUITE', 'SUPER', 'SWEET', 'TABLE', 'TAKEN', 'TASTE', 'TAXES', 'TEACH', 'TEETH', 'TERRY', 'TEXAS', 'THANK', 'THEFT', 'THEIR', 'THEME', 'THERE', 'THESE', 'THICK', 'THING', 'THINK', 'THIRD', 'THOSE', 'THREE', 'THREW', 'THROW', 'TIGHT', 'TIMES', 'TIRED', 'TITLE', 'TODAY', 'TOPIC', 'TOTAL', 'TOUCH', 'TOUGH', 'TOWER', 'TRACK', 'TRADE', 'TRAIN', 'TREAT', 'TREND', 'TRIAL', 'TRIED', 'TRIES', 'TRUCK', 'TRULY', 'TRUST', 'TRUTH', 'TWICE', 'UNDER', 'UNDUE', 'UNION', 'UNITY', 'UNTIL', 'UPPER', 'UPSET', 'URBAN', 'USAGE', 'USUAL', 'VALID', 'VALUE', 'VIDEO', 'VIRUS', 'VISIT', 'VITAL', 'VOICE', 'WASTE', 'WATCH', 'WATER', 'WHEEL', 'WHERE', 'WHICH', 'WHILE', 'WHITE', 'WHOLE', 'WHOSE', 'WOMAN', 'WOMEN', 'WORLD', 'WORRY', 'WORSE', 'WORST', 'WORTH', 'WOULD', 'WOUND', 'WRITE', 'WRONG', 'WROTE', 'YIELD', 'YOUNG', 'YOUTH'] 
```

## 效用函数

我们将定义几个实用函数来帮助我们在控制台上打印彩色文本。我们将使用与 Wordle 默认值相似的设置:

*   绿色=正确位置的正确字母
*   黄色=字母正确，但位置不正确
*   灰色=字母不正确

为此，我们将使用 Rich。使用它打印彩色文本非常容易，代码如下:

```
from rich.console import Console
from random import choice
from words import word_list

WELCOME_MESSAGE = f'\n[white on blue] WELCOME TO WORDLE [/]\n'
PLAYER_INSTRUCTIONS = "You may start guessing\n"
ALLOWED_GUESSES = 6

def correct_place(letter):
    return f'[black on green]{letter}[/]'

def correct_letter(letter):
    return f'[black on yellow]{letter}[/]'

def incorrect_letter(letter):
    return f'[black on white]{letter}[/]'

if __name__ == '__main__':
    console = Console()
    chosen_word = choice(word_list)
    console.print(WELCOME_MESSAGE)
    console.print(PLAYER_INSTRUCTIONS)
```

在上面的代码中，我们定义了三个函数来使用 Rich 打印彩色文本。我们还增加了一个欢迎信息和玩家说明，这将在主功能中使用。

允许的猜测设置为 6。在 main 函数中，我们首先从`rich.console`创建一个`Console`类的实例。然后我们从`words.py`中定义的`word_list`中随机选择一个词。

在最后一行，我们只是使用`console.print`打印欢迎消息和玩家说明。

## 游戏循环

我们将运行一个 while 循环，直到用完所有的猜测。让我们看看代码，然后解释它。

```
GUESS_STATEMENT = "\nEnter your guess"

SQUARES = {
    'correct_place': '🟩',
    'correct_letter': '🟨',
    'incorrect_letter': '⬛'
}

def check_guess(guess, answer):
    guessed = []
    wordle_pattern = []
    for i, letter in enumerate(guess):
        if answer[i] == guess[i]:
            guessed += correct_place(letter)
            wordle_pattern.append(SQUARES['correct_place'])
        elif letter in answer:
            guessed += correct_letter(letter)
            wordle_pattern.append(SQUARES['correct_letter'])
        else:
            guessed += incorrect_letter(letter)
            wordle_pattern.append(SQUARES['incorrect_letter'])
    return ''.join(guessed), ''.join(wordle_pattern)

def game(console, chosen_word):
    end_of_game = False
    already_guessed = []
    full_wordle_pattern = []
    all_words_guessed = []

    while not end_of_game:
        guess = Prompt.ask(GUESS_STATEMENT).upper()
        while len(guess) != 5 or guess in already_guessed:
            if guess in already_guessed:
                console.print("[red]You've already guessed this word!!\n[/]")
            else:
                console.print('[red]Please enter a 5-letter word!!\n[/]')
            guess = Prompt.ask(GUESS_STATEMENT).upper()
        already_guessed.append(guess)
        guessed, pattern = check_guess(guess, chosen_word)
        all_words_guessed.append(guessed)
        full_wordle_pattern.append(pattern)

        console.print(*all_words_guessed, sep="\n")
        if guess == chosen_word or len(already_guessed) == ALLOWED_GUESSES:
            end_of_game = True
    if len(already_guessed) == ALLOWED_GUESSES and guess != chosen_word:
        console.print(f"\n[red]WORDLE X/{ALLOWED_GUESSES}[/]")
        console.print(f'\n[green]Correct Word: {chosen_word}[/]')
    else:
        console.print(f"\n[green]WORDLE {len(already_guessed)}/{ALLOWED_GUESSES}[/]\n")
    console.print(*full_wordle_pattern, sep="\n")
```

`game()`函数接受两个参数——`console`和`chosen_word`(正确答案)。我们有一个布尔变量`end_of_game`来控制 while 循环。while 循环将一直运行，直到`end_of_game`变为真。

`already_guessed`是包含用户已经猜到的单词的列表。`full_wordle_pattern`列表将包含 Wordle 图案(由彩色方块组成)。`all_words_guessed`列表将包含带有颜色的单词。

之后，我们运行一个 while 循环，直到`end_of_game`并提示用户猜测单词。我们还不断检查用户猜测的单词是否包含五个字母，以及用户是否已经猜到该单词。

在这两种情况下，我们都会打印一个错误，并提示用户再次猜测。

如果用户已经猜出了一个单词，我们就把这个单词放在`already_guessed`列表中。然后我们使用一个名为`check_guess()`的函数来检查用户是否猜对了单词。

## `check_guess()`功能

该函数接受两个参数——用户猜测的单词和正确答案。它逐个字母地进行比较，然后使用我们之前定义的帮助函数为每个字母创建丰富的格式化字符串。然后将它们连接成一个字符串。

该函数返回两个字符串——格式丰富的猜测单词和该猜测单词的彩色方块。

返回的字符串存储在两个变量中，`guessed`和`pattern`，然后追加到各自的列表中，`all_words_guessed`和`full_wordle_pattern`。

在每个循环中，我们将打印由换行符(`\n`)分隔的`all_words_guessed`列表中的元素。如果用户猜对了这个单词，或者已经用完了允许的猜测次数，我们将把`end_of_game`设置为`True`，循环将终止。

在循环之外，我们将检查允许的猜测是否已经用尽，用户是否能够正确地猜出单词。如果是这种情况，我们将打印正确的 word 和 WORDLE X/6。但是在另一种情况下，我们将打印 WORDLE `n` /6，其中 n 是用户猜对单词的次数。

在最后，我们将打印由换行符(`\n`)分隔的 Wordle 模式列表。

在主函数中，我们将调用带有`console`和`chosen_word`参数的`game()`函数。

## 完整的项目代码

以下是 Wordle 克隆的完整代码:

```
from rich.prompt import Prompt
from rich.console import Console
from random import choice
from words import word_list

SQUARES = {
    'correct_place': '🟩',
    'correct_letter': '🟨',
    'incorrect_letter': '⬛'
}

WELCOME_MESSAGE = f'\n[white on blue] WELCOME TO WORDLE [/]\n'
PLAYER_INSTRUCTIONS = "You may start guessing\n"
GUESS_STATEMENT = "\nEnter your guess"
ALLOWED_GUESSES = 6

def correct_place(letter):
    return f'[black on green]{letter}[/]'

def correct_letter(letter):
    return f'[black on yellow]{letter}[/]'

def incorrect_letter(letter):
    return f'[black on white]{letter}[/]'

def check_guess(guess, answer):
    guessed = []
    wordle_pattern = []
    for i, letter in enumerate(guess):
        if answer[i] == guess[i]:
            guessed += correct_place(letter)
            wordle_pattern.append(SQUARES['correct_place'])
        elif letter in answer:
            guessed += correct_letter(letter)
            wordle_pattern.append(SQUARES['correct_letter'])
        else:
            guessed += incorrect_letter(letter)
            wordle_pattern.append(SQUARES['incorrect_letter'])
    return ''.join(guessed), ''.join(wordle_pattern)

def game(console, chosen_word):
    end_of_game = False
    already_guessed = []
    full_wordle_pattern = []
    all_words_guessed = []

    while not end_of_game:
        guess = Prompt.ask(GUESS_STATEMENT).upper()
        while len(guess) != 5 or guess in already_guessed:
            if guess in already_guessed:
                console.print("[red]You've already guessed this word!!\n[/]")
            else:
                console.print('[red]Please enter a 5-letter word!!\n[/]')
            guess = Prompt.ask(GUESS_STATEMENT).upper()
        already_guessed.append(guess)
        guessed, pattern = check_guess(guess, chosen_word)
        all_words_guessed.append(guessed)
        full_wordle_pattern.append(pattern)

        console.print(*all_words_guessed, sep="\n")
        if guess == chosen_word or len(already_guessed) == ALLOWED_GUESSES:
            end_of_game = True
    if len(already_guessed) == ALLOWED_GUESSES and guess != chosen_word:
        console.print(f"\n[red]WORDLE X/{ALLOWED_GUESSES}[/]")
        console.print(f'\n[green]Correct Word: {chosen_word}[/]')
    else:
        console.print(f"\n[green]WORDLE {len(already_guessed)}/{ALLOWED_GUESSES}[/]\n")
    console.print(*full_wordle_pattern, sep="\n")

if __name__ == '__main__':
    console = Console()
    chosen_word = choice(word_list)
    console.print(WELCOME_MESSAGE)
    console.print(PLAYER_INSTRUCTIONS)
    game(console, chosen_word) 
```

## 包扎

在本文中，我们创建了自己的 Wordle 终端版本。但是从现在开始，你仍然可以做很多事情。与真实的 Wordle 类似，您可以创建该项目的 web 版本。

如果有任何问题，请告诉我！与你的朋友分享。

代码库:[https://github.com/ashutoshkrris/Terminal-Wordle](https://github.com/ashutoshkrris/Terminal-Wordle)

你可以在我的博客上阅读这篇文章和我的其他文章。