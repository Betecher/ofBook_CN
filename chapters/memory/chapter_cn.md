# C++ 的内存管理

*作者 [Arturo Castro](http://arturocastro.net)*

正确地使用内存是使用 c++ 最棘手的部分之一。与其他语言, 如 Java, Python 和任何自带“垃圾回收”机制的语言的主要区别是, 在 c++ 中, 我们可以可视化地保留和释放内存, 而那些称为垃圾收集器的元素能为我们所用。

还有一个重要的区别, 那就是在 c++ 中我们有两个不同的内存区域, 堆和栈, 如果你过去只用过 Processing, Java 或 Python 等, 那你可能只使用过堆内存。

我们稍后会看到主要的区别是什么, 现在先让我们了解一下什么是内存, 还有当我们创建变量时程序会发生什么。

## 计算机内存和变量

至少这可以让你在高围度上了解计算机的内存工作原理。

计算机具有不同类型的存储器, 在本节中, 我们将讨论 RAM (随机存取内存)内存。这是一种计算机存储任意时刻执行的程序指令和正在使用的数据的内存类型。

你的计算机可能有 4Gb 的 RAM, 在 c++ 里我们可以访问大部分的内存, 而访问它所要做的就是创建变量。内存以字节为单位, 这是我们通常在 c++ 应用程序中使用的最小内存大小。每个数据类型, 如 char, int, float ...都有不同的大小, 所有都以字节为单位。这些尺寸对于不同的平台可能是不同的, 但​​最常见的是：

- char:  1 byte / 1 字节
- short: 2 bytes / 2 字节
- int:   4 bytes / 4 字节
- float  4 bytes / 4 字节
- double 8 bytes / 8 字节

其他类型可能具有可变的大小, 这取决于它们的内容, 例如数组或字符串。

例如, 当我们创建一个变量时: 

```cpp
int i;
```

我们正在做的是保留 4Gb 中的4个字节 来存储一个 int, 但我们存储 int 或其他类型的数据都不必在意, int, char, float 或者 string 只是大小不同, 但内存的类型总是相同的。

在内部, 计算机不会知道那个内存区域为'i', 而是标记内存地址。内存地址只是一个指向 4Gb 内存中特定字节的数字。

当我们创建一个变量 `int i` 时, 我们告诉程序保留4个字节的内存, 将这4个字节的第一个字节的地址与变量名称 `i` 相关联, 并限制这4个字节的数据类型存储只能是 ints。

![Int i](images/int_i.svg)

通常内存地址以[十六进制](http://en.wikipedia.org/wiki/Hexadecimal)表示。在 c++ 中, 可以通过使用 `＆` 运算符来获取变量的内存地址, 如：

```cpp
cout << &i << endl;
```

该 cout 的输出是我们刚刚创建的变量 `i` 的第一个字节的内存地址。

然后, 当我们为该变量赋值时, 发生的是我们通过声明变量来将该值存储在我们刚刚保留的内存区域, 所以当我们这样做时：

```cpp
i = 0;
```

我们的内存将看起来像这样：

![Int i equals 0](images/int_i_equals_0.svg)

形成int的字节在内存中的顺序取决于我们的计算机的体系结构, 你可以看到[小尾数和大尾数](http://en.wikipedia.org/wiki/Endianness)。这些术语指的是数据类型的字节如何在存储器中排序, 如果最有意义的字节首先或最后。大多数时候, 我们不需要知道这个顺序, 但大多数现代计算机体系结构使用的是小尾数。

如果你使用 c++ 一段时间, 你可能因为差劲的内存访问程序已经崩溃过了。通常你会看到的消息就像 `segmentation fault ...`。这是什么意思？

当您在程序中创建变量时, 即使在 c++ 中, 出于安全考虑, 您无法真正访问计算机中的所有内存。想象一下, 你的银行帐户在浏览器中打开, 如果任何程序可以访问计算机中的所有内存的话, 恶意应用程序就能访问浏览器的内存, 并获得该信息, 甚至修改它。为了规避风险, 操作系统为每个程序分配了内存块。当你的应用程序启动时, 它被分配一个 `segment` 的内存。后来当你创建变量, 如果有足够的内存, 那么就在你的 `segment` 里创建变量。当该段中没有更多可用内存时, 操作系统为应用程序分配一个新的内存, 应用程序开始使用该内存。如果尝试访问不属于该应用程序的内存地址, 操作系统只会终止该应用程序, 以避免可能的安全风险。

这是怎么发生的？大多数时候, 你只是不试图通过他们的数字访问内存地址, 所以有时你可能试图访问一个变量, 却得到一个分段的错误。大多数情况下, 这是因为你试图访问一个不再存在的变量, 通常是因为你存储了一个指针到一个内存区域, 然后释放或移动该内存到其他地方。我们稍后会详细讨论这个问题

## 堆栈变量, 函数中的变量与对象中的变量

正如我们在本章开头所说的那样, 在 c++ 中有两种类型的内存：栈 (stack) 和堆 (heap)。让我们先谈谈栈, 因为这是最容易使用的内存类型, 也是 OpenFrameworks 中更频繁使用的内存。

栈是在函数内部或在类的 .h 中创建变量时使用的内存类型, 只要不使用指针和关键字 new 即可。

它被称为栈, 因为它的组织像一个 [stack] (http://en.wikipedia.org/wiki/Stack_%28abstract_data_type%29)。在我们的应用程序中, 我们调用一个函数, 在内存中有一个区域分配给该函数 **调用** / **call**。那个特定的函数**调用** / **call**, 只有它, 在它持续的时间可以在那个区域创建变量。

当函数调用结束时, 这些变量消失。所以你不能做下列操作：

```cpp
void ofApp::setup(){
    int a = 0;
}

void ofApp::update(){
    a = 5; // 错误：a 不存在于 setup() {} 外部
}
```

另外, 因为我们谈论的是函数调用, 所以不能在栈变量中存储一个值, 并且期望它在再次调用函数时存在。

一般来说, 我们可以说变量存在于它们已经被定义的块中, c++ 中的块由定义了变量的 `{}` 来定义, 例如, 这样做也不可行：

```cpp
for (int i=0;i<10;i++){
    int a = 5;
}
cout << a << endl; // 错误: a 不存在于 for {} 之外
```

我们甚至可以这样做：

```cpp
void ofApp::setup(){
   {
        int a = 0;
        // do something with a
   }

   {
        int a = 0;
        // do something with a
   }
}
```

这不是很常见, 但有时用于定义函数内部变量的寿命, 主要是当该变量是持有资源的对象, 并且我们只想保持它们一个特定的持续时间。

变量的生命周期称为 `scope`。

除了在函数中创建变量外, 我们还可以在 .h 中的类声明中创建变量：

```cpp
class Ball{
public:
    void setup();

    float pos_x;
}
```

这些变量称为`实例变量` / `instance variables`, 因为我们的类的每个实例或对象都会得到一个副本。它们的行为方式或多或少与在函数中创建的栈变量相同。它们在定义它们的 {} 期间存在, 在这种情况下是它们所属的对象是 {}。

这些变量可以从类中的任何地方访问, 所以当我们需要可以从这个类的对象中的任何函数访问的数据时, 我们是这样创建的。

堆栈中的内存是有限的, 精确的大小, 取决于我们的计算机的架构, 操作系统, 甚至我们使用的编译器。在某些系统中, 它甚至可以在应用程序的运行期间更改, 但大多数时候我们不会触碰到这个限制。

即使我们在堆栈中创建所有的变量, 通常消耗大多数内存的对象, 例如一个向量或者在 openFrameworks 中的东西像是像素, 将创建大部分的内存在堆内部使用, 它只受限于的计算机的内存, 所以我们不需要担心它。

我们将在下面的章节中看到堆是如何工作的, 以及使用堆或堆栈的优点。


## 指针和引用

在谈论堆内存之前, 我们来看看指针和引用在 C++ 中是如何工作的, 当我们创建一个指针或引用时, 它们的语法和内存究竟发生了什么。

正如我们以前看到的, 我们可以通过执行以下操作获取变量的地址：

```cpp
cout << &i << endl;
```

这将给我们该变量使用的第一个字节的内存地址, 无论它的类型。当我们将该内存地址存储在另一个变量中, 这就是 c++ 中调用的指针。语法是：

```cpp
int i = 0;
int * a = &i;
```

我们在内存中得到的是：

![Pointer](images/pointer.svg)

指针通常占用4或8个字节 (取决于我们是在32或64位的应用程序), 我们将它表示为1个字节, 只是为了更容易理解, 但是你可以看到它只是另一个变量, 而不是包含值包含指向值的内存地址。这就是为什么它被称为指针。

指针可以指向堆或栈内存。

现在, 让我们详细的说一下在使用 c++ 编程时非常重要的一点。正如我们所见到的, 当我们声明一个变量时：

```cpp
int i;
```

我们得到了一个这样的内存布局:

![Int i](images/int_i.svg)

正如我们所看到的那样, 内存区域没有值。在其他语言中 (如 processing) 输入下列代码：

```java
int i;
println(i);
```

是不可行的, 编译器会告诉我们, 我们试图使用一个未初始化的变量。在 c++ 则不然, 这是完全合法的, 但该变量的内容是未定义的。大多数时候, 我们将得到0, 因为操作系统会清除内存, 然后再将其分配给我们的程序, 也是为了安全。但是如果我们重用了我们已经分配的内存, 那么该内存区域将包含任何内容, 并且我们的程序的结果将是未定义的。

例如, 如果我们有一个变量定义了我们将要绘制的东西的位置, 未能初始化它将导致该对象被绘制在任何地方。

大多数对象都有默认构造函数, 它们的值将被初始化为0, 因此在这种情况下的对象, 通常不需要给它们赋值。

当我们使用未初始化的指针时会发生什么？嗯, 因为指针包含一个内存地址, 如果我们在该内存区域获得的值指向一个不属于我们的程序的地址, 尝试检索或修改存储在该地址的值, 操作系统将终止我们的应用程序与分段故障信号。

回到指针, 我们可以创建一个指针：

```cpp
int i = 5;
int * p = &i;
```

现在, 如果我们尝试使用指针直接就像;

```cpp
cout << p <<< endl;
```

我们将得到的是一个内存地址而不是值5.所以我们如何访问指针指向的值, 我们可以使用相反的运算符 `＆`：因为`＆`给出了变量的地址, `*` 给出了内存地址指向的值, 所以我们可以这样做：

```cpp
cout << *p << endl;
```

我们现在得到的值为5。我们还可以这样：

```cpp
int j = *p;
cout << j << endl;
```

并且将再次获得值5, 因为我们对 j 中的 p 指向的值进行了复制。

`＆` 运算符称为*引用运算符* / *rederence operator*, 因为它提供了对变量及其内存地址的引用。 `*` 运算符是相反的, *解引用运算符* / *dereference operator*, 它给出了指针指向的值, 它引用了一个引用, 一个内存地址, 所以我们可以访问它的值而不是地址。

到目前为止, 我们使用原始值, ints真的, 但行为将是相同的任何其他原始值, 如float, short, char, unsigned int ... 其实在 c++ 中, 对象的处理也是一样的。

如果你习惯 Java, 例如你可能已经注意到, 在 Java 和 C++ 中：

```cpp
int a = 5;
int b = a;
a = 7;
cout << "a: " << a << " b: " << b << endl;
```

将表现相同 (当然要把 cout 改为 java 语言)。就是：`a` 将最终为7, `b` 将为5.当我们使用对象时, c++ 中的行为不同于 Java 的行为。例如, 假设我们有一个类 Ball：

```cpp
class Ball{
public:
    void setup();
    //...

    ofVec2f pos;
}
```

或在 processing 中类似的类;

```java
class Ball{
    void setup();

    PVector pos;
}
```

如果在 c++ 中你写：

```cpp
Ball b1;
b1.pos.set(20,20);
Ball b2;
b2 = b1;
b2.pos.set(30,30);
```

b1 pos 将最终为 20,20 和 b2 30,30, 而如果你在 java 中做相同的操作, b1 和 b2 将具有位置 30,30：

```cpp
Ball b1 = new Ball();
b1.pos.set(20,20);
Ball b2;
b2 = b1;
b2.pos.set(30,30);
```

注意在 Java 的情况下, 我们为第一个球而不是第二个球做了新的改变, 这是因为在 Java 中, 一个对象是堆中的指针, 所以当我们执行 `b2 = b1` 时, 实际上是 b2 成为了对 b1 的引用, 当我们后来改变 b2 时, 我们也改变 b1。

在 c++ 中则相反, 当我们使用 b2 = b1 时, 我们实际上将 b1 的变量的值复制到 b2 中, 所以我们仍然有2个不同的变量而不是引用。当我们修改 b2 时, b1 保持不变。

在两种语言中, `=` 都表示将右侧的值复制到 `=` 左侧的变量中。不同的是, 在 Java 中, 一个对象实际上是一个指向对象的指针, `b1` 或 `b2` 的内容不是对象本身, 而是它的内存地址, 而在 c++ 中 `b1` 实际上包含了对象本身。

下图或多或少能表示内存在 Java 和 C++ 中大致的样子：

![Objects Java C](images/objects_java_c.svg)

正如你可以看到在 c++ 对象在内存中只是他们的成员变量一个接一个。当我们使对象变量等于另一个变量时, 默认情况下, c++ 将所有对象复制到等号运算符的左侧。

如果现在我们有一个类, 会发生什么：

```cpp
class Particle{
public:
    void setup();
    //...

    ofVec2f pos;
    ParticleSystem * parent;
}
```

然后：

```cpp
Particle p1;
Particle p2;
ParticleSystem ps;

p1.pos.set(20,20);
p1.parent = &ps;
p2 = p1;
```

好像在 c++ 之前一样, p2 的内容将复制 p1 的内容, p1 的内容是一个 ofVec2f, 它支持2个浮点数 x 和 y, 然后是一个指向 ParticleSystem 的指针, 这就是被复制的, ParticleSystem 本身不会被复制, 只复制指向它的指针, 所以 p2 最终会有一个 p2 的位置的副本和一个指向同一个粒子系统的指针, 但是我们只有一个粒子系统。

![Object pointers](images/object_pointers.svg)

事实默认情况下就是复制, 并且对象可以存储在栈中永远作为一个指针并有一定的 adavantages。例如, 在 c++ 中, 像我们在上一个示例中使用的粒子的矢量或数组：

```cpp
vector<Particle> particles;
```

在内存中, 所有的粒子将是连续的, 除此之外, 访问它们会比使用指向存储器中的不同位置的指针更快。它还使得将 c++ 向量转换为 openGL 内存结构更加简单, 但这是另一章的主题。

除此之外, 我们需要知道, 当将对象作为参数传递给函数时, c++ 会默认地复制对象。例如：

```cpp
void moveParticle(Particle p){
    p.x += 10;
    p.y += 10;
}

...

Particle p1;
moveParticle(p1);
```

这是完全有效的代码, 但不会有任何效果, 因为函数将接收一个粒子的副本, 并修改该副本, 而不是原来的。

我们可以这样做：

```cpp
Particle moveParticle(Particle p){
    p.x += 10;
    p.y += 10;
    return p;
}
...

Particle p1;
p1 = moveParticle(p1);
```

因此, 我们将一个粒子的副本传递给函数, 函数修改它的值并返回一个修改的副本, 然后我们再次将其复制到 p1 中。看到我们在上一句中提到过多少次复制了吗？编译器会优化其中的一些, 对于小对象, 这是完全可行的, 但想象我们如果有这样的东西：

```cpp
vector<Particle> moveParticles(vector<Particle> ps){
    for(int i=0;i<ps.size();i++){
        ps[i].x += 10;
        ps[i].y += 10;
    }
    return ps;
}
...

vector<Particle> ps;
...
ps = moveParticles(ps);
```

如果我们有100万个粒子将会非常慢, 与 cpu 相比, 内存真的很慢, 所以通常应该避免任何涉及复制内存或分配新内存的内容。那么, 我们可以做什么来避免所有副本呢？

那么我们可以使用指针对吧？

```cpp
void moveParticle(Particle * p){
    p->x += 10;
    p->y += 10;
}
...

Particle p1;
moveParticle(&p1);
```

现在有一些新的东西, 注意如何引用指向一个对象的指针的变量, 而不是使用点, 我们使用 `->` 操作符, 我们要访问一个指向对象的指针, 而不是取消引用它：

```cpp
(*p).x +=10;
```

我们可以用 `->`

```cpp
p->x += 10;
```

所以我们的问题解决了, 使用指针, 而不是传递对象的副本, 我们传递一个引用, 它的内存地址, 所以函数将实际地修改初始值。

这个方法的主要问题是语法很奇怪, 想象如果我们为第二个例子传递一个指针, 一个具有向量的样子：

```cpp
vector<Particle> moveParticles(vector<Particle> ps){
    for(int i=0;i<ps.size();i++){
        ps[i].pos.x += 10;
        ps[i].pos.y += 10;
    }
    return ps;
}
...

vector<Particle> ps;
...
ps = moveParticles(ps);
```

现在函数看起来会是这样：

```cpp
void moveParticles(vector<Particle> * ps){
```

问题是现在我们不能使用 [] 运算符来访问向量中的元素, 因为 ps 不是向量, 而是向量的指针。更重要的是

```cpp
ps[i].x += 10;
```

将实际编译, 但绝大多数肯定会报错, 内存访问错误或分段错误。 ps 现在是一个指针, 当使用指针时, `[]` 的行为就好像我们有一个向量数组！

我们将在关于内存结构的部分中更深入地解释这一点, 但让我们看看如何传递一个没有指针语法的引用。在 c++ 中被称为引用, 它看起来像：

```cpp
void moveParticles(vector<Particle> & ps){
    for(int i=0;i<ps.size();i++){
        ps[i].pos.x += 10;
        ps[i].pos.y += 10;
    }
}

vector<Particle> ps;
...
moveParticles(ps);
```

现在我们传递一个对原始对象的引用, 而不是必须使用指针语法, 我们仍然可以使用它, 就像它是一个普通的对象。

>高级注意：有时候我们想使用引用来避免副本, 但仍然要确保我们传递对象的函数不会修改其内容, 在这种情况下, 建议这样使用 const：

    ofVec2f averagePosition(const vector<Particle> & ps){
        ofVec2f average;
        for(int i=0;i<ps.size();i++){
            average += ps[1].pos;
        }
        return average/float(ps.size());
    }
    vector<Particle> ps;
    ...
    ofVec2f averagePos = averagePosition(ps);


> const 只是使得它不可能修改变量, 即使它是一个引用, 并告诉任何人使用该函数, 他们可以传递他们的数据, 但它不会被改变, 任何修改该函数的人都知道, 应该保持不变和输入, 粒子系统不应该被修改。

在参数之外, 引用有一些特殊的特性。

首先, 一旦引用被创建, 就不能修改引用的内容, 例如我们这样写：

```cpp
ofVec2f & pos = p.pos;
pos.x = 5;
```

但是试图改变引用本身, 如：

```cpp
ofVec2f & pos = p.pos;
pos.x = 5;
pos = p2.pos;  // 错误, 引用只能在其声明上分配
```

也可以返回一个引用, 但根据引用指向它可能是一个坏主意：

```cpp
ofVec2f & averagePosition(const vector<Particle> & ps){
    ofVec2f average;
    for(int i=0;i<ps.size();i++){
        average += ps[1].pos;
    }
    average/=float(ps.size());
    return average;
}
```

这将会实际编译, 但可能会导致一个分段错误在某些时候, 甚至只是工作, 当调用此函数时我们可能会得到奇怪的值。问题是, 我们在栈中创建变量 `平均值` / `average`, 所以当函数返回时, 它将从内存中*删除* / *deleted*, 我们返回的引用将指向一个不再保留的内存区域, 很快, 当它被覆盖, 我们将获得无效的值或指向不属于我们的程序的内存区域的指针。

这是 c++ 中最恼人的问题之一, 它被称为 dangling 指针或引用, 它是由我们有一个指针或引用指向一个内存区域, 但是后来释放的某种方式引起的。

许多的现代编程语言解决这个问题依靠不同的策略, 例如 Java 就不会让这种情况发生, 因为对象一旦超出他们所在范围的最后一个引用就会被删除, 它使用的东西称为垃圾收集器, 不时通过遍历内存寻找无用的对象, 并删除它们。这解决了问题, 但使得很难知道哪些对象将被真正删除。 c++ 在其最新版本中, 还有其他的一些现代语言尝试使用定义对象所有权的新型的指针来解决这个问题, 我们将在本章的最后一节讨论智能指针。

## 堆中的变量

现在我们知道指针的语法和语义让我们看看如何使用堆。堆是所有应用程序共同的内存区域, 任何函数都可以在这个空间创建变量并与其他人共享, 为了使用它, 我们需要一个新的关键字 `new`：

```cpp
Particle * p1 = new Particle;
```

如果你知道 Processing 或 Java 的话, 这看起来有点像它, 对吧？实际上, 这与Java对象完全相同：当我们使用 `new` 时, 我们在堆中创建该变量而不是栈。 `new` 返回指向堆中内存地址的指针, 在 c++ 中, 我们需要指定变量 `p1` 在哪里存储, 就要使用声明中的 `*` 指针。

要访问指向对象的指针的变量或函数, 如我们之前所见, 我们使用 `->` 运算符, 因此我们可以这样做：

```cpp
Particle * p1 = new Particle;
p1->pos.set(20,20);
```

or:

```cpp
Particle * p1 = new Particle;
p1->setup();
```

任何变量的指针都可以声明, 而不需要初始化它：

```cpp
Particle * p1;
p1->setup() // 这将会编译, 但在执行应用程序时会失败
```

我们可以想象堆作为某种全局内存, 而不像栈是本地内存。在堆栈中, 只有拥有它的区块才可以访问它, 而在堆中创建的东西比创建它们的范围更大, 任何函数都可以访问它们, 只要他们有一个引用 (指针)。例如：

```cpp
Particle * createParticle(){
    return new Particle;
}

void modifyParticle(Particle * p){
    p->x += 10;
}

...

Particle * p = createParticle();
modifyParticle(p);
```

`createParticle` 在堆中创建一个新的 `Particle`, 所以即使 createParticle 完成了, `Particle` 仍然存在。我们可以在函数外部使用它, 将引用传递给其他函数...

那么我们怎么能说我们不想再使用那个变量呢？我们使用关键字 `delete`：

```cpp
Particle * p1 = new Particle;
p1->setup();
...
delete p1;
```

这在使用堆时很重要, 如果我们不这样做, 我们将会得到所谓的内存泄漏, 任何人都不会引用的内存, 却仍然在堆中, 当我们的应用程序使用越来越多的内存时, 直到它填充慢我们的计算机中的所有可用内存：

```cpp
void ofApp::draw(){
    Particle * p1 = new Particle;
    p1->setup();
    p1->draw();
}
```

每次我们调用 `draw`时, 它都会创建一个新的粒子, 每当 `draw` 调用完成后, 我们就丢掉了引用 *p1, 但是当函数调用结束时, 我们使用 new 分配的内存不会被释放, 所以我们的程序会慢慢使用越来越多的内存, 可以使用系统监视器检查它。

正如我们前面提到的, 堆栈内存有限, 所以有时我们需要使用堆, 试图在栈中创建100万个粒子可能会导致堆栈溢出。一般来说, 虽然大多数时候在 c++ 中我们不需要使用堆, 至少不是直接, 像vector 类, ofPixels 和其他内存结构允许我们使用堆内存但仍然有堆栈语义, 例如这个：

```cpp
void ofApp::draw(){
    vector<Particle> particles;
    for(int i=0; i<100; i++){
        particles.push_back(Particle())
        particles.back().setup();
        particles.back().draw();
    }
}
```

实际上是使用堆内存, 因为向量在内部使用它, 但是当当前调用绘制完成时, 向量析构函数会在粒子变量超出范围时立即释放该内存。

## 内存结构, 数组和向量

Arrays are the most simple way in c++ to create collections of objects, as any other type in c++ they can also be created in the stack or in the heap. Arrays in the stack have a limitation though, they need to have a predifined size that needs to be specified in its declaration and can't change afterwards:

数组是c ++中创建对象集合的最简单的方法, 和c ++中的任何其他类型一样, 它们也可以在堆栈或堆中创建。堆栈中的数组有一个限制, 但它们需要有一个预定义的大小, 需要在其声明中指定, 并且以后不能更改：

```cpp
int arr[10];
```

the same as with any other type, the previous declaration already reserves memory for 10 ints, we don't need to use new, and that memory will be uninitialized. To access them, as you might know from previous chapters you can just do:

与任何其他类型相同, 前面的声明已经为10个int保留了内存, 我们不需要使用new, 并且该内存将被初始化。要访问它们, 你可能从以前的章节可以知道, 你可以做：

```cpp
int arr[10];
arr[0] = 5;
int a = arr[0];
```

if we try to do:

如果我们试图做：

```cpp
int arr[10];
int a = arr[5];
```

the value of a will be undefined since the memory in the array is not initialized to any value when it's created. Also if we try to do:

a的值将是未定义的, 因为数组中的内存在创建时不会被初始化为任何值。另外, 如果我们试图做：

```cpp
int arr[10];
int a = arr[25];
```

most probably our application will crash if the memory address at arr + 25 is outside the memory that the operating system has assigned to our application.

很可能我们的应用程序将崩溃, 如果arr + 25的内存地址在操作系统分配给我们的应用程序的内存之外。

We've just said arr + 25? what does that mean? As we've seen before a variable is just some place in memory, we can get its memory address which is the first byte that is assigned to that variable in memory. With arrays is pretty much the same, for example since we know that an int occupies 4 bytes in memory, an array of 10 ints will occupy 40 bytes and those bytes are contiguous:

我们刚才说arr + 25？这意味着什么？正如我们在前面看到的一个变量只是内存中的一些地方, 我们可以得到它的内存地址, 这是分配给内存中该变量的第一个字节。与数组几乎是一样的, 例如, 因为我们知道一个int在内存中占用4个字节, 10 int的数组将占用40个字节, 这些字节是连续的：

![Array](images/array "")

Remember that memory addresses are expressed as hexadecimal so 40 == 0x0028. Now to take the address of an array, as with other variable we might want to use the `&` operator and indeed we can do it like:

记住内存地址表示为十六进制, 所以40 == 0x0028。现在取一个数组的地址, 和其他变量一样, 我们可能想使用`＆`运算符, 我们可以这样做：

```cpp
int arr[0];
int * a = &arr[0];
```

That gives us the address of the first element of the array which is indeed that of the array, but with arrays, the same variable is actually a pointer itself:

这给了我们数组的第一个元素的地址确实是数组的地址, 但是对于数组, 同一个变量实际上是一个指针本身：

```cpp
int arr[10];
int * a = &arr[0];
cout << "a: " << a << " arr: " << arr << endl;
```

will print the same value for both a and arr. So an array is just a pointer to a memory address with the only difference that, that memory address is the beginning of reserved memory enough to allocate, in our case, 10 ints. All those ints will be one after another, so when we do `arr[5]` we are just accessing the value that is in the memory address of our array + the size of 5 ints. If our array started in `0x0010`, and ints ocupy `4 bytes`, arr[5] would be `10 + 4 * 5 = 30` which in hexadecimal is `0x001E`. We can actually do this in our code:

将为a和arr打印相同的值。所以一个数组只是一个指向内存地址的指针, 唯一的区别是, 内存地址是足够分配保留内存的开始, 在我们的例子中是10个int。所有这些int将是一个接一个, 所以当我们做'arr [5]', 我们只是访问的值在我们的数组的内存地址+ 5 ints的大小。如果我们的数组在`0x0010`开始, ints ocupy`4字节, arr [5]将是`10 + 4 * 5 = 30`, 十六进制是`0x001E`。我们可以在我们的代码中这样做：

```cpp
int arr[10];
arr[5] = 20;
cout << "&arr[5]: " << &arr[5] << "arr+5: " << arr+5 << endl;
cout << "arr[5]: " << arr[5] << "*(arr+5): " << *(arr+5) << endl;
```

now, that's really weird and most of the time you won't use it, it's called pointer arithmetic. The first cout will print the address in memory of the int at position 5 in the array in the first case using the `&` operator to get the address of `arr[5]` and in the second directly by adding 5 to the first address of `arr` doing `arr+5`. In the second cout we print the value at that memory location, using `arr[5]` or dereferencing the address `arr+5` using the `*` operator.

现在, 这真的很奇怪, 大多数时候你不会使用它, 它被称为指针算术。第一个cout在第一种情况下使用`＆`运算符打印数组中位置5的int的地址, 得到arr [5]的地址, 在第二种情况下, 通过向第一行添加5 `arr`的地址做`arr + 5`。在第二个cout中, 我们使用`arr [5]`或者使用`*`运算符解除引用地址`arr + 5', 在该内存位置打印值。

Note that when we add `5` to the adress of the array it's not bytes we are adding but the size in bytes of the type it contains, in this case `+5` actually means `+20` bytes, you can check it by doing:

注意, 当我们在数组的地址中添加“5”时, 它不是字节数, 而是字节大小, 在这种情况下, '+ 5'实际上意味着+ 20字节, 你可以检查它通过做：

```cpp
int arr[10];
arr[5] = 7;
cout << "arr: " << arr << "arr+5: " << arr+5 << endl;
```

and substracting the hexadecimal values in a calculator. If you try to substract them in your program like:

并在计算器中减去十六进制值。如果你试图在你的程序中减去他们：

```cpp
int arr[10];
arr[5] = 20;
cout << "arr: " << arr << "arr+5: " << arr+5 << endl;
cout << "(arr+5) - arr: " << (arr+5) - arr << endl;
```

You will end up with `5` again because as we've said pointer arithmetic works with the type size not bytes.

你将再次以“5”结束, 因为我们已经说过指针运算的类型大小不是字节。

The syntax of pointer arithmetics is kind of complicated, and the idea of this part wasn't really to show pointer arithmetics itself but how arrays are just a bunch of values one after another in memory, so don't worry if you haven't fully understood the syntax, it's probably something you won't need to use. It is also important to remember that an array variable acts as a pointer so when we refer to it without using the `[]` operator we end up with a memory address not with the values it contains.

指针算术的语法是复杂的, 这个部分的想法并不是真正显示指针算术本身, 而是如何数组只是一堆值在内存中一个接一个, 所以不要担心, 如果你没有完全理解的语法, 这可能是你不需要使用的东西。同样重要的是记住一个数组变量作为一个指针, 所以当我们引用它而不使用`[]“运算符时, 我们最终得到的内存地址不是它包含的值。

The arrays we've created till now are created in the stack so be careful when using big arrays like this cause it might be problematic.

我们到目前为止创建的数组是在堆栈中创建的, 因此在使用大数组时要小心, 因为这可能会有问题。

Arrays in the heap are created like anything else in the heap, by using `new`:

通过使用`new`, 堆中的数组就像堆中的其他任何东西一样被创建：

```cpp
int arr[] = new int[10];
```

or

```cpp
int * arr = new int[10];
```

As you can see this confirms what we've said before, an array variable is just a pointer, when we call `new int[10]` it allocates memory to store 10 integers and returns the memory address of the first byte of the first integer in the array, we can keep it in a pointer like in the second example or using `int arr[]` which declares an array of unkown size.

正如你可以看到的, 这证实了我们之前所说的, 一个数组变量只是一个指针, 当我们调用new int [10]时, 它分配内存来存储10个整数, 并返回第一个字节的内存地址整数, 我们可以将它保存在一个指针, 就像在第二个例子中, 或者使用`int arr []`, 它声明一个未知大小的数组。

The same as other variables created in the heap we'll need to delete this manually so when we are done with it we need to use `delete` to deallocate that memory, in the case of arrays in the heap the syntax is slightly special:

与在堆中创建的其他变量一样, 我们需要手动删除它, 所以当我们完成它, 我们需要使用`delete`来释放内存, 在堆中的数组的情况下, 语法稍微有点特殊：

```cpp
int arr[] = new int[10];
...
delete[] arr;
```

if you fail to use the `[]` when deleting it, it'll only deallocate the first value and you'll end up with a memory leak.

如果你在删除它时没有使用`[]', 它只会释放第一个值, 你会发现内存泄漏。

There's also some problems with the syntax of arrays, for example this:

数组的语法也有一些问题, 例如：

```cpp
int arr[10];
int arrB[10];
arrB = arr;
```

will fail to compile. And this:

将无法编译。和这个：

```cpp
int arr[] = new int[10];
int arrB[] = new int[10];
arrB = arr;
```

will actually compile but as with other variables we are not copying the values that arr points to into arrB but instead the memory address. In this case will end up with 2 pointers pointing to the same memory location, the one that we created when creating arr and lose the memory that we allocated when initializing arrB. Again we have a memory leak, the memory allocated when doing `int arrB[] = new int[10];` is not referenced by any variable anymore so we can't delete it anymore. To copy arrays there's some **c** (not c++) functions like memcpy but their syntax is kind of complex, that's why when working with c++ is recommended to use vectors.

将实际编译, 但与其他变量一样, 我们不是将arr指向的值复制到arrB, 而是将内存地址。在这种情况下, 最终将有2个指向同一内存位置的指针, 这是我们在创建arr时创建的指针, 并丢失了在初始化arrB时分配的内存。再次, 我们有一个内存泄漏, 内存分配时做`int arrB [] = new int [10];`不再被任何变量引用, 所以我们不能删除它了。要复制数组, 有一些** c ** (不是c ++)函数像memcpy, 但它们的语法是复杂的, 这就是为什么使用c ++时推荐使用向量。

C++ vectors are very similar to arrays, indeed their layout in memory is the same as an array, they contain a bunch of values contiguous in memory and always allocated in the heap. The main difference is that we get a nicer syntax and *stack semantics*. To allocate a vector to contain 10 ints we can do:

C ++向量与数组非常相似, 确实它们在内存中的布局与数组相同, 它们包含一堆在内存中连续的值, 并且始终在堆中分配。主要区别是我们得到了更好的语法和*堆栈语义*。要分配一个包含10个int的向量, 我们可以这样做：

```cpp
vector<int> vec(10);
```

We can even give an initial value to those 10 ints in the initialization like:

我们甚至可以在初始化时给这10个int赋初始值：

```cpp
vector<int> vec(10,0);
```

And for example copying a vector into another, works as expected:

例如将一个向量复制到另一个, 工作原理：

```cpp
vector<int> vec(10,0);
vector<int> vecB = vec;
```

Will create a copy of the contents of vec in vecB. Also even if the memory that the vector uses is in the heap, when a vector goes out of scope, when the block in which it was declared ends, the vector is destroyed cause the vector itself is created in the stack, so going out of scope, triggers its destructor that takes care of deleting the memory that it has created in the heap:

将在vecB中创建vec的内容的副本。即使向量使用的内存在堆中, 当向量超出范围时, 当它被声明的块结束时, 向量被销毁, 因为向量本身是在堆栈中创建的, 所以离开范围, 触发它的析构函数, 它负责删除它在堆中创建的内存：

```cpp
void ofApp::update(){
    vector<int> vec(10);
    ...
}
```

That makes vectors easier to use than arrays since we don't need to care about deleting them, end up with dangling pointers, memory leaks... and their syntax is easier.

这使得向量比数组更容易使用, 因为我们不需要关心删除它们, 最终以悬挂指针, 内存泄漏...和他们的语法更容易。

Vectors have some more features and using them properly might be tricky mostly if we want to optimize for performance or use them in multithreaded applications, but that's not the scope of this chapter, you can find some tutorials about vectors, this is an introductory one in the openFrameworks site: [vectors basics](http://openframeworks.cc/tutorials/c++%20concepts/001_stl_vectors_basic.html) and this one explains more advanced concepts [std::vector](http://arturocastro.net/blog/2011/10/28/stl::vector/)

向量有一些更多的功能, 如果我们想优化性能或在多线程应用程序中使用它们, 那么正确地使用它们可能会很棘手, 但这不是本章的范围, 你可以找到一些关于向量的教程, 这是一个介绍性的openFrameworks网站：[矢量基础] (http://openframeworks.cc/tutorials/c++%20concepts/001_stl_vectors_basic.html), 这一个解释更高级的概念[std :: vector] (http://arturocastro.net/blog / 2011/10/28 / stl :: vector /)

## Other memory structures, lists and maps ##
##其他内存结构, 列表和映射##

Having objects in memory one after another is most of the time what we want, the access is really fast no matter if we want to access sequentially to each of them or randomly to anyone, since a vector is just an array internally, accesing let's say position 20 in it, just means that internally it just needs to get the memory address of the first position and add 20 to it. In soime cases though, vectors are not the most optimal memory structure. For example, if we want to frequnetly add  or remove elements in the middle of the vector, and you imagine the vector as a memory strip, that means that we need to move the rest of the vector till the end one position to the right and then insert the new element in the free location. In memory there's no such thing as move, moving contiguous memory means copying it and as we've said before, copying memory is a relatively slow operation.

在内存中一个接一个的对象是大多数时候我们想要的, 访问是真的很快, 无论我们想要顺序访问他们每个或随机到任何人, 因为一个向量只是一个内部数组, 访问让我们说位置20在它, 只是意味着在内部它只需要获得第一个位置的内存地址, 并添加20。在这种情况下, 向量不是最优的内存结构。例如, 如果我们要在向量的中间频繁地添加或删除元素, 并且将向量想象为内存条, 这意味着我们需要将向量的其余部分移动到右边的一个位置, 然后将新元素插入空闲位置。在内存中没有移动, 移动连续的内存意味着复制它, 如我们之前所说, 复制内存是一个相对较慢的操作。

![Vector inserting](images/vector_inserting "")

Sometimes, if there's not enough memory to move/copy the elements, one position to the right, the vector will need to copy the whole thing to a new memory location. If we are working with thousands of elements and doing this very frequently, like for example every frame, this can really slow things down a lot.

有时, 如果没有足够的内存来移动/复制元素, 一个位置在右边, 向量将需要将整个东西复制到一个新的内存位置。如果我们正在处理成千上万的元素, 并且这样做非常频繁, 例如每一帧, 这真的可以减慢很多事情。

To solve that, there's other memory structures like for example lists. In a list, memory, is not contiguous but instead each element has a pointer to the next and previous element so inserting one element just means changing those pointers to point to the newly added element. In a list we never need to move elements around but it's main disadvantage is that not being the elements contiguous in memory it's access can be slightly slower than a vector, also that we can't use it in certain cases like for example to upload data to the graphics card which always wants contiguos memory.

为了解决这个问题, 还有其他内存结构, 例如列表。在列表中, 存储器不是连续的, 而是每个元素具有指向下一个和前一个元素的指针, 因此插入一个元素只是意味着将这些指针改变为指向新添加的元素。在列表中, 我们不需要移动元素, 但它的主要缺点是不是内存中连续的元素, 它的访问可能比向量稍慢, 也不能在某些情况下使用它, 例如上传数据到总是想要连续存储器的显卡。

![List](images/list "")

Another problem of lists is that trying to access an element in the middle of the list (what is called random access) is slow since we always have to go through all the list till we arrive to the desired element. Lists are used then, when we seldom need to access randomly to a position of it and we need to add or remove elements in the middle frequently. For the specifics of the syntax of a list you can check the [c++ documentation on lists](http://www.cplusplus.com/reference/list/list/)

列表的另一个问题是, 试图访问列表中间的元素 (所谓的随机访问)是缓慢的, 因为我们总是必须遍历所有列表, 直到到达期望的元素。然后使用列表, 当我们很少需要随机访问它的位置, 我们需要频繁地添加或删除中间的元素。有关列表语法的详细信息, 您可以查看[c ++文档列表] (http://www.cplusplus.com/reference/list/list/)

There's several memory structures in the c++ standard library or other c++ libraries, apart from vectors and lists we are only going to see briefly maps.

在c ++标准库或其他c ++库中有几个内存结构, 除了向量和列表, 我们只会看到简要的映射。

Sometimes, we don't want to access things by their position or have an ordered list of elements but instead have something like an index or dictionary of elements that we can access by some key, that's what a map is. In a map we can store pairs of (key,value) and look for a value by it's key. For example let's say we have a collection of objects which have a name, if that name is unique for all the objects, we can store them in a map to be able to look for them by their name:

有时, 我们不想通过他们的位置访问东西或有一个有序的元素列表, 而是有一些像我们可以通过一些键访问的元素的索引或字典, 这是一个地图。在映射中, 我们可以存储 (key, value)对, 并通过它的键来查找值。例如, 假设我们有一个具有名称的对象集合, 如果该名称对于所有对象都是唯一的, 我们可以将它们存储在地图中, 以便能够通过其名称查找它们：

```cpp
map<string, NamedObject> objectsMap;

NamedObject o1;
o1.name = "object1";
objectsMap[o1.name] = o1;
```

Later on we can look for that object using it's name like:

后来我们可以使用它的名称来寻找该对象：

```cpp
objectsMap["object1"].doSomething();
```

Be careful though, if the object didn't exist before, using the `[]` operator will create a new one. If you are not sure, it's usually wise to try to find it first and only use it if it exists:

要小心, 如果对象以前不存在, 使用`[]`运算符将创建一个新的。如果你不确定, 通常是明智的尝试找到它, 只有使用它, 如果它存在：

```cpp
if(objectsMap.find("object1")!=objectsMap.end()){
    objectsMap["object1"].doSomething();
}
```

You can find the complete reference on maps in the [c++ documentation for maps](http://www.cplusplus.com/reference/map/map/)

您可以在[c ++ documentation for maps] (http://www.cplusplus.com/reference/map/map/)中的地图上找到完整的参考资料

## smart pointers ##
##智能指针##
As we've said before, traditional c pointers also called now *raw pointers* are sometimes problematic, the most frequent problems are dangling pointers: pointers that probably were once vañlid but now point to an invalid memory location, trying to dereference a NULL pointer, posible memory leaks if we fail to deallocate memory before loosing the reference to that memory address...

正如我们前面所说的, 传统的c指针也称为现在的* raw指针*有时是有问题的, 最常见的问题是悬空指针：指针可能曾经是vañlid但现在指向一个无效的内存位置, 试图取消引用一个NULL指针, 如果我们在释放对该内存地址的引用之前无法释放内存, 则会导致可能的内存泄漏...

Smart pointers try to solve that by adding what we've been calling stack semantics to memory allocation, the correct term for this is RAII: [Resource Acquisition Is Initialization](http://en.wikipedia.org/wiki/Resource_Acquisition_Is_Initialization) And means that the creation of an object in the stack, allocates the resources that it'll use later. When it's destructor is called because the variable goes out of scope, the destructor of the object is triggered which takes care of deallocating all the used resources. There's some more implications to RAII but for this chapter this is what matters to us more.


智能指针试图通过将我们一直称为堆栈语义的内容添加到内存分配中来解决这个问题, 正确的术语是RAII：[Resource Acquisition Is Initialization] (http://en.wikipedia.org/wiki/Resource_Acquisition_Is_Initialization)和意味着在堆栈中创建一个对象, 分配它稍后将使用的资源。当它的析构函数被调用, 因为变量超出范围, 对象的析构函数被触发, 它负责释放所有使用的资源。对RAII有更多的影响, 但对于这一章, 这是对我们更重要的。

Smart pointers use this technique to avoid all the problems that we've seen in raw pointers. They do this by also defining better who is the owner of some allocated memory or object. Till now we've seen how things allocated in the stack belong to the function or block that creates them we can return a copy of them (or in c++11 or later, move them) out of a function as a return value but their ownership is always clear.

智能指针使用这种技术来避免我们在原始指针中遇到的所有问题。他们这样做也通过定义更好的谁是一些分配的内存或对象的所有者。到目前为止, 我们已经看到了堆栈中分配的内容属于创建它们的函数或块, 我们可以返回它们的副本 (或在c ++ 11或更高版本中, 移动它们)作为返回值的函数, 但是他们的所有权始终清晰。

With heap memory though, ownership becomes way more fuzzy, someone might create a variable in the heap like:

使用堆内存, 所有权变得更模糊, 有人可能在堆中创建一个变量, 如：

```cpp
int * createFive(){
    int * a = new int;
    *a = 5;
    return a;
}
```

Now, when someone calls that function, who is the owner of the `new int`? Things can get even more complicated, what if we pass a pointer to that memory to another function or even an object?

现在, 当有人调用该函数时, 谁是`new int`的所有者？事情可以变得更复杂, 如果我们将一个指针传递给另一个函数甚至一个对象的内存呢？

```cpp
// ofApp.h
int * a;
SomeObject object;

// ofApp.cpp
void ofApp::setup(){
    a = createFive();
    object.setNumber(a);
}
```

who is now the owner of that memory? ofApp? object? The ownership defines among other things who is responsible for deleting that memory when it's not used anymore, now both ofApp and object have a reference to it, if ofApp deletes it before object is done with it, object might try to access it and crash the application, or the other way around. In this case it seems logical that ofApp takes care of deleting it since it knows about both object and the pointer to int a, but what if we change the example to :

谁现在是那个记忆的所有者？ ofApp？目的？所有权定义除其他事情之外谁负责删除该内存, 当它不再使用时, 现在的应用程序和对象都有它的引用, 如果ofApp在对象完成之前删除它, 对象可能会尝试访问它, 并崩溃应用程序, 或者其他方式。在这种情况下, 似乎onApp负责删除它, 因为它知道对象和指向int a的指针, 但如果我们更改示例如果, 

```cpp
// ofApp.h
SomeObject object;

// ofApp.cpp
void ofApp::setup(){
    int * a = createFive();
    object.setNumber(a);
}
```

or even:



```cpp
// ofApp.h
SomeObject object;

// ofApp.cpp
void ofApp::setup(){
    object.setNumber(createFive());
}
```


now ofApp doesn't know anymore about the allocated memory but both cases are possible so we actually need to know details of the implementation of object to know if we need to keep a reference of that variable to destroy it later or not. That, among other things breaks encapsulation, that you might now from chapter 1. We shouldn't need to know how object works internally to be able to use it. This makes the logic of our code really complicated and error prone.

现在ofApp不再了解已分配的内存, 但是这两种情况都是可能的, 所以我们实际上需要知道对象的实现细节, 知道我们是否需要保存该变量的引用以便以后销毁它。这包括封装, 你现在可能从第1章。我们不应该需要知道对象如何在内部工作, 以便能够使用它。这使得我们的代码的逻辑真的很复杂, 容易出错。

Smart pointers solve this by clearly defining who owns and object and by automatically deleting the allocated memory when the owner is destroyed. Sometimes, we need to share an object among several owners. For that cases we have a special type of smart pointers called shared pointers that defined a shader ownership and free the allocated memory only when all the owners cease to use the variable.

智能指针通过清楚地定义谁拥有和对象并且通过在所有者被销毁时自动删除分配的存储器来解决这个问题。有时, 我们需要在几个所有者之间共享一个对象。对于这种情况, 我们有一种特殊类型的智能指针, 称为共享指针, 它定义着色器所有权, 并在所有的所有者停止使用变量时释放分配的内存。

We are only going to see this briefly, there's lots of examples in the web about how to use smart pointers and reference to their syntax, the most important is to understand how they work by defining the ownership clearly compared to raw pointers and the problems they solve.

我们只是简单地看到这一点, 网上有很多关于如何使用智能指针和引用其语法的例子, 最重要的是通过定义所有权明确地相比原始指针和他们的问题他们的工作原理解决。

### unique_ptr

A unique_ptr, as it's name suggests, is a pointer that defines a unique ownership for an object, we can move it around and the object or function that has it at some point is the owner of it, no more than one reference at the same time is valid and when it goes out of scope it automatically deletes any memory that we might have allocated.

一个unique_ptr, 就像它的名字所暗示的, 是一个指针, 定义一个对象的唯一所有权, 我们可以移动它, 它的对象或函数在某一点是它的所有者, 在同一个不超过一个引用时间是有效的, 当它超出范围它自动删除我们可能已分配的任何内存。

To allocate memory using a unique_ptr we do:

要使用unique_ptr分配内存, 我们执行：

```cpp
void ofApp::setup(){
	unique_ptr<int> a(new int);
	*a = 5;
}
```

As you can see, once it's created it's syntax is the same as a raw pointer, we can use the `*` operator to dereference it and access or modiify it's value, if we are working with objects like:

正如你所看到的, 一旦创建它的语法与原始指针相同, 我们可以使用`*`操作符解除引用, 访问或修改它的值, 如果我们使用下面的对象：

```cpp
void ofApp::setup(){
	unique_ptr<Particle> p(new Particle);
	p->pos.set(20,20);
}
```

We can also use the `->` to access it's member variables and functions.

我们也可以使用` - >`来访问它的成员变量和函数。

When the function goes out of scope, being unique_ptr an object, it's destructor will get called, which internally will call `delete` on the allocated memory so we don't need to call delete on unique_ptr at all.

当函数超出范围, 是unique_ptr一个对象, 它的析构函数将被调用, 内部将在分配的内存上调用'delete', 所以我们不需要在unique_ptr上调用delete。

Now let's say we want to move a unique_ptr into a vector:

现在, 让我们假设我们将一个unique_ptr移动到一个向量：

```cpp
void ofApp::setup(){
	unique_ptr<int> a(new int);
	*a = 5;

	vector<unique_ptr<int> > v;
	v.push_back(a);  // error
}
```

That will generate a long error, depending on the compiler, really hard to understand. What's going on, is that `a` is still owned by ofApp::setup so we can't put it in the vector, what we can do is move it into the vector by explicitly saying that we want to move the ownership of that unique_ptr into the vector:

这将产生一个很长的错误, 这取决于编译器, 真的很难理解。发生了什么, 是'a'仍然由ofApp :: setup所有, 所以我们不能把它放在向量中, 我们可以做的是将它移动到向量中, 明确地说, 我们想移动的所有权unique_ptr into the vector：

```cpp
void ofApp::setup(){
	unique_ptr<int> a(new int);
	*a = 5;

	vector<unique_ptr<int> > v;
	v.push_back(move(a));
}
```

There's a problem that unique_ptr doesn't solve, we can still do:

有一个问题unique_ptr不解决, 我们仍然可以做：

```cpp
void ofApp::setup(){
	unique_ptr<int> a(new int);
	*a = 5;

	vector<unique_ptr<int> > v;
	v.push_back(move(a));

	cout << *a << endl;
}
```

The compiler won't fail there but if we try to execute the application it'll crash since `a` is not owned by ofApp::setup anymore, having to explicitly use `move` tries to solve that problem by making the syntax clearer. After using move, we can't use that variable anymore except through the vector. More modern langauages like [Rust](http://www.rust-lang.org/) completely solve this by making the compiler detect this kind of uses of moved variables and producing a compiler error. This will probably be solved at some point in c++ but by now you need to be careful to not use a moved variable.

编译器不会失败, 但是如果我们尝试执行应用程序, 它会崩溃, 因为`a`不是由ofApp ::设置不再拥有, 不得不明确使用`move`尝试解决这个问题, 通过使语法更清楚。使用move后, 我们不能再使用该变量, 除非通过向量。更现代的langauages像[Rust] (http://www.rust-lang.org/)通过使编译器检测这种移动变量的使用和产生编译器错误完全解决这个问题。这可能会在c ++的某个点解决, 但现在你需要小心不要使用移动的变量。

### shared_ptr

As we've seen before, sometimes having unique ownership is not enough, sometimes we need to share an object among several owners, in c++11 or later, this is solved through `shared_ptr`. The usage is pretty similar to `unique_ptr`, we create it like:

如前所述, 有时拥有唯一所有权是不够的, 有时我们需要在几个所有者之间共享一个对象, 在c ++ 11或更高版本中, 这是通过`shared_ptr`解决的。其用法非常类似于`unique_ptr`, 我们创建它：

```cpp
void ofApp::setup(){
	shared_ptr<int> a(new int);
	*a = 5;

	vector<shared_ptr<int> > v;
	v.push_back(a);
}
```

The difference is that now, both the vector and ofApp::setup, have a reference to that object, and doing:

区别是现在, 向量和ofApp :: setup都有对该对象的引用, 并做：

```cpp
void ofApp::setup(){
	shared_ptr<int> a(new int);
	*a = 5;

	vector<shared_ptr<int> > v;
	v.push_back(a);

	cout << *a << endl;
}
```

Is perfectly ok. The way a shared_ptr works is by keeping a count of how many references there are to it, whenever we make a copy of it, it increases that counter by one, whenever a reference is destroyed it decreases that reference by one. When the reference cound arrives to 0 it frees the allocated memory. That reference counting is done atomically, which means that we can share a shared_ptr across threads without having problems with the count. That doesn't mean that we can access the contained data safely in a multithreaded application, just that the reference count won't get wrong if we pass a shared_ptr accross different threads.

是完全确定。 shared_ptr的工作方式是通过保持对它有多少引用的计数, 每当我们复制一个副本时, 它将该计数器增加一个, 每当一个引用被销毁时, 它减少一个引用。当参考库到达0时, 释放分配的内存。该引用计数是原子性地完成的, 这意味着我们可以在线程之间共享shared_ptr, 而不会出现计数问题。这并不意味着我们可以在多线程应用程序中安全地访问包含的数据, 只要引用计数不会错误, 如果我们通过一个shared_ptr跨不同线程。
