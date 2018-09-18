Welcome to StackEdit!
===================


Hey! I'm your first Markdown document in **StackEdit**[^stackedit]. Don't delete me, I'm very helpful! I can be recovered anyway in the **Utils** tab of the <i class="icon-cog"></i> **Settings** dialog.

----------


Documents
-------------

StackEdit stores your documents in your browser, which means all your documents are automatically saved locally and are accessible **offline!**

> **Note:**

> - StackEdit is accessible offline after the application has been loaded for the first time.
> - Your local documents are not shared between different browsers or computers.
> - Clearing your browser's data may **delete all your local documents!** Make sure your documents are synchronized with **Google Drive** or **Dropbox** (check out the [<i class="icon-refresh"></i> Synchronization](#synchronization) section).

#### <i class="icon-file"></i> Create a document

The document panel is accessible using the <i class="icon-folder-open"></i> button in the navigation bar. You can create a new document by clicking <i class="icon-file"></i> **New document** in the document panel.

#### <i class="icon-folder-open"></i> Switch to another document

All your local documents are listed in the document panel. You can switch from one to another by clicking a document in the list or you can toggle documents using <kbd>Ctrl+[</kbd> and <kbd>Ctrl+]</kbd>.

#### <i class="icon-pencil"></i> Rename a document

You can rename the current document by clicking the document title in the navigation bar.

#### <i class="icon-trash"></i> Delete a document

You can delete the current document by clicking <i class="icon-trash"></i> **Delete document** in the document panel.

#### <i class="icon-hdd"></i> Export a document

You can save the current document to a file by clicking <i class="icon-hdd"></i> **Export to disk** from the <i class="icon-provider-stackedit"></i> menu panel.

> **Tip:** Check out the [<i class="icon-upload"></i> Publish a document](#publish-a-document) section for a description of the different output formats.


----------


Synchronization
-------------------

StackEdit can be combined with <i class="icon-provider-gdrive"></i> **Google Drive** and <i class="icon-provider-dropbox"></i> **Dropbox** to have your documents saved in the *Cloud*. The synchronization mechanism takes care of uploading your modifications or downloading the latest version of your documents.

> **Note:**

> - Full access to **Google Drive** or **Dropbox** is required to be able to import any document in StackEdit. Permission restrictions can be configured in the settings.
> - Imported documents are downloaded in your browser and are not transmitted to a server.
> - If you experience problems saving your documents on Google Drive, check and optionally disable browser extensions, such as Disconnect.

#### <i class="icon-refresh"></i> Open a document

You can open a document from <i class="icon-provider-gdrive"></i> **Google Drive** or the <i class="icon-provider-dropbox"></i> **Dropbox** by opening the <i class="icon-refresh"></i> **Synchronize** sub-menu and by clicking **Open from...**. Once opened, any modification in your document will be automatically synchronized with the file in your **Google Drive** / **Dropbox** account.

#### <i class="icon-refresh"></i> Save a document

You can save any document by opening the <i class="icon-refresh"></i> **Synchronize** sub-menu and by clicking **Save on...**. Even if your document is already synchronized with **Google Drive** or **Dropbox**, you can export it to a another location. StackEdit can synchronize one document with multiple locations and accounts.

#### <i class="icon-refresh"></i> Synchronize a document

Once your document is linked to a <i class="icon-provider-gdrive"></i> **Google Drive** or a <i class="icon-provider-dropbox"></i> **Dropbox** file, StackEdit will periodically (every 3 minutes) synchronize it by downloading/uploading any modification. A merge will be performed if necessary and conflicts will be detected.

If you just have modified your document and you want to force the synchronization, click the <i class="icon-refresh"></i> button in the navigation bar.

> **Note:** The <i class="icon-refresh"></i> button is disabled when you have no document to synchronize.

#### <i class="icon-refresh"></i> Manage document synchronization

Since one document can be synchronized with multiple locations, you can list and manage synchronized locations by clicking <i class="icon-refresh"></i> **Manage synchronization** in the <i class="icon-refresh"></i> **Synchronize** sub-menu. This will let you remove synchronization locations that are associated to your document.

> **Note:** If you delete the file from **Google Drive** or from **Dropbox**, the document will no longer be synchronized with that location.

----------


Publication
-------------

Once you are happy with your document, you can publish it on different websites directly from StackEdit. As for now, StackEdit can publish on **Blogger**, **Dropbox**, **Gist**, **GitHub**, **Google Drive**, **Tumblr**, **WordPress** and on any SSH server.

#### <i class="icon-upload"></i> Publish a document

You can publish your document by opening the <i class="icon-upload"></i> **Publish** sub-menu and by choosing a website. In the dialog box, you can choose the publication format:

- Markdown, to publish the Markdown text on a website that can interpret it (**GitHub** for instance),
- HTML, to publish the document converted into HTML (on a blog for example),
- Template, to have a full control of the output.

> **Note:** The default template is a simple webpage wrapping your document in HTML format. You can customize it in the **Advanced** tab of the <i class="icon-cog"></i> **Settings** dialog.

#### <i class="icon-upload"></i> Update a publication

After publishing, StackEdit will keep your document linked to that publication which makes it easy for you to update it. Once you have modified your document and you want to update your publication, click on the <i class="icon-upload"></i> button in the navigation bar.

> **Note:** The <i class="icon-upload"></i> button is disabled when your document has not been published yet.

#### <i class="icon-upload"></i> Manage document publication

Since one document can be published on multiple locations, you can list and manage publish locations by clicking <i class="icon-upload"></i> **Manage publication** in the <i class="icon-provider-stackedit"></i> menu panel. This will let you remove publication locations that are associated to your document.

> **Note:** If the file has been removed from the website or the blog, the document will no longer be published on that location.

----------


Markdown Extra
--------------------

StackEdit supports **Markdown Extra**, which extends **Markdown** syntax with some nice features.

> **Tip:** You can disable any **Markdown Extra** feature in the **Extensions** tab of the <i class="icon-cog"></i> **Settings** dialog.

> **Note:** You can find more information about **Markdown** syntax [here][2] and **Markdown Extra** extension [here][3].


### Tables

**Markdown Extra** has a special syntax for tables:

Item     | Value
-------- | ---
Computer | $1600
Phone    | $12
Pipe     | $1

You can specify column alignment with one or two colons:

| Item     | Value | Qty   |
| :------- | ----: | :---: |
| Computer | $1600 |  5    |
| Phone    | $12   |  12   |
| Pipe     | $1    |  234  |


### Definition Lists

**Markdown Extra** has a special syntax for definition lists too:

Term 1
Term 2
:   Definition A
:   Definition B

Term 3

:   Definition C

:   Definition D

	> part of definition D


### Fenced code blocks

GitHub's fenced code blocks are also supported with **Highlight.js** syntax highlighting:

```
// Foo
var bar = 0;
```

> **Tip:** To use **Prettify** instead of **Highlight.js**, just configure the **Markdown Extra** extension in the <i class="icon-cog"></i> **Settings** dialog.

> **Note:** You can find more information:

> - about **Prettify** syntax highlighting [here][5],
> - about **Highlight.js** syntax highlighting [here][6].


### Footnotes

You can create footnotes like this[^footnote].

  [^footnote]: Here is the *text* of the **footnote**.


### SmartyPants

SmartyPants converts ASCII punctuation characters into "smart" typographic punctuation HTML entities. For example:

|                  | ASCII                        | HTML              |
 ----------------- | ---------------------------- | ------------------
| Single backticks | `'Isn't this fun?'`            | 'Isn't this fun?' |
| Quotes           | `"Isn't this fun?"`            | "Isn't this fun?" |
| Dashes           | `-- is en-dash, --- is em-dash` | -- is en-dash, --- is em-dash |


### Table of contents

You can insert a table of contents using the marker `[TOC]`:

[TOC]


### MathJax

You can render *LaTeX* mathematical expressions using **MathJax**, as on [math.stackexchange.com][1]:

The *Gamma function* satisfying $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$ is via the Euler integral

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$

> **Tip:** To make sure mathematical expressions are rendered properly on your website, include **MathJax** into your template:

```
<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML"></script>
```

> **Note:** You can find more information about **LaTeX** mathematical expressions [here][4].


### UML diagrams

You can also render sequence diagrams like this:

```sequence
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```

And flow charts like this:

```flow
st=>start: Start
e=>end
op=>operation: My Operation
cond=>condition: Yes or No?

st->op->cond
cond(yes)->e
cond(no)->op
```

> **Note:** You can find more information:

> - about **Sequence diagrams** syntax [here][7],
> - about **Flow charts** syntax [here][8].

### Support StackEdit

[![](https://cdn.monetizejs.com/resources/button-32.png)](https://monetizejs.com/authorize?client_id=ESTHdCYOi18iLhhO&summary=true)

  [^stackedit]: [StackEdit](https://stackedit.io/) is a full-featured, open-source Markdown editor based on PageDown, the Markdown library used by Stack Overflow and the other Stack Exchange sites.


  [1]: http://math.stackexchange.com/
  [2]: http://daringfireball.net/projects/markdown/syntax "Markdown"
  [3]: https://github.com/jmcmanus/pagedown-extra "Pagedown Extra"
  [4]: http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference
  [5]: https://code.google.com/p/google-code-prettify/
  [6]: http://highlightjs.org/
  [7]: http://bramp.github.io/js-sequence-diagrams/
  [8]: http://adrai.github.io/flowchart.js/

[**stackedit.io**](https://stackedit.io/editor)

[**C++ 参考手册**](http://zh.cppreference.com/w/cpp)

[**C++程序员的阅读清单**](http://blog.jobbole.com/21544/)

[**GitBook**](https://github.com/xiaoweiChen/Cpp_Concurrency_In_Action)


[**C++11 并发指南系列**](http://www.cnblogs.com/haippy/p/3284540.html)


[**提升 C++ 技能的 7 种方法**](http://blog.jobbole.com/112246/?utm_source=blog.jobbole.com&utm_medium=relatedPosts)


[**ucontext-人人都可以实现的简单协程库**](https://blog.csdn.net/qq910894904/article/details/41911175)


# C++并发编程实战


## 第1章 你好，C++的并发世界!
本章主要内容  
- 何谓并发和多线程  
- 应用程序为什么要使用并发和多线程  
- C++的并发史  
- 一个简单的C++多线程程序

### 1.1 何谓并发
- 任务切换会给用户和应用程序造成一种“并发的假象”。因为这种假象，当应用在
任务切换的环境下和真正并发环境下执行相比，行为还是有着微妙的不同。

>特别是对内存模型不正确的假设(详见第5章) ??

> 在多线程环境中可能不会出现(详见第10章)。??


#### 1.1.2 并发的途径
- 多进程并发
操作系统在进程间提供的附加保护操作和更高级别的通信机制，意味着可以更容易编写安全(safe)的并发代码 使用独立的进程，实现并发还有一个额外的优势———可以使用远程连接(可能需要联网)的方  式，在不同的机器上运行独立的进程。虽然，这增加了通信成本，但在设计精良的系统上，  这可能是一个提高并行可用行和性能的低成本方式

> 在类似于Erlang编程环境中，将进程作为并发的基本构造块
- 多线程并发
线程很像轻量级的进程：每个线程相互独立运行，且线程可以在不同的指令序列中运行。但是，进程中的所有线程都共享地址空间，并且所有线程访问到大部分数据———全局变量仍然是全局的，指针、对象的引用或数据可以在线程之间传递。虽然，进程之间通常共享内存，但这种共享通常也是难以建立，且难以管理。因为，同一数据的内存地址在不同的进程中是不相同。
地址空间共享，以及缺少线程间数据的保护，使得操作系统的记录工作量减小，所以使用多  线程相关的开销远远小于使用多个进程。不过，共享内存的灵活性是有代价的：如果数据要被多个线程访问，那么程序员必须确保每个线程所访问到的数据是一致的(在本书第3、4、5和8章中会涉及，线程间数据共享可能会遇到的问题，以及如何使用工具来避免这些问题)。问题并非无解，只要在编写代码时适当地注意即可，这同样也意味着需要对线程通信做大量的工作。

- 多个单线程/进程间的通信(包含启动)要比单一进程中的多线程间的通信(包括启动)的开销大，若不考虑共享内存可能会带来的问题，多线程将会成为主流语言(包括C++)更青睐的并发途径。此外，C++标准并未对进程间通信提供任何原生支持，所以使用多进程的方式实现，这会依赖与平台相关的API。因此，本书只关注使用多线程的并发，并且在此之后所提到“并发”，均假设为多线程来实现

### 1.2 为什么使用并发？
>主要原因有两个：关注点分离(SOC) <片上系统 System On Chip>和性能

主要原因有两个：关注点分离(SOC)和性能。事实上，它们应该是使用并发的唯一原因；如果你观察的足够仔细，所有因素都可以归结到其中的一个原因(或者可能是两个都有。当然，除了像“就因为我愿意”这样的原因之外)。

#### 1.2.1 为了分离关注点
> 编写软件时，分离关注点是个好主意；通过将相关的代码与无关的代码分离，可以使程序更容易理解和测试，从而减少出错的可能性。即使一些功能区域中的操作需要在同一时刻发生的情况下，依旧可以使用并发分离不同的功能区域；若不显式地使用并发，就得编写一个任务切换框架，或者在操作中主动地调用一段不相关的代码。

#### 1.2.2 为了性能

两种方式利用并发提高性能：
> 第一，将一个单个任务分成几部分，且各自并行运行，从而降低总运行时间。这就是任务并行（task parallelism） 。虽然这听起来很直观，但它是一个相当复杂的过程，因为在各个部分之间可能存在着依赖。区别可能是在过程方面——一个线程执行算法的一部分，而另一个线程执行算法的另一个部分或是在数据方面——每个线程在不同的数据部分上执行相同的操作（第二种方式） 。后一种方法被称为数据并行（data  parallelism） 。
- 任务并行（task parallelism）
- 数据并行（data  parallelism）

易受这种并行影响的算法常被称为易并行(embarrassingly parallel)。尽管你会受到易并行化  代码影响，但这对于你来说是一件好事：我曾遇到过自然并行(naturally parallel)和便利并发  (conveniently concurrent)的算法。易并行算法具有良好的可扩展特性——当可用硬件线程的  数量增加时，算法的并行性也会随之增加。这种算法能很好的体现人多力量大。如果算法中  有不易并行的部分，你可以把算法划分成固定(不可扩展)数量的并行任务。第8章将会再来讨论，在线程之间划分任务的技巧。


>第二种方法是使用可并行的方式，来解决更大的问题；与其同时处理一个文件，不如酌情处理2个、10个或20个。虽然，这是数据并行的一种应用(通过对多组数据同时执行相同的操作)，但着重点不同。处理一个数据块仍然需要同样的时间，但在相同的时间内处理了更多的数据。当然，这种方法也有限制，并非在所有情况下都是有益的。不过，这种方法所带来的吞吐量提升，可以让某些新功能成为可能，例如，可以并行处理图片的各部分，就能提高视频的分辨率。

#### 1.2.3 什么时候不使用并发
> 不使用并发的唯一原因就是，收益比不上成本

- 尽管线程池(参见第9章)可以用来限制线程的数量，但这并不是灵丹妙药，它也有自己的问题。??
- 但当同样的技术用于需要处理大量链接的高需求服务器时，也会因为线程  
太多而耗尽系统资源。在这种场景下，谨慎地使用线程池可以对性能产生优化（参见第9章） 。??


### 1.3 C++中使的并发和多线程
#### 1.3.1 C++多线程历史
>使用带锁的资源获取就是初始化(RAII, Resource Acquisition Is Initialization)的习惯，来确保当退出相关作用域时互斥元解锁。

#### 1.3.2 新标准支持并发
> 全新的线程感知内存模型??
> C++标准库也扩展了：包含了用于管理线程(参见第2章)、保护共享数据(参见第3章)、线程间同步操作(参见第4章)，以及低级原子操作(参见第5章)的各种类。
> 
#### 1.3.3 C++线程库的效率

#### 1.3.4 平台相关的工具

### 1.4 开始入门

#### 1.4.1 你好，并发世界
- 每个线程都必须具有一个初始函数  (initial function)，新线程的执行在这里开始。对于应用程序来说，初始线程是main()，但是对于其他线程，可以在 std::thread 对象的构造函数中指定

### 1.5 小结
> C++中使用多线程并不复杂，复杂的是如何设计代码以实现其预期的行为。

## 第2章 线程管理

本章主要内容
- 启动新线程
- 等待线程与分离线程
- 线程唯一标识符

好的！看来你已经决定使用多线程了。先做点什么呢?启动线程、结束线程，还是如何监管线程？在C++标准库中只需要管理 std::thread 关联的线程，无需把注意力放在其他方面。不过，标准库太灵活，所以管理起来不会太容易。
本章将从基本开始：启动一个线程，等待这个线程结束，或放在后台运行。再看看怎么给已经启动的线程函数传递参数，以及怎么将一个线程的所有权从当前 std::thread 对象移交给另一个。最后，再来确定线程数，以及识别特殊线程。


### 2.1 线程管理的基础
>每个程序至少有一个线程：执行main()函数的线程，其余线程有其各自的入口函数。线程与原始线程(以main()为入口函数的线程)同时运行。如同main()函数执行完会退出一样，当线程执行完入口函数后，线程也会退出。在为一个线程创建了一个 std::thread 对象后，需要等待这个线程结束；不过，线程需要先进行启动。下面就来启动线程。

#### 2.1.1 启动线程
>线程在 std::thread 对象创建(为线程指定任务)时启动。最简单的情况下，任务也  
会很简单，通常是无参数无返回(void-returning)的函数。这种函数在其所属线程上运行，直到函数执行完毕，线程也就结束了。在一些极端情况下，线程运行时，任务中的函数对象需要通过某种通讯机制进行参数的传递，或者执行一系列独立操作;可以通过通讯机制传递信号，让线程停止。线程要做什么，以及什么时候启动，其实都无关紧要。***总之，使用C++线程库启动线程，可以归结为构造 std::thread 对象：***

- 为了让编译器识别 std::thread 类，这个简单的例子也要包含 <thread> 头文件。如同大多数C++标准库一样， std::thread 可以用可调用（callable） 类型构造，将带有函数调用符类型的实例传入 std::thread 类中，替换默认的构造函数。

```
class background_task
{
public:
  void operator()() const
  {
    do_something();
    do_something_else();
  }
};

background_task f;
std::thread my_thread(f);
```

- 代码中，**提供的函数对象会复制到新线程的存储空间当中**，函数对象的执行和调用都在线程的内存空间中进行。函数对象的副本应与原始函数对象保持一致，否则得到的结果会与我们的期望不同。

- 有件事需要注意，当把函数对象传入到线程构造函数中时，需要避免“**最令人头痛的语法解析** ”  (C++’s most vexing parse, 中文简介)。如果你传递了一个临时变量，而不是一个命名的变量；C++编译器会将其解析为函数声明，而不是类型对象的定义。
例如：

	>	~~std::thread my_thread(background_task());~~

这里相当与声明了一个名为my\_thread的函数，这个函数带有一个参数\(函数指针指向没有参数并返回background\_task对象的函数\)，返回一个`std::thread`对象的函数，而非启动了一个线程。

使用在前面命名函数对象的方式，或使用多组括号①，或使用新统一的初始化语法②，可以避免这个问题。
如下所示：
```
std::thread my_thread((background_task()));  // 1
std::thread my_thread{background_task()};    // 2
```
**使用lambda表达式也能避免这个问题**。lambda表达式是C++11的一个新特性，它允许使用一个可以捕获局部变量的局部函数\(可以避免传递参数，参见2.2节\)。想要具体的了解lambda表达式，可以阅读附录A的A.5节。之前的例子可以改写为lambda表达式的类型：

```
std::thread my_thread([]{
  do_something();
  do_something_else();
});
```

>启动了线程，你需要明确是要等待线程结束(加入式——参见2.1.2节)，还是让其自主运行(分离式——参见2.1.3节)。如果 std::thread 对象销毁之前还没有做出决定，程序就会终止( std::thread 的析构函数会调用 std::terminate() )。**因此，即便是有异常存在，也需要确保线程能够正确的加入(joined)或分离(detached)**。2.1.3节中，会介绍对应的方法来处理这两种情况。需要注意的是，必须在 std::thread 对象销毁之前做出决定——加入或分离线程之前。如果线程就已经结束，想再去分离它，线程可能会在 std::thread 对象销毁之后继续运行下去。  
如果不等待线程，就必须保证线程结束之前，可访问的数据得有效性。这不是一个新问题——单线程代码中，对象销毁之后再去访问，也会产生未定义行为——不过，线程的生命周期增加了这个问题发生的几率。


这种情况很可能发生在线程还没结束，函数已经退出的时候，这时线程函数还持有函数局部变量的指针或引用。下面的清单中就展示了这样的一种情况。

清单2.1  函数已经结束，线程依旧访问局部变量

```
struct func
{
  int& i;
  func(int& i_) : i(i_) {}
  void operator() ()
  {
    for (unsigned j=0 ; j<1000000 ; ++j)
    {
      do_something(i);           // 1. 潜在访问隐患：悬空引用
    }
  }
};

void oops()
{
  int some_local_state=0;
  func my_func(some_local_state);
  std::thread my_thread(my_func);
  my_thread.detach();          // 2. 不等待线程结束
}                              // 3. 新线程可能还在运行
```

这个例子中，已经决定不等待线程结束\(使用了detach\(\)②\)，所以当oops\(\)函数执行完成时③，新线程中的函数可能还在运行。如果线程还在运行，它就会去调用do\_something\(i\)函数①，这时就会访问已经销毁的变量。如同一个单线程程序——允许在函数完成后继续持有局部变量的指针或引用；当然，这从来就不是一个好主意——这种情况发生时，错误并不明显，会使多线程更容易出错。

**处理这种情况的常规方法：使线程函数的功能齐全，将数据复制到线程中，而非复制到[共享数据](https://blog.csdn.net/fanyun_01/article/details/78145431)中。如果使用一个可调用的对象作为线程函数，这个对象就会复制到线程中，而后原始对象就会立即销毁。但对于对象中包含的指针和引用还需谨慎**，例如清单2.1所示。使用一个能访问局部变量的函数去创建线程是一个糟糕的主意\(除非**十分确定**线程会在函数完成前结束\)。此外，可以通过join\(\)函数来确保线程在函数完成前结束。

#### 2.1.2 等待线程完成
如果需要等待线程，相关的`std::thread`实例需要使用join\(\)。清单2.1中，将`my_thread.detach()`替换为`my_thread.join()`，就可以确保局部变量在线程完成后，才被销毁。在这种情况下，因为原始线程在其生命周期中并没有做什么事，使得用一个独立的线程去执行函数变得收益甚微，但在实际编程中，原始线程要么有自己的工作要做；要么会启动多个子线程来做一些有用的工作，并等待这些线程结束。

**join\(\)是简单粗暴的等待线程完成或不等待。**当你需要对等待中的线程有更灵活的控制时，比如，看一下某个线程是否结束，或者只等待一段时间\(超过时间就判定为超时\)。想要做到这些，你需要使用其他机制来完成，比如条件变量和_期待_\(futures\)，相关的讨论将会在第4章继续。**调用join\(\)的行为，还清理了线程相关的存储部分，这样`std::thread`对象将不再与已经完成的线程有任何关联**。这意味着，只能对一个线程使用一次join\(\);一旦已经使用过join\(\)，`std::thread`对象就不能再次加入了，当对其使用joinable\(\)时，将返回false。

#### 2.1.3 特殊情况下的等待


如前所述，需要对一个还未销毁的`std::thread`对象使用join\(\)或detach\(\)。如果想要分离一个线程，可以在线程启动后，直接使用detach\(\)进行分离。**如果打算等待对应线程，则需要细心挑选调用join\(\)的位置。当在线程运行之后产生异常，在join\(\)调用之前抛出，就意味着这次join调用会被跳过**。

避免应用被抛出的异常所终止，就需要作出一个决定。通常，当倾向于在无异常的情况下使用join\(\)时，需要在异常处理过程中调用join\(\)，从而避免生命周期的问题。下面的程序清单是一个例子。

清单 2.2 等待线程完成

```
struct func; // 定义在清单2.1中
void f()
{
  int some_local_state=0;
  func my_func(some_local_state);
  std::thread t(my_func);
  try
  {
    do_something_in_current_thread();
  }
  catch(...)
  {
    t.join();  // 1
    throw;
  }
  t.join();  // 2
}
```

清单2.2中的代码使用了`try/catch`块确保访问本地状态的线程退出后，函数才结束。当函数正常退出时，会执行到②处；当函数执行过程中抛出异常，程序会执行到①处。`try/catch`块能轻易的捕获轻量级错误，所以这种情况，并非放之四海而皆准。如需确保线程在函数之前结束——查看是否因为线程函数使用了局部变量的引用，以及其他原因——而后再确定一下程序可能会退出的途径，无论正常与否，可以提供一个简洁的机制，来做解决这个问题。

一种方式是使用 “**资源获取即初始化方式”\(RAII，Resource Acquisition Is Initialization\)**，并且提供一个类，在析构函数中使用**join\(\)**，如同下面清单中的代码。看它如何简化f\(\)函数。

清单 2.3 使用RAII等待线程完成

```
class thread_guard
{
  std::thread& t;
public:
  explicit thread_guard(std::thread& t_):
    t(t_)
  {}
  ~thread_guard()
  {
    if(t.joinable()) // 1
    {
      t.join();      // 2
    }
  }
  thread_guard(thread_guard const&)=delete;   // 3
  thread_guard& operator=(thread_guard const&)=delete;
};

struct func; // 定义在清单2.1中

void f()
{
  int some_local_state=0;
  func my_func(some_local_state);
  std::thread t(my_func);
  thread_guard g(t); // 从2.7清单返回这里审视 没有转移线程的所有权 线程t还是不够安全 可能被 detach() 或者 join() 导致意外 
  do_something_in_current_thread();
}    // 4
```

当线程执行到④处时，局部对象就要被逆序销毁了。因此，thread\_guard对象g是第一个被销毁的，这时线程在析构函数中被加入②到原始线程中。即使do\_something\_in\_current\_thread抛出一个异常，这个销毁依旧会发生。

在thread\_guard的析构函数的测试中，首先判断线程是否已加入①，如果没有会调用join\(\)②进行加入。这很重要，因为**join\(\)只能对给定的对象调用一次，所以对给已加入的线程再次进行加入操作时，将会导致错误**。

**拷贝构造函数和拷贝赋值操作被标记为`=delete`③，是为了不让编译器自动生成它们。直接对一个对象进行拷贝或赋值是危险的，因为这可能会弄丢已经加入的线程。通过删除声明，任何尝试给thread\_guard对象赋值的操作都会引发一个编译错误**。

>***想要了解删除函数的更多知识，请参阅附录A的A.2节***。

如果不想等待线程结束，可以_分离_\(_detaching\)线程，从而避免_异常安全\*\(exception-safety\)问题。不过，这就打破了线程与`std::thread`对象的联系，即使线程仍然在后台运行着，分离操作也能确保`std::terminate()`在`std::thread`对象销毁才被调用。

#### 2.1.4 后台运行线程

使用detach\(\)会让线程在后台运行，这就意味着主线程不能与之产生直接交互。也就是说，不会等待这个线程结束；**如果线程分离，那么就不可能有`std::thread`对象能引用它，分离线程的确在后台运行，所以分离线程不能被加入。不过C++运行库保证，当线程退出时，相关资源的能够正确回收，后台线程的归属和控制C++运行库都会处理**。

通常称分离线程为_守护线程_\(daemon threads\),UNIX中守护线程是指，没有任何显式的用户接口，并在后台运行的线程。这种线程的特点就是长时间运行；线程的生命周期可能会从某一个应用起始到结束，可能会在后台监视文件系统，还有可能对缓存进行清理，亦或对数据结构进行优化。另一方面，分离线程的另一方面只能确定线程什么时候结束，_发后即忘_\(fire and forget\)的任务就使用到线程的这种方式。

如2.1.2节所示，调用`std::thread`成员函数detach\(\)来分离一个线程。之后，相应的`std::thread`对象就与实际执行的线程无关了，并且这个线程也无法加入：

```
std::thread t(do_background_work);
t.detach();
assert(!t.joinable());
```

为了从`std::thread`对象中分离线程\(前提是有可进行分离的线程\),**不能对没有执行线程的`std::thread`对象使用detach\(\),也是join\(\)的使用条件，并且要用同样的方式进行检查——当`std::thread`对象使用t.joinable\(\)返回的是true，就可以使用t.detach\(\)。**

试想如何能让一个文字处理应用同时编辑多个文档。无论是用户界面，还是在内部应用内部进行，都有很多的解决方法。虽然，这些窗口看起来是完全独立的，每个窗口都有自己独立的菜单选项，但他们却运行在同一个应用实例中。一种内部处理方式是，让每个文档处理窗口拥有自己的线程；每个线程运行同样的的代码，并隔离不同窗口处理的数据。如此这般，打开一个文档就要启动一个新线程。因为是对独立的文档进行操作，所以没有必要等待其他线程完成。因此，这里就可以让文档处理窗口运行在分离的线程上。

下面代码简要的展示了这种方法：

清单2.4 使用分离线程去处理其他文档

```
void edit_document(std::string const& filename)
{
  open_document_and_display_gui(filename);
  while(!done_editing())
  {
    user_command cmd=get_user_input();
    if(cmd.type==open_new_document)
    {
      std::string const new_name=get_filename_from_user();
      std::thread t(edit_document,new_name);  // 1
      t.detach();  // 2
    }
    else
    {
       process_user_input(cmd);
    }
  }
}
```

如果用户选择打开一个新文档，需要启动一个新线程去打开新文档①，并分离线程②。与当前线程做出的操作一样，新线程只不过是打开另一个文件而已。所以，edit\_document函数可以复用，通过传参的形式打开新的文件。

这个例子也展示了传参启动线程的方法：不仅可以向`std::thread`构造函数①传递函数名，还可以传递函数所需的参数\(实参\)。C++线程库的方式也不是很复杂。当然，也有其他方法完成这项功能，比如:使用一个带有数据成员的成员函数，代替一个需要传参的普通函数。


### 2.2 向线程函数传递参数

清单2.4中，向`std::thread`构造函数中的可调用对象，或函数传递一个参数很简单。**需要注意的是，默认参数要拷贝到线程独立内存中，即使参数是引用的形式，也可以在新线程中进行访问。**再来看一个例子：

```
void f(int i, std::string const& s);
std::thread t(f, 3, "hello");
```

代码创建了一个调用f(3, "hello")的线程。注意，函数f需要一个`std::string`对象作为第二个参数，但这里使用的是字符串的字面值，也就是`char const *`类型。之后，在线程的上下文中完成字面值向`std::string`对象的转化。需要特别要注意，当指向动态变量的指针作为参数传递给线程的情况，代码如下：

```
void f(int i,std::string const& s);
void oops(int some_param)
{
  char buffer[1024]; // 1
  sprintf(buffer, "%i",some_param);
  std::thread t(f,3,buffer); // 2
  t.detach();
}
```

这种情况下，buffer②是一个指针变量，指向本地变量，然后本地变量通过buffer传递到新线程中②。并且，函数有很有可能会在字面值转化成`std::string`对象之前*崩溃*(oops)，从而导致一些未定义的行为。并且想要依赖隐式转换将字面值转换为函数期待的`std::string`对象，**但因`std::thread`的构造函数会复制提供的变量，就只复制了没有转换成期望类型的字符串字面值。**

解决方案就是在传递到`std::thread`构造函数之前就将字面值转化为`std::string`对象：

```
void f(int i,std::string const& s);
void not_oops(int some_param)
{
  char buffer[1024];
  sprintf(buffer,"%i",some_param);
  std::thread t(f,3,std::string(buffer));  // 使用std::string，避免悬垂指针
  t.detach();
}
```

**还可能遇到相反的情况：期望传递一个引用，但整个对象被复制了。当线程更新一个引用传递的数据结构时，这种情况就可能发生，**比如：

```
void update_data_for_widget(widget_id w,widget_data& data); // 1
void oops_again(widget_id w)
{
  widget_data data;
  std::thread t(update_data_for_widget,w,data); // 2
  display_status();
  t.join();
  process_widget_data(data); // 3
}
```

虽然update_data_for_widget①的第二个参数期待传入一个引用，但是`std::thread`的构造函数②并不知晓；构造函数无视函数期待的参数类型，并盲目的拷贝已提供的变量。当线程调用update_data_for_widget函数时，**传递给函数的参数是data变量内部拷贝的引用，而非数据本身的引用。因此，当线程结束时，内部拷贝数据将会在数据更新阶段被销毁，且process_widget_data将会接收到没有修改的data变量③。可以使用`std::ref`将参数转换成引用的形式，**从而可将线程的调用改为以下形式：

```
std::thread t(update_data_for_widget,w,std::ref(data));
```

在这之后，update_data_for_widget就会接收到一个data变量的引用，而非一个data变量拷贝的引用。

如果你熟悉`std::bind`??，就应该不会对以上述传参的形式感到奇怪，因为**`std::thread`构造函数和`std::bind`的操作都在标准库中定义好了，可以传递一个成员函数指针作为线程函数，并提供一个合适的对象指针作为第一个参数**：

```
class X
{
public:
  void do_lengthy_work();
};
X my_x;
std::thread t(&X::do_lengthy_work,&my_x); // 1
```

这段代码中，新线程将my_x.do_lengthy_work()作为线程函数；my_x的地址①作为指针对象提供给函数。也可以为成员函数提供参数：`std::thread`构造函数的第三个参数就是成员函数的第一个参数，以此类推(代码如下，译者自加)。

```
class X
{
public:
  void do_lengthy_work(int);
};
X my_x;
int num(0);
std::thread t(&X::do_lengthy_work, &my_x, num);
```

有趣的是，**提供的参数可以*移动*，但不能*拷贝*。"移动"是指:原始对象中的数据转移给另一对象，而转移的这些数据就不再在原始对象中保存了(译者：比较像在文本编辑的"剪切"操作)。**`std::unique_ptr`就是这样一种类型(译者：C++11中的智能指针)，这种类型为动态分配的对象提供内存自动管理机制(译者：类似垃圾回收)。同一时间内，只允许一个`std::unique_ptr`实现指向一个给定对象，并且当这个实现销毁时，指向的对象也将被删除。**移动构造函数(move constructor)和移动赋值操作符(move assignment operator)允许一个对象在多个`std::unique_ptr`实现中传递(有关"移动"的更多内容，请参考附录A的A.1.1节)。使用"移动"转移原对象后，就会留下一个*空指针*(NULL)。移动操作可以将对象转换成可接受的类型，例如:函数参数或函数返回的类型。当原对象是一个临时变量时，自动进行移动操作，但当原对象是一个命名变量，那么转移的时候就需要使用`std::move()`进行显示移动。**下面的代码展示了`std::move`的用法，展示了`std::move`是如何转移一个动态对象到一个线程中去的：
```
void process_big_object(std::unique_ptr<big_object>);

std::unique_ptr<big_object> p(new big_object);
p->prepare_data(42);
std::thread t(process_big_object,std::move(p));
```

在`std::thread`的构造函数中指定`std::move(p)`,big_object对象的所有权就被首先转移到新创建线程的的内部存储中，之后传递给process_big_object函数。

标准线程库中和`std::unique_ptr`在所属权上有相似语义类型的类有好几种，`std::thread`为其中之一。**虽然，`std::thread`实例不像`std::unique_ptr`那样能占有一个动态对象的所有权，但是它能占有其他资源：每个实例都负责管理一个执行线程。执行线程的所有权可以在多个`std::thread`实例中互相转移，这是依赖于`std::thread`实例的*可移动*且*不可复制*性。不可复制性保证了在同一时间点，一个`std::thread`实例只能关联一个执行线程；可移动性使得程序员可以自己决定，哪个实例拥有实际执行线程的所有权**。

### 2.3 转移线程所有权

假设要写一个在后台启动线程的函数，想通过新线程返回的所有权去调用这个函数，而不是等待线程结束再去调用；或完全与之相反的想法：创建一个线程，并在函数中转移所有权，都必须要等待线程结束。总之，新线程的所有权都需要转移。

这就是移动引入`std::thread`的原因，C++标准库中有很多_资源占有_\(resource-owning\)类型，比如`std::ifstream`,`std::unique_ptr`还有`std::thread`都是可移动，但不可拷贝。这就说明执行线程的所有权可以在`std::thread`实例中移动，下面将展示一个例子。例子中，创建了两个执行线程，并且在`std::thread`实例之间\(t1,t2和t3\)转移所有权：

```
void some_function();
void some_other_function();
std::thread t1(some_function);            // 1
std::thread t2=std::move(t1);            // 2
t1=std::thread(some_other_function);    // 3
std::thread t3;                            // 4
t3=std::move(t2);                        // 5
t1=std::move(t3);                        // 6 赋值操作将使程序崩溃
```

当显式使用`std::move()`创建t2后②，t1的所有权就转移给了t2。之后，t1和执行线程已经没有关联了；执行some\_function的函数现在与t2关联。

然后，与一个临时`std::thread`对象相关的线程启动了③。**为什么不显式调用`std::move()`转移所有权呢？因为，所有者是一个临时对象——移动操作将会隐式的调用**。

t3使用默认构造方式创建④，与任何执行线程都没有关联。调用`std::move()`将与t2关联线程的所有权转移到t3中⑤。因为t2是一个命名对象，需要显式的调用`std::move()`。移动操作⑤完成后，t1与执行some\_other\_function的线程相关联，t2与任何线程都无关联，t3与执行some\_function的线程相关联。

最后一个移动操作，将some\_function线程的所有权转移⑥给t1。不过，t1已经有了一个关联的线程\(执行some\_other\_function的线程\)，**所以这里系统直接调用`std::terminate()`终止程序继续运行。这样做（不抛出异常，`std::terminate()`是[_noexcept_](http://www.baidu.com/link?url=5JjyAaqAzTTXfKVx1iXU2L1aR__8o4wfW4iotLW1BiUCTzDHjbGcX7Qx42FOcd0K4xe2MDFgL5r7BCiVClXCDq)函数\)是为了保证与`std::thread`的析构函数的行为一致。2.1.1节中，需要在线程对象被析构前，显式的等待线程完成，或者分离它；进行赋值时也需要满足这些条件\(说明：不能通过赋一个新值给`std::thread`对象的方式来"丢弃"一个线程\)。**

`std::thread`支持移动，就意味着线程的所有权可以在函数外进行转移，就如下面程序一样。

清单2.5 函数返回`std::thread`对象

```
std::thread f()
{
  void some_function();
  return std::thread(some_function);
}

std::thread g()
{
  void some_other_function(int);
  std::thread t(some_other_function,42);
  return t;
}
```

当所有权可以在函数内部传递，就允许`std::thread`实例可作为参数进行传递，代码如下：

```
void f(std::thread t);
void g()
{
  void some_function();
  f(std::thread(some_function));
  std::thread t(some_function);
  f(std::move(t));
}
```

**`std::thread`支持移动的好处是可以创建thread\_guard类的实例\(定义见 清单2.3\)，并且拥有其线程的所有权。当thread\_guard对象所持有的线程已经被引用，移动操作就可以避免很多不必要的麻烦；这意味着，当某个对象转移了线程的所有权后，它就不能对线程进行加入或分离。**为了确保线程程序退出前完成，下面的代码里定义了scoped\_thread类。现在，我们来看一下这段代码：

清单2.6 scoped\_thread的用法

```
class scoped_thread
{
  std::thread t;
public:
  explicit scoped_thread(std::thread t_):                 // 1
    t(std::move(t_))
  {
    if(!t.joinable())                                     // 2
      throw std::logic_error(“No thread”);
  }
  ~scoped_thread()
  {
    t.join();                                            // 3
  }
  scoped_thread(scoped_thread const&)=delete;
  scoped_thread& operator=(scoped_thread const&)=delete;
};

//struct func;  定义在清单2.1中
struct func
{
  int& i;
  func(int& i_) : i(i_) {}
  void operator() ()
  {
    for (unsigned j=0 ; j<1000000 ; ++j)
    {
      do_something(i);           // 1. 潜在访问隐患：悬空引用
    }
  }
};

void f()
{
  int some_local_state;
  scoped_thread t(std::thread(func(some_local_state)));    // 4
  do_something_in_current_thread();
}                                                        // 5
```

与清单2.3相似，不过这里新线程是直接传递到scoped\_thread中④，而非创建一个独立的命名变量。当主线程到达f\(\)函数的末尾时，scoped\_thread对象将会销毁，然后加入③到的构造函数①创建的线程对象中去。而在清单2.3中的thread\_guard类，就要在析构的时候检查线程是否"可加入"。这里把检查放在了构造函数中②，并且当线程不可加入时，抛出异常。

**`std::thread`对象的容器，如果这个容器是移动敏感的\(比如，标准中的`std::vector<>`\)，那么移动操作同样适用于这些容器。**了解这些后，就可以写出类似清单2.7中的代码，代码量产了一些线程，并且等待它们结束。

清单2.7 量产线程，等待它们结束

```
void do_work(unsigned id);

void f()
{
  std::vector<std::thread> threads;
  for(unsigned i=0; i < 20; ++i)
  {
    threads.push_back(std::thread(do_work,i)); // 产生线程
  } 
  std::for_each(threads.begin(),threads.end(),
                  std::mem_fn(&std::thread::join)); // 对每个线程调用join()
}
```

我们经常需要线程去分割一个算法的总工作量，所以在算法结束的之前，所有的线程必须结束。清单2.7说明线程所做的工作都是独立的，并且结果仅会受到共享数据的影响。如果f\(\)有返回值，这个返回值就依赖于线程得到的结果。在写入返回值之前，程序会检查使用共享数据的线程是否终止。操作结果在不同线程中转移的替代方案，我们会在第4章中再次讨论。

**将`std::thread`放入`std::vector`是向线程自动化管理迈出的第一步：并非为这些线程创建独立的变量，并且将他们直接加入，可以把它们当做一个组。创建一组线程\(数量在运行时确定\)，可使得这一步迈的更大，而非像清单2.7那样创建固定数量的线程**。



### 2.4 运行时决定线程数量

**`std::thread::hardware_concurrency()`在新版C++标准库中是一个很有用的函数。这个函数将返回能同时并发在一个程序中的线程数量。例如，多核系统中，返回值可以是CPU核芯的数量。返回值也仅仅是一个提示，当系统信息无法获取时，函数也会返回0。**但是，这也无法掩盖这个函数对启动线程数量的帮助。

清单2.8实现了一个并行版的`std::accumulate`。代码中将整体工作拆分成小任务交给每个线程去做，其中设置最小任务数，是为了避免产生太多的线程。程序可能会在操作数量为0的时候抛出异常。比如，`std::thread`构造函数无法启动一个执行线程，就会抛出一个异常。在这个算法中讨论异常处理，已经超出现阶段的讨论范围，这个问题我们将在第8章中再来讨论。

清单2.8 原生并行版的`std::accumulate`

```
template<typename Iterator,typename T>
struct accumulate_block
{
  void operator()(Iterator first,Iterator last,T& result)
  {
    result=std::accumulate(first,last,result);
  }
};

template<typename Iterator,typename T>
T parallel_accumulate(Iterator first,Iterator last,T init)
{
  unsigned long const length=std::distance(first,last);

  if(!length) // 1
    return init;

  unsigned long const min_per_thread=25;
  unsigned long const max_threads=
      (length+min_per_thread-1)/min_per_thread; // 2

  unsigned long const hardware_threads=
      std::thread::hardware_concurrency();

  unsigned long const num_threads=  // 3
      std::min(hardware_threads != 0 ? hardware_threads : 2, max_threads);

  unsigned long const block_size=length/num_threads; // 4

  std::vector<T> results(num_threads);
  std::vector<std::thread> threads(num_threads-1);  // 5

  Iterator block_start=first;
  for(unsigned long i=0; i < (num_threads-1); ++i)
  {
    Iterator block_end=block_start;
    std::advance(block_end,block_size);  // 6
    threads[i]=std::thread(     // 7
        accumulate_block<Iterator,T>(),
        block_start,block_end,std::ref(results[i]));
    block_start=block_end;  // 8
  }
  accumulate_block<Iterator,T>()(
      block_start,last,results[num_threads-1]); // 9
  std::for_each(threads.begin(),threads.end(),
       std::mem_fn(&std::thread::join));  // 10 这里会阻塞等待线程执行完??

  return std::accumulate(results.begin(),results.end(),init); // 11
}
```

函数看起来很长，但不复杂。如果输入的范围为空①，就会得到init的值。反之，如果范围内多于一个元素时，都需要用范围内元素的总数量除以线程(块)中最小任务数，从而确定启动线程的最大数量②，这样能避免无谓的计算资源的浪费。比如，一台32芯的机器上，只有5个数需要计算，却启动了32个线程。

计算量的最大值和硬件支持线程数中，较小的值为启动线程的数量③。因为上下文频繁的切换会降低线程的性能，所以你肯定不想启动的线程数多于硬件支持的线程数量。当`std::thread::hardware_concurrency()`返回0，你可以选择一个合适的数作为你的选择；在本例中,我选择了"2"。你也不想在一台单核机器上启动太多的线程，因为这样反而会降低性能，有可能最终让你放弃使用并发。

每个线程中处理的元素数量,是范围中元素的总量除以线程的个数得出的④。对于分配是否得当，我们会在后面讨论。

现在，确定了线程个数，通过创建一个`std::vector<T>`容器存放中间结果，并为线程创建一个`std::vector<std::thread>`容器⑤。这里需要注意的是，启动的线程数必须比num_threads少1个，因为在启动之前已经有了一个线程(主线程)。

使用简单的循环来启动线程：block_end迭代器指向当前块的末尾⑥，并启动一个新线程为当前块累加结果⑦。当迭代器指向当前块的末尾时，启动下一个块⑧。

启动所有线程后，⑨中的线程会处理最终块的结果。对于分配不均，因为知道最终块是哪一个，那么这个块中有多少个元素就无所谓了。

当累加最终块的结果后，可以等待`std::for_each`⑩创建线程的完成(如同在清单2.7中做的那样)，之后使用`std::accumulate`将所有结果进行累加⑪。

结束这个例子之前，需要明确：T类型的加法运算不满足结合律(比如，对于float型或double型，在进行加法操作时，系统很可能会做截断操作)，因为对范围中元素的分组，会导致parallel_accumulate得到的结果可能与`std::accumulate`得到的结果不同。同样的，这里对迭代器的要求更加严格：必须都是向前迭代器，而`std::accumulate`可以在只传入迭代器的情况下工作。对于创建出results容器，需要保证T有默认构造函数。对于算法并行，通常都要这样的修改；不过，需要根据算法本身的特性，选择不同的并行方式。算法并行会在第8章有更加深入的讨论。需要注意的：**因为不能直接从一个线程中返回一个值，所以需要传递results容器的引用到线程中去。另一个办法，通过地址来获取线程执行的结果；第4章中，我们将使用*期望*(futures)完成这种方案。**

当线程运行时，所有必要的信息都需要传入到线程中去，包括存储计算结果的位置。不过，并非总需如此：有时候这是识别线程的可行方案，可以传递一个标识数，例如清单2.7中的i。不过，当需要标识的函数在调用栈的深层，同时其他线程也可调用该函数，那么标识数就会变的捉襟见肘。好消息是在设计C++的线程库时，就有预见了这种情况，在之后的实现中就给每个线程附加了唯一标识符。


### 2.5 识别线程

线程标识类型是`std::thread::id`，可以通过两种方式进行检索。第一种，可以通过调用`std::thread`对象的成员函数`get_id()`来直接获取。如果`std::thread`对象没有与任何执行线程相关联，`get_id()`将返回`std::thread::type`默认构造值，这个值表示“没有线程”。第二种，当前线程中调用`std::this_thread::get_id()`(这个函数定义在`<thread>`头文件中)也可以获得线程标识。

`std::thread::id`对象可以自由的拷贝和对比,因为标识符就可以复用。如果两个对象的`std::thread::id`相等，那它们就是同一个线程，或者都“没有线程”。如果不等，那么就代表了两个不同线程，或者一个有线程，另一没有。

线程库不会限制你去检查线程标识是否一样，`std::thread::id`类型对象提供相当丰富的对比操作；比如，提供为不同的值进行排序。这意味着允许程序员将其当做为容器的键值，做排序，或做其他方式的比较。按默认顺序比较不同值的`std::thread::id`，所以这个行为可预见的：当`a<b`，`b<c`时，得`a<c`，等等。标准库也提供`std::hash<std::thread::id>`容器，所以`std::thread::id`也可以作为无序容器的键值。

`std::thread::id`实例常用作检测线程是否需要进行一些操作，比如：当用线程来分割一项工作(如清单2.8)，主线程可能要做一些与其他线程不同的工作。**这种情况下，启动其他线程前，它可以将自己的线程ID通过`std::this_thread::get_id()`得到，并进行存储。就是算法核心部分(所有线程都一样的),每个线程都要检查一下，其拥有的线程ID是否与初始线程的ID相同**。

```
std::thread::id master_thread;
void some_core_part_of_algorithm()
{
  if(std::this_thread::get_id()==master_thread)
  {
    do_master_thread_work();
  }
  do_common_work();
}
```

另外，当前线程的`std::thread::id`将存储到一个数据结构中。之后在这个结构体中对当前线程的ID与存储的线程ID做对比，来决定操作是被“允许”，还是“需要”(permitted/required)??。

> **[线程局部存储（或者叫线程本地存储 Thread Local Storage, TLS)](https://blog.csdn.net/xtx1990/article/details/8231190)**

同样，作为线程和本地存储不适配的替代方案??，线程ID在容器中可作为键值。例如，容器可以存储其掌控下每个线程的信息，或在多个线程中互传信息。

`std::thread::id`可以作为一个线程的通用标识符，当标识符只与语义相关(比如，数组的索引)时，就需要这个方案了。也可以使用输出流(`std::cout`)来记录一个`std::thread::id`对象的值。

```
std::cout<<std::this_thread::get_id();
```

具体的输出结果是严格依赖于具体实现的，C++标准的唯一要求就是要保证ID比较结果相等的线程，必须有相同的输出。

### 2.6 本章总结

本章讨论了C++标准库中基本的线程管理方式：启动线程，等待结束和不等待结束(因为需要它们运行在后台)。并了解应该如何在线程启动前，向线程函数中传递参数，如何转移线程的所有权，如何使用线程组来分割任务。最后，讨论了使用线程标识来确定关联数据，以及特殊线程的特殊解决方案。虽然，现在已经可以纯粹的依赖线程，使用独立的数据，做独立的任务(如同清单2.8)，但在某些情况下，线程确实需要有共享数据。第3章会讨论共享数据和线程的直接关系。第4章会讨论在(有/没有)共享数据情况下的线程同步操作。

## 第3章 线程间共享数据

**本章主要内容**

- 共享数据带来的问题<br>
- 使用互斥量保护数据<br>
- 数据保护的替代方案<br>

上一章中，我们已经对线程管理有所了解了，现在让我们来看一下“共享数据的那些事”。

想象一下，你和你的朋友合租一个公寓，公寓中只有一个厨房和一个卫生间。当你的朋友在卫生间时，你就会不能使用了(除非你们特别好，好到可以在同时使用一个房间)。这个问题也会出现在厨房，假如:厨房里有一个组合式烤箱，当在烤香肠的时候，也在做蛋糕，就可能得到我们不想要的食物(香肠味的蛋糕)。此外，在公共空间将一件事做到一半时，发现某些需要的东西被别人借走，或是当离开的一段时间内有些东西被变动了地方，这都会令我们不爽。

同样的问题，也困扰着线程。当线程在访问共享数据的时候，必须定一些规矩，用来限定线程可访问的数据位。还有，一个线程更新了共享数据，需要对其他线程进行通知。从易用性的角度，同一进程中的多个线程进行数据共享，有利有弊。错误的共享数据使用是产生并发bug的一个主要原因，并且后果要比香肠味的蛋糕更加严重。

**本章就以在C++中进行安全的数据共享为主题。避免上述及其他潜在问题的发生的同时，将共享数据的优势发挥到最大。**

### 3.1 共享数据带来的问题

当涉及到共享数据时，问题很可能是因为共享数据修改所导致。如果共享数据是只读的，那么只读操作不会影响到数据，更不会涉及对数据的修改，所以所有线程都会获得同样的数据。但是，当一个或多个线程要修改共享数据时，就会产生很多麻烦。这种情况下，就必须小心谨慎，才能确保一切所有线程都工作正常。

**不变量**(invariants)的概念对程序员们编写的程序会有一定的帮助——对于特殊结构体的描述；比如，“变量包含列表中的项数”。不变量通常会在一次更新中被破坏，特别是比较复杂的数据结构，或者一次更新就要改动很大的数据结构。

双链表中每个节点都有一个指针指向列表中 下一个节点，还有一个指针指向前一个节点。其中不变量就是节点A中指向“下一个”节点B的指针，还有前向指针。为了从列表中删除一个节点，其两边节点的指针都需要更新。当其中一边更新完成时，不变量就被破坏了，直到另一边也完成更新；在两边都完成更新后，不变量就又稳定了。

从一个列表中删除一个节点的步骤如下(如图3.1)

1. 找到要删除的节点N<br>
2. 更新前一个节点指向N的指针，让这个指针指向N的下一个节点<br>
3. 更新后一个节点指向N的指针，让这个指正指向N的前一个节点<br>
4. 删除节点N<br>

![](https://github.com/xiaoweiChen/Cpp_Concurrency_In_Action/blob/master/images/chapter3/3-1.png?raw=true)

图3.1 从一个双链表中删除一个节点

图中b和c在相同的方向上指向和原来已经不一致了，这就破坏了不变量。

线程间潜在问题就是修改共享数据，致使不变量遭到破坏。当不做些事来确保在这个过程中不会有其他线程进行访问的话，可能就有线程访问到刚刚删除一边的节点；这样的话，线程就读取到要删除节点的数据(因为只有一边的连接被修改，如图3.1(b))，所以不变量就被破坏。破坏不变量的后果是多样，当其他线程按从左往右的顺序来访问列表时，它将跳过被删除的节点。在一方面，如有第二个线程尝试删除图中右边的节点，那么可能会让数据结构产生永久性的损坏，使程序崩溃。无论结果如何，都是并行代码常见错误：**条件竞争**。

#### 3.1.1 条件竞争

假设你去电影院买电影票。如果去的是一家大电影院，有很多收银台，很多人就可以在同一时间买电影票。当另一个收银台也在卖你想看的这场电影的电影票，那么你的座位选择范围就取决于在之前已预定的座位。当只有少量的座位剩下，这就意味着，这可能是一场抢票比赛，看谁能抢到最后一张票。这就是一个条件竞争的例子：你的座位(或者你的电影票)都取决于两种购买方式的相对顺序。

并发中竞争条件的形成，取决于一个以上线程的相对执行顺序，每个线程都抢着完成自己的任务。大多数情况下，即使改变执行顺序，也是良性竞争，其结果可以接受。**例如，有两个线程同时向一个处理队列中添加任务，因为系统提供的不变量保持不变，所以谁先谁后都不会有什么影响。当不变量遭到破坏时，才会产生条件竞争**，比如双向链表的例子。并发中对数据的条件竞争通常表示为恶性条件竞争，我们对不产生问题的良性条件竞争不感兴趣。`C++`标准中也定义了数据竞争这个术语，一种特殊的条件竞争：**并发的去修改一个独立对象(参见5.1.2节)，数据竞争是(可怕的)未定义行为的起因。**

**恶性条件竞争通常发生于完成对多于一个的数据块的修改时**，例如，对两个连接指针的修改(如图3.1)。因为操作要访问两个独立的数据块，独立的指令将会对数据块将进行修改，并且其中一个线程可能正在进行时，另一个线程就对数据块进行了访问。因为出现的概率太低，条件竞争很难查找，也很难复现。如CPU指令连续修改完成后，即使数据结构可以让其他并发线程访问，问题再次复现的几率也相当低。当系统负载增加时，随着执行数量的增加，执行序列的问题复现的概率也在增加，这样的问题只可能会出现在负载比较大的情况下。**条件竞争通常是时间敏感的，所以程序以调试模式运行时，它们常会完全消失，因为调试模式会影响程序的执行时间(即使影响不多)**。

当你以写多线程程序为生，条件竞争就会成为你的梦魇；编写软件时，我们会使用大量复杂的操作，用来避免恶性条件竞争。

#### 3.1.2 避免恶性条件竞争

这里提供一些方法来解决恶性条件竞争，**最简单的办法就是对数据结构采用某种保护机制，确保只有进行修改的线程才能看到不变量被破坏时的中间状态。从其他访问线程的角度来看，修改不是已经完成了，就是还没开始**。`C++`标准库提供很多类似的机制，下面会逐一介绍。

**另一个选择是对数据结构和不变量的设计进行修改，修改完的结构必须能完成一系列不可分割的变化，也就是保证每个不变量保持稳定的状态，这就是所谓的无锁编程??**。不过，这种方式很难得到正确的结果??。如果到这个级别，无论是内存模型上的细微差异，还是线程访问数据的能力，都会让工作变的复杂。内存模型将在第5章讨论，无锁编程将在第7章讨论。

另一种处理条件竞争的方式是，**使用事务的方式去处理数据结构的更新(这里的"处理"就如同对数据库进行更新一样)。所需的一些数据和读取都存储在事务日志中，然后将之前的操作合为一步，再进行提交。当数据结构被另一个线程修改后，或处理已经重启的情况下，提交就会无法进行，这称作为“软件事务内存”。理论研究中，这是一个很热门的研究领域。这个概念将不会在本书中再进行介绍，因为在`C++`中没有对STM进行直接支持。**但是，基本思想会在后面提及。

**保护共享数据结构的最基本的方式，是使用C++标准库提供的互斥量**。



### 3.2 使用互斥量保护共享数据

当程序中有共享数据，肯定不想让其陷入条件竞争，或是不变量被破坏。那么，将所有访问共享数据结构的代码都标记为互斥岂不是更好？这样任何一个线程在执行这些代码时，其他任何线程试图访问共享数据结构，就必须等到那一段代码执行结束。于是，一个线程就不可能会看到被破坏的不变量，除非它本身就是修改共享数据的线程。

当访问共享数据前，使用互斥量将相关数据锁住，再当访问结束后，再将数据解锁。线程库需要保证，当一个线程使用特定互斥量锁住共享数据时，其他的线程想要访问锁住的数据，都必须等到之前那个线程对数据进行解锁后，才能进行访问。这就保证了所有线程能看到共享数据，而不破坏不变量。

互斥量是`C++`中一种最通用的数据保护机制，但它不是“银弹”；精心组织代码来保护正确的数据(见3.2.2节)，并在接口内部避免竞争条件(见3.2.3节)是非常重要的。但互斥量自身也有问题，也会造成死锁(见3.2.4节)，或是对数据保护的太多(或太少)(见3.2.8节)。

### 3.2.1 C++中使用互斥量

C++中通过实例化`std::mutex`创建互斥量，通过调用成员函数lock()进行上锁，unlock()进行解锁。不过，**不推荐实践中直接去调用成员函数，因为调用成员函数就意味着，必须记住在每个函数出口都要去调用unlock()，也包括异常的情况。C++标准库为互斥量提供了一个RAII语法的模板类`std::lock_guard`，其会在构造的时候提供已锁的互斥量，并在析构的时候进行解锁，从而保证了一个已锁的互斥量总是会被正确的解锁。下面的程序清单中，展示了如何在多线程程序中，使用`std::mutex`构造的`std::lock_guard`实例，对一个列表进行访问保护。`std::mutex`和`std::lock_guard`都在`<mutex>`头文件中声明。**

清单3.1 使用互斥量保护列表

```
#include <list>
#include <mutex>
#include <algorithm>

std::list<int> some_list;    // 1
std::mutex some_mutex;    // 2

void add_to_list(int new_value)
{
  std::lock_guard<std::mutex> guard(some_mutex);    // 3
  some_list.push_back(new_value);
}

bool list_contains(int value_to_find)
{
  std::lock_guard<std::mutex> guard(some_mutex);    // 4
  return std::find(some_list.begin(),some_list.end(),value_to_find) != some_list.end();
}
```

清单3.1中有一个全局变量①，这个全局变量被一个**全局的互斥量**保护②。add_to_list()③和list_contains()④函数中使用`std::lock_guard<std::mutex>`，使得这两个函数中对数据的访问是互斥的：list_contains()不可能看到正在被add_to_list()修改的列表。

**虽然某些情况下，使用全局变量没问题，但在大多数情况下，互斥量通常会与保护的数据放在同一个类中，而不是定义成全局变量。这是面向对象设计的准则：将其放在一个类中，就可让他们联系在一起，也可对类的功能进行封装，并进行数据保护。**在这种情况下，函数add_to_list和list_contains可以作为这个类的成员函数。互斥量和要保护的数据，在类中都需要定义为private成员，这会让访问数据的代码变的清晰，并且容易看出在什么时候对互斥量上锁。当所有成员函数都会在调用时对数据上锁，结束时对数据解锁，那么就保证了数据访问时不变量不被破坏。

当然，也不是总是那么理想，聪明的你一定注意到了：当其中一个成员函数返回的是保护数据的指针或引用时，会破坏对数据的保护。具有访问能力的指针或引用可以访问(并可能修改)被保护的数据，而不会被互斥锁限制。**互斥量保护的数据需要对接口的设计相当谨慎，要确保互斥量能锁住任何对保护数据的访问，并且不留后门。**

#### 3.2.2 精心组织代码来保护共享数据

使用互斥量来保护数据，并不是仅仅在每一个成员函数中都加入一个`std::lock_guard`对象那么简单；一个迷失的指针或引用，将会让这种保护形同虚设。不过，检查迷失指针或引用是很容易的，只要没有成员函数通过返回值或者输出参数的形式向其调用者返回指向受保护数据的指针或引用，数据就是安全的。如果你还想往祖坟上刨，就没这么简单了。**在确保成员函数不会传出指针或引用的同时，检查成员函数是否通过指针或引用的方式来调用也是很重要的(尤其是这个操作不在你的控制下时)。函数可能没在互斥量保护的区域内，存储着指针或者引用，这样就很危险。更危险的是：将保护数据作为一个运行时参数**，如同下面清单中所示那样。

清单3.2 无意中传递了保护数据的引用

```
class some_data
{
  int a;
  std::string b;
public:
  void do_something();
};

class data_wrapper
{
private:
  some_data data;
  std::mutex m;
public:
  template<typename Function>
  void process_data(Function func)
  {
    std::lock_guard<std::mutex> l(m);//使用互斥量保护data 
    func(data);    // 1 传递“保护”数据给用户函数 这个函数是可能是引用传递参数，把共享数据传到外面，危险
  }
};

some_data* unprotected;
//把模板函数定义为引用传递 
void malicious_function(some_data& protected_data)
{
  unprotected=&protected_data;//共享数据被传出来了，不再受互斥量保护 
}

data_wrapper x;
void foo()
{
  x.process_data(malicious_function);    // 2 传递一个恶意函数
  unprotected->do_something();    // 3 在无保护的情况下访问保护数据
}
```

例子中process_data看起来没有任何问题，`std::lock_guard`对数据做了很好的保护，但调用用户提供的函数func①，就意味着foo能够绕过保护机制将函数`malicious_function`传递进去②，在没有锁定互斥量的情况下调用`do_something()`。

**这段代码的问题在于根本没有保护，只是将所有可访问的数据结构代码标记为互斥。**函数`foo()`中调用`unprotected->do_something()`的代码未能被标记为互斥。这种情况下，C++线程库无法提供任何帮助，只能由程序员来使用正确的互斥锁来保护数据。从乐观的角度上看，还是有方法可循的：**切勿将受保护数据的指针或引用传递到互斥锁作用域之外，无论是函数返回值，还是存储在外部可见内存，亦或是以参数的形式传递到用户提供的函数中去。**

> **切勿将受保护数据的指针或引用传递到互斥锁作用域之外，无论是函数返回值，还是存储在外部可见内存，亦或是以参数的形式传递到用户提供的函数中去。**

虽然这是在使用互斥量保护共享数据时常犯的错误，但绝不仅仅是一个潜在的陷阱而已。下一节中，你将会看到，即便是使用了互斥量对数据进行了保护，条件竞争依旧可能存在。

### 3.2.3 发现接口内在的条件竞争

**因为使用了互斥量或其他机制保护了共享数据，就不必再为条件竞争所担忧吗？并不是，你依旧需要确定数据受到了保护。**回想之前双链表的例子，为了能让线程安全地删除一个节点，需要确保防止对这三个节点(待删除的节点及其前后相邻的节点)的并发访问。如果只对指向每个节点的指针进行访问保护，那就和没有使用互斥量一样，条件竞争仍会发生——除了指针，整个数据结构和整个删除操作需要保护。这种情况下最简单的解决方案就是使用互斥量来保护整个链表，如清单3.1所示。

**尽管链表的个别操作是安全的，但不意味着你就能走出困境；**即使在一个很简单的接口中，依旧可能遇到条件竞争。例如，构建一个类似于`std::stack`结构的栈(清单3.3)，除了构造函数和swap()以外，需要对`std::stack`提供五个操作：push()一个新元素进栈，pop()一个元素出栈，top()查看栈顶元素，empty()判断栈是否是空栈，size()了解栈中有多少个元素。即使修改了top()，使其返回一个拷贝而非引用(即遵循了3.2.2节的准则 )，对内部数据使用一个互斥量进行保护，不过这个接口仍存在条件竞争。这个问题不仅存在于基于互斥量实现的接口中，在无锁实现的接口中，条件竞争依旧会产生。**这是接口的问题，与其实现方式无关**。

>3.2.2节的准则 在确保成员函数不会传出指针或引用的同时，检查成员函数是否通过指针或引用的方式来调用也是很重要的(尤其是这个操作不在你的控制下时)。函数可能没在互斥量保护的区域内，存储着指针或者引用，这样就很危险。更危险的是：将保护数据作为一个运行时参数 ;切勿将受保护数据的指针或引用传递到互斥锁作用域之外，无论是函数返回值，还是存储在外部可见内存，亦或是以参数的形式传递到用户提供的函数中去。

清单3.3 `std::stack`容器的实现

```
template<typename T,typename Container=std::deque<T> >
class stack
{
public:
  explicit stack(const Container&);
  explicit stack(Container&& = Container());
  template <class Alloc> explicit stack(const Alloc&);
  template <class Alloc> stack(const Container&, const Alloc&);
  template <class Alloc> stack(Container&&, const Alloc&);
  template <class Alloc> stack(stack&&, const Alloc&);
  
  bool empty() const;
  size_t size() const;
  T& top();  T const& top() const;//即使修改了top()，使其返回一个拷贝而非引用(即遵循了3.2.2节的准则)，对内部数据使用一个互斥量进行保护，不过这个接口仍存在条件竞争。
  void push(T const&);
  void push(T&&);
  void pop();
  void swap(stack&&);
};
```

虽然empty()和size()可能在被调用并返回时是正确的，但其的结果是不可靠的；当它们返回后，其他线程就可以自由地访问栈，并且可能push()多个新元素到栈中，也可能pop()一些已在栈中的元素。这样的话，之前从empty()和size()得到的结果就有问题了。

特别地，当栈实例是**非共享的**，如果栈非空，使用empty()检查再调用top()访问栈顶部的元素是安全的。如下代码所示：

```
stack<int> s;
if (! s.empty()){    // 1
  int const value = s.top();    // 2
  s.pop();    // 3
  do_something(value);
}
```

以上是单线程安全代码：对一个空栈使用top()是未定义行为。对于**共享的栈对象**，这样的调用顺序就不再安全了，因为在调用empty()①和调用top()②之间，可能有来自另一个线程的pop()调用并删除了最后一个元素。**这是一个经典的条件竞争，使用互斥量对栈内部数据进行保护，但依旧不能阻止条件竞争的发生，这就是接口固有的问题。**

怎么解决呢？问题发生在接口设计上，所以解决的方法也就是改变接口设计。有人会问：怎么改？在这个简单的例子中，当调用top()时，发现栈已经是空的了，那么就抛出异常。虽然这能直接解决这个问题，但这是一个笨拙的解决方案，这样的话，即使empty()返回false的情况下，你也需要异常捕获机制。本质上，这样的改变会让empty()成为一个多余函数。

当仔细的观察过之前的代码段，就会发现另一个潜在的条件竞争在调用top()②和pop()③之间。假设两个线程运行着前面的代码，并且都引用同一个栈对象s。这并非罕见的情况，当为性能而使用线程时，多个线程在不同的数据上执行相同的操作是很平常的，并且共享同一个栈可以将工作分摊给它们。假设，一开始栈中只有两个元素，**这时任一线程上的empty()和top()都存在竞争**，只需要考虑可能的执行顺序即可。

当栈被一个内部互斥量所保护时，只有一个线程可以调用栈的成员函数，所以调用可以很好地交错，并且do_something()是可以并发运行的。在表3.1中，展示一种可能的执行顺序。

表3.1 一种可能执行顺序

| Thread A                   | Thread B                   |
| -------------------------- | -------------------------- |
| if (!s.empty);             |                            |
|                            | if(!s.empty);              |
| int const value = s.top(); |                            |
|                            | int const value = s.top(); |
| s.pop();                   |                            |
| do_something(value);       | s.pop();                   |
|                            | do_something(value);       |

当线程运行时，调用两次top()，栈没被修改，所以每个线程能得到同样的值。不仅是这样，在调用top()函数调用的过程中(两次)，pop()函数都没有被调用。这样，在其中一个值再读取的时候，虽然不会出现“写后读”的情况，但其值已被处理了两次。这种条件竞争，比未定义的empty()/top()竞争更加严重；虽然其结果依赖于do_something()的结果，但因为看起来没有任何错误，就会让这个Bug很难定位。

**这就需要接口设计上有较大的改动，提议之一就是使用同一互斥量来保护top()和pop()。** Tom Cargill[1]指出当一个对象的拷贝构造函数在栈中抛出一个异常，这样的处理方式就会有问题。??在Herb Sutter[2]看来，这个问题可以从“异常安全”的角度完美解决，不过潜在的条件竞争，可能会组成一些新的组合。??

说一些大家没有意识到的问题：假设有一个`stack<vector<int>>`，vector是一个动态容器，当你拷贝一个vetcor，标准库会从堆上分配很多内存来完成这次拷贝。当这个系统处在重度负荷，或有严重的资源限制的情况下，这种内存分配就会失败，所以vector的拷贝构造函数可能会抛出一个`std::bad_alloc`异常。当vector中存有大量元素时，这种情况发生的可能性更大。当pop()函数返回“弹出值”时(也就是从栈中将这个值移除)，会有一个潜在的问题：这个值被返回到调用函数的时候，栈才被改变；但当拷贝数据的时候，调用函数抛出一个异常会怎么样？ 如果事情真的发生了，要弹出的数据将会丢失；它的确从栈上移出了，但是拷贝失败了！`std::stack`的设计人员将这个操作分为两部分：**先获取顶部元素(top())，然后从栈中移除(pop())。这样，在不能安全的将元素拷贝出去的情况下，栈中的这个数据还依旧存在，没有丢失。当问题是堆空间不足，应用可能会释放一些内存，然后再进行尝试。**

不幸的是，这样的分割却制造了本想避免或消除的条件竞争。幸运的是，**我们还有的别的选项，但是使用这些选项是要付出代价的。**

**选项1： 传入一个引用**  
>  主要目的  防止堆空间不足导致保护共享数据过程中丢失数据 , 但是对于一些类型，从时间和资源的角度上来看这样做是不现实的

第一个选项是将变量的引用作为参数，传入pop()函数中获取想要的“弹出值”：

```
std::vector<int> result;
some_stack.pop(result);
```

大多数情况下，这种方式还不错，但有明显的缺点：**需要构造出一个栈中类型的实例，用于接收目标值。对于一些类型，这样做是不现实的，因为临时构造一个实例，从时间和资源的角度上来看，都是不划算。对于其他的类型，这样也不总能行得通，因为构造函数需要的一些参数，在代码的这个阶段不一定可用。最后，需要可赋值的存储类型，这是一个重大限制：即使支持移动构造，甚至是拷贝构造(从而允许返回一个值)，很多用户自定义类型可能都不支持赋值操作**。

**选项2：无异常抛出的拷贝构造函数或移动构造函数**
> 主要目的  不让异常中断线程  虽然安全，但非可靠， 这种方式的局限性太强

对于有返回值的pop()函数来说，只有“异常安全”方面的担忧(当返回值时可以抛出一个异常)。很多类型都有拷贝构造函数，它们不会抛出异常，并且随着新标准中对“右值引用”的支持(详见附录A，A.1节)，很多类型都将会有一个移动构造函数，即使他们和拷贝构造函数做着相同的事情，它也不会抛出异常。一个有用的选项可以限制对线程安全的栈的使用，并且能让栈安全的返回所需的值，而不会抛出异常。

虽然安全，但非可靠。尽管能在编译时可使用`std::is_nothrow_copy_constructible`和`std::is_nothrow_move_constructible`类型特征，让拷贝或移动构造函数不抛出异常，但是这种方式的局限性太强。用户自定义的类型中，会有不抛出异常的拷贝构造函数或移动构造函数的类型， 那些有抛出异常的拷贝构造函数，但没有移动构造函数的类型往往更多（这种情况会随着人们习惯于C++11中的右值引用而有所改变)。如果这些类型不能被存储在线程安全的栈中，那将是多么的不幸。

**选项3：返回指向弹出值的指针**
> 主要目的 指针的优势是自由拷贝，并且不会产生异常 ，相较于非线程安全版本，这个方案的开销相当大

第三个选择是返回一个指向弹出元素的指针，而不是直接返回值。指针的优势是自由拷贝，并且不会产生异常，这样你就能避免Cargill提到的异常问题了。缺点就是返回一个指针需要对对象的内存分配进行管理，对于简单数据类型(比如：int)，内存管理的开销要远大于直接返回值。对于选择这个方案的接口，使用`std::shared_ptr`是个不错的选择；不仅能避免内存泄露(因为当对象中指针销毁时，对象也会被销毁)，而且标准库能够完全控制内存分配方案，也就不需要new和delete操作。这种优化是很重要的：**因为堆栈中的每个对象，都需要用new进行独立的内存分配，相较于非线程安全版本，这个方案的开销相当大。**


> 前面 1，3项都是在避免 创建栈中类型的实例     2项是避免 拷贝构造 移动构造 异常

**选项4：“选项1 + 选项2”或 “选项1 + 选项3”**

对于通用的代码来说，灵活性不应忽视。当你已经选择了选项2或3时，再去选择1也是很容易的。这些选项提供给用户，**让用户自己选择对于他们自己来说最合适，最经济的方案。**

**例：定义线程安全的堆栈**

清单3.4中是一个接口没有条件竞争的堆栈类定义，它实现了选项1和选项3：重载了pop()，使用一个局部引用去存储弹出值，并返回一个`std::shared_ptr<>`对象。它有一个简单的接口，只有两个函数：push()和pop();

清单3.4 线程安全的堆栈类定义(概述)

```
#include <exception>
#include <memory>  // For std::shared_ptr<>

struct empty_stack: std::exception
{
  const char* what() const throw();// 这个函数不会throw任何exception
};

template<typename T>
class threadsafe_stack
{
public:
  threadsafe_stack();
  threadsafe_stack(const threadsafe_stack&);
  threadsafe_stack& operator=(const threadsafe_stack&) = delete; // 1 赋值操作被删除

  void push(T new_value);
  std::shared_ptr<T> pop();//选项3 返回弹出元素的指针
  void pop(T& value);//选项1 使用引用
  bool empty() const;
};
```

削减接口可以获得最大程度的安全,甚至限制对栈的一些操作。栈是不能直接赋值的，因为赋值操作已经删除了①(详见附录A，A.2节)，并且这里没有swap()函数。栈可以拷贝的，假设栈中的元素可以拷贝。当栈为空时，pop()函数会抛出一个empty_stack异常，所以在empty()函数被调用后，其他部件还能正常工作。如选项3描述的那样，使用`std::shared_ptr`可以避免内存分配管理的问题，并避免多次使用new和delete操作。堆栈中的五个操作，现在就剩下三个：push(), pop()和empty()(这里empty()都有些多余)。**简化接口更有利于数据控制，可以保证互斥量将一个操作完全锁住。**下面的代码将展示一个简单的实现——封装`std::stack<>`的线程安全堆栈。

清单3.5 扩充(线程安全)堆栈

```
#include <exception>
#include <memory>
#include <mutex>
#include <stack>

struct empty_stack: std::exception
{
  const char* what() const throw() {
	return "empty stack!";
  };//不抛出任何异常
};

template<typename T>
class threadsafe_stack
{
private:
  std::stack<T> data;
  mutable std::mutex m;//被mutable修饰的变量，将永远处于可变的状态，即使在一个const函数中 
  
public:
  threadsafe_stack()
	: data(std::stack<T>()){}
  
  threadsafe_stack(const threadsafe_stack& other)
  {
    std::lock_guard<std::mutex> lock(other.m);
    data = other.data; // 1 在构造函数体中的执行拷贝
  }

  threadsafe_stack& operator=(const threadsafe_stack&) = delete;

  void push(T new_value)
  {
    std::lock_guard<std::mutex> lock(m);
    data.push(new_value);
  }
  
  std::shared_ptr<T> pop()
  {
    std::lock_guard<std::mutex> lock(m);
    if(data.empty()) throw empty_stack(); // 在调用pop前，检查栈是否为空
	
    std::shared_ptr<T> const res(std::make_shared<T>(data.top())); // 在修改堆栈前，分配出返回值
    data.pop();
    return res;
  }
  
  void pop(T& value)
  {
    std::lock_guard<std::mutex> lock(m);
    if(data.empty()) throw empty_stack();
	
    value=data.top();
    data.pop();
  }
  
  bool empty() const
  {
    std::lock_guard<std::mutex> lock(m);
    return data.empty();
  }
};
```

堆栈可以拷贝——拷贝构造函数对互斥量上锁，再拷贝堆栈。构造函数体中①的拷贝使用互斥量来确保复制结果的正确性，这样的方式比成员初始化列表好。

之前对top()和pop()函数的讨论中，**恶性条件竞争已经出现，因为锁的粒度太小，需要保护的操作并未全覆盖到。不过，锁住的颗粒过大同样会有问题。**还有一个问题，一个全局互斥量要去保护全部共享数据，在一个系统中存在有大量的共享数据时，因为线程可以强制运行，甚至可以访问不同位置的数据，抵消了并发带来的性能提升。在第一版为多处理器系统设计Linux内核中，就使用了一个全局内核锁。虽然这个锁能正常工作，但在双核处理系统的上的性能要比两个单核系统的性能差很多，四核系统就更不能提了。太多请求去竞争占用内核，使得依赖于处理器运行的线程没有办法很好的工作。随后修正的Linux内核加入了一个细粒度锁方案，因为少了很多内核竞争，这时四核处理系统的性能就和单核处理的四倍差不多了。

使用多个互斥量保护所有的数据，细粒度锁也有问题。如前所述，当增大互斥量覆盖数据的粒度时，只需要锁住一个互斥量。但是，这种方案并非放之四海皆准，比如：互斥量正在保护一个独立类的实例；这种情况下，锁的状态的下一个阶段，不是离开锁定区域将锁定区域还给用户，就是有独立的互斥量去保护这个类的全部实例。当然，这两种方式都不理想。

**一个给定操作需要两个或两个以上的互斥量时，另一个潜在的问题将出现：死锁。与条件竞争完全相反——不同的两个线程会互相等待，从而什么都没做。**

### 3.2.4 死锁：问题描述及解决方案

试想有一个玩具，这个玩具由两部分组成，必须拿到这两个部分，才能够玩。例如，一个玩具鼓，需要一个鼓锤和一个鼓才能玩。现在有两个小孩，他们都很喜欢玩这个玩具。当其中一个孩子拿到了鼓和鼓锤时，那就可以尽情的玩耍了。当另一孩子想要玩，他就得等待另一孩子玩完才行。再试想，鼓和鼓锤被放在不同的玩具箱里，并且两个孩子在同一时间里都想要去敲鼓。之后，他们就去玩具箱里面找这个鼓。其中一个找到了鼓，并且另外一个找到了鼓锤。现在问题就来了，除非其中一个孩子决定让另一个先玩，他可以把自己的那部分给另外一个孩子；但当他们都紧握着自己所有的部分而不给予，那么这个鼓谁都没法玩。

现在没有孩子去争抢玩具，但线程有对锁的竞争：一对线程需要对他们所有的互斥量做一些操作，其中每个线程都有一个互斥量，且等待另一个解锁。这样没有线程能工作，因为他们都在等待对方释放互斥量。这种情况就是死锁，它的最大问题就是由两个或两个以上的互斥量来锁定一个操作。

避免死锁的一般建议，就是让两个互斥量总以相同的顺序上锁：总在互斥量B之前锁住互斥量A，就永远不会死锁。某些情况下是可以这样用，因为不同的互斥量用于不同的地方。不过，事情没那么简单，比如：当有多个互斥量保护同一个类的独立实例时，一个操作对同一个类的两个不同实例进行数据的交换操作，为了保证数据交换操作的正确性，就要避免数据被并发修改，并确保每个实例上的互斥量都能锁住自己要保护的区域。不过，选择一个固定的顺序(例如，实例提供的第一互斥量作为第一个参数，提供的第二个互斥量为第二个参数)，可能会适得其反：**在参数交换了之后**，两个线程试图在相同的两个实例间进行数据交换时，程序又死锁了！

很幸运，C++标准库有办法解决这个问题，**`std::lock`——可以一次性锁住多个(两个以上)的互斥量，并且没有副作用(死锁风险)。**下面的程序清单中，就来看一下怎么在一个简单的交换操作中使用`std::lock`。

清单3.6 交换操作中使用`std::lock()`和`std::lock_guard`

```
// 这里的std::lock()需要包含<mutex>头文件
class some_big_object;
void swap(some_big_object& lhs,some_big_object& rhs);
class X
{
private:
  some_big_object some_detail;
  std::mutex m;
public:
  X(some_big_object const& sd):some_detail(sd){}

  friend void swap(X& lhs, X& rhs)
  {
    if(&lhs==&rhs)
      return;
    std::lock(lhs.m,rhs.m); // 1
    std::lock_guard<std::mutex> lock_a(lhs.m,std::adopt_lock); // 2
    std::lock_guard<std::mutex> lock_b(rhs.m,std::adopt_lock); // 3
    swap(lhs.some_detail,rhs.some_detail);
  }
};
```

首先，检查参数是否是不同的实例，因为操作试图获取`std::mutex`对象上的锁，所以当其被获取时，结果很难预料。(一个互斥量可以在同一线程上多次上锁，标准库中`std::recursive_mutex`提供这样的功能。详情见3.3.3节)。然后，调用`std::lock()`①锁住两个互斥量，并且两个`std:lock_guard`实例已经创建好②③。提供`std::adopt_lock`参数除了表示`std::lock_guard`对象可获取锁之外，还将锁交由`std::lock_guard`对象管理，而不需要`std::lock_guard`对象再去构建新的锁。

这样，就能保证在大多数情况下，函数退出时互斥量能被正确的解锁(保护操作可能会抛出一个异常)，也允许使用一个简单的“return”作为返回。**还有，需要注意的是，当使用`std::lock`去锁lhs.m或rhs.m时，可能会抛出异常；这种情况下，异常会传播到`std::lock`之外。当`std::lock`成功的获取一个互斥量上的锁，并且当其尝试从另一个互斥量上再获取锁时，就会有异常抛出，第一个锁也会随着异常的产生而自动释放，所以`std::lock`要么将两个锁都锁住，要不一个都不锁。??**

虽然`std::lock`可以在这情况下(获取两个以上的锁)避免死锁，但它没办法帮助你获取其中一个锁。这时，不得不依赖于开发者的纪律性(译者：也就是经验)，来确保你的程序不会死锁。这并不简单：死锁是多线程编程中一个令人相当头痛的问题，并且死锁经常是不可预见的，因为在大多数时间里，所有工作都能很好的完成。不过，也一些相对简单的规则能帮助写出“无死锁”的代码。


[**ADD 参看  以全局的固定顺序获取多个锁来避免死锁**](https://blog.csdn.net/noaboutfengyue/article/details/43340279)

[**ADD 参看  C++并发编程1 - 让我们开始管理多线程**](https://blog.csdn.net/jonathan321/article/details/52679471)
[**ADD 参看  C++并发编程2——为保护数据加锁（一）**](https://blog.csdn.net/jonathan321/article/details/52698281)
[**ADD 参看  C++并发编程2——为共享数据加锁（二）**](https://blog.csdn.net/jonathan321/article/details/52698300)
[**ADD 参看  C++并发编程2——为共享数据加锁（三）**](https://blog.csdn.net/jonathan321/article/details/52698307)
[**ADD 参看  C++并发编程2——为共享数据加锁（四）**](https://blog.csdn.net/jonathan321/article/details/52698335)


### 3.2.5 避免死锁的进阶指导

虽然锁是产生死锁的一般原因，但也不排除死锁出现在其他地方。无锁的情况下，仅需要每个`std::thread`对象调用join()，两个线程就能产生死锁。这种情况下，没有线程可以继续运行，因为他们正在互相等待。这种情况很常见，一个线程会等待另一个线程，其他线程同时也会等待第一个线程结束，所以三个或更多线程的互相等待也会发生死锁。为了避免死锁，这里的指导意见为：当机会来临时，不要拱手让人。以下提供一些个人的指导建议，如何识别死锁，并消除其他线程的等待。

**避免嵌套锁**

第一个建议往往是最简单的：一个线程已获得一个锁时，再别去获取第二个。如果能坚持这个建议，因为每个线程只持有一个锁，锁上就不会产生死锁。即使互斥锁造成死锁的最常见原因，也可能会在其他方面受到死锁的困扰(比如：线程间的互相等待)。当你需要获取多个锁，使用一个`std::lock`来做这件事(对获取锁的操作上锁)，避免产生死锁。

**避免在持有锁时调用用户提供的代码**

第二个建议是次简单的：因为代码是用户提供的，你没有办法确定用户要做什么；用户程序可能做任何事情，包括获取锁。你在持有锁的情况下，调用用户提供的代码；如果用户代码要获取一个锁，就会违反第一个指导意见，并造成死锁(有时，这是无法避免的)。当你正在写一份通用代码，例如3.2.3中的栈，每一个操作的参数类型，都在用户提供的代码中定义，就需要其他指导意见来帮助你。

**使用固定顺序获取锁**


当硬性条件要求你获取两个以上(包括两个)的锁，并且不能使用`std::lock`单独操作来获取它们;那么最好在每个线程上，用固定的顺序获取它们获取它们(锁)。3.2.4节中提到一种当需要获取两个互斥量时，避免死锁的方法：关键是如何在线程之间，以一定的顺序获取锁。一些情况下，这种方式相对简单。比如，3.2.3节中的栈——每个栈实例中都内置有互斥量，但是对数据成员存储的操作上，栈就需要带调用用户提供的代码。虽然，可以添加一些约束，对栈上存储的数据项不做任何操作，对数据项的处理仅限于栈自身。这会给用户提供的栈增加一些负担，但是一个容器很少去访问另一个容器中存储的数据，即使发生了也会很明显，所以这对于通用栈来说并不是一个特别沉重的负担。

其他情况下，这就不会那么简单了，例如：3.2.4节中的交换操作，这种情况下你可能同时锁住多个互斥量(但是有时不会发生)。当回看3.1节中那个链表连接例子时，将会看到列表中的每个节点都会有一个互斥量保护。为了访问列表，线程必须获取他们感兴趣节点上的互斥锁。当一个线程删除一个节点，它必须获取三个节点上的互斥锁：将要删除的节点，两个邻接节点(因为他们也会被修改)。同样的，为了遍历链表，线程必须保证在获取当前节点的互斥锁前提下，获得下一个节点的锁，要保证指向下一个节点的指针不会同时被修改。一旦下一个节点上的锁被获取，那么第一个节点的锁就可以释放了，因为没有持有它的必要性了。

这种“手递手”锁的模式允许多个线程访问列表，为每一个访问的线程提供不同的节点。但是，为了避免死锁，节点必须以同样的顺序上锁：如果两个线程试图用互为反向的顺序，使用“手递手”锁遍历列表，他们将执行到列表中间部分时，发生死锁。**当节点A和B在列表中相邻，当前线程可能会同时尝试获取A和B上的锁。另一个线程可能已经获取了节点B上的锁，并且试图获取节点A上的锁——经典的死锁场景。**

当A、C节点中的B节点正在被删除时，如果有线程在已获取A和C上的锁后，还要获取B节点上的锁时，当一个线程遍历列表的时候，这样的情况就可能发生死锁。这样的线程可能会试图首先锁住A节点或C节点(根据遍历的方向)，但是后面就会发现，它无法获得B上的锁，因为线程在执行删除任务的时候，已经获取了B上的锁，并且同时也获取了A和C上的锁。

**这里提供一种避免死锁的方式，定义遍历的顺序，所以一个线程必须先锁住A才能获取B的锁，在锁住B之后才能获取C的锁。这将消除死锁发生的可能性，在不允许反向遍历的列表上。类似的约定常被用来建立其他的数据结构。**

**使用锁的层次结构**

虽然，这对于定义锁的顺序，的确是一个特殊的情况，**但锁的层次的意义在于提供对运行时约定是否被坚持的检查。**这个建议需要对你的应用进行分层，并且识别在给定层上所有可上锁的互斥量。当代码试图对一个互斥量上锁，在该层锁已被低层持有时，上锁是不允许的。你可以在运行时对其进行检查，通过分配层数到每个互斥量上，以及记录被每个线程上锁的互斥量。下面的代码列表中将展示两个线程如何使用分层互斥。

清单3.7 使用层次锁来避免死锁

```
hierarchical_mutex high_level_mutex(10000); // 1
hierarchical_mutex low_level_mutex(5000);  // 2

int do_low_level_stuff();

int low_level_func()
{
  std::lock_guard<hierarchical_mutex> lk(low_level_mutex); // 3
  return do_low_level_stuff();
}

void high_level_stuff(int some_param);

void high_level_func()
{
  std::lock_guard<hierarchical_mutex> lk(high_level_mutex); // 4
  high_level_stuff(low_level_func()); // 5 高层 调 低层
}

void thread_a()  // 6
{
  high_level_func();
}

hierarchical_mutex other_mutex(100); // 7
void do_other_stuff();

void other_stuff()
{
  high_level_func();  // 8
  do_other_stuff();
}

void thread_b() // 9
{
  std::lock_guard<hierarchical_mutex> lk(other_mutex); // 10
  other_stuff();
}
```

thread_a()⑥遵守规则，所以它运行的没问题。另一方面，thread_b()⑨无视规则，因此在运行的时候肯定会失败。thread_a()调用high_level_func()，让high_level_mutex④上锁(其层级值为10000①)，为了获取high_level_stuff()的参数对互斥量上锁，之后调用low_level_func()⑤。low_level_func()会对low_level_mutex上锁，这就没有问题了，因为这个互斥量有一个低层值5000②。

thread_b()运行就不会顺利了。首先，它锁住了other_mutex⑩，这个互斥量的层级值只有100⑦。这就意味着，超低层级的数据已被保护。当other_stuff()调用high_level_func()⑧时，就违反了层级结构：high_level_func()试图获取high_level_mutex，这个互斥量的层级值是10000，要比当前层级值100大很多。因此hierarchical_mutex将会产生一个错误，可能会是抛出一个异常，或直接终止程序。在层级互斥量上产生死锁，是不可能的，因为互斥量本身会严格遵循约定顺序，进行上锁。这也意味，当多个互斥量在是在同一级上时，不能同时持有多个锁，所以“手递手”锁的方案需要每个互斥量在一条链上，并且每个互斥量都比其前一个有更低的层级值，这在某些情况下无法实现。

例子也展示了另一点，`std::lock_guard<>`模板与用户定义的互斥量类型一起使用。虽然hierarchical_mutex不是`C++`标准的一部分，但是它写起来很容易；一个简单的实现在列表3.8中展示出来。尽管它是一个用户定义类型，它可以用于`std::lock_guard<>`模板中，因为它的实现有三个成员函数为了满足互斥量操作：lock(), unlock() 和 try_lock()。虽然你还没见过try_lock()怎么使用，但是其使用起来很简单：当互斥量上的锁被一个线程持有，它将返回false，而不是等待调用的线程，直到能够获取互斥量上的锁为止。在`std::lock()`的内部实现中，try_lock()会作为避免死锁算法的一部分。

列表3.8 简单的层级互斥量实现

```
class hierarchical_mutex
{
  std::mutex internal_mutex;
  
  unsigned long const hierarchy_value;//准备加锁mutex的层号
  unsigned long previous_hierarchy_value;//在解锁时恢复原先现场就要记录先前mutex的层号 
   
  static thread_local unsigned long this_thread_hierarchy_value;  // 1 //该线程中当前已经加锁的mutex的层号// 线程本地的静态成员变量
  
  void check_for_hierarchy_violation()
  {
    if(this_thread_hierarchy_value <= hierarchy_value)  // 2
    {
      throw std::logic_error(“mutex hierarchy violated”);
    }
  }//检查是否满足hierarchical_mutex规则
  
  void update_hierarchy_value()
  {
    previous_hierarchy_value=this_thread_hierarchy_value;  // 3
    this_thread_hierarchy_value=hierarchy_value;
  }
  
public:
  explicit hierarchical_mutex(unsigned long value):
      hierarchy_value(value),
      previous_hierarchy_value(0)
  {}
  
  //满足hierarchical_mutex规则时加锁并更新内部记录变量
  void lock()
  {
    check_for_hierarchy_violation();
    internal_mutex.lock();  // 4
    update_hierarchy_value();  // 5
  }
  
  //恢复到调用lock之前的现场，解锁
  void unlock()
  {
    this_thread_hierarchy_value=previous_hierarchy_value;  // 6
    internal_mutex.unlock();
  }
  
  //mutex::try_lock()检查mutex是否可以成功lock，如果可以就lock，如果不行立刻返回false.
  bool try_lock()
  {
    check_for_hierarchy_violation();
    if(!internal_mutex.try_lock())  // 7
      return false;
    update_hierarchy_value();
    return true;
  }
};

//使用UNSIGND_MAX初始化表明刚开始任何层的mutex都可以加锁成功
// 线程本地的静态成员变量初始化
thread_local unsigned long hierarchical_mutex::this_thread_hierarchy_value(ULONG_MAX);  // 8
```

这里重点是使用了thread_local的值来代表当前线程的层级值：this_thread_hierarchy_value①。它被初始化为最大值⑧，所以最初所有线程都能被锁住。因为其声明中有thread_local，所以每个线程都有其拷贝副本，这样线程中变量状态完全独立，当从另一个线程进行读取时，变量的状态也完全独立。参见附录A，A.8节，有更多与thread_local相关的内容。

所以，第一次线程锁住一个hierarchical_mutex时，this_thread_hierarchy_value的值是ULONG_MAX。由于其本身的性质，这个值会大于其他任何值，所以会通过check_for_hierarchy_vilation()②的检查。在这种检查方式下，lock()代表内部互斥锁已被锁住④。一旦成功锁住，你可以更新层级值了⑤。

**当你现在锁住另一个hierarchical_mutex时，还持有第一个锁，?? this_thread_hierarchy_value的值将会显示第一个互斥量的层级值。第二个互斥量的层级值必须小于已经持有互斥量检查函数②才能通过。**

现在，最重要的是为当前线程存储之前的层级值，所以你可以调用unlock()⑥对层级值进行保存；否则，就锁不住任何互斥量(第二个互斥量的层级数高于第一个互斥量)，即使线程没有持有任何锁。因为保存了之前的层级值，只有当持有internal_mutex③，且在解锁内部互斥量⑥之前存储它的层级值，才能安全的将hierarchical_mutex自身进行存储。这是因为hierarchical_mutex被内部互斥量的锁所保护着。

try_lock()与lock()的功能相似，除了在调用internal_mutex的try_lock()⑦失败时，不能持有对应锁，所以不必更新层级值，并直接返回false。

虽然是运行时检测，但是它没有时间依赖性——不必去等待那些导致死锁出现的罕见条件。同时，设计过程需要去拆分应用，互斥量在这样的情况下可以消除可能导致死锁的可能性。这样的设计练习很有必要去做一下，即使你之后没有去做，代码也会在运行时进行检查。

**超越锁的延伸扩展**

如我在本节开头提到的那样，死锁不仅仅会发生在锁之间；死锁也会发生在任何同步构造中(可能会产生一个等待循环)，因此这方面也需要有指导意见，例如：要去避免获取嵌套锁等待一个持有锁的线程是一个很糟糕的决定，因为线程为了能继续运行可能需要获取对应的锁。类似的，如果去等待一个线程结束，它应该可以确定这个线程的层级，这样一个线程只需要等待比起层级低的线程结束即可。可以用一个简单的办法去确定，以添加的线程是否在同一函数中被启动，如同在3.1.2节和3.3节中描述的那样。

当代码已经能规避死锁，`std::lock()`和`std::lock_guard`能组成简单的锁覆盖大多数情况，但是有时需要更多的灵活性。在这些情况，可以使用标准库提供的`std::unique_lock`模板。如` std::lock_guard`，这是一个参数化的互斥量模板类，并且它提供很多RAII类型锁用来管理`std::lock_guard`类型，可以让代码更加灵活。

### 3.2.6 std::unique_lock——灵活的锁??

`std::unqiue_lock`使用更为自由的不变量，**这样`std::unique_lock`实例不会总与互斥量的数据类型相关??**，使用起来要比`std:lock_guard`更加灵活。首先，可将`std::adopt_lock`作为第二个参数传入构造函数，对互斥量进行管理；也可以将`std::defer_lock`作为第二个参数传递进去，表明互斥量应保持解锁状态。这样，就可以被`std::unique_lock`对象(不是互斥量)的lock()函数的所获取，或传递`std::unique_lock`对象到`std::lock()`中。清单3.6可以轻易的转换为清单3.9，使用`std::unique_lock`和`std::defer_lock`①，而非`std::lock_guard`和`std::adopt_lock`。代码长度相同，几乎等价，唯一不同的就是：`std::unique_lock`会占用比较多的空间，并且比`std::lock_guard`稍慢一些。保证灵活性要付出代价，这个代价就是允许`std::unique_lock`实例不带互斥量：信息已被存储，且已被更新。

清单3.9 交换操作中`std::lock()`和`std::unique_lock`的使用

```
class some_big_object;
void swap(some_big_object& lhs,some_big_object& rhs);
class X
{
private:
  some_big_object some_detail;
  std::mutex m;
public:
  X(some_big_object const& sd):some_detail(sd){}
  friend void swap(X& lhs, X& rhs)
  {
    if(&lhs==&rhs)
      return;
    std::unique_lock<std::mutex> lock_a(lhs.m,std::defer_lock); // 1 
    std::unique_lock<std::mutex> lock_b(rhs.m,std::defer_lock); // 1 std::def_lock 留下未上锁的互斥量
    std::lock(lock_a,lock_b); // 2 互斥量在这里上锁

	//清单3.6 lock_guard
	//std::lock(lhs.m,rhs.m); // 1
    //std::lock_guard<std::mutex> lock_a(lhs.m,std::adopt_lock); // 2
    //std::lock_guard<std::mutex> lock_b(rhs.m,std::adopt_lock); // 3
 
   swap(lhs.some_detail,rhs.some_detail);
  }
};
```

列表3.9中，因为`std::unique_lock`支持lock(), try_lock()和unlock()成员函数，所以能将`std::unique_lock`对象传递到`std::lock()`②。这些同名的成员函数在低层做着实际的工作，并且仅更新`std::unique_lock`实例中的标志，来确定该实例是否拥有特定的互斥量，这个标志是为了确保unlock()在析构函数中被正确调用。如果实例拥有互斥量，那么析构函数必须调用unlock()；但当实例中没有互斥量时，析构函数就不能去调用unlock()。这个标志可以通过owns_lock()成员变量进行查询。

可能如你期望的那样，这个标志被存储在某个地方。因此，`std::unique_lock`对象的体积通常要比`std::lock_guard`对象大，当使用`std::unique_lock`替代`std::lock_guard`，因为会对标志进行适当的更新或检查，就会做些轻微的性能惩罚。当`std::lock_guard`已经能够满足你的需求，那么还是建议你继续使用它。当需要更加灵活的锁时，最好选择`std::unique_lock`，因为它更适合于你的任务。你已经看到一个递延锁的例子，另外一种情况是锁的所有权需要从一个域转到另一个域。

### 3.2.7 不同域中互斥量所有权的传递??

`std::unique_lock`实例没有与自身相关的互斥量，一个互斥量的所有权可以通过移动操作，在不同的实例中进行传递。某些情况下，这种转移是自动发生的，例如:当函数返回一个实例；另些情况下，需要显式的调用`std::move()`来执行移动操作。从本质上来说，需要依赖于源值是否是左值——一个实际的值或是引用——或一个右值——一个临时类型。当源值是一个右值，为了避免转移所有权过程出错，就必须显式移动成左值。`std::unique_lock`是可移动，但不可赋值的类型。附录A，A.1.1节有更多与移动语句相关的信息。

**一种使用可能是允许一个函数去锁住一个互斥量，并且将所有权移到调用者上，所以调用者可以在这个锁保护的范围内执行额外的动作。**

下面的程序片段展示了：函数get_lock()锁住了互斥量，然后准备数据，返回锁的调用函数：

```
std::unique_lock<std::mutex> get_lock()
{
  extern std::mutex some_mutex;
  std::unique_lock<std::mutex> lk(some_mutex);
  prepare_data();
  return lk;  // 1
}
void process_data()
{
  std::unique_lock<std::mutex> lk(get_lock());  // 2
  do_something();
}
```

lk在函数中被声明为自动变量，它不需要调用`std::move()`，可以直接返回①(编译器负责调用移动构造函数)。process_data()函数直接转移`std::unique_lock`实例的所有权②，调用do_something()可使用的正确数据(数据没有受到其他线程的修改)。

通常这种模式会用于已锁的互斥量，其依赖于当前程序的状态，或依赖于传入返回类型为`std::unique_lock`的函数(或以参数返回)。这样的用法不会直接返回锁，不过网关类的一个数据成员可用来确认已经对保护数据的访问权限进行上锁。这种情况下，所有的访问都必须通过网关类：当你想要访问数据，需要获取网关类的实例(如同前面的例子，通过调用get_lock()之类函数)来获取锁。之后你就可以通过网关类的成员函数对数据进行访问。当完成访问，可以销毁这个网关类对象，将锁进行释放，让别的线程来访问保护数据。这样的一个网关类可能是可移动的(所以他可以从一个函数进行返回)，在这种情况下锁对象的数据必须是可移动的。

`std::unique_lock`的灵活性同样也允许实例在销毁之前放弃其拥有的锁。可以使用unlock()来做这件事，如同一个互斥量：`std::unique_lock`的成员函数提供类似于锁定和解锁互斥量的功能。`std::unique_lock`实例在销毁前释放锁的能力，当锁没有必要在持有的时候，可以在特定的代码分支对其进行选择性的释放。这对于应用性能来说很重要，因为持有锁的时间增加会导致性能下降，其他线程会等待这个锁的释放，避免超越操作。

### 3.2.8 锁的粒度

3.2.3节中，已经对锁的粒度有所了解：锁的粒度是一个*摆手术语*(hand-waving term)，用来描述通过一个锁保护着的数据量大小。*一个细粒度锁*(a fine-grained lock)能够保护较小的数据量，*一个粗粒度锁*(a coarse-grained lock)能够保护较多的数据量。选择粒度对于锁来说很重要，为了保护对应的数据，保证锁有能力保护这些数据也很重要。我们都知道，在超市等待结账的时候，正在结账的顾客突然意识到他忘了拿蔓越莓酱，然后离开柜台去拿，并让其他的人都等待他回来；或者当收银员，准备收钱时，顾客才去翻钱包拿钱，这样的情况都会让等待的顾客很无奈。当每个人都检查了自己要拿的东西，且能随时为拿到的商品进行支付，那么的每件事都会进行的很顺利。

这样的道理同样适用于线程：如果很多线程正在等待同一个资源(等待收银员对自己拿到的商品进行清点)，当有线程持有锁的时间过长，这就会增加等待的时间(别等到结账的时候，才想起来蔓越莓酱没拿)。在可能的情况下，锁住互斥量的同时只能对共享数据进行访问；试图对锁外数据进行处理。特别是做一些费时的动作，比如：对文件的输入/输出操作进行上锁。文件输入/输出通常要比从内存中读或写同样长度的数据慢成百上千倍，所以除非锁已经打算去保护对文件的访问，要么执行输入/输出操作将会将延迟其他线程执行的时间，这很没有必要(因为文件锁阻塞住了很多操作)，这样多线程带来的性能效益会被抵消。

`std::unique_lock`在这种情况下工作正常，在调用unlock()时，代码不需要再访问共享数据；而后当再次需要对共享数据进行访问时，就可以再调用lock()了。下面代码就是这样的一种情况：

```
void get_and_process_data()
{
  std::unique_lock<std::mutex> my_lock(the_mutex);
  some_class data_to_process=get_next_data_chunk();
  my_lock.unlock();  // 1 不要让锁住的互斥量越过process()函数的调用
  result_type result=process(data_to_process);
  my_lock.lock(); // 2 为了写入数据，对互斥量再次上锁
  write_result(data_to_process,result);
}
```

不需要让锁住的互斥量越过对process()函数的调用，所以可以在函数调用①前对互斥量手动解锁，并且在之后对其再次上锁②。

这能表示只有一个互斥量保护整个数据结构时的情况，不仅可能会有更多对锁的竞争，也会增加锁持锁的时间。较多的操作步骤需要获取同一个互斥量上的锁，所以持有锁的时间会更长。成本上的双重打击也算是为向细粒度锁转移提供了双重激励和可能。

如同上面的例子，锁不仅是能锁住合适粒度的数据，还要控制锁的持有时间，以及什么操作在执行的同时能够拥有锁。一般情况下，执行必要的操作时，尽可能将持有锁的时间缩减到最小。这也就意味有一些浪费时间的操作，比如：获取另外一个锁(即使你知道这不会造成死锁)，或等待输入/输出操作完成时没有必要持有一个锁(除非绝对需要)。

清单3.6和3.9中，交换操作需要锁住两个互斥量，其明确要求并发访问两个对象。假设用来做比较的是一个简单的数据类型(比如:int类型)，将会有什么不同么？int的拷贝很廉价，所以可以很容易的进行数据复制，并且每个被比较的对象都持有该对象的锁，在比较之后进行数据拷贝。这就意味着，在最短时间内持有每个互斥量，并且你不会在持有一个锁的同时再去获取另一个。下面的清单中展示了一个在这样情景中的Y类，并且展示了一个相等比较运算符的等价实现。

列表3.10 比较操作符中一次锁住一个互斥量

```
class Y
{
private:
  int some_detail;
  mutable std::mutex m;
  int get_detail() const
  {
    std::lock_guard<std::mutex> lock_a(m);  // 1
    return some_detail;
  }
public:
  Y(int sd):some_detail(sd){}

  friend bool operator==(Y const& lhs, Y const& rhs)
  {
    if(&lhs==&rhs)
      return true;
    int const lhs_value=lhs.get_detail();  // 2
    int const rhs_value=rhs.get_detail();  // 3
    return lhs_value==rhs_value;  // 4
  }
};
```

在这个例子中，比较操作符首先通过调用get_detail()成员函数检索要比较的值②③，函数在索引值时被一个锁保护着①。比较操作符会在之后比较索引出来的值④。注意：虽然这样能减少锁持有的时间，一个锁只持有一次(这样能消除死锁的可能性)，**这里有一个微妙的语义操作同时对两个锁住的值进行比较。**

列表3.10中，当操作符返回true时，那就意味着在这个时间点上的lhs.some_detail与在另一个时间点的rhs.some_detail相同。这两个值在读取之后，可能会被任意的方式所修改；两个值会在②和③处进行交换，这样就会失去比较的意义。等价比较可能会返回true，来表明这两个值时相等的，实际上这两个值相等的情况可能就发生在一瞬间。这样的变化要小心，语义操作是无法改变一个问题的比较方式：**当你持有锁的时间没有达到整个操作时间，就会让自己处于条件竞争的状态。**

有时，只是没有一个合适粒度级别，因为并不是所有对数据结构的访问都需要同一级的保护。这个例子中，就需要寻找一个合适的机制，去替换`std::mutex`。

-------

[1] Tom Cargill, “Exception Handling: A False Sense of Security,” in C++ Report 6, no. 9 (November–December 1994). Also available at http://www.informit.com/content/images/020163371x/supplements/Exception_Handling_Article.html.

[2] Herb Sutter, Exceptional C++: 47 Engineering Puzzles, Programming Problems, and Solutions (Addison Wesley Pro-fessional, 1999).

### 3.3 保护共享数据的替代设施

互斥量是最通用的机制，但其并非保护共享数据的唯一方式。这里有很多替代方式可以在特定情况下，提供更加合适的保护。

一个特别极端(但十分常见)的情况就是，共享数据在并发访问和初始化时(都需要保护)，但是之后需要进行隐式同步。这可能是因为数据作为只读方式创建，所以没有同步问题；或者因为必要的保护作为对数据操作的一部分，所以隐式的执行。任何情况下，数据初始化后锁住一个互斥量，纯粹是为了保护其初始化过程(这是没有必要的)，并且这会给性能带来不必要的冲击。出于以上的原因，`C++`标准提供了一种纯粹保护共享数据初始化过程的机制。

#### 3.3.1 保护共享数据的初始化过程

假设你与一个共享源，构建代价很昂贵,可能它会打开一个数据库连接或分配出很多的内存。

*延迟初始化*(Lazy initialization)在单线程代码很常见——每一个操作都需要先对源进行检查，为了了解数据是否被初始化，然后在其使用前决定，数据是否需要初始化：

```
std::shared_ptr<some_resource> resource_ptr;
void foo()
{
  if(!resource_ptr)
  {
    resource_ptr.reset(new some_resource);  // 1
  }
  resource_ptr->do_something();
}
```

当共享数据对于并发访问是安全的，①是转为多线程代码时，需要保护的，但是下面天真的转换会使得线程资源产生不必要的序列化。这是因为每个线程必须等待互斥量，为了确定数据源已经初始化了。

清单 3.11 使用一个互斥量的延迟初始化(线程安全)过程

```
std::shared_ptr<some_resource> resource_ptr;
std::mutex resource_mutex;

void foo()
{
  std::unique_lock<std::mutex> lk(resource_mutex);  // 所有线程在此序列化
  if(!resource_ptr)
  {
    resource_ptr.reset(new some_resource);  // 只有初始化过程需要保护
  }
  lk.unlock();
  resource_ptr->do_something();
}
```


[**双重检查锁模式扩展**](http://blog.jobbole.com/86392/)


这段代码相当常见了，也足够表现出没必要的线程化问题，很多人能想出更好的一些的办法来做这件事，包括**声名狼藉的双重检查锁模式**：

```
void undefined_behaviour_with_double_checked_locking()
{
  if(!resource_ptr)  // 1
  {
    std::lock_guard<std::mutex> lk(resource_mutex);
    if(!resource_ptr)  // 2
    {
      resource_ptr.reset(new some_resource);  // 3
    }
  }
  resource_ptr->do_something();  // 4
}
```

指针第一次读取数据不需要获取锁①，并且只有在指针为NULL时才需要获取锁。然后，当获取锁之后，指针会被再次检查一遍② (这就是双重检查的部分)，避免另一的线程在第一次检查后再做初始化，并且让当前线程获取锁。

这个模式为什么声名狼藉呢？因为这里有潜在的条件竞争，未被锁保护的读取操作①没有与其他线程里被锁保护的写入操作③进行同步。因此就会产生条件竞争，这个条件竞争不仅覆盖指针本身，还会影响到其指向的对象；即使一个线程知道另一个线程完成对指针进行写入，它可能没有看到新创建的some_resource实例，然后调用do_something()④后，得到不正确的结果。**这个例子是在一种典型的条件竞争——数据竞争**，`C++`标准中这就会被指定为“未定义行为”。这种竞争肯定是可以避免的。可以阅读第5章，那里有更多对内存模型的讨论，包括数据竞争的构成。

C++标准委员会也认为条件竞争的处理很重要，所以C++标准库提供了`std::once_flag`和`std::call_once`来处理这种情况。比起锁住互斥量，并显式的检查指针，每个线程只需要使用`std::call_once`，在`std::call_once`的结束时，就能安全的知道指针已经被其他的线程初始化了。使用`std::call_once`比显式使用互斥量消耗的资源更少，特别是当初始化完成后。下面的例子展示了与清单3.11中的同样的操作，这里使用了`std::call_once`。在这种情况下，初始化通过调用函数完成，同样这样操作使用类中的函数操作符来实现同样很简单。如同大多数在标准库中的函数一样，或作为函数被调用，或作为参数被传递，`std::call_once`可以和任何函数或可调用对象一起使用。

```
std::shared_ptr<some_resource> resource_ptr;
std::once_flag resource_flag;  // 1

void init_resource()
{
  resource_ptr.reset(new some_resource);
}

void foo()
{
  std::call_once(resource_flag,init_resource);  // 可以完整的进行一次初始化
  resource_ptr->do_something();
}
```

在这个例子中，`std::once_flag`①和初始化好的数据都是命名空间区域的对象，但是`std::call_once()`可仅作为延迟初始化的类型成员，如同下面的例子一样：

清单3.12 使用`std::call_once`作为类成员的延迟初始化(线程安全)

```
class X
{
private:
  connection_info connection_details;
  connection_handle connection;
  std::once_flag connection_init_flag;

  void open_connection()
  {
    connection=connection_manager.open(connection_details);
  }
public:
  X(connection_info const& connection_details_):
      connection_details(connection_details_)
  {}
  void send_data(data_packet const& data)  // 1
  {
    std::call_once(connection_init_flag,&X::open_connection,this);  // 2
    connection.send_data(data);
  }
  data_packet receive_data()  // 3
  {
    std::call_once(connection_init_flag,&X::open_connection,this);  // 2
    return connection.receive_data();
  }
};
```

例子中第一个调用send_data()①或receive_data()③的线程完成初始化过程。使用成员函数open_connection()去初始化数据，也需要将this指针传进去。和其在在标准库中的函数一样，其接受可调用对象，比如`std::thread`的构造函数和`std::bind()`，通过向`std::call_once()`②传递一个额外的参数来完成这个操作。

值得注意的是，`std::mutex`和`std::one_flag`的实例就不能拷贝和移动，所以当你使用它们作为类成员函数，如果你需要用到他们，你就得显示定义这些特殊的成员函数。

还有一种情形的初始化过程中潜存着条件竞争：其中一个局部变量被声明为static类型。这种变量的在声明后就已经完成初始化；对于多线程调用的函数，这就意味着这里有条件竞争——抢着去定义这个变量。在很多在前C++11编译器(译者：不支持C++11标准的编译器)，在实践过程中，这样的条件竞争是确实存在的，因为在多线程中，每个线程都认为他们是第一个初始化这个变量线程；或一个线程对变量进行初始化，而另外一个线程要使用这个变量时，初始化过程还没完成。在C++11标准中，这些问题都被解决了：初始化及定义完全在一个线程中发生，并且没有其他线程可在初始化完成前对其进行处理，条件竞争终止于初始化阶段，这样比在之后再去处理好的多。在只需要一个全局实例情况下，这里提供一个`std::call_once`的替代方案

```
class my_class;
my_class& get_my_class_instance()
{
  static my_class instance;  // 线程安全的初始化过程
  return instance;
}
```

多线程可以安全的调用get_my_class_instance()①函数，不用为数据竞争而担心。

对于很少有更新的数据结构来说，只在初始化时保护数据。在大多数情况下，这种数据结构是只读的，并且多线程对其并发的读取也是很愉快的，不过一旦数据结构需要更新，就会产生竞争。

#### 3.3.2 保护很少更新的数据结构

试想，为了将域名解析为其相关IP地址，我们在缓存中的存放了一张DNS入口表。通常，给定DNS数目在很长的一段时间内保持不变。虽然，在用户访问不同网站时，新的入口可能会被添加到表中，但是这些数据可能在其生命周期内保持不变。所以定期检查缓存中入口的有效性，就变的十分重要了；但是，这也需要一次更新，也许这次更新只是对一些细节做了改动。

虽然更新频度很低，但更新也有可能发生，并且当这个可缓存被多个线程访问，这个缓存就需要处于更新状态时得到保护，这也为了确保每个线程读到都是有效数据。

没有使用专用数据结构时，这种方式是符合预期，并且为并发更新和读取特别设计的(更多的例子在第6和第7章中介绍)。这样的更新要求线程独占数据结构的访问权，直到其完成更新操作。当更新完成，数据结构对于并发多线程访问又会是安全的。使用`std::mutex`来保护数据结构，显的有些反应过度(因为在没有发生修改时，它将削减并发读取数据的可能性)。这里需要另一种不同的互斥量，这种互斥量常被称为“读者-作者锁”，因为其允许两种不同的使用方式：一个“作者”线程独占访问和共享访问，让多个“读者”线程并发访问。

虽然这样互斥量的标准提案已经交给标准委员会，但是`C++`标准库依旧不会提供这样的互斥量[3]。因为建议没有被采纳，这个例子在本节中使用的是Boost库提供的实现(Boost采纳了这个建议)。你将在第8章中看到，这种锁的也不能包治百病，其性能依赖于参与其中的处理器数量，同样也与读者和作者线程的负载有关。为了确保增加复杂度后还能获得性能收益，目标系统上的代码性能就很重要。

比起使用`std::mutex`实例进行同步，不如使用`boost::shared_mutex`来做同步。对于更新操作，可以使用`std::lock_guard<boost::shared_mutex>`和`std::unique_lock<boost::shared_mutex>`上锁。作为`std::mutex`的替代方案，与`std::mutex`所做的一样，这就能保证更新线程的独占访问。因为其他线程不需要去修改数据结构，所以其可以使用`boost::shared_lock<boost::shared_mutex>`获取访问权。这与使用`std::unique_lock`一样，除非多线程要在同时获取同一个`boost::shared_mutex`上有共享锁。唯一的限制：当任一线程拥有一个共享锁时，这个线程就会尝试获取一个独占锁，直到其他线程放弃他们的锁；同样的，当任一线程拥有一个独占锁时，其他线程就无法获得共享锁或独占锁，直到第一个线程放弃其拥有的锁。

如同之前描述的那样，下面的代码清单展示了一个简单的DNS缓存，使用`std::map`持有缓存数据，使用`boost::shared_mutex`进行保护。

清单3.13 使用`boost::shared_mutex`对数据结构进行保护

```
#include <map>
#include <string>
#include <mutex>
#include <boost/thread/shared_mutex.hpp>

class dns_entry;

class dns_cache
{
  std::map<std::string,dns_entry> entries;
  mutable boost::shared_mutex entry_mutex;
public:
  dns_entry find_entry(std::string const& domain) const
  {
    boost::shared_lock<boost::shared_mutex> lk(entry_mutex);  // 1
    std::map<std::string,dns_entry>::const_iterator const it=
       entries.find(domain);
    return (it==entries.end())?dns_entry():it->second;
  }
  void update_or_add_entry(std::string const& domain,
                           dns_entry const& dns_details)
  {
    std::lock_guard<boost::shared_mutex> lk(entry_mutex);  // 2
    entries[domain]=dns_details;
  }
};
```

清单3.13中，find_entry()使用`boost::shared_lock<>`来保护共享和只读权限①；这就使得多线程可以同时调用find_entry()，且不会出错。另一方面，update_or_add_entry()使用`std::lock_guard<>`实例，当表格需要更新时②，为其提供独占访问权限；update_or_add_entry()函数调用时，独占锁会阻止其他线程对数据结构进行修改，并且阻止线程调用find_entry()。

#### 3.3.3 嵌套锁

当一个线程已经获取一个`std::mutex`时(已经上锁)，并对其再次上锁，这个操作就是错误的，并且继续尝试这样做的话，就会产生未定义行为。然而，在某些情况下，一个线程尝试获取同一个互斥量多次，而没有对其进行一次释放是可以的。之所以可以，是因为`C++`标准库提供了`std::recursive_mutex`类。其功能与`std::mutex`类似，除了你可以从同一线程的单个实例上获取多个锁。互斥量锁住其他线程前，你必须释放你拥有的所有锁，所以当你调用lock()三次时，你也必须调用unlock()三次。正确使用`std::lock_guard<std::recursive_mutex>`和`std::unique_lock<std::recursive_mutex>`可以帮你处理这些问题。

大多数情况下，当你需要嵌套锁时，就要对你的设计进行改动。嵌套锁一般用在可并发访问的类上，所以其拥互斥量保护其成员数据。每个公共成员函数都会对互斥量上锁，然后完成对应的功能，之后再解锁互斥量。不过，有时成员函数会调用另一个成员函数，这种情况下，第二个成员函数也会试图锁住互斥量，这就会导致未定义行为的发生。“变通的”解决方案会将互斥量转为嵌套锁，第二个成员函数就能成功的进行上锁，并且函数能继续执行。

但是，**这样的使用方式是不推荐的，因为其过于草率，并且不合理。特别是，当锁被持有时，对应类的不变量通常正在被修改。这意味着，当不变量正在改变的时候，第二个成员函数还需要继续执行。一个比较好的方式是，从中提取出一个函数作为类的私有成员，并且让其他成员函数都对其进行调用，这个私有成员函数不会对互斥量进行上锁(在调用前必须获得锁)。然后，你仔细考虑一下，在这种情况调用新函数时，数据的状态。**

-------

[3] Howard E. Hinnant, “Multithreading API for C++0X—A Layered Approach,” C++ Standards Committee Paper N2094, http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2006/n2094.html.

### 3.4 本章总结

本章讨论了当两个线程间的共享数据发生恶性条件竞争会带来多么严重的灾难，还讨论了如何使用`std::mutex`，和如何避免这些问题。如你所见，互斥量并不是灵丹妙药，其还有自己的问题(比如：死锁)，虽然C++标准库提供了一类工具来避免这些(例如：`std::lock()`)。你还见识了一些用于避免死锁的先进技术，之后了解了锁所有权的转移，以及一些围绕如何选取适当粒度锁产生的问题。最后，讨论了在具体情况下，数据保护的替代方案，例如:`std::call_once()`和`boost::shared_mutex`。

还有一个方面没有涉及到，那就是等待其他线程作为输入的情况。我们的线程安全栈，仅是在栈为空时，抛出一个异常，所以当一个线程要等待其他线程向栈压入一个值时(这是一个线程安全栈的主要用途之一)，它不得不多次尝试去弹出一个值，当捕获抛出的异常时，再次进行尝试。这种消耗资源的检查，没有任何意义。并且，不断的检查会影响系统中其他线程的运行，这反而会妨碍程序的进展。我们需要一些方法让一个线程等待其他线程完成任务，但在等待过程中不占用CPU。第4章中，会去建立一些工具，用于保护共享数据，还会介绍一些线程同步操作的机制；第6章中，如何构建更大型的可复用的数据类型。


## 第4章 同步并发操作

**本章主要内容**

- 等待事件<br>
- 带有期望的等待一次性事件<br>
- 在限定时间内等待<br>
- 使用同步操作简化代码<br>

在上一章中，我们看到各种在线程间保护共享数据的方法。当你不仅想要保护数据，还想对单独的线程进行同步。例如，在第一个线程完成前，可能需要等待另一个线程执行完成。通常情况下，线程会等待一个特定事件的发生，或者等待某一条件达成(为true)。这可能需要定期检查“任务完成”标识，或将类似的东西放到共享数据中，但这与理想情况还是差很多。像这种情况就需要在线程中进行同步，`C++`标准库提供了一些工具可用于同步操作，形式上表现为*条件变量*(condition variables)和*期望*(futures)。

在本章，将讨论如何使用条件变量等待事件，以及介绍期望，和如何使用它简化同步操作。

### 4.1 等待一个事件或其他条件
	
假设你在旅游，而且正在一辆在夜间运行的火车上。在夜间，如何在正确的站点下车呢？一种方法是整晚都要醒着，然后注意到了哪一站。这样，你就不会错过你要到达的站点，但是这样会让你感到很疲倦。另外，你可以看一下时间表，估计一下火车到达目的地的时间，然后在一个稍早的时间点上设置闹铃，然后你就可以安心的睡会了。这个方法听起来也很不错，也没有错过你要下车的站点，但是当火车晚点的时候，你就要被过早的叫醒了。当然，闹钟的电池也可能会没电了，并导致你睡过站。理想的方式是，无论是早或晚，只要当火车到站的时候，有人或其他东西能把你唤醒，就好了。

这和线程有什么关系呢？好吧，让我们来联系一下。当一个线程等待另一个线程完成任务时，它会有很多选择。第一，它可以持续的检查共享数据标志(用于做保护工作的互斥量)，直到另一线程完成工作时对这个标志进行重设。不过，就是一种浪费：线程消耗宝贵的执行时间持续的检查对应标志，并且当互斥量被等待线程上锁后，其他线程就没有办法获取锁，这样线程就会持续等待。因为以上方式对等待线程限制资源，并且在完成时阻碍对标识的设置。这种情况类似与，保持清醒状态和列车驾驶员聊了一晚上：驾驶员不得不缓慢驾驶，因为你分散了他的注意力，所以火车需要更长的时间，才能到站。同样的，等待的线程会等待更长的时间，这些线程也在消耗着系统资源。

第二个选择是在等待线程在检查间隙，使用`std::this_thread::sleep_for()`进行周期性的间歇(详见4.3节)：

```
bool flag;
std::mutex m;

void wait_for_flag()
{
  std::unique_lock<std::mutex> lk(m);
  while(!flag)
  {
    lk.unlock();  // 1 解锁互斥量
    std::this_thread::sleep_for(std::chrono::milliseconds(100));  // 2 休眠100ms
    lk.lock();   // 3 再锁互斥量
  }
}
```

这个循环中，在休眠前②，函数对互斥量进行解锁①，并且在休眠结束后再对互斥量进行上锁，所以另外的线程就有机会获取锁并设置标识。

这个实现就进步很多，因为当线程休眠时，线程没有浪费执行时间，但是很难确定正确的休眠时间。太短的休眠和没有休眠一样，都会浪费执行时间；太长的休眠时间，可能会让任务等待线程醒来。休眠时间过长是很少见的情况，因为这会直接影响到程序的行为，当在高节奏游戏中，它意味着丢帧，或在一个实时应用中超越了一个时间片。

第三个选择(也是优先的选择)是，使用C++标准库提供的工具去等待事件的发生。通过另一线程触发等待事件的机制是最基本的唤醒方式(例如：流水线上存在额外的任务时)，这种机制就称为“条件变量”。从概念上来说，一个条件变量会与多个事件或其他条件相关，并且一个或多个线程会等待条件的达成。当某些线程被终止时，为了唤醒等待线程(允许等待线程继续执行)终止的线程将会向等待着的线程广播“条件达成”的信息。

### 4.1.1 等待条件达成

C++标准库对条件变量有两套实现：`std::condition_variable`和`std::condition_variable_any`。这两个实现都包含在`<condition_variable>`头文件的声明中。两者都需要与一个互斥量一起才能工作(互斥量是为了同步)；前者仅限于与`std::mutex`一起工作，而后者可以和任何满足最低标准的互斥量一起工作，从而加上了*_any*的后缀。因为` std::condition_variable_any`更加通用，这就可能从体积、性能，以及系统资源的使用方面产生额外的开销，所以`std::condition_variable`一般作为首选的类型，当对灵活性有硬性要求时，我们才会去考虑`std::condition_variable_any`。

所以，如何使用`std::condition_variable`去处理之前提到的情况——当有数据需要处理时，如何唤醒休眠中的线程对其进行处理？以下清单展示了一种使用条件变量做唤醒的方式。

清单4.1 使用`std::condition_variable`处理数据等待

```
std::mutex mut;
std::queue<data_chunk> data_queue;  // 1
std::condition_variable data_cond;

void data_preparation_thread()
{
  while(more_data_to_prepare())
  {
    data_chunk const data=prepare_data();
    std::lock_guard<std::mutex> lk(mut);
    data_queue.push(data);  // 2
    data_cond.notify_one();  // 3 //notify_one()通知条件变量时，处理数据的线程从睡眠状态中苏醒，重新获取互斥锁，并且对条件再次检查，在条件满足的情况下(true)，从wait()返回并继续持有锁。当条件不满足时(false)，线程将对互斥量解锁，并且重新开始等待。
  }
}

void data_processing_thread()
{
  while(true)
  {
    std::unique_lock<std::mutex> lk(mut);  // 4 首先对互斥量上锁
    data_cond.wait(
         lk,[]{return !data_queue.empty();});  // 5 // false wait()函数将解锁互斥量 阻塞或等待状态 
    data_chunk data=data_queue.front();
    data_queue.pop();
    lk.unlock();  // 6
    process(data);
    if(is_last_chunk(data))
      break;
  }
}
```

首先，你拥有一个用来在两个线程之间传递数据的队列①。当数据准备好时，使用`std::lock_guard`对队列上锁，将准备好的数据压入队列中②，之后线程会对队列中的数据上锁。然后调用`std::condition_variable`的notify_one()成员函数，对等待的线程(如果有等待线程)进行通知③。

在另外一侧，你有一个正在处理数据的线程，这个线程首先对互斥量上锁，但在这里`std::unique_lock`要比`std::lock_guard`④更加合适——且听我细细道来。线程之后会调用`std::condition_variable`的成员函数wait()，传递一个锁和一个lambda函数表达式(作为等待的条件⑤)。Lambda函数是`C++11`添加的新特性，它可以让一个匿名函数作为其他表达式的一部分，并且非常合适作为标准函数的谓词，例如wait()函数。在这个例子中，简单的lambda函数`[]{return !data_queue.empty();}`会去检查data_queue是否不为空，当data_queue不为空——那就意味着队列中已经准备好数据了。附录A的A.5节有Lambda函数更多的信息。

wait()会去检查这些条件(通过调用所提供的lambda函数)，当条件满足(lambda函数返回true)时返回。**如果条件不满足(lambda函数返回false)，wait()函数将解锁互斥量，并且将这个线程(上段提到的处理数据的线程)置于阻塞或等待状态。当准备数据的线程调用notify_one()通知条件变量时，处理数据的线程从睡眠状态中苏醒，重新获取互斥锁，并且对条件再次检查，在条件满足的情况下(true)，从wait()返回并继续持有锁。当条件不满足时(false)，线程将对互斥量解锁，并且重新开始等待。这就是为什么用`std::unique_lock`而不使用`std::lock_guard`——等待中的线程必须在等待期间解锁互斥量，并在这这之后对互斥量再次上锁，而`std::lock_guard`没有这么灵活。如果互斥量在线程休眠期间保持锁住状态，准备数据的线程将无法锁住互斥量，也无法添加数据到队列中；同样的，等待线程也永远不会知道条件何时满足。**

清单4.1使用了一个简单的lambda函数用于等待⑤，这个函数用于检查队列何时不为空，不过任意的函数和可调用对象都可以传入wait()。当你已经写好了一个函数去做检查条件(或许比清单中简单检查要复杂很多)，那就可以直接将这个函数传入wait()；不一定非要放在一个lambda表达式中。**在调用wait()的过程中，一个条件变量可能会去检查给定条件若干次；然而，它总是在互斥量被锁定时这样做，当且仅当提供测试条件的函数返回true时，它就会立即返回。当等待线程重新获取互斥量并检查条件时，如果它并非直接响应另一个线程的通知，这就是所谓的伪唤醒**(spurious wakeup)。因为任何伪唤醒的数量和频率都是不确定的，这里不建议使用一个有副作用的函数做条件检查。当你这样做了，就必须做好多次产生副作用的心理准备。

解锁`std::unique_lock`的灵活性，不仅适用于对wait()的调用；它还可以用于有待处理但还未处理的数据⑥。处理数据可能是一个耗时的操作，并且如你在第3章见到的，你就知道持有锁的时间过长是一个多么糟糕的主意。

使用队列在多个线程中转移数据(如清单4.1)是很常见的。做得好的话，同步操作可以限制在队列本身，同步问题和条件竞争出现的概率也会降低。鉴于这些好处，现在从清单4.1中提取出一个通用线程安全的队列。

#### 4.1.2 使用条件变量构建线程安全队列

当你正在设计一个通用队列时，花一些时间想想有哪些操作需要添加到队列实现中去，就如之前在3.2.3节看到的线程安全的栈。可以看一下C++标准库提供的实现，找找灵感；`std::queue<>`容器的接口展示如下：

清单4.2 `std::queue`接口

```
template <class T, class Container = std::deque<T> >
class queue {
public:
  explicit queue(const Container&);
  explicit queue(Container&& = Container());
  template <class Alloc> explicit queue(const Alloc&);
  template <class Alloc> queue(const Container&, const Alloc&);
  template <class Alloc> queue(Container&&, const Alloc&);
  template <class Alloc> queue(queue&&, const Alloc&);

  void swap(queue& q);

  bool empty() const;
  size_type size() const;

  T& front();
  const T& front() const;
  T& back();
  const T& back() const;

  void push(const T& x);
  void push(T&& x);
  void pop();
  template <class... Args> void emplace(Args&&... args);
};
```

当你忽略构造、赋值以及交换操作时，你就剩下了三组操作：1. 对整个队列的状态进行查询(empty()和size());2.查询在队列中的各个元素(front()和back())；3.修改队列的操作(push(), pop()和emplace())。这就和3.2.3中的栈一样了，因此你也会遇到在固有接口上的条件竞争。因此，你需要**将front()和pop()合并成一个函数调用**，就像之前在栈实现时合并top()和pop()一样。与清单4.1中的代码不同的是：当使用队列在多个线程中传递数据时，***接收线程通常需要等待数据的压入。*** **这里我们提供pop()函数的两个变种：try_pop()和wait_and_pop()。try_pop() ，尝试从队列中弹出数据，总会直接返回(当有失败时)，即使没有值可检索；wait_and_pop()，将会等待有值可检索的时候才返回。**当你使用之前栈的方式来实现你的队列，你实现的队列接口就可能会是下面这样：

清单4.3 线程安全队列的接口

```
#include <memory> // 为了使用std::shared_ptr

template<typename T>
class threadsafe_queue
{
public:
  threadsafe_queue();
  threadsafe_queue(const threadsafe_queue&);
  threadsafe_queue& operator=(
      const threadsafe_queue&) = delete;  // 不允许简单的赋值

  void push(T new_value);

  bool try_pop(T& value);  // 1 在引用变量中存储着检索值，所以它可以用来返回队列中值的状态；当检索到一个变量时，他将返回true，否则将返回false
  std::shared_ptr<T> try_pop();  // 2 用来直接返回检索值的。当没有值可检索时，这个函数可以返回NULL指针

  void wait_and_pop(T& value);
  std::shared_ptr<T> wait_and_pop();

  bool empty() const;
};
```

就像之前对栈做的那样，在这里你将很多构造函数剪掉了，并且禁止了对队列的简单赋值。和之前一样，你也需要提供**两个版本的**try_pop()和wait_for_pop()。第一个重载的try_pop()①在引用变量中存储着检索值，所以它可以用来返回队列中值的状态；当检索到一个变量时，他将返回true，否则将返回false(详见A.2节)。第二个重载②就不能做这样了，因为它是用来直接返回检索值的。当没有值可检索时，这个函数可以返回NULL指针。

那么问题来了，如何将以上这些和清单4.1中的代码相关联呢？好吧，我们现在就来看看怎么去关联。你可以从之前的代码中提取push()和wait_and_pop()，如以下清单所示。

清单4.4 从清单4.1中提取push()和wait_and_pop()

```
#include <queue>
#include <mutex>
#include <condition_variable>

template<typename T>
class threadsafe_queue
{
private:
  std::mutex mut;
  std::queue<T> data_queue;
  std::condition_variable data_cond;
public:
  void push(T new_value)
  {
    std::lock_guard<std::mutex> lk(mut);//
    data_queue.push(new_value);
    data_cond.notify_one();
  }

  void wait_and_pop(T& value)
  {
    std::unique_lock<std::mutex> lk(mut);//
    data_cond.wait(lk,[this]{return !data_queue.empty();});//在条件满足的情况下(true)，从wait()返回并继续持有锁。当条件不满足时(false)，线程将对互斥量解锁，并且重新开始等待
    value=data_queue.front();
    data_queue.pop();
  }
};
threadsafe_queue<data_chunk> data_queue;  // 1

void data_preparation_thread()
{
  while(more_data_to_prepare())
  {
    data_chunk const data=prepare_data();
    data_queue.push(data);  // 2
  }
}

void data_processing_thread()
{
  while(true)
  {
    data_chunk data;
    data_queue.wait_and_pop(data);  // 3
    process(data);
    if(is_last_chunk(data))
      break;
  }
}
```

线程队列的实例中包含有互斥量和条件变量，所以独立的变量就不需要了①，并且调用push()也不需要外部同步②。当然，wait_and_pop()还要兼顾条件变量的等待③。

另一个wait_and_pop()函数的重载写起来就很琐碎了，剩下的函数就像从清单3.5实现的栈中一个个的粘过来一样。最终的队列实现如下所示。

**清单4.5 使用条件变量的线程安全队列(完整版)**

```
#include <queue>
#include <memory>
#include <mutex>
#include <condition_variable>

template<typename T>
class threadsafe_queue
{
private:
  mutable std::mutex mut;  // 1 互斥量必须是可变的 
  std::queue<T> data_queue;
  std::condition_variable data_cond;
public:
  threadsafe_queue()
  {}
  threadsafe_queue(threadsafe_queue const& other)
  {
    std::lock_guard<std::mutex> lk(other.mut);
    data_queue=other.data_queue;
  }

  void push(T new_value)
  {
    std::lock_guard<std::mutex> lk(mut);
    data_queue.push(new_value);
    data_cond.notify_one();// ??并不一定能唤醒 可能所有wait 都没有阻塞 所以会有伪唤醒(伪唤醒发生在mutex 上锁的时候)去检测条件 弥补不足??
  }

  //防止接口内在的条件竞争  这里提供引用版本 和 指针版本 支持引用和指针类型的数据
  void wait_and_pop(T& value)
  {
    std::unique_lock<std::mutex> lk(mut);
    data_cond.wait(lk,[this]{return !data_queue.empty();});//在条件满足的情况下(true)，从wait()返回并继续持有锁。当条件不满足时(false)，线程将对互斥量解锁，并且重新开始等待。
    value=data_queue.front();
    data_queue.pop();
  }

  std::shared_ptr<T> wait_and_pop()
  {
    std::unique_lock<std::mutex> lk(mut);
    data_cond.wait(lk,[this]{return !data_queue.empty();});
    std::shared_ptr<T> res(std::make_shared<T>(data_queue.front()));
    data_queue.pop();
    return res;
  }
  //防止接口内在的条件竞争  这里提供引用版本 和 指针版本 支持引用和指针类型的数据
  bool try_pop(T& value)
  {
    std::lock_guard<std::mutex> lk(mut);
    if(data_queue.empty())
      return false;
    value=data_queue.front();
    data_queue.pop();
    return true;
  }

  std::shared_ptr<T> try_pop()
  {
    std::lock_guard<std::mutex> lk(mut);
    if(data_queue.empty())
      return std::shared_ptr<T>();
    std::shared_ptr<T> res(std::make_shared<T>(data_queue.front()));
    data_queue.pop();
    return res;
  }

  bool empty() const
  {
    std::lock_guard<std::mutex> lk(mut);
    return data_queue.empty();
  }
};
```

empty()是一个const成员函数，并且传入拷贝构造函数的other形参是一个const引用；因为其他线程可能有这个类型的非const引用对象，并调用变种成员函数，所以这里有必要对互斥量上锁。如果锁住互斥量是一个可变操作，那么这个互斥量对象就会标记为可变的①，之后他就可以在empty()和拷贝构造函数中上锁了。

条件变量在多个线程等待同一个事件时，也是很有用的。当线程用来分解工作负载，并且只有一个线程可以对通知做出反应，与清单4.1中使用的结构完全相同；运行多个数据实例——*处理线程*(processing thread)。当新的数据准备完成，调用notify_one()将会触发一个正在执行wait()的线程，去检查条件和wait()函数的返回状态(因为你仅是向data_queue添加一个数据项)。 **这里不保证线程一定会被通知到，即使只有一个等待线程被通知时，所有处线程也有可能都在处理数据。**

另一种可能是，很多线程等待同一事件，对于通知他们都需要做出回应。这会发生在共享数据正在初始化的时候，当处理线程可以使用同一数据时，就要等待数据被初始化(有不错的机制可用来应对；可见第3章，3.3.1节)，或等待共享数据的更新，比如，*定期重新初始化*(periodic reinitialization)。在这些情况下，准备线程准备数据数据时，就会通过条件变量调用notify_all()成员函数，而非直接调用notify_one()函数。顾名思义，这就是全部线程在都去执行wait()(检查他们等待的条件是否满足)的原因。

> 3.3.1节使用 std::call_once std::once_flag 线程初始化 或者 static数据 函数包装返回引用  是线程安全的

**当等待线程只等待一次，当条件为true时，它就不会再等待条件变量了，所以一个条件变量可能并非同步机制的最好选择。尤其是，条件在等待一组可用的数据块时。**在这样的情况下，*期望*(future)就是一个适合的选择。



### 4.2 使用**期望**等待**一次性事件**

假设你乘飞机去国外度假。当你到达机场，并且办理完各种登机手续后，你还需要等待机场广播通知你登机，可能要等很多个小时。你可能会在候机室里面找一些事情来打发时间，比如：读书，上网，或者来一杯价格不菲的机场咖啡，不过从根本上来说你就在等待一件事情：机场广播能够登机的时间。给定的飞机班次再之后没有可参考性；当你在再次度假的时候，你可能会等待另一班飞机。

`C++`标准库模型将这种**一次性事件**称为*期望*(future)。当一个线程需要等待一个特定的一次性事件时，在某种程度上来说它就需要知道这个事件在未来的表现形式。之后，这个线程会周期性(较短的周期)的等待或检查，事件是否触发(检查信息板)；在检查期间也会执行其他任务(品尝昂贵的咖啡)。另外，在等待任务期间它可以先执行另外一些任务，直到对应的任务触发，而后等待期望的状态会变为*就绪*(ready)。一个“期望”可能是数据相关的(比如，你的登机口编号)，也可能不是。当事件发生时(并且期望状态为就绪)，这个“期望”就不能被重置。

在C++标准库中，有两种“期望”，使用两种类型模板实现，声明在<future>头文件中: 唯一*期望*(unique futures)(`std::future<>`)和*共享期望*(shared futures)(`std::shared_future<>`)。这是仿照`std::unique_ptr`和`std::shared_ptr`。**`std::future`的实例只能与一个指定事件相关联，而`std::shared_future`的实例就能关联多个事件。后者的实现中，所有实例会在同时变为就绪状态，并且他们可以访问与事件相关的任何数据。**这种数据关联与模板有关，比如`std::unique_ptr` 和`std::shared_ptr`的模板参数就是相关联的数据类型。在与数据无关的地方，可以使用`std::future<void>`与`std::shared_future<void>`的特化模板。虽然，我希望用于线程间的通讯，**但是“期望”对象本身并不提供同步访问。当多个线程需要访问一个独立“期望”对象时，他们必须使用互斥量或类似同步机制对访问进行保护，如在第3章提到的那样。不过，在你将要阅读到的4.2.5节中，多个线程会对一个`std::shared_future<>`实例的副本进行访问，而不需要期望同步，即使他们是同一个异步结果。**

最基本的一次性事件，就是一个后台运行出的计算结果。在第2章中，你已经了解了`std::thread` 执行的任务不能有返回值，并且我能保证，这个问题将在使用“期望”后解决——现在就来看看是怎么解决的。

#### 4.2.1 带返回值的后台任务

假设，你有一个需要长时间的运算，你需要其能计算出一个有效的值，但是你现在并不迫切需要这个值。可能你已经找到了生命、宇宙，以及万物的答案，就像道格拉斯·亚当斯[1]一样。你可以启动一个新线程来执行这个计算，但是这就意味着你必须关注如何传回计算的结果，因为`std::thread`并不提供直接接收返回值的机制。这里就需要`std::async`函数模板(也是在头文`<future>`中声明的)了。

当任务的结果你不着急要时，你可以使用`std::async`启动一个异步任务。与`std::thread`对象等待的方式不同，`std::async`会返回一个`std::future`对象，这个对象持有最终计算出来的结果。当你需要这个值时，你只需要调用这个对象的get()成员函数；并且会阻塞线程直到“期望”状态为就绪为止；之后，返回计算结果。下面清单中代码就是一个简单的例子。

清单4.6 使用`std::future`从异步任务中获取返回值

```
#include <future>
#include <iostream>

int find_the_answer_to_ltuae();
void do_other_stuff();
int main()
{
  std::future<int> the_answer=std::async(find_the_answer_to_ltuae);
  do_other_stuff();
  std::cout<<"The answer is "<<the_answer.get()<<std::endl;
}
```

[**std::async扩展**](https://blog.csdn.net/lijinqi1987/article/details/78909479)

与`std::thread` 做的方式一样，`std::async`允许你通过添加额外的调用参数，向函数传递额外的参数。当第一个参数是一个指向成员函数的指针，第二个参数提供有这个函数成员类的具体对象(不是直接的，就是通过指针，还可以包装在`std::ref`中)，剩余的参数可作为成员函数的参数传入。否则，第二个和随后的参数将作为函数的参数，或作为指定可调用对象的第一个参数。就如`std::thread`，当参数为*右值*(rvalues)时，拷贝操作将使用移动的方式转移原始数据。这就允许使用“只移动”类型作为函数对象和参数。来看一下下面的程序清单：

 清单4.7 使用`std::async`向函数传递参数
 
```
#include <string>
#include <future>
struct X
{
  void foo(int,std::string const&);
  std::string bar(std::string const&);
};
X x;
auto f1=std::async(&X::foo,&x,42,"hello");  // 调用p->foo(42, "hello")，p是指向x的指针
auto f2=std::async(&X::bar,x,"goodbye");  // 调用tmpx.bar("goodbye")， tmpx是x的拷贝副本
struct Y
{
  double operator()(double);
};
Y y;
auto f3=std::async(Y(),3.141);  // 调用tmpy(3.141)，tmpy通过Y的移动构造函数得到
auto f4=std::async(std::ref(y),2.718);  // 调用y(2.718)
X baz(X&);
std::async(baz,std::ref(x));  // 调用baz(x)
class move_only
{
public:
  move_only();
  move_only(move_only&&)
  move_only(move_only const&) = delete;
  move_only& operator=(move_only&&);
  move_only& operator=(move_only const&) = delete;
  
  void operator()();
};
auto f5=std::async(move_only());  // 调用tmp()，tmp是通过std::move(move_only())构造得到
```

[**Effective Modern C++ 条款36 如果异步执行是必需的，指定std::launch::async策略**](https://blog.csdn.net/big_yellow_duck/article/details/52512445)

在默认情况下，“期望”是否进行等待取决于`std::async`是否启动一个线程，或是否有任务正在进行同步。在大多数情况下(估计这就是你想要的结果)，但是你也可以在函数调用之前，向`std::async`传递一个额外参数。这个参数的类型是`std::launch`，还可以是`std::launch::defered`，用来表明函数调用被延迟到wait()或get()函数调用时才执行，`std::launch::async` 表明函数必须在其所在的独立线程上执行，`std::launch::deferred | std::launch::async`表明实现可以选择这两种方式的一种。最后一个选项是默认的。当函数调用被延迟，它可能不会在运行了??。如下所示：

```
auto f6=std::async(std::launch::async,Y(),1.2);  // 在新线程上执行
auto f7=std::async(std::launch::deferred,baz,std::ref(x));  // 在wait()或get()调用时执行
auto f8=std::async(
              std::launch::deferred | std::launch::async,
              baz,std::ref(x));  // 实现选择执行方式
auto f9=std::async(baz,std::ref(x));
f7.wait();  //  调用延迟函数 // 调用延迟函数，后台运行，此时如果有结果就前行，否则阻塞
```

在本章的后面和第8章中，你将会再次看到这段程序，使用`std::async`会让分割算法到各个任务中变的容易，这样程序就能并发的执行了。不过，这不是让一个`std::future`与一个任务实例相关联的唯一方式；你也可以将任务包装入一个`std::packaged_task<>`实例中，或通过编写代码的方式，使用`std::promise<>`类型模板显示设置值。与`std::promise<>`对比，`std::packaged_task<>`具有更高层的抽象，所以我们从“高抽象”的模板说起。



[**packaged_task**](https://zh.cppreference.com/w/cpp/thread/packaged_task)

#### 4.2.2 任务与期望

**`std::packaged_task<>`对一个函数或可调用对象，绑定一个期望。当`std::packaged_task<>` 对象被调用，它就会调用相关函数或可调用对象，将期望状态置为就绪，返回值也会被存储为相关数据。这可以用在构建线程池的结构单元(可见第9章)，或用于其他任务的管理**，比如在任务所在线程上运行任务，或将它们顺序的运行在一个特殊的后台线程上。当一个粒度较大的操作可以被分解为独立的子任务时，其中每个子任务就可以包含在一个`std::packaged_task<>`实例中，之后这个实例将传递到任务调度器或线程池中。对任务的细节进行抽象，调度器仅处理`std::packaged_task<>`实例，而非处理单独的函数。

[**C++ 符号修饰和函数签名**](https://www.cnblogs.com/wfwenchao/articles/4140388.html)

`std::packaged_task<>`的模板参数是一个函数签名，比如void()就是一个没有参数也没有返回值的函数，或int(std::string&, double*)就是有一个非const引用的`std::string`和一个指向double类型的指针，并且返回类型是int。当你构造出一个`std::packaged_task<>`实例时，你必须传入一个函数或可调用对象，这个函数或可调用的对象需要能接收指定的参数和返回可转换为指定返回类型的值。类型可以不完全匹配；你可以用一个int类型的参数和返回一个float类型的函数，来构建`std::packaged_task<double(double)>`的实例，因为在这里，类型可以隐式转换。

指定函数签名的返回类型可以用来标识，从get_future()返回的`std::future<>`的类型，不过函数签名的参数列表，可用来指定“打包任务”的函数调用操作符。例如，模板偏特化`std::packaged_task<std::string(std::vector<char>*,int)>`将在下面的代码清单中使用。

清单4.8 `std::packaged_task<>`的偏特化

```
template<>
class packaged_task<std::string(std::vector<char>*,int)>
{
public:
  template<typename Callable>
  explicit packaged_task(Callable&& f);
  std::future<std::string> get_future();
  void operator()(std::vector<char>*,int);
};
```
??
这里的`std::packaged_task`对象是一个可调用对象，并且它可以包含在一个`std::function`对象中，传递到`std::thread`对象中，就可作为线程函数；传递另一个函数中，就作为可调用对象，或可以直接进行调用。当`std::packaged_task`作为一个函数调用时，可为函数调用操作符提供所需的参数，并且返回值作为异步结果存储在`std::future`，可通过get_future()获取。你可以把一个任务包含入`std::packaged_task`，并且在检索期望之前，需要将`std::packaged_task`对象传入，以便调用时能及时的找到。

当你需要异步任务的返回值时，你可以等待期望的状态变为“就绪”。下面的代码就是这么个情况。

**线程间传递任务**

很多图形架构需要特定的线程去更新界面，所以当一个线程需要界面的更新时，它需要发出一条信息给正确的线程，让特定的线程来做界面更新。`std::packaged_task`提供了完成这种功能的一种方法，**且不需要发送一条自定义信息**给图形界面相关线程。下面来看看代码。

清单4.9 使用`std::packaged_task`执行一个图形界面线程

```
#include <deque>
#include <mutex>
#include <future>
#include <thread>
#include <utility>

std::mutex m;
std::deque<std::packaged_task<void()> > tasks;

bool gui_shutdown_message_received();
void get_and_process_gui_message();

void gui_thread()  // 1 图形界面线程
{
  while(!gui_shutdown_message_received())  // 2 循环直到收到一条关闭图形界面的信息后关闭
  {
    get_and_process_gui_message();  // 3 进行轮询界面消息处理
    std::packaged_task<void()> task;
    {
      std::lock_guard<std::mutex> lk(m);
      if(tasks.empty())  // 4 当队列中没有任务它将再次循环
        continue;
      task=std::move(tasks.front());  // 5 在队列中提取出一个任务
      tasks.pop_front();
    }
    task();  // 6 然后释放队列上的锁，并且执行任务 这里，“期望”与任务相关，当任务执行完成时，其状态会被置为“就绪”状态。
  }
}

std::thread gui_bg_thread(gui_thread);//图形界面线程

template<typename Func>
std::future<void> post_task_for_gui_thread(Func f)//将一个任务传入队列
{
  std::packaged_task<void()> task(f);  // 7 提供一个打包好的任务
  std::future<void> res=task.get_future();  // 8 调用get_future()成员函数获取“期望”对象
  std::lock_guard<std::mutex> lk(m);  // 9
  tasks.push_back(std::move(task));  // 10 任务被推入列表
  return res;//“期望”将返回调用函数  等待“期望”改变状态；否则，则会丢弃这个“期望”
}
```

这段代码十分简单：图形界面线程①循环直到收到一条关闭图形界面的信息后关闭②，进行轮询界面消息处理③，例如用户点击，和执行在队列中的任务。当队列中没有任务④，它将再次循环；除非，他能在队列中提取出一个任务⑤，然后释放队列上的锁，并且执行任务⑥。这里，“期望”与任务相关，当任务执行完成时，其状态会被置为“就绪”状态。

将一个任务传入队列，也很简单：提供的函数⑦可以提供一个打包好的任务，可以通过这个任务⑧调用get_future()成员函数获取“期望”对象，并且在任务被推入列表⑨之前，“期望”将返回调用函数⑩。当需要知道线程执行完任务时，向图形界面线程发布消息的代码，会等待“期望”改变状态；否则，则会丢弃这个“期望”。??

这个例子使用`std::packaged_task<void()>`创建任务，其包含了一个无参数无返回值的函数或可调用对象(如果当这个调用有返回值时，返回值会被丢弃)。这可能是最简单的任务，如你之前所见，`std::packaged_task`也可以用于一些复杂的情况——通过指定一个不同的函数签名作为模板参数，你不仅可以改变其返回类型(因此该类型的数据会存在期望相关的状态中)，而且也可以改变函数操作符的参数类型。这个例子可以简单的扩展成允许任务运行在图形界面线程上，且接受传参，还有通过`std::future`返回值，而不仅仅是完成一个指标。

这些任务能作为一个简单的函数调用来表达吗？还有，这些任务的结果能从很多地方得到吗？这些情况可以使用第三种方法创建“期望”来解决：使用`std::promise`对值进行显示设置。

#### 4.2.3 使用std::promises

当你有一个应用，需要处理很多网络连接，它会使用不同线程尝试连接每个接口，因为这能使网络尽早联通，尽早执行程序。当连接较少的时候，这样的工作没有问题(也就是线程数量比较少)。不幸的是，随着连接数量的增长，这种方式变的越来越不合适；因为大量的线程会消耗大量的系统资源，还有可能造成上下文频繁切换(当线程数量超出硬件可接受的并发数时)，这都会对性能有影响。最极端的例子就是，因为系统资源被创建的线程消耗殆尽，系统连接网络的能力会变的极差。在不同的应用程序中，存在着大量的网络连接，因此不同应用都会拥有一定数量的线程(可能只有一个)来处理网络连接，每个线程处理可同时处理多个连接事件。

考虑一个线程处理多个连接事件，来自不同的端口连接的数据包基本上是以乱序方式进行处理的；同样的，数据包也将以乱序的方式进入队列。在很多情况下，另一些应用不是等待数据成功的发送，就是等待一批(新的)来自指定网络接口的数据接收成功。

`std::promise<T>`提供设定值的方式(类型为T)，这个类型会和后面看到的`std::future<T>` 对象相关联。一对`std::promise/std::future`会为这种方式提供一个可行的机制；在期望上可以阻塞等待线程，同时，提供数据的线程可以使用组合中的“承诺”来对相关值进行设置，以及将“期望”的状态置为“就绪”。

**可以通过get_future()成员函数来获取与一个给定的`std::promise`相关的`std::future`对象，就像是与`std::packaged_task`相关。当“承诺”的值已经设置完毕(使用set_value()成员函数)，对应“期望”的状态变为“就绪”，并且可用于检索已存储的值。当你在设置值之前销毁`std::promise`，将会存储一个异常。在4.2.4节中，会详细描述异常是如何传送到线程的。**

清单4.10中，是单线程处理多接口的实现，如同我们所说的那样。在这个例子中，你可以使用一对`std::promise<bool>/std::future<bool>`找出一块传出成功的数据块；与“期望”相关值只是一个简单的“成功/失败”标识。对于传入包，与“期望”相关的数据就是数据包的有效负载。

清单4.10 使用“承诺”解决单线程多连接问题

```
#include <future>

void process_connections(connection_set& connections)
{
  while(!done(connections))  // 1 直到done()返回true为止
  {
    for(connection_iterator  // 2 每一次循环，程序都会依次的检查每一个连接
            connection=connections.begin(),end=connections.end();
          connection!=end;
          ++connection)
    {
      if(connection->has_incoming_data())  // 3 检索是否有数据
      {
        data_packet data=connection->incoming();
        std::promise<payload_type>& p=
            connection->get_promise(data.id);  // 4 这里假设输入数据包 data_packet  是具有ID和有效负载的(有实际的数在其中)。一个ID映射到一个`std::promise`(可能是在相关容器中进行的依次查找)并且值是设置在包的有效负载中的。
        p.set_value(data.payload);
      }
      if(connection->has_outgoing_data())  // 5 发送已入队的传出数据
      {
        outgoing_packet data=
            connection->top_of_outgoing_queue();
        connection->send(data.payload);
        data.promise.set_value(true);  // 6  对于传出包，包是从传出队列中进行检索的，实际上从接口直接发送出去。当发送完成，与传出数据相关的“承诺”将置为true，来表明传输成功
      }
    }
  }
}
```

函数process_connections()中，直到done()返回true①为止。每一次循环，程序都会依次的检查每一个连接②，检索是否有数据③或正在发送已入队的传出数据⑤。这里假设输入数据包是具有ID和有效负载的(有实际的数在其中)。一个ID映射到一个`std::promise`(可能是在相关容器中进行的依次查找)④，并且值是设置在包的有效负载中的。对于传出包，包是从传出队列中进行检索的，实际上从接口直接发送出去。当发送完成，与传出数据相关的“承诺”将置为true，来表明传输成功⑥。这是否能映射到实际网络协议上，取决于网络所用协议；这里的“承诺/期望”组合方式可能会在特殊的情况下无法工作，但是它与一些操作系统支持的异步输入/输出结构类似。

上面的代码完全不理会异常，它可能在想象的世界中，一切工作都会很好的执行，但是这有悖常理。有时候磁盘满载，有时候你会找不到东西，有时候网络会断，还有时候数据库会奔溃。当你需要某个操作的结果时，你就需要在对应的线程上执行这个操作，因为代码可以通过一个异常来报告错误；不过使用`std::packaged_task`或`std::promise`，就会带来一些不必要的限制(在所有工作都正常的情况下)。因此，C++标准库提供了一种在以上情况下清理异常的方法，并且允许他们将异常存储为相关结果的一部分。

#### 4.2.4 为“期望”存储“异常”

看完下面短小的代码段，思考一下，当你传递-1到square_root()中时，它将抛出一个异常，并且这个异常将会被调用者看到：

```
double square_root(double x)
{
  if(x<0)
  {
    throw std::out_of_range(“x<0”);
  }
  return sqrt(x);
}
```

假设调用square_root()函数不是当前线程，

```
double y=square_root(-1);
```

你将这样的调用改为异步调用：

```
std::future<double> f=std::async(square_root,-1);
double y=f.get();
```

如果行为是完全相同的时候，其结果是理想的；在任何情况下，y获得函数调用的结果，当线程调用f.get()时，就能再看到异常了，即使在一个单线程例子中。

好吧，事实的确如此：函数作为`std::async`的一部分时，当在调用时抛出一个异常，那么这个异常就会存储到“期望”的结果数据中，之后“期望”的状态被置为“就绪”，之后调用get()会抛出这个存储的异常。(注意：标准级别没有指定重新抛出的这个异常是原始的异常对象，还是一个拷贝；不同的编译器和库将会在这方面做出不同的选择)。当你将函数打包入`std::packaged_task`任务包中后，在这个任务被调用时，同样的事情也会发生；当打包函数抛出一个异常，这个异常将被存储在“期望”的结果中，准备在调用get()再次抛出。

当然，通过函数的显式调用，`std::promise`也能提供同样的功能。当你希望存入的是一个异常而非一个数值时，你就需要调用set_exception()成员函数，而非set_value()。这通常是用在一个catch块中，并作为算法的一部分，为了捕获异常，使用异常填充“承诺”：

```
extern std::promise<double> some_promise;
try
{
  some_promise.set_value(calculate_value());
}
catch(...)
{
  some_promise.set_exception(std::current_exception());
}
```

这里使用了`std::current_exception()`来检索抛出的异常；可用`std::copy_exception()`作为一个替换方案，`std::copy_exception()`会直接存储一个新的异常而不抛出：

```
some_promise.set_exception(std::copy_exception(std::logic_error("foo ")));
```

这就比使用try/catch块更加清晰，当异常类型是已知的，它就应该优先被使用；不是因为代码实现简单，而是它给编译器提供了极大的代码优化空间。

另一种向“期望”中存储异常的方式是，在没有调用“承诺”上的任何设置函数前，或正在调用包装好的任务时，销毁与`std::promise`或`std::packaged_task`相关的“期望”对象。在这任何情况下，当“期望”的状态还不是“就绪”时，调用`std::promise`或`std::packaged_task`的析构函数，将会存储一个与`std::future_errc::broken_promise`错误状态相关的`std::future_error`异常；通过创建一个“期望”，你可以构造一个“承诺”为其提供值或异常；你可以通过销毁值和异常源，去违背“承诺”。在这种情况下，编译器没有在“期望”中存储任何东西，等待线程可能会永远的等下去。

直到现在，所有例子都在用`std::future`。不过，**`std::future`也有局限性，在很多线程在等待的时候，只有一个线程能获取等待结果。当多个线程需要等待相同的事件的结果，你就需要使用`std::shared_future`来替代`std::future`了。**

#### 4.2.5 多个线程的等待

虽然`std::future`可以处理所有在线程间数据转移的必要同步，但是调用某一特殊` std::future`对象的成员函数，就会让这个线程的数据和其他线程的数据不同步。当多线程在没有额外同步的情况下，访问一个独立的`std::future`对象时，就会有数据竞争和未定义的行为。这是因为：`std::future`模型独享同步结果的所有权，并且通过调用get()函数，一次性的获取数据，这就让并发访问变的毫无意义——只有一个线程可以获取结果值，因为在第一次调用get()后，就没有值可以再获取了。

如果你的并行代码没有办法让多个线程等待同一个事件，先别太失落；`std::shared_future`可以来帮你解决。因为`std::future`是只移动的，所以其所有权可以在不同的实例中互相传递，但是只有一个实例可以获得特定的同步结果；而`std::shared_future`实例是可拷贝的，所以多个对象可以引用同一关联“期望”的结果。

在每一个`std::shared_future`的独立对象上成员函数调用返回的结果还是不同步的，所以为了在多个线程访问一个独立对象时，避免数据竞争，必须使用锁来对访问进行保护。优先使用的办法：为了替代只有一个拷贝对象的情况，可以让每个线程都拥有自己对应的拷贝对象。这样，当每个线程都通过自己拥有的`std::shared_future`对象获取结果，那么多个线程访问共享同步结果就是安全的。可见图4.1。

![](https://github.com/xiaoweiChen/Cpp_Concurrency_In_Action/blob/master/images/chapter4/4-1.png?raw=true) 

图4.1 使用多个`std::shared_future`对象来避免数据竞争

有可能会使用`std::shared_future`的地方，例如，实现类似于复杂的电子表格的并行执行；每一个单元格有单一的终值，这个终值可能是有其他单元格中的数据通过公式计算得到的。公式计算得到的结果依赖于其他单元格，然后可以使用一个`std::shared_future`对象引用第一个单元格的数据。当每个单元格内的所有公式并行执行后，这些任务会以期望的方式完成工作；不过，当其中有计算需要依赖其他单元格的值，那么它就会被阻塞，直到依赖单元格的数据准备就绪。这将让系统在最大程度上使用可用的硬件并发。

`std::shared_future`的实例同步`std::future`实例的状态。当`std::future`对象没有与其他对象共享同步状态所有权，那么所有权必须使用`std::move`将所有权传递到`std::shared_future`，其默认构造函数如下：

```
std::promise<int> p;
std::future<int> f(p.get_future());
assert(f.valid());  // 1 "期望" f 是合法的
std::shared_future<int> sf(std::move(f));
assert(!f.valid());  // 2 "期望" f 现在是不合法的
assert(sf.valid());  // 3 sf 现在是合法的
```

这里，“期望”f开始是合法的①，因为它引用的是“承诺”p的同步状态，但是在转移sf的状态后，f就不合法了②，而sf就是合法的了③。

如其他可移动对象一样，转移所有权是对右值的隐式操作，所以你可以通过`std::promise`对象的成员函数get_future()的返回值，直接构造一个`std::shared_future`对象，例如：

```
std::promise<std::string> p;
std::shared_future<std::string> sf(p.get_future());  // 1 隐式转移所有权
```

这里转移所有权是隐式的；用一个右值构造`std::shared_future<>`，得到`std::future<std::string>`类型的实例①。

`std::future`的这种特性，可促进`std::shared_future`的使用，容器可以自动的对类型进行推断，从而初始化这个类型的变量(详见附录A，A.6节)。`std::future`有一个share()成员函数，可用来创建新的`std::shared_future` ，并且可以直接转移“期望”的所有权。这样也就能保存很多类型，并且使得代码易于修改：

```
std::promise< std::map< SomeIndexType, SomeDataType, SomeComparator,
     SomeAllocator>::iterator> p;
auto sf=p.get_future().share();
```

在这个例子中，sf的类型推到为`std::shared_future<std::map<SomeIndexType, SomeDataType, SomeComparator, SomeAllocator>::iterator>`，一口气还真的很难念完。当比较器或分配器有所改动，你只需要对“承诺”的类型进行修改即可；“期望”的类型会自动更新，与“承诺”的修改进行匹配。

有时候你需要限定等待一个事件的时间，不论是因为你在时间上有硬性规定(一段指定的代码需要在某段时间内完成)，还是因为在事件没有很快的触发时，有其他必要的工作需要特定线程来完成。为了处理这种情况，很多等待函数具有用于指定超时的变量。

--------

[1] 在《银河系漫游指南》(*The Hitchhiker’s Guide to the Galaxy*)中, 计算机在经过深度思考后，将“人生之匙和宇宙万物”的答案确定为42。



### 4.3 限定等待时间

之前介绍过的所有阻塞调用，将会阻塞一段不确定的时间，将线程挂起直到等待的事件发生。在很多情况下，这样的方式很不错，但是在其他一些情况下，你就需要限制一下线程等待的时间了。这允许你发送一些类似“我还存活”的信息，无论是对交互式用户，或是其他进程，亦或当用户放弃等待，你可以按下“取消”键直接终止等待。

介绍两种可能是你希望指定的超时方式：一种是“时延”的超时方式，另一种是“绝对”超时方式。第一种方式，需要指定一段时间(例如，30毫秒)；第二种方式，就是指定一个时间点(例如，协调世界时[UTC]17:30:15.045987023，2011年11月30日)。多数等待函数提供变量，对两种超时方式进行处理。处理持续时间的变量以“_for”作为后缀，处理绝对时间的变量以"_until"作为后缀。

所以，当`std::condition_variable`的两个成员函数wait_for()和wait_until()成员函数分别有两个负载，这两个负载都与wait()成员函数的负载相关——其中一个负载只是等待信号触发，或时间超期，亦或是一个虚假的唤醒，并且醒来时，会检查锁提供的谓词，并且只有在检查为true时才会返回(这时条件变量的条件达成)，或直接而超时。

在我们观察使用超时函数的细节前，让我们来检查一下时间在C++中指定的方式，就从时钟开始吧！

#### 4.3.1 时钟

对于C++标准库来说，时钟就是时间信息源。特别是，时钟是一个类，提供了四种不同的信息：

* 现在时间

* 时间类型

* 时钟节拍

* 通过时钟节拍的分布，判断时钟是否稳定

时钟的当前时间可以通过调用静态成员函数now()从时钟类中获取；例如，`std::chrono::system_clock::now()`是将返回系统时钟的当前时间。特定的时间点类型可以通过time_point的数据typedef成员来指定，所以some_clock::now()的类型就是some_clock::time_point。

时钟节拍被指定为1/x(x在不同硬件上有不同的值)秒，这是由时间周期所决定——一个时钟一秒有25个节拍，因此一个周期为`std::ratio<1, 25>`，当一个时钟的时钟节拍每2.5秒一次，周期就可以表示为`std::ratio<5, 2>`。当时钟节拍直到运行时都无法知晓，可以使用一个给定的应用程序运行多次，周期可以用执行的平均时间求出，其中最短的时间可能就是时钟节拍，或者是直接写在手册当中。这就不保证在给定应用中观察到的节拍周期与指定的时钟周期相匹配。

当时钟节拍均匀分布(无论是否与周期匹配)，并且不可调整，这种时钟就称为稳定时钟。当is_steady静态数据成员为true时，表明这个时钟就是稳定的，否则，就是不稳定的。通常情况下，`std::chrono::system_clock`是不稳定的，因为时钟是可调的，即是这种是完全自动适应本地账户的调节。这种调节可能造成的是，首次调用now()返回的时间要早于上次调用now()所返回的时间，这就违反了节拍频率的均匀分布。稳定闹钟对于超时的计算很重要，所以C++标准库提供一个稳定时钟`std::chrono::steady_clock`。C++标准库提供的其他时钟可表示为`std::chrono::system_clock`(在上面已经提到过)，它代表了系统时钟的“实际时间”，并且提供了函数可将时间点转化为time_t类型的值；`std::chrono::high_resolution_clock` 可能是标准库中提供的具有最小节拍周期(因此具有最高的精度[分辨率])的时钟。它实际上是typedef的另一种时钟，这些时钟和其他与时间相关的工具，都被定义在<chrono>库头文件中。

我们马上来看一下时间点是如何表示的，但在这之前，我们先看一下持续时间是怎么表示的。

#### 4.3.2 时延

时延是时间部分最简单的；`std::chrono::duration<>`函数模板能够对时延进行处理(线程库使用到的所有C++时间处理工具，都在`std::chrono`命名空间内)。第一个模板参数是一个类型表示(比如，int，long或double)，第二个模板参数是制定部分，表示每一个单元所用秒数。例如，当几分钟的时间要存在short类型中时，可以写成`std::chrono::duration<short, std::ratio<60, 1>>`，因为60秒是才是1分钟，所以第二个参数写成`std::ratio<60, 1>`。另一方面，当需要将毫秒级计数存在double类型中时，可以写成`std::chrono::duration<double, std::ratio<1, 1000>>`，因为1秒等于1000毫秒。

标准库在`std::chrono`命名空间内，为延时变量提供一系列预定义类型：nanoseconds[纳秒] , microseconds[微秒] , milliseconds[毫秒] , seconds[秒] , minutes[分]和hours[时]。比如，你要在一个合适的单元表示一段超过500年的时延，预定义类型可充分利用了大整型，来表示所要表示的时间类型。当然，这里也定义了一些国际单位制(SI, [法]le Système international d'unités)分数，可从`std::atto(10^(-18))`到`std::exa(10^(18))`(题外话：当你的平台支持128位整型);也可以指定自定义时延类型，例如，`std::duration<double, std::centi>`，就可以使用一个double类型的变量表示1/100。

当不要求截断值的情况下(时转换成秒是没问题，但是秒转换成时就不行)时延的转换是隐式的。显示转换可以由`std::chrono::duration_cast<>`来完成。

```
std::chrono::milliseconds ms(54802);
std::chrono::seconds s=
       std::chrono::duration_cast<std::chrono::seconds>(ms);
```

这里的结果就是截断的，而不是进行了舍入，所以s最后的值将为54。

延迟支持计算，所以你能够对两个时延变量进行加减，或者是对一个时延变量乘除一个常数(模板的第一个参数)来获得一个新延迟变量。例如，5*seconds(1)与seconds(5)或minutes(1)-seconds(55)一样。在时延中可以通过count()成员函数获得单位时间的数量。例如，`std::chrono::milliseconds(1234).count()`就是1234。

基于时延的等待可由`std::chrono::duration<>`来完成。例如，你等待一个“期望”状态变为就绪已经35毫秒：

```
std::future<int> f=std::async(some_task);
if(f.wait_for(std::chrono::milliseconds(35))==std::future_status::ready)
  do_something_with(f.get());
```

等待函数会返回一个状态值，来表示等待是超时，还是继续等待。在这种情况下，你可以等待一个“期望”，所以当函数等待超时时，会返回`std::future_status::timeout`；当“期望”状态改变，函数会返回`std::future_status::ready`；当“期望”的任务延迟了，函数会返回`std::future_status::deferred`。基于时延的等待是使用内部库提供的稳定时钟，来进行计时的；所以，即使系统时钟在等待时被调整(向前或向后)，35毫秒的时延在这里意味着，的确耗时35毫秒。当然，难以预料的系统调度和不同操作系统的时钟精度都意味着：在线程中，从调用到返回的实际时间可能要比35毫秒长。

时延中没有特别好的办法来处理以上情况，所以我们暂且停下对时延的讨论。现在，我们就要来看看“时间点”是怎么样工作的。

#### 4.3.3 时间点

时钟的时间点可以用`std::chrono::time_point<>`的类型模板实例来表示，实例的第一个参数用来指定所要使用的时钟，第二个函数参数用来表示时间的计量单位(特化的`std::chrono::duration<>`)。一个时间点的值就是时间的长度(在指定时间的倍数内)，例如，指定“unix时间戳”(*epoch*)为一个时间点。时间戳是时钟的一个基本属性，但是不可以直接查询，或在C++标准中已经指定。通常，unix时间戳表示1970年1月1日 00:00，即计算机启动应用程序时。时钟可能共享一个时间戳，或具有独立的时间戳。当两个时钟共享一个时间戳时，其中一个time_point类型可以与另一个时钟类型中的time_point相关联。这里，虽然你无法知道unix时间戳是什么，但是你可以通过对指定time_point类型使用time_since_epoch()来获取时间戳。这个成员函数会返回一个时延值，这个时延值是指定时间点到时钟的unix时间戳锁用时。

例如，你可能指定了一个时间点`std::chrono::time_point<std::chrono::system_clock, std::chrono::minutes>`。这就与系统时钟有关，且实际中的一分钟与系统时钟精度应该不相同(通常差几秒)。

你可以通过`std::chrono::time_point<>`实例来加/减时延，来获得一个新的时间点，所以`std::chrono::hight_resolution_clock::now() + std::chrono::nanoseconds(500)`将得到500纳秒后的时间。当你知道一块代码的最大时延时，这对于计算绝对时间的超时是一个好消息，当等待时间内，等待函数进行多次调用；或，非等待函数且占用了等待函数时延中的时间。

你也可以减去一个时间点(二者需要共享同一个时钟)。结果是两个时间点的时间差。这对于代码块的计时是很有用的，例如：

```
auto start=std::chrono::high_resolution_clock::now();
do_something();
auto stop=std::chrono::high_resolution_clock::now();
std::cout<<”do_something() took “
  <<std::chrono::duration<double,std::chrono::seconds>(stop-start).count()
  <<” seconds”<<std::endl;
```

`std::chrono::time_point<>`实例的时钟参数可不仅是能够指定unix时间戳的。当你想一个等待函数(绝对时间超时的方式)传递时间点时，时间点的时钟参数就被用来测量时间。当时钟变更时，会产生严重的后果，因为等待轨迹随着时钟的改变而改变，并且知道调用时钟的now()成员函数时，才能返回一个超过超时时间的值。当时钟向前调整，这就有可能减小等待时间的总长度(与稳定时钟的测量相比)；当时钟向后调整，就有可能增加等待时间的总长度。

如你期望的那样，后缀为_unitl的(等待函数的)变量会使用时间点。通常是使用某些时钟的`::now()`(程序中一个固定的时间点)作为偏移，虽然时间点与系统时钟有关，可以使用`std::chrono::system_clock::to_time_point()` 静态成员函数，在用户可视时间点上进行调度操作。例如，当你有一个对多等待500毫秒的，且与条件变量相关的事件，你可以参考如下代码：

清单4.11 等待一个条件变量——有超时功能

```
#include <condition_variable>
#include <mutex>
#include <chrono>

std::condition_variable cv;
bool done;
std::mutex m;

bool wait_loop()
{
  auto const timeout= std::chrono::steady_clock::now()+
      std::chrono::milliseconds(500);
  std::unique_lock<std::mutex> lk(m);
  while(!done)
  {
    if(cv.wait_until(lk,timeout)==std::cv_status::timeout)
      break;
  }
  return done;
}
```

这种方式是我们推荐的，当你没有什么事情可以等待时，可在一定时限中等待条件变量。在这种方式中，循环的整体长度是有限的。如你在4.1.1节中所见，当使用条件变量(且无事可待)时，你就需要使用循环，这是为了处理假唤醒。当你在循环中使用wait_for()时，你可能在等待了足够长的时间后结束等待(在假唤醒之前)，且下一次等待又开始了。这可能重复很多次，使得等待时间无边无际。

到此，有关时间点超时的基本知识你已经了解了。现在，让我们来了解一下如何在函数中使用超时。

#### 4.3.4 具有超时功能的函数

使用超时的最简单方式就是，对一个特定线程添加一个延迟处理；当这个线程无所事事时，就不会占用可供其他线程处理的时间。你在4.1节中看过一个例子，你循环检查“done”标志。两个处理函数分别是`std::this_thread::sleep_for()`和`std::this_thread::sleep_until()`。他们的工作就像一个简单的闹钟：当线程因为指定时延而进入睡眠时，可使用sleep_for()唤醒；或因指定时间点睡眠的，可使用sleep_until唤醒。sleep_for()的使用如同在4.1节中的例子，有些事必须在指定时间范围内完成，所以耗时在这里就很重要。另一方面，sleep_until()允许在某个特定时间点将调度线程唤醒。这有可能在晚间备份，或在早上6:00打印工资条时使用，亦或挂起线程直到下一帧刷新时进行视频播放。

当然，休眠只是超时处理的一种形式；你已经看到了，超时可以配合条件变量和“期望”一起使用。超时甚至可以在尝试获取一个互斥锁时(当互斥量支持超时时)使用。`std::mutex`和`std::recursive_mutex`都不支持超时锁，但是`std::timed_mutex`和`std::recursive_timed_mutex`支持。这两种类型也有try_lock_for()和try_lock_until()成员函数，可以在一段时期内尝试，或在指定时间点前获取互斥锁。表4.1展示了C++标准库中支持超时的函数。参数列表为“延时”(*duration*)必须是`std::duration<>`的实例，并且列出为*时间点*(time_point)必须是`std::time_point<>`的实例。

表4.1 可接受超时的函数

<table border=1>
  <td>类型/命名空间</td>
  <td>函数</td>
  <td>返回值</td>
<tr>
  <td rowspan=2> std::this_thread[namespace] </td>
  <td> sleep_for(duration) </td>
  <td rowspan=2>N/A</td>
</tr>
<tr>
  <td>sleep_until(time_point)</td>
</tr>
<tr>
  <td rowspan = 2>std::condition_variable 或 std::condition_variable_any</td>
  <td>wait_for(lock, duration)</td>
  <td rowspan = 2>std::cv_status::time_out 或 std::cv_status::no_timeout</td>
</tr>
<tr>
  <td>wait_until(lock, time_point)</td>
</tr>
<tr>
  <td rowspan = 2> </td>
  <td> wait_for(lock, duration, predicate)</td>
  <td rowspan = 2>bool —— 当唤醒时，返回谓词的结果</td>
</tr>
<tr>
  <td>wait_until(lock, duration, predicate)</td>
</tr>
<tr>
  <td rowspan = 2>std::timed_mutex 或 std::recursive_timed_mutex</td>
  <td>try_lock_for(duration)</td>
  <td rowspan = 2> bool —— 获取锁时返回true，否则返回fasle</td>
</tr>
<tr>
  <td>try_lock_until(time_point)</td>
</tr>
<tr>
  <td rowspan = 2>std::unique_lock&lt;TimedLockable&gt;</td>
  <td>unique_lock(lockable, duration)</td>
  <td>N/A —— 对新构建的对象调用owns_lock();</td>
</tr>
<tr>
  <td>unique_lock(lockable, time_point)</td>
  <td>当获取锁时返回true，否则返回false</td>
</tr>
<tr>
  <td rowspan = 2></td>
  <td>try_lock_for(duration)</td>
  <td rowspan = 2>bool —— 当获取锁时返回true，否则返回false</td>
</tr>
<tr>
  <td>try_lock_until(time_point)</td>
</tr>
<tr>
  <td rowspan = 3>std::future&lt;ValueType&gt;或std::shared_future&lt;ValueType&gt;</td>
  <td>wait_for(duration)</td>
  <td>当等待超时，返回std::future_status::timeout</td>
</tr>
<tr>
  <td rowspan = 2>wait_until(time_point)</td>
  <td>当“期望”准备就绪时，返回std::future_status::ready</td>
</tr>
<tr>
  <td>当“期望”持有一个为启动的延迟函数，返回std::future_status::deferred</td>
</tr>
</table>

现在，我们讨论的机制有：条件变量、“期望”、“承诺”还有打包的任务。是时候从更高的角度去看待这些机制，怎么样使用这些机制，简化线程的同步操作。

### 4.4 使用同步操作简化代码

同步工具的使用在本章称为构建块，你可以之关注那些需要同步的操作，而非具体使用的机制。当需要为程序的并发时，这是一种可以帮助你简化你的代码的方式，提供更多的函数化的方法。比起在多个线程间直接共享数据，每个任务拥有自己的数据会应该会更好，并且结果可以对其他线程进行广播，这就需要使用“期望”来完成了。

#### 4.4.1 使用“期望”的函数化编程

术语*函数化编程*(functional programming)引用于一种编程方式，这种方式中的函数结果只依赖于传入函数的参数，并不依赖外部状态。当一个函数与数学概念相关时，当你使用相同的函数调用这个函数两次，这两次的结果会完全相同。`C++`标准库中很多与数学相关的函数都有这个特性，例如，sin(正弦),cos(余弦)和sqrt(平方根)；当然，还有基本类型间的简单运算，例如，3+3，6*9，或1.3/4.7。一个纯粹的函数不会改变任何外部状态，并且这种特性完全限制了函数的返回值。

很容易想象这是一种什么样的情况，特别是当并行发生时，因为在第三章时我们讨论过，很多问题发生在共享数据上。当共享数据没有被修改，那么就不存在条件竞争，并且没有必要使用互斥量去保护共享数据。这可对编程进行极大的简化，例如Haskell语言[2]，在Haskell中函数默认就是这么的“纯粹”；这种纯粹对的方式，在并发编程系统中越来越受欢迎。因为大多数函数都是纯粹的，那么非纯粹的函数对共享数据的修改就显得更为突出，所以其很容易适应应用的整体结构。

函数化编程的好处，并不限于那些将“纯粹”作为默认方式(范型)的语言。`C++`是一个多范型的语言，其也可以写出FP类型的程序。在`C++11`中这种方式要比`C++98`简单许多，因为`C++11`支持lambda表达式(详见附录A，A.6节)，还加入了[Boost](http://zh.wikipedia.org/wiki/Boost_C%2B%2B_Libraries)和[TR1](http://zh.wikipedia.org/wiki/C%2B%2B_Technical_Report_1)中的`std::bind`，以及自动可以自行推断类型的自动变量(详见附录A，A.7节)。“期望”作为拼图的最后一块，它使得*函数化编程模式并发化*(FP-style concurrency)在`C++`中成为可能；一个“期望”对象可以在线程间互相传递，并允许其中一个计算结果依赖于另外一个的结果，而非对共享数据的显式访问。

**快速排序 FP模式版**

为了描述在*函数化*(PF)并发中使用“期望”，让我们来看看一个简单的实现——快速排序算法。该算法的基本思想很简单：给定一个数据列表，然后选取其中一个数为“中间”值，之后将列表中的其他数值分成两组——一组比中间值大，另一组比中间值小。之后对小于“中间”值的组进行排序，并返回排序好的列表；再返回“中间”值；再对比“中间”值大的组进行排序，并返回排序的列表。图4.2中展示了10个整数在这种方式下进行排序的过程。


![](https://github.com/xiaoweiChen/Cpp_Concurrency_In_Action/blob/master/images/chapter4/4-2.png?raw=true)
图4.2 FP-模式的递归排序

下面清单中的代码是FP-模式的顺序实现，它需要传入列表，并且返回一个列表，而非与`std::sort()`做同样的事情。
(译者：`std::sort()`是无返回值的，因为参数接收的是迭代器，所以其可以对原始列表直进行修改与排序。可参考[sort()](http://www.cplusplus.com/reference/algorithm/sort/?kw=sort))

清单4.12 快速排序——顺序实现版

```
template<typename T>
std::list<T> sequential_quick_sort(std::list<T> input)
{
  if(input.empty())
  {
    return input;
  }
  std::list<T> result;
  result.splice(result.begin(),input,input.begin());  // 1 将输入的首个元素(中间值)放入结果列表中
  T const& pivot=*result.begin();  // 2

  auto divide_point=std::partition(input.begin(),input.end(),
             [&](T const& t){return t<pivot;});  // 3

  std::list<T> lower_part;
  lower_part.splice(lower_part.end(),input,input.begin(),
             divide_point);  // 4
  auto new_lower(
             sequential_quick_sort(std::move(lower_part)));  // 5
  auto new_higher(
             sequential_quick_sort(std::move(input)));  // 6

  result.splice(result.end(),new_higher);  // 7
  result.splice(result.begin(),new_lower);  // 8
  return result;
}
```

虽然接口的形式是FP模式的，但当你使用FP模式时，你需要做大量的拷贝操作，所以在内部你会使用“普通”的命令模式。你选择第一个数为“中间”值，使用splice()①将输入的首个元素(中间值)放入结果列表中。虽然这种方式产生的结果可能不是最优的(会有大量的比较和交换操作)，但是对`std::list`做任何事都需要花费较长的时间，因为链表是遍历访问的。你知道你想要什么样的结果，所以你可以直接将要使用的“中间”值提前进行拼接。现在你还需要使用“中间”值进行比较，所以这里使用了一个引用②，为了避免过多的拷贝。之后，你可以使用`std::partition`将序列中的值分成小于“中间”值的组和大于“中间”值的组③。最简单的方法就是使用lambda函数指定区分的标准；使用已获取的引用避免对“中间”值的拷贝(详见附录A，A.5节，更多有关lambda函数的信息)。

`std::partition()`对列表进行重置，并返回一个指向首元素(*不*小于“中间”值)的迭代器。迭代器的类型全称可能会很长，所以你可以使用auto类型说明符，让编译器帮助你定义迭代器类型的变量(详见附录A，A.7节)。

现在，你已经选择了FP模式的接口；所以，当你要使用递归对两部分排序是，你将需要创建两个列表。你可以用splice()函数来做这件事，将input列表小于divided_point的值移动到新列表lower_part④中。其他数继续留在input列表中。而后，你可以使用递归调用⑤⑥的方式，对两个列表进行排序。这里显式使用`std::move()`将列表传递到类函数中，这种方式还是为了避免大量的拷贝操作。最终，你可以再次使用splice()，将result中的结果以正确的顺序进行拼接。new_higher指向的值放在“中间”值的后面⑦，new_lower指向的值放在“中间”值的前面⑧。

**快速排序 FP模式线程强化版**

因为还是使用函数化模式，所以使用“期望”很容易将其转化为并行的版本，如下面的程序清单所示。其中的操作与前面相同，不同的是它们现在并行运行。

清单4.13 快速排序——“期望”并行版

```
template<typename T>
std::list<T> parallel_quick_sort(std::list<T> input)
{
  if(input.empty())
  {
    return input;
  }
  std::list<T> result;
  result.splice(result.begin(),input,input.begin());
  T const& pivot=*result.begin();

  auto divide_point=std::partition(input.begin(),input.end(),
                [&](T const& t){return t<pivot;});

  std::list<T> lower_part;
  lower_part.splice(lower_part.end(),input,input.begin(),
                divide_point);

  std::future<std::list<T> > new_lower(  // 1
                std::async(&parallel_quick_sort<T>,std::move(lower_part)));

  auto new_higher(
                parallel_quick_sort(std::move(input)));  // 2

  result.splice(result.end(),new_higher);  // 3
  result.splice(result.begin(),new_lower.get());  // 4
  return result;
}
```

这里最大的变化是，当前线程不对小于“中间”值部分的列表进行排序，使用`std::async()`①在另一线程对其进行排序。大于部分列表，如同之前一样，使用递归的方式进行排序②。通过递归调用parallel_quick_sort()，你就可以利用可用的硬件并发了。`std::async()`会启动一个新线程，这样当你递归三次时，就会有八个线程在运行了；当你递归十次(对于大约有1000个元素的列表)，如果硬件能处理这十次递归调用，你将会创建1024个执行线程。当运行库认为这样做产生了太多的任务时(也许是因为数量超过了硬件并发的最大值)，运行库可能会同步的切换新产生的任务。当任务过多时(已影响性能)，这些任务应该在使用get()函数获取的线程上运行，而不是在新线程上运行，这样就能避免任务向线程传递的开销。值的注意的是，这完全符合`std::async`的实现，为每一个任务启动一个线程(甚至在任务超额时；在`std::launch::deferred`没有明确规定的情况下)；或为了同步执行所有任务(在`std::launch::async`有明确规定的情况下)。当你依赖运行库的自动缩放，建议你去查看一下你的实现文档，了解一下将会有怎么样的行为表现。

比起使用`std::async()`，你可以写一个spawn_task()函数对`std::packaged_task`和`std::thread`做简单的包装，如清单4.14中的代码所示；你需要为函数结果创建一个`std::packaged_task`对象， 可以从这个对象中获取“期望”，或在线程中执行它，返回“期望”。其本身并不提供太多的好处(并且事实上会造成大规模的超额任务)，但是它会为转型成一个更复杂的实现铺平道路，将会实现向一个队列添加任务，而后使用线程池的方式来运行它们。我们将在第9章再讨论线程池。使用`std::async`更适合于当你知道你在干什么，并且要完全控制在线程池中构建或执行过任务的线程。

清单4.14 spawn_task的简单实现

```
template<typename F,typename A>
std::future<std::result_of<F(A&&)>::type>
   spawn_task(F&& f,A&& a)
{
  typedef std::result_of<F(A&&)>::type result_type;
  std::packaged_task<result_type(A&&)>
       task(std::move(f)));
  std::future<result_type> res(task.get_future());
  std::thread t(std::move(task),std::move(a));
  t.detach();
  return res;
}
```

其他先不管，回到parallel_quick_sort函数。因为你只是直接递归去获取new_higher列表，你可以如之前一样对new_higher进行拼接③。但是，new_lower列表是`std::future<std::list<T>>`的实例，而非是一个简单的列表，所以你需要调用get()成员函数在调用splice()④之前去检索数值。在这之后，等待后台任务完成，并且将结果移入splice()调用中；get()返回一个包含结果的右值引用，所以这个结果是可以移出的(详见附录A，A.1.1节，有更多有关右值引用和移动语义的信息)。

即使假设，使用`std::async()`是对可用硬件并发最好的选择，但是这样的并行实现对于快速排序来说，依然不是最理想的。其中，`std::partition`做了很多工作，即使做了依旧是顺序调用，但就现在的情况来说，已经足够好了。如果你对实现最快并行的可能性感兴趣的话，你可以去查阅一些学术文献。

因为避开了共享易变数据，函数化编程可算作是并发编程的范型；并且也是*通讯顺序进程*(CSP,Communicating Sequential Processer[3],)的范型，这里线程理论上是完全分开的，也就是没有共享数据，但是有通讯通道允许信息在不同线程间进行传递。这种范型被[Erlang语言](http://www.erlang.org)所采纳，并且在[MPI](http://www.mpi-forum.org)(*Message Passing Interface*，消息传递接口)上常用来做C和`C++`的高性能运算。现在你应该不会在对学习它们而感到惊奇了吧，因为只需遵守一些约定，`C++`就能支持它们；在接下来的一节中，我们会讨论实现这种方式。

### 4.4.2 使用消息传递的同步操作

CSP的概念十分简单：当没有共享数据，每个线程就可以进行独立思考，其行为纯粹基于其所接收到的信息。每个线程就都有一个状态机：当线程收到一条信息，它将会以某种方式更新其状态，并且可能向其他线程发出一条或多条信息，对于消息的处理依赖于线程的初始化状态。这是一种正式写入这些线程的方式，并且以有限状态机的模式实现，但是这不是唯一的方案；状态机可以在应用程序中隐式实现。这种方法咋任何给定的情况下，都更加依赖于特定情形下明确的行为要求和编程团队的专业知识。无论你选择用什么方式去实现每个线程，任务都会分成独立的处理部分，这样会消除潜在的混乱(数据共享并发)，这样就让编程变的更加简单，且拥有低错误率。

真正通讯顺序处理是没有共享数据的，所有消息都是通过消息队列传递，但是因为`C++`线程共享一块地址空间，所以达不到真正通讯顺序处理的要求。这里就需要有一些约定了：作为一款应用或者是一个库的作者，我们有责任确保在我们的实现中，线程不存在共享数据。当然，为了线程间的通信，消息队列是必须要共享的，具体的细节可以包含在库中。

试想，有一天你要为实现ATM(自动取款机)写一段代码。这段代码需要处理，人们尝试取钱时和银行之间的交互情况，以及控制物理器械接受用户的卡片，显示适当的信息，处理按钮事件，吐出现金，还有退还用户的卡。

一种处理所有事情的方法是让代码将所有事情分配到三个独立线程上去：一个线程去处理物理机械，一个去处理ATM机的逻辑，还有一个用来与银行通讯。这些线程可以通过信息进行纯粹的通讯，而非共享任何数据。比如，当有人在ATM机上插入了卡片或者按下按钮，处理物理机械的线程将会发送一条信息到逻辑线程上，并且逻辑线程将会发送一条消息到机械线程，告诉机械线程可以分配多少钱，等等。

一种为ATM机逻辑建模的方式是将其当做一个状态机。线程的每一个状态都会等待一条可接受的信息，这条信息包含需要处理的内容。这可能会让线程过渡到一个新的状态，并且循环继续。在图4.3中将展示，有状态参与的一个简单是实现。在这个简化实现中，系统在等待一张卡插入。当有卡插入时，系统将会等待用户输入它的PIN(类似身份码的东西)，每次输入一个数字。用户可以将最后输入的数字删除。当数字输入完成，PIN就需要验证。当验证有问题，你的程序就需要终止，就需要为用户退出卡，并且继续等待其他人将卡插入到机器中。当PIN验证通过，你的程序要等待用户取消交易或选择取款。当用户选择取消交易，你的程序就可以结束，并返还卡片。当用户选择取出一定量的现金，你的程序就要在吐出现金和返还卡片前等待银行方面的确认，或显示“余额不足”的信息，并返还卡片。很明显，一个真正的ATM机要考虑的东西更多、更复杂，但是我们来说这样描述已经足够了。

![](../../images/chapter4/4-3.png)

图4.3 一台ATM机的状态机模型(简化)

我们已经为你的ATM机逻辑设计了一个状态机，你可以使用一个类实现它，这个类中有一个成员函数可以代表每一个状态。每一个成员函数可以等待从指定集合中传入的信息，以及当他们到达时进行处理，这就有可能触发原始状态向另一个状态的转化。每种不同的信息类型由一个独立的struct表示。清单4.15展示了ATM逻辑部分的简单实现(在以上描述的系统中，有主循环和对第一状态的实现)，并且一直在等待卡片插入。

如你所见，所有信息传递所需的的同步，完全包含在“信息传递”库中(基本实现在附录C中，是清单4.15代码的完整版)

清单4.15 ATM逻辑类的简单实现

```
struct card_inserted
{
  std::string account;
};

class atm
{
  messaging::receiver incoming;
  messaging::sender bank;
  messaging::sender interface_hardware;
  void (atm::*state)();

  std::string account;
  std::string pin;

  void waiting_for_card()  // 1
  {
    interface_hardware.send(display_enter_card());  // 2
    incoming.wait().  // 3
      handle<card_inserted>(
      [&](card_inserted const& msg)  // 4
      {
       account=msg.account;
       pin="";
       interface_hardware.send(display_enter_pin());
       state=&atm::getting_pin;
      }
    );
  }
  void getting_pin();
public:
  void run()  // 5
  {
    state=&atm::waiting_for_card;  // 6
    try
    {
      for(;;)
      {
        (this->*state)();  // 7
      }
    }
    catch(messaging::close_queue const&)
    {
    }
  }
};
```

如之前提到的，这个实现对于实际ATM机的逻辑来说是非常简单的，但是他能让你感受到信息传递编程的方式。这里无需考虑同步和并发问题，只需要考虑什么时候接收信息和发送信息即可。为ATM逻辑所设的状态机运行在独立的线程上，与系统的其他部分一起，比如与银行通讯的接口，以及运行在独立线程上的终端接口。这种程序设计的方式被称为*参与者模式*([Actor model](http://zh.wikipedia.org/wiki/%E5%8F%83%E8%88%87%E8%80%85%E6%A8%A1%E5%BC%8F))——在系统中有很多独立的(运行在一个独立的线程上)参与者，这些参与者会互相发送信息，去执行手头上的任务，并且它们不会共享状态，除非是通过信息直接传入的。

运行从run()成员函数开始⑤，其将会初始化waiting_for_card⑥的状态，然后反复执行当前状态的成员函数(无论这个状态时怎么样的)⑦。状态函数是简易atm类的成员函数。wait_for_card函数①依旧很简单：它发送一条信息到接口，让终端显示“等待卡片”的信息②，之后就等待传入一条消息进行处理③。这里处理的消息类型只能是card_inserted类的，这里使用一个lambda函数④对其进行处理。当然，你可以传递任何函数或函数对象，去处理函数，但对于一个简单的例子来说，使用lambda表达式是最简单的方式。注意handle()函数调用是连接到wait()函数上的；当收到的信息类型与处理类型不匹配，收到的信息会被丢弃，并且线程继续等待，直到接收到一条类型匹配的消息。

lambda函数自身，只是将用户的账号信息缓存到一个成员变量中去，并且清除PIN信息，再发送一条消息到硬件接口，让显示界面提示用户输入PIN，然后将线程状态改为“获取PIN”。当消息处理程序结束，状态函数就会返回，然后主循环会调用新的状态函数⑦。

如图4.3，getting_pin状态函数会负载一些，因为其要处理三个不同的信息类型。具体代码展示如下：

清单4.16 简单ATM实现中的getting_pin状态函数

```
void atm::getting_pin()
{
  incoming.wait()
    .handle<digit_pressed>(  // 1
      [&](digit_pressed const& msg)
      {
        unsigned const pin_length=4;
        pin+=msg.digit;
        if(pin.length()==pin_length)
        {
          bank.send(verify_pin(account,pin,incoming));
          state=&atm::verifying_pin;
        }
      }
      )
    .handle<clear_last_pressed>(  // 2
      [&](clear_last_pressed const& msg)
      {
        if(!pin.empty())
        {
          pin.resize(pin.length()-1);
        }
      }
      )
    .handle<cancel_pressed>(  // 3
      [&](cancel_pressed const& msg)
      {
        state=&atm::done_processing;
      }
      );
}
```

这次需要处理三种消息类型，所以wait()函数后面接了三个handle()函数调用①②③。每个handle()都有对应的消息类型作为模板参数，并且将消息传入一个lambda函数中(其获取消息类型作为一个参数)。因为这里的调用都被连接在了一起，wait()的实现知道它是等待一条digit_pressed消息，或是一条clear_last_pressed肖息，亦或是一条cancel_pressed消息，其他的消息类型将会被丢弃。

这次当你获取一条消息时，无需再去改变状态。比如，当你获取一条digit_pressed消息时，你仅需要将其添加到pin中，除非那些数字是最终的输入。(清单4.15中)主循环⑦将会再次调用getting_pin()去等待下一个数字(或清除数字，或取消交易)。

这里对应的动作如图4.3所示。每个状态盒的实现都由一个不同的成员函数构成，等待相关信息并适当的更新状态。

如你所见，在一个并发系统中这种编程方式可以极大的简化任务的设计，因为每一个线程都完全被独立对待。因此，在使用多线程去分离关注点时，需要你明确如何分配线程之间的任务。

---------

[2] 详见 http://www.haskell.org/.

[3] 《通信顺序进程》(*Communicating Sequential Processes*), C.A.R. Hoare, Prentice Hall, 1985. 免费在线阅读地址 http://www.usingcsp.com/cspbook.pdf.

# 4.5 本章总结

同步操作对于使用并发编写一款多线程应用来说，是很重要的一部分：如果没有同步，线程基本上就是独立的，也可写成单独的应用，因其任务之间的相关性，它们可作为一个群体直接执行。本章，我们讨论了各式各样的同步操作，从基本的条件变量，到“期望”、“承诺”，再到打包任务。我们也讨论了替代同步的解决方案：函数化模式编程，完全独立执行的函数，不会受到外部环境的影响；还有，消息传递模式，以消息子系统为中介，向线程异步的发送消息。

我们已经讨论了很多C++中的高层工具，现在我们来看一下底层工具是如何让一切都工作的：C++内存模型和原子操作。 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDk0ODUzODA1XX0=
-->