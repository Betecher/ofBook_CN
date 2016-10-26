# C++ 语言基础

*作者 [Josh Nimoy](http://jtnimoy.net)*

>
> 未来的魔术师将会用数学方程式。
>
> **--Aleister Crowley, 1911**

## 生机勃勃!


此文章将介绍大家怎么使用 C++ 语言写出一个电脑程序。也许你的知识可能不够多, 不过你的读写能力将会随着这篇文章提高你的理解,因为它是众多作品的基础以及出发点。除此之外, 这里的教程都会不断升级以及善用之前所学到的一点一滴, 也就是所你不可以跳过任何一个教程主题, 以免跟不上。万一你遇到任何困难或不明白, 请在下一个教程之前寻求帮助。跟随这一系列精密的教学不止可以掌握 openFrameworks 的基础了解, 它还让你了解基本的电脑操作。


## 迭代


我在90年代里做了最多绘画以及粉刷性的艺术作品, 一个高中AP艺术生, 一头翩翩起舞的乌黑马尾, 戴着一副圆框眼镜,穿着一系列使用Liquitex 基本丙烯酸颜料泼, 甩, 点缀, 涂抹的独特衣着。由于我对经济课堂上太不感兴趣, 我利用那段时间尝试了解了TI-82 图形计算器, 然后意外的发现一些非比寻常的事情。不像我家里的小计算机, TI-82有一本很厚的使用手册, 记载的都是一些三角函数以及看似很高端的科学, 此时, 我渴望探索的视线里发现了一个性感的黑色三角形在白色作为背景的情况下, 发现了好多往上下不懂排列的三角型在里面, 正如图1显示。

![Figure 1: TI-82 rendering of the Sierpinski triangle, Courtesy of Texas Instruments](images/sierpinski-fractal-ti82.png "Figure 1: TI-82 rendering of the Sierpinski triangle, Courtesy of Texas Instruments")


这个分形是有名的 [谢尔宾斯基三角形](https://www.wolframalpha.com/input/?i=sierpinski+triangle), 基本上用了25个电脑指示来完成的谢尔宾斯程序. 我仔细的看了当中的代码, 发现了几个 numeric operations, 个人觉得不会太难, 大致上都是 “do this” 或者 “if something then do another thing” 的命令字眼。我照着书里的代码写进图形计算器, 然后实行那个程序。刚开始 LCD 潘饿了完全没显示任何东西。慢慢的, 有几个像素开始随机转黑色。此时还太随机, 看不到任何形状, 不过再过多几秒, 荧幕上逐渐地看到三角形的痕迹。再经过一段时间, 我的计算机呈现了和书里一摸一样的图像。此时的我完全被这个结果惊呆了。怎么想也想不通, 这是什么奇迹啊, 采用了这么少指示就可以获取这么复杂的成果了吗？此荧幕有多于六千个像素, 我只是用了仅仅25个指示就得到了这么赞的－作品。我可以从中参考以及改变成一个新作品吗？我从来没有想过这么神奇的结果背后尽然只有一点指示。的此时我找到了人生新的目的, 我得了解这个程序, 因为它在我的定义里已经是非常重要的一部分了。于是, 我尝试改变了里面的一些数值再从新实行此程序。这时, 荧幕上还是呈现白色背景, 不过这次的图形截然不同了, 而且这次的构图渐渐的往左边移动, 一直到黑色像素完全离开荧幕里。这下我更大胆了, 我直接把一个英语指示给换掉, 然后这个程序出现错误, 无法实行了。

![Figure 2: The human loop of a programmer.](images/programmer-cycle.png "Figure 2: The human loop of a programmer.")


图2所示的是一个无限重复的循环, 我已经非常高兴地实践了几十年, 但我仍热衷于此。 每个新的周期永远不会让我惊讶。 当我追求创建一个程序的意义, 以及创建软件艺术的意义。 迭代演化计算机指令列表的过程总是呈现出与艺术奖励一样的逻辑挑战。 几乎没有挑战是不可能解决的, 特别是可以和其他人协作及相互帮助时, 或者通过将我的难题分裂成更小的问题。 如果你已经在另一个编程环境中编写过代码, 如 Processing, Javascript, 或者甚至 HTML 和 CSS, 那么这第一个重要的课程可能很直接。

![Figure 3: Don't get the wrong idea.](images/profit-not.png "Figure 3: Don't get the wrong idea.")


对于那些刚刚熟悉了写小程序意味着什么的人, 重要的是要理解代码编写过程的迭代性质。图3中的轶事显示了什么是这个过程的 *错误理解*。你不可能只一次性得把代码敲进编辑器, 然后按下编译键并看到你完成的结果。这过程是自然的, 并且几乎都是从小程序开始, 有大量的错误（Bugs）, 并缓慢地向期望的结果或行动的目标发展。事实上, 前面的假设是一个彻底的程序员的错误, 这是很常见的。即使在古早的时候, 程序是手写在纸上, 作者仍然需要痴迷扫读代码, 以找出错误, 因此该过程是迭代渐进的。在学习 C++ 语言时, 我将提供一些小代码示例, 您可以在您的机器上进行编译。不同的是把书中的代码输入到编辑器, 然后（如果你的手指不滑）程序将会神奇地运行。我故意删除故障排除经验, 以便隔离 C++ 语言本身的主题。然后, 我们将处理最常见的任务 *调试*（修复错误）。

## 编译我的第一个程序


让我们从最小, 最直接的 C++ 程序开始, 然后使用方便的环境来测试本章的小代码片段。为了做到这一点, 我们必须要有一个 *编译器*, 这是一个程序, 将一些代码转换为一个实际的可运行的应用程序, 有时被称为可执行文件。 C++ 的编译器大多可以免费下载, 在很多时候是开源的。我们生成的应用程序不会自动显示在苹果的 App Store, Google Play, Steam, Ubuntu Apps Directory 或 Pi Store 等地方。相反, 它们是你的个人私人程序文件, 你随后可以手动共享它们。在下一章 *oF设置和项目结构* 中, 编译器将位于本地计算机上, 能够离线运行。现在, 我们将不耐烦, 并使用Sphere研究实验室的一个方便的工具在网络上编译一些随意的 C++。请打开您的网络浏览器并转到[ideone]（http://ideone.com）（http://ideone.com）。


你会立即注意到有一个编辑器已经包含一些代码, 但它可能设置的是另一种语言。 如果它不是 C++ 模式的话, 让我们把语言切换到 C++ 14。 在编辑器左下角, 按下 "stdin" 左边的按钮, 如图4所示。此按钮的标签可以是任何显示的语言。

![Figure 4](images/where-is-says-java.png "Figure 4")


菜单的下拉列表有许多编程语言。 请选择 C++ 14, 如图5所示。

![Figure 5](images/choose-c14.png "Figure 5")


请注意编辑器中的代码的变动, 看起来像图6。

![Figure 6](images/empty-template.png "Figure 6")


这只是一个空代码模板, 没有任何作用, 也不创建产生错误。 左侧槽中的数字表示代码的行号。 按标有 *Run* 的绿色按钮。 您将在注释中看到代码 “Success” 的副本, 标有 *stdin*（标准输入）的部分将为空。 *stdout*（标准输出）也将为空。


### 字体排版的小插曲


网络上的大多数字体都是可变宽度的, 意味着字母宽度不同, 阅读起来非常舒适。 字体也可以是固定宽度的, 意味着所有的字母（即使是W和小写字母i）是相同的宽度。 虽然这可能看起来有趣, 像打字机一样土气, 但是它服务于一个重要的目的。 固定宽度的字体使一块文本像一块游戏板, 或像棋子或图形纸。 计算机编程代码通常以固定宽度排版呈现, 因为它是 ASCII-art 艺术的一种形式。 缩进, 空格字符和重复模式都是很重要的, 以便于阅读和比较代码。 每个我认识的除了艺术家Jeremy Rotsztain之外的程序员, 都使用某种方式的等宽字体为他们的代码字体。 这里有一些字体建议: Courier, Andale Mono, Monaco, Profont, Monofur, Proggy, Droid Sans Mono, Deja Vu Sans Mono, Consolas 和 Inconsolata。 从现在起, 你将看到字体样式切换到 `this inline style (内联样式)` 。。。

```cpp
and this style encased in a block . . .
```

。。。这意味着你在看代码。


### 注释


现在请按代码编辑器左上角的 *Edit* 按钮（图7）。

![Figure 7](images/ideone-edit.png "Figure 7")


您将看到一个略有不同的编辑配置, 但相同的模板代码仍然可以在顶部编辑。 我们现在将编辑代码。 找到第5行：

```cpp
// your code goes here .
```

以双斜线开头的行称为注释。 您可以键入任何您需要的, 以便以您理解的方式注释代码。 有时, 通过在代码前放置两个正斜杠来 “注释代码” 是很有用的, 因为它会取消激活 C++ 代码而不删除它。 C++ 中的注释也可以占用多行, 或者像标签一样插入。 开始和结束注释模式的语法不同。 `/ * and the * /` 之间的所有内容都成为注释：

```cpp
/* 
this is a multi-line comment. 
still in comment mode.
*/
```


请删除第5行的代码, 并将其替换为以下语句：

```cpp
cout << "Hello World" << endl;
```

这行代码告诉计算机将 “Hello World” 说成一个称为 *standard output* （aka. *stdout*）的隐含文本空间。 当编写程序时, 需要知道有 *stdout* 的存在。 程序能将文本 "print" 给它。 其他时候, 它只是一个窗口窗格放在在你的编码工具中~~, ~~只用于~~排除故障~~为故障排除 **[t：更好的短语？]**。


你可以把几乎任何东西放在这些引号之间。 引用的短语是文本的调用的 *字符串*。 更具体地说, 它是一个 *c-string literal* 字符串。 我们将在本章的后面更多地介绍字符串。 在代码中, 块 `cout <<` 部分意味着“以格式化的方式向stdout发送以下内容”。 最后一个块 `<< endl` 意思是“在 helloworld 消息的结尾处添加一个回车符（end-of-line）”。 最后, 在这行代码的最后, 你会看到一个分号（;）。


在 C++ 中, 分号就像句子末尾的句号或句号。 我们必须在每个语句之后键入一个分号, 通常这是在代码行的结尾。 如果忘记键入该分号, 编译将失败。 分号是十分有用的, 因为它们允许多个语句共享一行, 或单个语句占用多行, 释放程序员灵活和表达与一个空格。 通过添加分号, 您可以确保编译器不会混淆：您帮助它, 并显示它在语句结束的地方。 当第一次学习 C 或 C++ 时, 忘记分号可能是一个非常常见的错误, 但是有必要对代码进行编译。 请特别注意确保您的代码语句以分号结尾。


当你打字时, 也许你注意到文本本身变成了多色。这个方便的功能称为 *syntax-coloring*（或语法高亮）, 并且可以潜意识地增强读取代码的能力, 解决格式错误的语法, 并协助搜索。每个工具都有自己的语法着色系统, 所以如果你想改变颜色, 请期望它不仅仅是一个文字处理器, 它的颜色是你自己添加到文档的东西。代码编辑器不会让我将一个发光的水色颜色的字体“TRON.TTF”分配给*只是*`endl`（这意味着行尾）。相反, 我可以为整个类别的语法选择一种特殊的风格, 看到我的代码样式的所有部分, 只要它是那种类型的代码。在这种情况下, `cout`和`endl`都被认为是关键字, 所以工具将它们变成黑色。如果这些东西在其他地方显示为不同的颜色, 请相信它是和以前一样的代码, 因为不同的代码编辑器提供不同的语法着色。整个代码现在应该看起来像这样：

```cpp
#include <iostream.h>
using namespace std;

int main(){
	cout << "Hello World" << endl;
	return 0;
}
```


现在按下右下角的绿色	*ideone it* 按钮, 并观察输出控制台, 它是代码编辑器的下半部分, 就在该绿色按钮的上方。 您将看到橙色状态消息, 如 “等待编译”, “编译中” 和 “运行”。 不久之后, 程序将在云中执行, 标准输出应显示在该网页上。 您应该在图8中看到新消息。

![Figure 8](images/hello-world.png "Figure 8: Hello World appears in output window")


你做了这些。 现在给自己一点勉励。 你刚刚写了你的第一行 C++ 代码; 你分析它, 编译它, 运行它, 并看到输出。


## 超越 Hello World


现在我们已经进入状态了, 让我们回去分析代码的其他部分。 第一行是一个include语句：

```cpp
#include <iostream>
```

与 Java 和 CSS 中的 *import* 类似, `＃include`类似于告诉编译器从文件中该位置处的一个名为 *iostream.h* 的文件中剪切和粘贴一些其他有用的代码, 以便可以依赖于它的代码 在您的新代码。 在这种情况下, iostream.h *提供* `cout` 和 `endl` 作为我可以在我的代码中使用的工具, 只需键入他们的名字。 在 C++ 中, 以 **.h** 结尾的文件名称为头文件, 并且它包含将包含在实际 C++ 实现文件中的代码, 其文件名将以 **.cpp** 结尾。 有许多标准头文件建立在 C++ 中, 提供各种基本服务 - 实际上太多了, 这里就不赘述了。 如果这还不够, 那么在项目中添加一个外部库（包括它的头部）也是很常见的。 您还可以将自己的头文件定义为您编写的代码的一部分, 但语法略有不同：

```cpp
#include "MyCustomInclude.h"
```

在openFrameworks中, 双引号用于包括不属于系统安装的头文件。


### 紧跟着 # 的是什么?


虽然说来话长, 但这是值得理解的概念。 include 语句不是真正的 C++ 代码（注意没有分号）。 它是一个完全独立的编译器传递的一部分, 称为*预处理器*。 它发生在你的实际程序化指令被处理之前。 它们就像代码编译器的指令, 而不是计算机在编译后运行的指令。 在这些*预处理器指令*之前使用 磅(pound) / 哈希(hash) 符号, 可以清楚地在文件中找到它们, 这也是一个重要的原因。 它们应该被视为不同的语言, 与实际的 C++ 代码混合在一起。 并没有太多的 C++ 预处理器指令 - 它们常常用于聚集其他代码。 这里有一些你可能会看到。

`#define`
`#elif`
`#else`
`#endif`
`#error`
`#if`
`#ifdef`
`#include`
`#line`
`#pragma`
`#undef`


让我们做一个实验。 在代码编辑器中, 请注释掉第1行的include指令, 然后运行代码。请在行的开头插入两个相邻的正斜杠, 注释掉代码行。

```cpp
//#include <iostream>
```

语法高亮将变为所有绿色, 这意味着它现在只是一个注释。 通过按右下角的大绿色按钮运行代码, 您将在输出窗格中看到新的内容。

```
prog.cpp: In function 'int main()':
prog.cpp:5:2: error: 'cout' was not declared in this scope
  cout << "Hello World" << endl;
  ^
prog.cpp:5:27: error: 'endl' was not declared in this scope
  cout << "Hello World" << endl;
                           ^
```


编译器发现一个错误, 没有运行程序。相反, 为了帮助你解决它, 编译器指出你的错误代码处。第一部分, *prog.cpp*：告诉你包含错误的文件。在这种情况下, ideone.com 将您的代码保存到默认文件名中。接下来, 它显示`在函数 'int main()'`: 文件中显示包含错误的代码的特定部分, 在这种情况下, 位于 {curly brace} 之间的函数称为 *main*。 （稍后我们将讨论函数和花括号）。在下一行, 我们看到`prog.cpp：5：2：`。 5是从文件顶部开始的多少行, 2是从行开始向右多少个字符。接下来, 我们看到 `error：'cout' 没有在这个范围内声明`。 这是一条消息, 描述它认为在代码中是错误的。在这种情况下, 它是相当正确的。 iostream.h 消失了, 因此没有向我们提供 `cout`, 因此当我们尝试发送 “Hello World” 时, 编译失败。在接下来的几行中, 你会看到一行代码包含了诡异的`cout`, 在它下面的行上还有一个额外的小插入符, 它应该是一个指向代码中的字符的箭头。在这种情况下, 箭头应该位于 `cout` 中的 'c' 下面。系统正在显示您的指令是否有故障。显示第二个错误, 此时, 编译器会报错说没有 endl。当然, 我们知道, 为了修复错误, 我们需要 `include <iostream.h>`, 所以让我们现在修复它。请取消注释第1行, 然后重新运行代码。

```cpp
#include <iostream>
```

当使用 openFrameworks 时, 您可以选择任意工具和平台。 每一种都以不同的方式显示错误。 有时, 编辑器会打开并突出显示您的代码, 放置一个包含更多信息的气泡框。 其他时候, 编辑器将不显示任何内容, 但是编译输出将显示格式类似于上面的原始错误。 虽然有时我们从编译中收到几个错误有时是有用的, 但如果你专注于理解和修复报告的第一个错误, 它可以节省很多的精力。 修复了 top 错误后, 所有后续错误很可能会优雅地消失, 所有的错误都已经由您的第一个修复覆盖。 通过注释掉顶部的单行代码, 我们造成了两个错误。


### 命名空间一览

看到第二行代码, 我们看到:

```cpp
using namespace std;
```

假设您加入了一个社交网站, 并要求您选择用户名。我的名字是Joshua Nimoy - 用户名可能是JNIMOY。我提交页面, 它返回一个错误, 告诉我用户名已经采取, 我必须选择另一个, 因为我的父亲 Joseph Nimoy, 他比我先注册了JNIMOY。所以我必须使用我的中间初始T, 并创建一个唯一的用户名, JTNIMOY。我刚刚创建并解决了一个 *命名空间冲突* *namespace conflict*。命名空间是一组唯一的名称, 即没有一个相同。它可以有相同的名称, 只要它们是两个单独的命名空间的一部分。命名空间帮助程序员避免因为覆盖彼此的符号或篡改彼此的名字而妨碍合作。命名空间还提供一个整洁和整洁的组织系统, 以帮助我们找到我们要找的。在openFrameworks中, 一切都以 `of` 开头。 。 。像 `ofSetBackground` 和 `ofGraphics`。这是一种执行命名空间分离的技术, 因为其他程序员创建的任何其他名称都不太可能以 `of` 开头。 OpenGL使用相同的技术。 OpenGL API（应用程序编程接口）中的每个名称都以`gl`开头, 例如 `glBlendFunc` 和 `glPopMatrix`。然而, 在 C++ 中, 没有必要为您的名称提供严格规范的字符前缀, 因为语言提供了自己的命名空间语法。在第2行中, `using namespace std;` 告诉编译器这个 .cpp 文件将使用 `std` 命名空间中的所有名称。扰流警报！这两个名称是 `cout` 和 `endl`。让我们现在做一个实验, 注释掉第2行, 然后运行代码。你认为编译器会返回什么样的错误？

```cpp
/* using namespace std; */
```

这是一个非常类似的错误, 以前, 它找不到 `cout` 和 `endl`, 但这一次, 有建议替代添加到消息列表。

```
prog.cpp:5:2: note: suggested alternative:
In file included from prog.cpp:1:0:
/usr/include/c++/4.8/iostream:61:18: note:   'std::cout'
   extern ostream cout;  /// Linked to standard output
                  ^
```

编译器说：“嘿, 我搜索了 `cout`, 我在文件中包含的命名空间中找到它, 这里是 `std :: cout` ”, 在这种情况下, 编译器是正确的。 它希望我们使用 `cout` 的方式 *更明确*, 所以我们在左侧表示它的命名空间 `std`（标准）, 用双冒号 (::) 连接。 它就像自称为 “Nimoy::Joshua”。 继续我们的实验, 编辑第5行, 为 `cout` 和 `endl` 添加了显式命名空间。

```cpp
std::cout << "Hello World" << std::endl;
```

When you run the code, you will see it compiles just fine, and succeeds in printing "Hello World". Even the line that says `using namespace std;` is still commented out. Now imagine you are writing a program to randomly generate lyrics of a song. Obviously, you would be using `cout` quite a bit. Having to type `std::` before all your `cout`s would get really tedious, and one of the reasons a programming language adds these features is to reduce typing. So although line 2 `using namespace std;` was not necessary, having it in place (along with other `using namespace` statements) can keep one's C++ code easy to type and read, through implied context.

当你运行代码, 你会看到它的编译就好, 并成功打印“Hello World”。 甚至说“using namespace std;`的行仍然被注释掉。 现在想象你正在编写一个程序来随机生成歌曲的歌词。 显然, 你会使用`cout`相当多。 在所有的`cout`之前键入`std ::`会变得很乏味, 编程语言增加这些特性的原因之一是减少打字。 因此, 尽管第2行'using namespace std;`不是必需的, 但是通过隐含的上下文, 将它放在适当位置（与其他`using namespace`语句一起）可以保持一个C ++代码容易输入和读取。

Say I'm at a Scrabble party in Manhattan, and I am the only Josh. People can just call me Josh when it's my turn to play. However, if Josh Noble joins us after dinner, it gets a bit confusing and we start to call the Joshes by first and last name for clarity. In C++, the same is also true. I would be `Nimoy::Josh` and he would be `Noble::Josh`. It's alright to have two different `cout` names, one from the `std` namespace, and another from the `improved` namespace, as long as both are expressed with explicit namespaces; `std::cout` and `improved::cout`. In fact, the compiler will complain if you don't.
You will see more double-colon syntax (::) when I introduce classes.

说我在曼哈顿的一个拼字游戏派对, 我是唯一的乔希。 当我轮到我玩时, 人们可以叫我Josh。 然而, 如果乔希·诺布尔在晚饭后加入我们, 它有点混乱, 我们开始叫首先和名字为了清楚。 在C ++中, 同样也是如此。 我会是“Nimoy :: Josh”, 他会是“Noble :: Josh”。 有两个不同的`cout`名称, 一个来自`std`命名空间, 另一个来自`improved`命名空间, 只要两者都用显式命名空间表示就好了。 `std :: cout`和`improved :: cout`。 事实上, 编译器会抱怨如果你不这样做。
当我介绍类时, 你会看到更多的双冒号语法（：:)。


## 函数

继续, 让我们看看第4行：

```cpp
int main() {
```

This is the first piece of code that has a beginning and an end, such that it "wraps around" another piece of code. But more importantly, a function *represents* the statements enclosed within it. The closing end of this *function* is the closing curly brace on line 7:

这是具有开始和结束的第一段代码, 使得它“包裹”另一段代码。 但更重要的是, 函数*表示*包含在其中的语句。 这个*函数*的结束端是第7行的结束大括号：

```cpp
}
```

In C++, we enclose groups of code statements inside functions, and each function can be seen as a little program inside the greater program, as in the simplified diagram in figure 9.

在C ++中, 我们将一组代码语句封装在函数中, 每个函数可以看作是较大程序中的一个小程序, 如图9中的简化图。

![Figure 9: Many Functions](images/program-anatomy.png "Figure 9. A program contains many functions, and each function contains zero or more statements.")

Each of these functions has a name by which we can call it. To call a function is to execute the code statements contained inside that function. The basic convenience in doing this is less typing, and we will talk about the other advantages later. Like a board game, a program has a starting position. More precisely, the program has an *entry-point* expected by the compiler to be there. That entry-point is a function called *main*. The code you write inside the *main* function is the first code that executes in your program, and therefore it is responsible for calling any other functions in your program. Who calls your *main* function? The operating system does! Let's break down the syntax of the main function in this demo. Again, for all you Processing coders, this is old news.

每个函数都有一个名字, 我们可以通过它来调用它。 调用函数是执行该函数中包含的代码语句。 这样做的基本方便是减少打字, 我们稍后将讨论其他优点。 像棋盘游戏一样, 一个程序有一个起始位置。 更确切地说, 程序具有编译器预期的*入口点*。 该入口点是一个名为* main *的函数。 在* main *函数中编写的代码是在程序中执行的第一个代码, 因此它负责调用程序中的任何其他函数。 谁调用你的* main *函数？ 操作系统！ 让我们在这个演示中分解main函数的语法。 再一次, 对于所有你处理编码, 这是老消息。

![Figure 10: The Function](images/function-anatomy.png)

When defining a function, the first token is the advertised return type. Functions can optionally return a value, like an answer to a question, a solution to a problem, the result of a task, or the product of a process. In this case, *main* promises to return an `int`, or *integer* type, which is a whole number with no fraction or decimal component. Next token is the name of our function. The system expects the word "main" in all lower-case, but you will later define your own functions and we will get into naming. Next is an opening and closing parenthesis. Yes, it seems kind of strange to have it there, since there is nothing inside it. Later, we will see what goes in there — but never leave out the pair of parentheses with functions because in a certain way, that is the major hint to the human that it's a function. In fact, from now on, when I refer to a function by name. I'll suffix it with a ( ), for example `main()` when the function requires no parameter and I'll suffix it with a (...), for example `main(...)`, when the function requires one or more parameters

当定义一个函数时, 第一个令牌是通告的返回类型。函数可以选择性地返回一个值, 例如问题的答案, 问题的解决方案, 任务的结果或过程的产物。在这种情况下, * main *承诺返回一个`int', 或者* integer *类型, 它是一个没有分数或小数部分的整数。下一个令牌是我们的函数的名称。系统期望所有小写字母中的“main”一词, 但随后您将定义自己的函数, 我们将进入命名。接下来是开始和结束括号。是的, 它似乎有点奇怪, 它在那里, 因为它里面什么都没有。后来, 我们将看到在那里发生了什么 - 但从来没有遗漏一对括号与函数, 因为在某种程度上, 这是人类的主要暗示, 它是一个函数。事实上, 从现在开始, 当我按名称引用一个函数。当函数不需要参数时, 我将使用一个（）函数后缀（例如`main（）`）, 如果`main（...）`函数需要一个或多个参数

Next, we see an opening curly bracket. Sometimes this opening curly bracket is on the same line as the preceding closing parenthesis, and other times, you will see it on its own new line. It depends on the personal style of the coder, project, or group — and both are fine. For a complete reference on different indent styles, see the Wikipedia article on Indent Style (http://en.wikipedia.org/wiki/Indent_style).

接下来, 我们看到一个开放的大括号。 有时, 这个打开的大括号与前面的右括号在同一行, 其他时候, 你会看到它在新的一行。 这取决于编码器, 项目或组的个人风格 - 两者都很好。 有关不同缩进样式的完整参考, 请参阅维基百科关于缩进样式的文章（http://en.wikipedia.org/wiki/Indent_style）。

In between this opening curly bracket and the closing one, we place our code statements that actually tell the computer to go do something. In this example, I only have one statement, and that is the required `return`. If you leave this out for a function whose return type is `int`, then the compiler will complain that you broke your promise to return an int. In this case, the operating system interprets a 0 as "nothing went wrong". Just for fun, see what happens when you change the 0 to a 1, and run the code.

在这个开放的大括号和闭包之间, 我们放置我们的代码语句, 实际告诉计算机去做某事。 在这个例子中, 我只有一个语句, 这是必需的'返回'。 如果你离开这个函数的返回类型是int, 那么编译器会抱怨你打破了你的承诺返回一个int。 在这种情况下, 操作系统将0解释为“没有出错”。 只是为了好玩, 看看当你改变0到1, 会发生什么, 并运行代码。

## Custom Functions
## 自定义函数

We will now define our own function and make use of it as a word template. Type the sample code into your editor and run it.

我们不会定义我们自己的函数, 并将它用作一个字模板。 在编辑器中输入示例代码并运行它。

```cpp
#include <iostream>
using namespace std;

void greet(string person){
	cout << "Hi there " << person << "." << endl;
}

int main() {
	greet("moon");
	greet("red balloon");
	greet("comb");
	greet("brush");
	greet("bowl full of mush");
	return 0;
}
```

The output shows a familiar bedtime story.

这输出展示了一个熟悉的睡前小故事。

```
Hi there moon.
Hi there red balloon.
Hi there comb.
Hi there brush.
Hi there bowl full of mush.
```

In this new code, notice the second function `greet(...)` which looks the same but different from `main()`. It has the same curly brackets to hold the code block, but the return type is different. It has the same pair of parentheses, but this time there is something inside. And what about that required return statement? The *void* keyword is used in place of a return type when a function does not return anything. So, since `greet(...)` has a *void* return type, the compiler will not complain should you leave out the `return`. In the parentheses, you see `string person`. This is a parameter, an input-value for the function to use. In this case, it's a bit like find-and-replace. Down in `main()`, you see I call `greet(...)` five times, and each time, I put a different string in quotes between the parentheses. Those are *arguments*.

在这个新的代码中, 注意第二个函数`greet（...）`, 它看起来是相同的, 但不同于`main（）`。 它具有相同的大括号来保存代码块, 但返回类型不同。 它有同一对括号, 但这一次里面有一些东西。 那么所需的返回语句呢？ 当函数不返回任何内容时, * void *关键字用于替换返回类型。 所以, 因为`greet（...）`有一个* void *返回类型, 编译器不会抱怨, 如果你省略`return`。 在括号中, 你看到`string person`。 这是一个参数, 是要使用的函数的输入值。 在这种情况下, 它有点像查找和替换。 在`main（）`中, 你看到我调用`greet（...）`五次, 每次我在括号之间用不同的字符串引用。 那些是*参数*。

As an aside, to help in the technicality of discerning between when to call them *arguments* and when to call them *parameters*, see this code example:

另外, 为了帮助辨别何时调用*参数*和何时调用*参数*, 请参见此代码示例：

```cpp

void myFunction(int parameter1, int parameter2){
	//todo: code
}

int main(){
	int argument1 = 4;
	int argument2 = 5;
	myFunction(argument1,argument2);
	return 0;
}

```

Getting back to the previous example, those five lines of code are all ***function calls***. They are telling `greet(...)` to execute, and passing it the one string argument so it can do its job. That one string argument is made available to `greet(...)`'s inner code via the argument called `person`. To see the order of how things happen, take a look at Figure 11.

回到前面的例子, 这五行代码都是***函数调用***。 他们告诉“greet（...）”执行, 并传递一个字符串参数, 所以它可以做它的工作。 一个字符串参数通过名为“person”的参数提供给`greet（...）'的内部代码。 要查看事件的发生顺序, 请参见图11。

![Figure 11. Function Call Flow](images/function-call.png "Figure 11. Function Call Flow")

The colorful line in Figure 11 is the path drawn by an imaginary playback head that steps over the code as it executes. We start at the blue part and go in through the main entry-point, then encounter `greet(...)`, which is where a *jump* happens. As the line turns green, it escapes out of `main()` temporarily so it can go follow along `greet(...)` for a while. About where the line turns yellow, you see it finished executing the containing code inside `greet(...)` and does a second jump (the return) this time going back to the previous saved place, where it continues to the next statement. The most obvious advantage we can see in this example is the reduction of complexity from that long `cout` statement to a simple call to `greet(...)`. If we must call `greet(...)` five times, having the routine *encapsulated* into a function gives it convenience power. Let's say you wanted to change the greeting from "Good night" to "Show's over ". Rather than updating all the lines of code you cut-and-pasted, you could just edit the one function, and all the uses of the function would change their behavior along with it, in a synchronized way. Furthermore, code can grow to be pretty complex. It helps to break it down into small routines, and use those routines as your own custom building blocks when thinking about how to build the greater software. By using functions, you are liberated from the need to meticulously represent every detail of your system; therefore a function is one kind of *abstraction* just like abstraction in art. This sort of abstraction is called *encapsulation of complexity* because it's like taking the big complex thing and putting it inside a nice little capsule, making that big complex thing seem smaller and simpler. It's a very powerful idea — not just in code.

图11中的彩色线是由虚拟的播放头绘制的路径, 其在代码执行时跨越代码。我们从蓝色部分开始, 通过主入口点进入, 然后遇到`greet（...）`, 这是一个* jump *发生的地方。当线变成绿色时, 它临时从`main（）`中逃出来, 所以它可以沿着`greet（...）`一段时间。关于该行变成黄色的位置, 您会看到它在“greet（...）”中执行了包含的代码, 然后执行第二次跳转（返回）, 此时返回到上一个保存的地方, 继续执行下一条语句。在这个例子中我们可以看到的最明显的优势是将复杂性从这个长的`cout`语句减少到一个简单的调用`greet（...）`。如果我们必须调用`greet（...）`五次, 将例程*封装为一个函数, 这给了它方便的权力。假设您想将问候语从“晚安”改为“显示结束”。而不是更新你剪切和粘贴的所有代码行, 你可以只编辑一个函数, 并且函数的所有使用将以同步方式改变它们的行为。此外, 代码可以变得相当复杂。它有助于将其分解为小例程, 并在考虑如何构建更大的软件时将这些例程作为您自己的自定义构建块。通过使用函数, 你被解放出来, 需要精心地代表你的系统的每一个细节;因此一个函数是一种*抽象*就像在艺术中的抽象。这种抽象称为*复杂性*的封装, 因为它像把大的复杂的东西放在一个漂亮的小盒子里, 使得这个复杂的东西看起来更小更简单。这是一个非常有力的想法 - 不只是在代码中。

## Encapsulation of Complexity
## 复杂的封装

Imagine actor Laurence Fishburne wearing tinted pince-nez glasses, offering you two options that are pretty complicated to explain. On the one hand, he is willing to help you escape from the evil Matrix so that you may fulfill your destiny as the hacker hero, but it involves living life on life's terms and that is potentially painful but whatever. The story must go on and by the way, there is a pretty girl. On the other hand, he is also willing to let you forget this all happened, and mysteriously plant you back in your tiny apartment where you can go on living a lie, none the wiser. These two options are explained in the movie *The Matrix* and then the main character is offered the choice in the form of colored pills, as a way to simplify an otherwise wordy film scenario. The two complex choices are encapsulated into a simple analogy that is much easier for movie audiences to swallow. See Figure 12.

想象一下演员Laurence Fishburne穿着有色眼镜, 为你提供两个相当复杂的选项来解释。 一方面, 他愿意帮助你逃离邪恶的矩阵, 以便你可以完成你的命运作为黑客英雄, 但它涉及生活的生活的生活条件, 这是潜在的痛苦, 但无论如何。 故事必须继续, 顺便说一句, 有一个漂亮的女孩。 另一方面, 他也愿意让你忘记这一切发生, 神秘地把你种在你的小公寓里, 你可以去生活谎言, 没有一个更聪明。 这两个选项在电影*矩阵*中解释, 然后主要字符提供彩色药片的形式的选择, 作为一种方式来简化否则冗长的电影场景。 两个复杂的选择被封装成一个简单的类比, 这对电影观众来说更容易吞下。 参见图12。

![Figure 12. Red Pill and Blue Pill from The Matrix](images/red-blue-pills.png "Figure 12. Red Pill and Blue Pill from The Matrix")

Rather than repeating back the entire complicated situation, Neo (the main character) needed only to swallow one of the pills. Even if it were real medicine, the idea of encapsulating complexity still applies. Most of us do not have the expertise to practice medicine in the most effective way, and so we trust physicians and pharmacologists to create just the right blend of just the right herbs and chemicals. When you swallow a pill, it is like calling that function because you have the advantage of not needing to understand the depths of the pill. You simply trust that the pill will cause an outcome. The same is true with code. Most of the time, a function was written by someone else, and if that person is a good developer, you are free to remain blissfully ignorant of their function's inner workings as long as you grasp how to properly call their function. In this way, you are the *higher-level* coder, meaning that you simply call the function but you did not write it. Someone who creates a project in openFrameworks is sitting on the shoulders of the openFrameworks layer. openFrameworks sits on the shoulders of the OpenGL Utility Toolkit, which sits on OpenGL itself, and so on. In other words, an openFrameworks project is a *higher-level* application of C++, a language with a reputation for *lower-level* programming. As illustrated in Figure 13, I sometimes run into a problem when I tell people I wrote an interactive piece in C++.

不是重复整个复杂的情况, Neo（主角）只需要吞下一个药丸。即使它是真正的医学, 封装复杂性的想法仍然适用。我们大多数人没有以最有效的方式实践医学的专业知识, 因此我们相信医生和药理学家创造恰到好处的恰当的草药和化学品的混合。当你吞下药丸, 它就像调用这个函数, 因为你有优势, 不需要了解药丸的深度。你只是相信丸会导致结果。代码也是如此。大多数时候, 一个函数是由别人写的, 如果那个人是一个好的开发者, 只要你掌握如何正确调用函数, 你就可以自由地保持对函数的内部工作的无知。这样, 你是*高级*编码器, 意味着你只需调用函数, 但你没有写它。在openFrameworks中创建项目的人正坐在openFrameworks层的肩膀上。 openFrameworks坐在OpenGL实用工具包的肩膀上, 它位于OpenGL本身, 等等。换句话说, openFrameworks项目是C ++的一个更高级别的*应用程序, 一种具有低级*编程声誉的语言。如图13所示, 当我告诉人们我用C ++编写了一个交互式文章时, 我有时会遇到一个问题。

![Figure 13. Standing on Shoulders of Giants](images/shoulders-of-giants.png "Figure 13. Standing on Shoulders of Giants")

There are a few advantages to using C++ over the other options (mostly scripting) for your new media project. The discussion can get quite religious (read: heated) among those who know the details. If you seek to learn C++, then usually it is because you seek faster runtime performance, because C++ has more libraries that you can snap into your project, or because your mentor is working in that language. An oF project is considered higher-level because it is working with a greater encapsulation of complexity, and that is something to be proud of.

使用C ++比其他选项（主要是脚本）为您的新媒体项目有一些优势。 在那些知道细节的人中, 讨论可以相当宗教（阅读：热）。 如果你试图学习C ++, 通常是因为你寻求更快的运行时性能, 因为C ++有更多的库, 你可以捕捉到你的项目, 或者因为你的导师工作在那种语言。 oF项目被认为是更高级别的, 因为它正在以更大的复杂性封装工作, 这是值得自豪的。

## Variables (part 1)
## 变量 (第一部分)

> A “thing” is a “think”, a unit of thought
> 
> -- Alan Watts
> 
> 事物即思考, 即思想的联合。
> 
> -- Alan Watts


Please enter the following program into ideone and run it.

请输入下面的程序到 ideone 里并编译它。

```cpp
#include <iostream>
using namespace std;

int main(){
	cout << "My friend is " << 42 << " years old." << endl;
	cout << "The answer to the life the universe and everything is " << 42 << "." << endl;
	cout << "That number plus 1 is " << (42+1) << "." << endl;
	return 0;
}
```

The output looks like this:

输出看起来应该是这样:

```
My friend is 42 years old.
The answer to the life the universe and everything is 42.
That number plus 1 is 43.
```

We understand from a previous lesson that stuff you put between the `<<` operators will get formatted into the `cout` object, and magically end up in the output console. Notice in the last line, I put a bit of light arithmetic (42+1) between parentheses, and it evaluated to 43. That is called an *expression*, in the mathematics sense. These three lines of code all say something about the number 42, and so they all contain a *literal* integer. A literal value is the contents typed directly into the code; some would say "hard wired" because the value is fixed once it is compiled in with the rest.

从上一课我们可以理解, 在`<<`运算符之间放置的东西将被格式化为`cout`对象, 并且神奇地出现在输出控制台中。 注意在最后一行, 我把一点点算术（42 + 1）在括号之间, 它评估到43.在数学意义上, 这被称为*表达式*。 这三行代码都是关于数字42的, 所以它们都包含一个*字面*整数。 字面值是直接键入代码的内容; 有些人会说“硬接线”, 因为一旦它编译在其余的值是固定的。


If I want to change that number, I can do what I know from word processing, and "find-and-replace" the 42 to a new value. Now what if I had 100,000 particles in a 3D world. Some have 42s that need changing, but other 42s that should not be changed? Things can get both heavy and complex when you write code. The most obvious application of *variables* is that they are a very powerful find-and-replace mechanism, but you'll see that variables are useful for more than that. So let's declare an integer at the top of the code and use it in place of the literal 42s.

如果我想改变那个数字, 我可以做我知道从文字处理, 和“查找和替换”42一个新的值。 现在如果我在3D世界中有100,000颗粒子。 一些有42s需要改变, 但其他42s不应该改变？ 当你编写代码时, 事情会变得沉重和复杂。 *变量*最明显的应用是它们是一个非常强大的查找和替换机制, 但是你会看到变量对此有用。 所以让我们在代码的顶部声明一个整数, 并使用它代替文字42。

```cpp
#include <iostream>
using namespace std;

int main(){
	
	int answer = 42;
	
	cout << "My friend is " << answer << " years old." << endl;
	cout << "The answer to the life the universe and everything is " << answer << "." << endl;
	cout << "That number plus 1 is " << (answer+1) << "." << endl;
	return 0;
}
```

Now that I am using the variable `answer`, I only need to change that one number in my code, and it will show up in all three sentences as 42. That can be more elegant than find-and-replace. Figure 18 shows the syntax explanation for declaring and initializing a variable on the same line.

现在我使用的变量`answer`, 我只需要改变一个数字在我的代码, 它将显示在所有三个句子为42。这可能比寻找和替换更优雅。 图18显示了在同一行上声明和初始化变量的语法解释。

![Figure 18. Variable declaration and initialization](images/variable-declaration.png "Figure 18. Variable declaration and initialization")

It is also possible to declare a variable and initialize it on two separate lines. That would look like:

也可以声明一个变量并在两个单独的行上初始化它。 它看起来像：

```cpp
int answer;
answer = 42;
```

In this case, there is a moment after you declare that variable when its answer may be unpredictable and glitchy because in C (unlike Java), fresh variables are not set to zero for free — you need to do it. If you don't, the variable can come up with unpredictable values — computer memory-garbage from the past. So, unless you intend to make glitch art, please always initialize your variable to some number upon declaring it, even if that number is zero.

在这种情况下, 在您声明该变量后, 有一个时刻, 当它的答案可能是不可预测的和毛刺, 因为在C（不像Java）, 新鲜变量没有设置为零免费 - 你需要这样做。 如果你不这样做, 变量可以得出不可预测的值 - 计算机内存 - 从过去的垃圾。 所以, 除非你打算制作毛刺艺术, 在声明它时, 请始终初始化一些数字, 即使这个数字是零。

### Naming your variable
### 给你的变量命名

Notice the arrow below saying "must be a valid name". We invent new names to give our namespaces, functions, variables, and other constructs we define in code (classes, structs, enums, and other things I haven't taught you). The rules for defining a new identifier in code are strict in a similar way that choosing a password on a website might be.

注意下面的箭头说“必须是有效的名称”。 我们发明了新的名字来给我们在代码中定义的命名空间, 函数, 变量和其他结构（类, 结构, 枚举和其他我没有教过的东西）。 在代码中定义新标识符的规则与在网站上选择密码类似的方式是严格的。

+ Your identifier must contain only letters, numbers, and underscores.
+ 您的标识符只能包含字母, 数字和下划线。
+ it cannot begin with a number, but it can certainly begin with an underscore.
+ 它不能以数字开头, 但它可以肯定以下划线开头。
+ The name cannot be the same as one of the language keywords (for example, the word `void`)
+ 该名称不能与其中一个语言关键字相同（例如, 单词“void”）

The following identifiers are okay.

以下标识符是合格的。

```
a
A
counter1
_x_axis
perlin_noise_frequency
_         // a single underscore is fine
___       // several underscores are fine
```

Notice lowercase a is a different identifier than uppercase A. Identifiers in C++ are case-sensitive.
The following identifiers are not okay.

注意小写字母a是与大写字母A不同的标识符。C ++中的标识符区分大小写。
以下标识符不正确。

```cpp
1infiniteloop         // should not start with a number
transient-mark-mode   // dashes should be underscores
@jtnimoy              // should not contain an @
the locH of sprite 1  // should not contain spaces
void                  // should not be a reserved word
int                   // should not be a reserved word
```

naming your variable `void_int`, although confusing, would not cause any compiler errors because the underscore joins the two keywords into a new identifier. Occasionally, you will find yourself running into `unqualified id` errors. Here is a list of C++ reserved keywords to avoid when naming variables. C++ needs them so that it can provide a complete programming language.

命名你的变量`void_int`, 虽然令人困惑, 不会导致任何编译器错误, 因为下划线将两个关键字连接到一个新的标识符。 偶尔, 你会发现自己遇到了“不合格的id”错误。 这里是一个C ++保留关键字的列表, 以避免在命名变量时。 C ++需要它们, 以便它可以提供一个完整的编程语言。

```
alignas alignof and and_eq asm auto bitand bitor bool break case catch
char char16_t char32_t class compl const constexpr const_cast continue
decltype default delete do double dynamic_cast else enum explicit
export extern false final float for friend goto if inline int long
mutable namespace new noexcept not not_eq nullptr operator or or_eq
override private protected public register reinterpret_cast return
short signed sizeof static static_assert static_cast struct switch
template this thread_local throw true try typedef typeid typename
union unsigned using virtual void volatile wchar_t while xor xor_eq
```

### Naming conventions
### 命名组件

> Differences of habit and language are nothing at all if our aims are identical and our hearts are open.
>
> **--Albus Dumbledore**
> 语言和习惯的不同是微不足道的, 如果我们的目标是想通的, 并保持开放的心态。
> 
> **--Albus Dumbledore**

Identifiers (variables included) are written with different styles to indicate their various properties, such as type of construct (variable, function, or class?), data type (integer or string?), scope (global or local?), level of privacy, etc. You may see some identifiers capitalized at the beginning and using `CamelCase`, while others remain all `lower_case_using_underscores_to_separate_the_words`. Global constants are found to be named with `ALL_CAPS_AND_UNDERSCORES`. Another way of doing lower-case naming is to start with a lowercase `letterThenCamelCaseFromThere`. You may also see a hybrid, like `ClassName__functionName__variable_name`. These different styles can indicate different categories of identifiers.

标识符（包括的变量）用不同的样式写成, 以指示它们的各种属性, 例如结构类型（变量, 函数或类？）, 数据类型（整数或字符串？）, 范围 隐私等。你可能会看到一些标识符在开头大写, 并使用`CamelCase`, 而其他仍然是`lower_case_using_underscores_to_separate_the_words`。 发现全局常量使用`ALL_CAPS_AND_UNDERSCORES`命名。 另一种做小写命名的方法是从小写字母“letterThenCamelCaseFromThere”开始。 你也可以看到一个混合, 如`ClassName__functionName__variable_name`。 这些不同的样式可以指示不同类别的标识符。

More obsessively, programmers may sometimes use what is affectionately nicknamed *Hungarian Notation*, adding character badges to an identifier to say things about it but also reduce the legibility, for example `dwLightYears` and `szLastName`. Naming conventions are not set in stone, and certainly not enforced by the compiler. Collaborators generally need to agree on these subtle naming conventions so that they don't confuse one another, and it takes discipline on everyone's part to remain consistent with whatever convention was decided. The subject of naming convention in code is still a comically heated debate among developers, just like deciding which line to put the curly brace, and whether to use tabs to indent. Like a lot of things in programming, someone will always tell you you're doing it wrong. That doesn't necessarily mean you are doing it wrong.

更强调的是, 程序员有时可能会使用亲切的昵称*匈牙利语符号*, 向标识符添加字符徽章来说明事情, 但也会降低可读性, 例如“dwLightYears”和“szLastName”。 命名约定不是一成不变的, 当然不是由编译器强制执行的。 合作者通常需要就这些微妙的命名约定达成一致, 以使它们不会相互混淆, 并且要求每个人都遵守规则, 以便与任何决定的惯例保持一致。 代码中的命名约定的主题仍然是开发者之间的热烈争论, 就像决定使用大括号的那一行, 以及是否使用制表符缩进。 像编程中的很多东西, 有人总是会告诉你你做错了。 这并不一定意味着你做错了。

### Variables change
### 改变变量

We call them variables because their values *vary* during runtime. They are most useful as a bucket where we put something (let's say water) for safe keeping. As that usually goes, we end up going back to the bucket and using some of the water, or mixing a chemical into the water, or topping up the bucket with more water, etc. A variable is like an empty bucket where you can put your stuff. Figure 19 shows a bucket from the game *Minecraft*.

我们称之为变量, 因为它们的值*在运行时间变化*。 他们最有用的一个桶, 我们把东西（我们说水）安全保存。 正如通常那样, 我们最后回到桶, 使用一些水, 或者将化学品混入水中, 或者用更多的水补足桶等。变量就像一个空桶, 你可以放置 你的东西。 图19显示了游戏* Minecraft *中的一个桶。

![Figure 19. Bucket, courtesy of Mojang AB](images/minecraft-bucket.png "Figure 19. Bucket, courtesy of Mojang AB")

If a computer program is like a little brain, then a variable is like a basic unit of remembrance. Jotting down a small note in my sketchbook is like storing a value into a variable for later use. Let's see an example of a variable changing its value.

如果一个计算机程序像一个小脑子, 那么一个变量就像一个基本的记忆单元。 在我的写生簿中记下一个小笔记就像将一个值存储到一个变量中以供以后使用。 让我们看一个变量改变它的值的例子。

```cpp
#include <iostream>
using namespace std;

int main(){
	int counter = 0;
	cout << counter;
	counter = 1;
	cout << counter;
	counter = 2;
	cout << counter;
	counter = 3;
	cout << counter;
	counter = 4;
	cout << counter;
	counter = 5;
	cout << counter;
	return 0;
}
```

The output should be `012345`. Notice the use of the equal sign. It is different than what we are accustomed to from arithmetic. In the traditional context, a single equal sign means the expressions on both sides would evaluate to the same value. In C, that is actually a double equal (==) and we will talk about it later. A single equal sign means "Solve the expression on the right side and store the answer into the variable named on the left side". It takes some getting used to if you haven't programmed before. If I were a beginning coder (as my inner child is perpetually), I would perhaps enjoy some alternative syntax to command the computer to store a value into a variable. Something along the lines of: `3 => counter` as found in the language *ChucK* by Princeton sound lab, or perhaps something a bit more visual, as my repurposing of the Minecraft crafting table in figure 20.

输出应为`012345`。 注意使用等号。 它不同于我们习惯的算术。 在传统上下文中, 单个等号意味着两侧的表达式将被评估为相同的值。 在C中, 这实际上是一个双等号（==）, 我们将在以后讨论。 单个等号表示“解答右侧的表达式并将答案存储到左侧命名的变量中”。 如果你以前没有编程, 需要一些习惯。 如果我是一个开始的编码器（因为我的内在的孩子永远）, 我可能会喜欢一些替代语法命令计算机存储一个值到一个变量。 在普林斯顿声音实验室的语言* ChucK *中发现的某些东西：`3 =>计数器, 或者可能是一些更加直观的东西, 就像我在图20中的Minecraft工艺表一样。

![Figure 20. Minecraft crafting table repurposed for variable assignment](images/minechuck.png "Figure 20. Minecraft crafting table repurposed for variable assignment")

The usefulness of having the variable name on the left side rather than the right becomes apparent in practice since the expressions can get quite lengthy! Beginning a line with `varname =` ends up being easier for the eyeball to scan because it's guaranteed to be two symbols long before starting in on whatever madness you plan on typing after the equals sign.

在左侧而不是右侧具有变量名的有用性在实践中变得明显, 因为表达式可能变得相当冗长！ 用`varname =`开始一行会更容易让眼球扫描, 因为它保证有两个符号长, 然后才开始计划在等号后输入的任何疯狂。

Analyzing the previous code example, we see the number increments by 1 each time before it is output. I am repeatedly storing literal integers into the variable. Since a programming language knows basic arithmetic, let us now try the following modification:

分析前面的代码示例, 我们看到数字在输出之前每次增加1。 我反复存储文本整数到变量。 由于编程语言知道基本算术, 现在让我们尝试以下修改：

```cpp
#include <iostream>
using namespace std;

int main(){
	int counter = 0;
	cout << counter;
	counter = counter + 1;
	cout << counter;
	counter = counter + 1;
	cout << counter;
	counter = counter + 1;
	cout << counter;
	counter = counter + 1;
	cout << counter;
	counter = counter + 1;
	cout << counter;
	return 0;
}
```

The output should still be `012345`. By saying `counter = counter + 1`, I am incrementing `counter` by 1. More specifically, I am using `counter` in the right-hand "addition" expression, and the result of that (one moment later) gets stored into `counter`. This seems a bit funny because it talks about `counter` during two different times. It reminds me of the movie series, *Back to the Future*, in which Marty McFly runs into past and future versions of himself. See Figure 21.

输出仍应为`012345`。 通过说“counter = counter + 1”, 我将计数器增加1.更具体地说, 我在右边的“加法”表达式中使用“counter”, 并且结果（一会儿之后）被存储 进入`counter'。 这似乎有点有趣, 因为它在两个不同的时间谈论“计数器”。 它让我想起了电影系列, “回到未来*”, 其中马蒂·麦克弗利遇到过去和未来的版本的自己。 参见图21。

![Figure 21. The future Marty uses the past Marty](images/futuremarty.png "Figure 21. The future Marty uses the past Marty")

Great Scott, that could make someone dizzy! But after doing it a few times, you'll see it doesn't get much more complicated than what you see there. This is a highly *practical* use of science fiction, and you probably aren't attempting to challenge the fabric of spacetime (unless you are Kyle McDonald, or maybe a Haskell coder<!-- i mean these things as a high compliment, not as mockery -->). The point here is to modify the contents of computer memory, so we have `counter` from one instruction ago, in the same way that there might already be water in our bucket when we go to add water to it. Figure 22 shows `bucket = bucket + water`.

伟大的斯科特, 这可能让某人晕了！ 但是做了几次后, 你会发现它不会比你看到的复杂得多。 这是一个高度*实用的科幻小说的使用, 你可能不是试图挑战时空的结构（除非你是凯尔麦克唐纳, 或者也许是一个Haskell编码器<！ - 我的意思是这些东西作为高恭维,  不是嘲弄 - >）。 这里的要点是修改计算机内存的内容, 所以我们有一个指令前面的`counter', 就像我们去加水一样。 图22显示了“bucket = bucket + water”。

![Figure 22. bucket = bucket + water](images/minecraft-inc.png "Figure 22. bucket = bucket + water")

Incrementing by one, or adding some value to a variable is in fact so commonplace in all programming that there is even syntactic sugar for it. *Syntactic Sugar* is a redundant grammar added to a programming language for reasons of convenience. It helps reduce typing, can increase comprehension or expressiveness, and (like sugar) makes the programmer happier. The following statements all add 1 to `counter`.

增加一个, 或者添加一些值到变量在所有的编程中是非常常见的, 甚至有语法糖。 * Syntactic Sugar *是为了方便起见而添加到编程语言的冗余语法。 它有助于减少打字, 可以增加理解或表达力, （如糖）使程序员更快乐。 以下语句都将1添加到“counter”。

```cpp
counter = counter + 1; // original form
counter += 1;          // "increment self by" useful because it's less typing.
counter++;             // "add 1 to self" useful because you don't need to type a 1.
++counter;             // same as above, but with a subtle difference.
```

Let's test this in the program.

让我们测试一下。

```cpp
#include <iostream>
using namespace std;

int main(){
	int counter = 0;
	cout << counter;
	counter++;
	cout << counter;
	counter++;
	cout << counter;
	counter++;
	cout << counter;
	counter++;
	cout << counter;
	counter++;
	cout << counter;
	return 0;
}
```

Yes, it's a lot less typing, and there are many ways to make it more concise. Here is one way.

是的, 这是一个更少的打字, 有很多方法, 使其更简洁。 这里有一种方法。

```cpp
#include <iostream>
using namespace std;

int main(){
	int counter = 0;
	cout << counter++;
	cout << counter++;
	cout << counter++;
	cout << counter++;
	cout << counter++;
	cout << counter++;
	return 0;
}
```

The answer is still `012345`. The postfix incrementing operator will increment the variable even while it sits inside an expression. Now let's try the prefix version.

答案仍然是“012345”。 后缀增量操作符将增加变量, 即使它位于表达式内。 现在让我们尝试前缀版本。

```cpp
#include <iostream>
using namespace std;

int main(){
	int counter = 0;
	cout << ++counter;
	cout << ++counter;
	cout << ++counter;
	cout << ++counter;
	cout << ++counter;
	cout << ++counter;
	return 0;
}
```

If you got the answer `123456`, that is no mistake! The prefix incrementing operator is different from its postfix sister in this very way. With `counter` initialized as 0, `++counter` would evaluate to 1, while `counter++` would still evaluate to 0 (but an incremented version of `counter` would be left over for later use). The output for the following example is `1112`.

如果你得到答案`123456`, 这是没有错误！ 前缀增量运算符与其后缀姐妹以这种方式不同。 在计数器初始化为0的情况下, “++ counter”将计算为1, 而“counter ++”仍将计算为0（但是计数器的增量版本将留作后来使用）。 以下示例的输出为`1112`。

```cpp
#include <iostream>
using namespace std;

int main(){
	int counter = 0;
	cout << ++counter; // 1: increments before evaluating
	cout << counter;   // 1: has NOT changed.
	cout << counter++; // 1: increments after evaluating
	cout << counter;   // 2: evidence of change.
	return 0;
}
```

For arithmetic completeness, I should mention that the subtractive *decrementing* operator (counter--) also exists. Also, as you might have guessed by now, if one can say `counter + 1`, then a C compiler would also recognize the other classic arithmetic like `counter - 3` (subtraction), `counter * 2` (asterisk is multiplication), `counter / 2` (division), and overriding the order of operations by using parentheses, such as `(counter + 1) / 2` evaluating to a different result than `counter + 1 / 2`. Putting a negative sign before a variable will also do the right thing and negate it, as if it were being subtracted from zero. C extends this basic palette of maths operators with boolean logic and bitwise manipulation; I will introduce them in Variables part 2.

对于算术完备性, 我应该提到减法*减量*运算符（counter--）也存在。 另外, 你现在可能已经猜到, 如果一个人可以说“counter + 1”, 那么C编译器也会识别其他经典算法, 如“counter-3”（减法）, “counter * 2” ）, `counter / 2`（除法）, 并通过使用括号来重写操作的顺序, 例如`（counter + 1）/ 2`计算结果与计数器+ 1/2不同。 在变量之前放一个负号也会做正确的事情, 否定它, 就好像它被从零减去一样。 C使用布尔逻辑和按位操作扩展了数学运算符的基本调色板; 我将在变量第2部分中介绍它们。

There are a few more essentials to learn about variables, but we're going to take what we've learned so far and run with it in the name of fun. In the meantime, give yourself another pat on the back for making it this far! You learned what variables are, and how to perform basic arithmetic on them. You also learned what the ++ operator does when placed before and after a variable name.

还有一些更多的要点来了解变量, 但我们将采取我们迄今为止学到的, 并以乐趣的名义运行它。 在此期间, 给自己另一个拍在背上, 使它这么远！ 你学习了什么变量, 以及如何对它们执行基本算术。 你还学习了当放在变量名之前和之后++操作符的作用。

The C++ language gets its name from being the C language plus one.

C ++语言的名称从C语言加一。

## Conclusion
## 总结

Congratulations on getting through the first few pages of this introduction to C++. With these basic concepts, you should be able to explore plenty far on your own, but I will admit that it is not enough to prepare you to comprehend the code examples in the rest of ofBook. Because of limited paper resources, what you've seen here is a "teaser" chapter for a necessarily lengthier introduction to the C++ language. That version of this chapter got so big that it is now its own book — available unabridged on the web, and possibly in the future as its own companion book alongside ofBook. Teaching the C++ language to non-programmers is indeed a large subject all itself, which could not be effectively condensed to 35 pages, let alone the 100+ page book it grew to be. If you're serious about getting into openFrameworks, I highly recommend you stop and read the unabridged version of this chapter before continuing in ofBook, so that you may understand what you are reading. You will find those materials [here](https://github.com/openframeworks/ofBook/blob/master/chapters/cplusplus_basics/unabridged.md)

恭喜您完成本C ++简介的前几页。有了这些基本概念, 你应该能够自己探索很多, 但我承认, 还不足以让你理解本书其余部分的代码示例。由于纸张资源有限, 您在这里看到的是一个“前导”章节, 用于对C ++语言进行必要的更长的介绍。这个版本的本章变得如此之大, 以至于它现在是自己的书 - 在网络上可以毫无瑕疵地, 可能在未来作为自己的伴随书与书。将C ++语言教给非程序员确实是一个很大的课题, 它本身无法有效地集中到35页, 更不用说100+的页面书。如果你认真进入openFrameworks, 我强烈建议你在继续读书之前停止阅读本章的未修订版本, 这样你就可以理解你正在阅读的内容。你会发现这些材料[这里]（https://github.com/openframeworks/ofBook/blob/master/chapters/cplusplus_basics/unabridged.md）

## PS.
## 附上.

Stopping the chapter here is by no means intended to separate what is important to learn about C++ from what is not important. We have simply run out of paper. In lieu of how important the rest of this intro to C++ is, and based on ofZach's teaching experience, here is more of what you'll find in the unabridged version:

停止这里的章节绝不是意在分离什么是重要的, 了解C ++从什么不重要。 我们只是用完了纸。 代替这个介绍C ++的其余部分是多么重要, 并基于Zach的教学经验, 这里更多的是你会发现在未删节的版本：

+ Variables exist for different periods of time - some for a long time, and some for a small blip in your program's lifecycle. This subject of *scope* is covered in the unabridged version of this book, entitled *Variables (part 2)*.
+ 变量存在不同的时间段 - 有些是长时间, 有些是程序的生命周期中的小blip。 *范围*的主题在本书的未减损版本中标题为*变量（第2部分）*。

+ Variables have a *data type*. For example, one holds a number while another holds some text. More about that in *Fundamental Types*.
+ 变量具有*数据类型*。 例如, 一个保存一个数字, 而另一个保存一些文本。 更多关于*基本类型*。

+ It's important to reiterate that unlike Processing, variables do not necessarily start with a zero value. You must initialize them with your desired value, and otherwise there's no telling what will be waiting there for you. You'll find additional discussion of this phenomenon in the introduction to arrays.
+ 重要的是要重申, 与Processing不同, 变量不一定从零开始。 你必须用你想要的值初始化他们, 否则没有告诉你会在那里等你。 你将在数组的介绍中找到关于这种现象的更多讨论。

> The best way to predict your future is to create it.
>
> **--Abraham Lincoln**
> 预测你未来的最佳方式就是去创造它。
> 
> **--Abraham Lincoln**

