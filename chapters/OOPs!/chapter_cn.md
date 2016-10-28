# Ooops! = 面向对象编程 + 类

## 概述

本教程是关于 openFrameworks 中面向对象编程的一个快速和实用介绍, 也是关于如何构建和使用自己的类的指南。到本章结束, 你应该明白如何创建自己的对象, 并有很多球跳跃在你的屏幕上！

![balls screenshot](images/balls_screenshot.png "balls screenshot")

## 什么是面向对象编程

面向对象编程 (OOP)是用于使用对象及其交互的编程范例。你可以假设一个“类”作为一个 cookie 切割器, 可以创建许多 cookie, “对象”。

OOP 中常常使用的以下的术语和定义:

- 类定义了一个东西的特征 - 对象及其行为, 它不仅定义其属性和属性, 还定义它可以做什么。

- 对象是类的实例。

- 方法是对象的用处。

##如何构建自己的类 (简单的类)

类和对象是面向对象编程的基本部分。烹饪, 或者编码都非常有趣, 我们倾向于在厨房做实验, 让我们继续把一个饼干切割器的经典比喻作为一个类, 定义它的交互, 能力和可用性, 以及将饼干作为对象。

每个类都有两个文件：头文件, 就是以'.h'结尾的声明文件和一个以'.cpp'结尾的实现文件。打个非常简单的比方, 这把头文件 (.h)比作一个配方, 就是你的 cookie 的主要成分的列表。实现文件 (.cpp)是我们将要做的工作, 如何混合和工作, 做出完美的饼干！

那就让让我们来看看它是如何工作的：

首先让我们创建两个类文件：

如果你使用 Xcode 作为你的 IDE (它代表：集成开发环境), 选择src文件夹, 左点击 (或 CTRL + 点击), 在弹出菜单上选择'新文件', 你会被采取到一个新的窗口菜单, 选择你正在开发的适当的平台 (OS X 或 iOS), 选择 C++ 类, 最后选择一个名称 (我们使用'球')。你会自动看到 'src' 文件夹中的两个文件：'Ball.h' 和 'Ball.cpp'。

如果你使用 Code::Blocks 在 “examples” 目录中创建一个来自空的新项目 (或者检查 ProjectGenerator)。复制文件夹 “empty” 并将其重命名为 “OOP”。切换到这个新目录, 复制 “emptyExample” 并将其重命名为 “ball1”。在 “ball1” 目录中, 将 “emptyExample.workspace” 重命名为 “ball1.workspace”, 将 “emptyExample.cbp” 重命名为 “ball1.cbp”。现在你有一个专用的目录和一个专门的项目了。用 Code::Blocks 打开 “ball1.cbp”, 右键单击 “emptyExample” 工作区, 选择“属性” (列表中的最后一个条目)并更改项目的标题。项目中的 “src” 目录包含本章需要编辑的所有文件。通过使用 '文件' -> '新' -> '空文件'或按 Tab + Ctrl + N, 在 'src' 目录中添加两个新文件。一个文件应命名为 “Ball.h”, 另一个文件应为 “Ball.cpp”。

现在让我们编辑你的类头文件 (.h)。随意删除其所有内容, 让我们从头开始：

在头文件 (.h)中声明一个类。在这种情况下, 文件名应为 Ball.h.
按照下面的代码, 并输入到你自己的 Ball.h 文件, 请注意我的注释, 它可以帮助你理解代码。


```cpp
#ifndef _BALL // 如果这个类没有被定义, 程序可以定义它
#define _BALL // 通过使用这个if语句, 防止类被多次调用, 否则会混淆编译器
#include "ofMain.h" // 我们需要包含这个来引用openFrameworks框架
类

class Ball{
    
    public: // 在这里放置公共函数和变量声明
    
    //方法, 等同于你创建的类对象的具体函数
    void setup();  // setup() 方法, 使用它来设置对象的初始状态
    void update(); // update() 方法, 用它来刷新你的对象属性
    void draw();   // draw() 方法, 你将在这里做对象的绘制
    
    // 变量
    float x;       // 位置
    float y;
    float speedY;  // 速度和方向
    float speedX;
    int dim;       // 大小
    ofColor color; // 使用 ofColor 类的 color
    
    Ball();        // constructor 用于初始化对象, 如果没有传递属性, 程序会将它们设置为默认值
    
    private:       // 这里放置私有函数或变量声明
};// 别忘了分号

#endif
```

我们已经生命力 Ball 类的头文件 (成分列表), 现在让我们到烹饪部分看看这些成分可以做什么！

请注意 `#include` 标记。这是一种告诉 [编译器](http://www.cplusplus.com/doc/tutorial/introduction/ "Compiler introduction on cplusplus.com") ([维基百科](https://en.wikipedia.org/wiki/Compiler "Wikipedia on compilers")) 关于要包括在实现文件中的任何文件。当程序被编译时, 这些 `#include` 标签将被它们所引用的原始文件所替换。

'if 语句' (#ifndef)是一种防止可能出现的头文件重复的方法。这被称为 [include guard / 包含守卫](https://en.wikipedia.org/wiki/Include_guard "Wikipedia on include guards")。使用此模式有助于编译器只包含文件一次, 并避免重复。但是我们现在不用担心, 我们以后会再提到。

我们现在将为一个球对象创建一个类。这个球将具有颜色, 速度和方向属性, 它将移动穿过屏幕和反弹。我们将使用随机属性创建这些属性中的一些, 但是我们会小心地为其运动行为创建正确的逻辑。

在这里教你如何编写类 *.cpp 文件, 实现文件：

```cpp
#include "Ball.h"
Ball::Ball(){
}

void Ball::setup(){
    x = ofRandom(0, ofGetWidth());      // 创建一些随机的位置
    y = ofRandom(0, ofGetHeight());
    
    speedX = ofRandom(-1, 1);           // 创建随机速度与方向
    speedY = ofRandom(-1, 1);
    
    dim = 20;
    
    //定义数字颜色的一种方法是通过以0-255的值单独寻址其3个组件 (红色, 绿色, 蓝色), 在此示例中, 我们将每个组件设置为随机值
    color.set(ofRandom(255), ofRandom(255), ofRandom(255));
    
}

void Ball::update(){
    if (x < 0) {
        x = 0;
        speedX *= -1;
    } else if (x > ofGetWidth()){
        x = ofGetWidth();
        speedX *= -1;
    }
    
    if (y < 0) {
        y = 0;
        speedY *= -1;
    } else if (y > ofGetHeight()){
        y = ofGetHeight();
        speedY *= -1;
    }
    
    x += speedX;
    y += speedY;
}

void Ball::draw(){
    ofSetColor(color);
    ofDrawCircle(x, y, dim);
}
```

现在, 这是一个简单的程序, 我们可以把它写在我们的 .App (.h 和 .cpp)文件中, 如果我们想在其他地方重用这个代码, 这将是非常有意义的。面向对象编程的优点之一是重用。想象一下, 我们要创造数千个这样的球。如果没有 OOP 的话代码可以很容易混乱。

通过创建我们自己的类, 我们可以重新创建尽可能多的对象, 并在需要时调用适当的方法, 保持我们的代码干净, 高效。在一个更实际的例子中, 想到为每个用户界面 (UI)元素 (按钮, 滑块等)创建一个类, 然后将它们部署到你的程序中, 而且在将来包含和重用它们程式。

## 从你的类中创建一个对象

现在我们已经创建了一个类, 接下来让我们做出真正的对象！在你的 ofApp.h (头文件)中, 我们必须声明一个新的对象, 但首先我们需要在我们的程序中包含 (或给出指令)你的 Ball 类。为此, 我们需要写：

```cpp
#include "Ball.h"
```

在你的 ofApp.h 文件的顶部。然后我们可以在我们的程序中最终声明一个类的实例。在 `ofApp` 类中添加以下行, 就在最后的 “};” 之上。

```cpp
Ball myBall;
```

现在让我们的球跳跃在屏幕上！转到你的项目 ofApp.cpp (实现)文件。我们已经创建了对象, 我们只需要设置它, 然后更新它的值并通过调用它的方法来绘制它。

在 ofApp.cpp 的 `setup()` 函数中添加以下代码：

```cpp
myBall.setup(); // 调用对象的设置方法
```

在 `update()` 函数中添加：

```cpp
myBall.update() //调用对象的update方法
```

并在 `draw()` 函数中添加：

```cpp
myBall.draw(); // 调用 draw 方法来绘制对象
```

编译并运行！这时, 你应该看到一个弹跳的球！完美！


## 从你的类中创建对象

现在, 你可能问自己为什么你去这么多的麻烦, 以创建一个弹跳的球。你可以做到这一点 (并且可能已经做过了), 并不需要使用类。事实上, 使用类的一个优点是能够创建具有相同特征的多个单独的对象。所以, 让我们现在试试看！回到你的 ofApp.h 文件并创建几个新对象：


```cpp
Ball myBall1;
Ball myBall2;
Ball myBall3;
```

在实现文件 (ofApp.cpp) 中, 为每个对象调用相应的方法
在 ofApp 的 `setup()` 函数中：

```cpp
myBall1.setup();
myBall2.setup();
myBall3.setup();
```

在ofApp的 `update()` 函数中：

```cpp
myBall1.update();
myBall2.update();
myBall3.update();
```

在 `draw()` 函数中：

```cpp
myBall1.draw();
myBall2.draw();
myBall3.draw();
```

## 从你的类中创建更多的对象

我们刚刚创建了3个对象, 但是你可能已经看到了, 要手动创建 10, 100 或者 1000 的对象是多么乏味。手动一个一个将是一个漫长而痛苦的过程, 但是通过自动化对象创建和函数调用, 可以很容易地解决这个问题。只需使用一对复循环, 就可以让这个过程更简单和更清洁。而不是一个一个地声明一个对象的列表, 我们将创建一个类型 `Ball` 的对象数组。

我们还将介绍另一个新元素：常量。常量就是在任何 #include 之后设置常量作为 #define CONSTANT_NAME 的值。这是一种设置在程序中不会改变的值的方法。
在 ofApp 类头文件中, 就在你定义球对象的地方, 你还可以在这里定义我们将用于对象数量的常量：

```cpp
#define NBALLS 10
```

我们现在将使用常量 NBALLS 的值来定义我们的对象数组的大小：

```cpp
Ball myBall[NBALLS];
```

数组是相同类型的项目的索引列表。索引用于访问列表中的特定项目。这个索引通常从 0 开始, 所以第一个 `Ball` (object) 在 myBall[0] 中找到。

只有少数编程语言使用 1 作为开始数组的索引。如果尝试访问无效的索引 (大于数组的大小或负数), 你会得到一个错误。有关数组的更多信息, 请查看 “C++ 基础” 一章。在我们的实现文件中, 我们创建一个对象数组, 并通过 'for' 循环调用它们的方法。

在 `setup()` 函数中添加：

```cpp
for(int i=0; i<NBALLS; i++){
    myBall[i].setup();
}
```

在 `update()` 函数中添加：

```cpp
for(int i=0; i<NBALLS; i++){
    myBall[i].update();
}
```

在 `draw()` 函数中添加：

```cpp
for(int i=0; i<NBALLS; i++){
    myBall[i].draw();
}
```

通过使用 for 循环, 为 `myBall` 数组中的每个 `Ball` 对象调用 `setup ()`, `update ()`和 `draw ()`方法, 没有对象必须手动触摸。

## 从你的类中创建更多的对象：属性和构造函数

如我们所见, 每个对象都有一组由其变量 (位置, 速度, 方向和维度)定义的属性。面向对象编程的另一个优点是所创建的对象可以对它们的每个属性赋予不同的值。为了让我们更好地控制每个对象, 我们有一个方法, 允许我们定义这些特性, 并让我们访问它们。因为我们希望在创建对象后立即执行此操作, 所以让我们在名为 `setup ()` 的方法中执行此操作。我们将修改它来传递一些对象属性, 让我们说它的位置和维度。首先让我们在球定义文件 (*.h) 中做些改动：

```cpp
void setup(float _x, float _y, int _dim);
```

我们需要更新Ball实现 (*.cpp) 文件以反映这些更改。

```cpp
void Ball::setup(float _x, float _y, int _dim){
    x = _x;
    y = _y;
    dim = _dim;

    speedX = ofRandom(-1, 1);
    speedY = ofRandom(-1, 1);
}
```

你的 Ball.cpp 文件应该如下所示：

```cpp
#include "Ball.h"

Ball::Ball(){
};

void Ball::setup(float _x, float _y, int _dim){
    x = _x;
    y = _y;
    dim = _dim;

    speedX = ofRandom(-1, 1);
    speedY = ofRandom(-1, 1);

    color.set(ofRandom(255), ofRandom(255), ofRandom(255));
}

void Ball::update(){
    if(x < 0 ){
        x = 0;
        speedX *= -1;
    } else if(x > ofGetWidth()){
        x = ofGetWidth();
    speedX *= -1;
    }

    if(y < 0 ){
        y = 0;
        speedY *= -1;
    } else if(y > ofGetHeight()){
        y = ofGetHeight();
        speedY *= -1;
    }

    x+=speedX;
    y+=speedY;
}

void Ball::draw(){
    ofSetColor(color);
    ofCircle(x, y, dim);
}
```

现在在 ofApp.cpp 文件中, 我们需要在我们启动应用程序时立即运行这个新实现的方法, 以便在每个对象创建时反映不同的设置。所以, 在 `ofApp::setup()` 做些更改:

```cpp
for (int i = 0 ; i < NBALLS; i++) {
	//根据它在数组中的位置定义每个球的大小
	int size = (i+1) * 10;
	//生成大于0且小于应用程序屏幕宽度的随机值
	int randomX = ofRandom(0, ofGetWidth());
	//生成大于0并小于我们的应用程序屏幕高度的随机值
	int randomY = ofRandom(0, ofGetHeight());
	     
	myBall[i].setup(randomX, randomY, size);
}
```

如你所见, 现在可以直接控制对象属性的创建。现在, 我们只需要使用上面的for循环, 通过球更新并在相应的函数中绘制它们。

```cpp
myBall[i].update();

myBall[i].draw();
```

## 快速创建对象

虽然很多时候你已经有了预定义数量的对象, 你需要创建和使用数组是正确的选择, 还有其他方法来创建多个对象, 提供其他优点: 欢迎向量登场!

向量是非常伟大的, 因为他们允许创建对象的集合, 不会限制预定义数量的元素。它们是相当动态的, 允许你在运行时添加对象 (例如, 当你的程序运行时), 但是当你需要更多的对象时, 也可以删除它们。它们相当于有弹性的数组。

所以, 让我们使用他们！
注意：在这本书中, 你会听到两种不同类型的向量。请不要混淆 stl::vectors (我们讨论的弹性数组类型) 和数学向量 (例如: 力)。

要了解有关 stl::vector的更多信息, 请查看“ C++基础 ”一章或以下简短的在线教程：[http://www.openframeworks.cc/tutorials/c++%20concepts/001_stl_vectors_basic.html](http://www.openframeworks.cc/tutorials/c++%20concepts/001_stl_vectors_basic.html)

回到我们所爱的 ofApp.h 文件, 让我们定义一个 `Ball` 对象的向量：

```cpp
vector <Ball> myBall;
```

在这个表达式中, 我们创建一个类型 (向量) 的类型 (球指针), 并命名为myBall。
现在, 让我们来到我们的 (.cpp), 开始烹饪！
忽略 ofApp 中的 `setup ()`, `update ()` 和 `draw ()` 方法, 让我们跳转到 `ofApp::mouseDragged (...)` 方法。这种方法不断地监听鼠标拖动动作, 如果它改变了它显示它的值 (位置和按钮状态) 给我们。

```cpp
void ofApp::mouseDragged(int x, int y, int button){
}
```

在这种方法中, 我们正在监听鼠标的拖动活动, 我们将使用它来创建交互！因此, 让我们创建一些代码来创建 `Ball`s, 并在我们拖动鼠标时将它们添加到我们的程序中。
你的鼠标或触控板的拖动活动是无处不在的, 简单但也是非常直觉的数据源, 我们将使用这种简单的接口来创建交互！让我们添加一些代码来创建 `Ball`s, 并在我们拖动鼠标时将它们添加到我们的程序中。

```cpp
void ofApp::mouseDragged(int x, int y, int button){
    Ball tempBall;									// create the ball object
    tempBall.setup(x,y, ofRandom(10,40));			// setup its initial state
    myBall.push_back(tempBall);						// add it to the vector
}
```

我们的代码中的一些新东西：我们从声明一个临时对象开始, 把它当作是一个真实对象的占位符 - 这将是在向量内！ - 我们通过将 `x` 和 `y` 鼠标拖动坐标赋值给它的设置变量来定义它的初始属性。然后, 我们使用这个临时对象作为占位符来添加 `Ball` 对象到我们的向量。

回到我们的更新和绘制方法。我们可以添加所需的 “for循环” 来遍历向量中的对象来更新和绘制它们, 就像我们对数组一样。这一次, 虽然我们没有声明一个变量存储最大数量的对象, 而是向量对象提供了一个方便的方法, 我们可以调用来知道它们的大小 (`size ()`)。

请参见下面关于 `update ()` 的代码。

```cpp
for (int i = 0; i<myBall.size(); i++) {
    myBall[i].update();
}
```

以及 `draw()`：

```cpp
for (int i = 0 ; i<myBall.size(); i++) {
    myBall[i].draw();
}
```

现在 'for' 循环会遍历向量中的所有对象, 而不需要事先指定项目的确切数量。这都归功于 `size()` 函数。

## 使用向量来创建和删除

如果你运行前面的代码, 你会看到, 在很短的时间内, 你会创建大量的球, 但在某些时候, 你的系统可能会变得迟钝, 因为在屏幕上只能装下这么多的对象。正如我们刚刚提到的向量是非常特殊的, 因为我们可以动态添加和删除元素。这是他们的魔力：向量是弹性的！

所以, 在我们必须实现一个方法删除它们。

在 `ofApp :: MousePressed (...)` 调用中, 我们将循环遍历我们的向量, 并检查鼠标的坐标与特定 `Ball` 位置之间的距离。如果这个距离小于'Ball'半径, 那么我们知道我们点击它, 我们可以删除它。因为我们使用 `vector.erase (...)` 方法, 我们需要使用一个迭代器 ( `myBall.begin ()` )。迭代器指向一个更大的包含组中的一些元素, 并且能够遍历该范围的元素。将它们视为路径或链接。在这种情况下, 它们是一个快捷方式, 引用向量的第一个元素作为访问我们真正想要擦除的向量元素 (`i`)的开始点, 因此 `myBall.begin()+i`。


```cpp
for (int i =0; i < myBall.size(); i++) {
	  // ofDist 方法让我们检测两个坐标之间的距离
    float distance = ofDist(x,y, myBall[i].x, myBall[i].y);
    if (distance < myBall[i].dim) {
			// 我们需要使用迭代器来引用我们需要删除的向量
        myBall.erase(myBall.begin()+i);
    }
}
```

但总有一个时间点, 你可能想摧毁所有的向量, 这里也有一个非常方便的方法来帮助你: `clear()`。
随意尝试, 尝试自己使用它！

```cpp
balls.clear();
```

## 多态性简介 (继承)

你现在已经发现了 OOP 的力量：创建一个类, 并从中创建尽可能多的对象, 根据应用程序的需要添加和删除。现在, 让我们回到我们的烹饪比喻, 想象你的饼干, 即使共享相同的饼干和面团, 也可以使用一些不同的变化, 以添加一些不用的变化, 比如 cookie 罐的选择！这也是 OOP 和继承的力量。它允许我们使用一个基类并添加一些特定的行为, 覆盖一个类的一些行为, 创建一个具有稍微不同行为的实例/对象的子集。这个伟大的事情是它的可重用性。我们使用父类作为起点, 使用它的所有功能, 但我们覆盖其中的一个方法, 给它更多的灵活性。回到我们的 `Ball` 类的初始版本, 我们将基于它的主要特征 (运动行为和形状)构建一些子类, 但是我们将在它的绘制方法中使用不同的颜色来区分每个继承的子类。

你的头文件应如下所示：

```cpp
#ifndef _BALL // if this class hasn't been defined, the program can define it
#define _BALL // by using this if statement you prevent the class to be called more than once which would         confuse the compiler
#include "ofMain.h"


class Ball {
    public: // place public functions or variables declarations here

    void setup();
    void update();
    void draw();

    // variables
    float x;
    float y;
    float speedY;
    float speedX;
    int dim;

    ofColor color;

    Ball();

    private:
};
#endif
```

让我们对实现文件进行一些细微的修改。允许将随机尺寸的最小值和最大值更改为较大值, 并将位置设置为屏幕中心。使源代码如下所示：

```cpp
#include "Ball.h"

Ball::Ball(){
}

void Ball::setup(){

    x = ofGetWidth()*.5;
    y = ofGetHeight()*.5;
    dim = ofRandom(200,250);

    speedX = ofRandom(-1, 1);
    speedY = ofRandom(-1, 1);

    color.set(ofRandom(255), ofRandom(255), ofRandom(255));
}
```

我们可以保留 `update()` 和 `draw()` 函数。
现在, 让我们开始创建这个父类的子版本。
创建一个新的类文件集, 并命名为 `BallBlue`。请随意复制下面的代码。
它的'.h'应该看起来像这样：

```cpp
#pragma once                // 另一种更现代的方法来防止编译器多次包含此文件
#include "ofMain.h"
#include "Ball.h"            // 我们需要包括父类, 编译器将包括母/基类, 所以我们可以访问所有的方法并继承

class BallBlue : public Ball {     // 设置类继承自'Ball'
    public:
        virtual void draw();             // 这是我们想要与父类不同的唯一方法
};
```

在 '.cpp' 文件中, 我们需要指定我们想要的新的 `draw ()` 方法的行为与父类中的行为不同。

```cpp
#include "BallBlue.h"


void BallBlue::draw(){
    ofSetColor(ofColor::blue);    // 这是设置蓝色的快捷方式
    ofCircle(x, y, dim);    
}
```

现在你可以试着创建两个新类： `BallRed` 和 `BallGreen` 基于 `Ball` 类, 如 `BallBlue`。
回到你的 'ofApp.h'。包括新建的类, 并在你的 'ofApp.cpp' 文件中创建每个类的一个实例。初始化它们并调用他们的 `update ()` 和 `draw ()` 方法。一个快速的窍门！在调用 `draw ()` 方法之前, 调用：

```cpp
ofEnableBlendMode(OF_BLENDMODE_ADD);
```

这会让你的应用程序绘制方法具有添加混合模式。关于这个, 可以看 “图形” 一章。

希望你喜欢这个简短的教程！

玩的开心！

