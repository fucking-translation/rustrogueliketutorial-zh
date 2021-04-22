# 第一节：Hello Rust

---

***关于这个教程***

*此教程是免费且开源的，所有代码均使用MIT许可证 - 所以你可以随意使用它。我希望你可以享受这个学习的过程，并制作出很棒的游戏。!*

*如果你喜欢这个教程并希望我继续写作，请考虑支持 [my Patreon](https://www.patreon.com/blackfuture).*

---

这个系列教程主要是教授如果制作roguelikes游戏(以及其他游戏的扩展), 它也会帮助你学会如何使用Rust和RLTK - 我们将使用*Roguelike Tool Kit* 来提供输入和输出。即使你不想使用Rust，我也希望您能够从这个教程中的结构，想法，和通用游戏开发建议中收益。

## 为什么选择Rust？

Rust在2010年诞生，但是直到最近才达到稳定的状态 - 也就是说，当语言现在发生改动时，你写的代码可能就不能正常工作了。Rust的开发工作仍在继续，它的全新章节（如异步系统）正在趋于稳定。 此教程将会远离Rust开发的边缘，使用稳定的特性。

Rust被设计为“更好的系统级语言” - 也就是说，它和`C++`一样是底层语言，但是却很少会像C++一样搬起石头砸自己的脚(shoot yourself in the foot)，它关注于避免很多使C++开发变得困难的陷进，尤其是它投入了大量的精力在内存和线程安全上：它被设计成很难编写出破坏其内存安全或造成条件竞争的程序(这不是不可能，但你必须尝试!)。它迅速吸引了人们的注意，从Mozilla到Microsoft的每个人都对它表现出浓厚的兴趣 - 并用它编写了越来越多的工具。

Rust也被设计成比C++拥有更好的生态系统。`Cargo`提供了一个完整的包管理器（就像是C++中的`vcpkg`，`conan`，但是cargo却更容易集成），一个完整的构建系统（和`cmake`，`make`，`meson`类似，但是更具标准化）。它还不能像C/C++一样运行在大多数平台上，但是这个清单在不断的增长。

我在朋友的督促下尝试了Rust，发现它虽然不能代替我日常工具箱中的C++ - 但是有时候它确实可以使一个项目脱引而出。它的语法需要一段时间来适应，但是它确实可以很好的融入现有的基础架构中。

## 学习Rust

如果你使用过其他的编程语言，那么这里有很多有用的帮助！

* [The Rust Programming Language Book](https://doc.rust-lang.org/book/)对这门语言提供了一个卓越的，自上而下的介绍。
* [Learn Rust by Example](https://doc.rust-lang.org/rust-by-example/)更接近我首选的学习方式(我已经学习过很多门语言)，它为你可能会遇到的大多数场景提供了通用的示例。
* [24 Days of Rust](https://zsiciarz.github.io/24daysofrust/index.html) 提供了一些针对网络的24天学习Rust的课程。
* [Rust's Ownership Model for JavaScript Developers](https://blog.thoughtram.io/rust/2015/05/11/rusts-ownership-model-for-javascript-developers.html)对来自JS或者其他高级语言的同学或许有用。

如果你在这里没有找到你想要的东西，可能其他人已经写了一个`crate`(类似于其他语言的package，但是cargo将其命名为crate)以供使用。一旦你有了一个开发环境，你可以执行`cargo search <my term>`来寻找有用的类库。你也可以直接访问[crates.io](https://crates.io/)查看Cargo中提供的crate的完整列表 - 包含完整的文档和示例。

如果你是一个完全的编程新手，这里有一个坏消息：Rust是一个相对年轻的语言，所以没有很多“从头开始学Rust编程”的资料。你可能发现从一个高级语言开始会更容易，然后再“向下”(更接近底层)过渡到Rust。但是，如果你决定尝试以下，上面的资料应该可以帮助你入门。

## 获取Rust

在你大多数平台，[rustup](https://rustup.rs/)已经足够让你获取一个Rust工具链。在Windows中，它很容易下载 - 当安装过程结束后你就可以获取可用的Rust环境。在Unix衍生的系统中（类如Linux，OSX），它提供了一个可以安装环境的命令行指令。

一旦安装完成，在命令行中输入`cargo --version`检查其是否生效。你应该可以看见诸如`cargo 1.36.0 (c4fcfb725 2019-05-15)` (版本可能会随着时间而改变)的文本。

## 搭建舒适的开发环境

你想要为你的开发工作创建一个目录(个人使用`users/herbert/dev/rust` - 但是这是个人的选择。它其实可以是任何你喜欢的目录)。你也需要一个文本编辑器。我是[Visual Studio Code](https://code.visualstudio.com/)的脑残粉，你可以选择任何更习惯的编辑器。如果你选择Visual Studio Code，我推荐安装下面的扩展：

* `Better TOML` : 更好的读取toml文件。Rust会经常用到。
* `C/C++` : 使用C++的调试系统来调试Rust代码。
* `Rust (rls)` : 不是最快的 ，但是可以提供语法高亮和错误检查。

一旦你配置好了环境，打开编辑器并导航到你指定的文件夹(在VS Code中，使用`File -> Open Folder`并选择文件夹)。

## 创建一个项目

既然你已经在选定的文件夹中，你想要在这里打开一个终端窗口。在VS Code中，使用`Terminal -> New Terminal`。否则，通常是打开一个命令行并使用`cd`命令来到指定的文件夹中。

Rust有一个内置的软件包管理器，叫`cargo`。Cargo可以为你创建项目模版! 所以为了创建你的新项目，执行`cargo init hellorust`过了一会，一个新的文件呢目录就出现在你的项目中 - 名为`hellorust`。
它将会包含下面的文件和目录：

```
src\main.rs
Cargo.toml
.gitignore
```

它们是：

* 如果你在使用git，那么`.gitignore`会很方便 - 它可以防止你意外的将不需要的文件放入git仓库中。如果你没有使用git，可以忽略这个。
* `src\main.rs` 是一个简单的Rust的“hello world”程序源码。
* `Cargo.toml` 定义了你的项目以及你应该如何构建它。

### Rust快速入门 - 解刨(Anatomy)Hello World

自动生成的`main.rs`文件开起来像这样：

```rust
fn main() {
    println!("Hello, world!");
}
```

如果你使用过其他的编程语言，这看起来可能有些熟悉 - 但是语法/关键字可能不同。*Rust*开始时是介于[ML](https://en.wikipedia.org/wiki/ML_(programming_language))和C之间的混搭，旨在创造一种灵活的“系统级”语言(意味着：你可以为CPU编写裸机代码，而不需要像Java或C#一样借助虚拟机)。在此过程中，它继承了两种语言的许多语法。我在使用它的第一周时发现语法看起来很“糟糕”，但在那之后再用起来就觉得很自然了。就像人类语言一样，大脑需要一段时间来记住它的语法和设计。

那么，这些都意味着什么呢？

1. `fn`是Rust中*函数*的关键字。在JavaScript或Java中，这可能会读作`function main()`。在C中，这可能会读作`void main()`(虽然`main`在C中旨在返回一个`int`)。在C#中，它可能会是`static void Main(...)`。
2. `main`是函数的*名称*。在这里，这个名称是一种特殊的情况：当操作系统加载一个程序到内存中时，它需要知道它首先应该运行什么 - Rust做了一些额外的工作来标志`main`是第一个需要运行的函数。如果你想要你的程序可以运行起来，通常需要一个`main`函数，除非你开发的是一个*类库*(为其他程序提供的函数集合)。
3. `()`中包含函数的参数。在这里，没有任何参数 - 所以我们使用空的括号即可。
4. `{`表明一个代码块的开始。在这里，代码块是函数的主体。所有在`{`和`}`之间的东西都是函数的内容：其中指令依次执行。代码块(denote)也意味着*作用域* - 因此，你在函数内定义的所有内容均具有该函数的访问权限。也就是说，你在`cheese`函数内部定义了一个变量 - 该变量对`mouse`函数不可见(反之亦然)。解决这种问题有很多办法，我们将在构建游戏时对其进行介绍。
5. `println!`是一个*宏*。你可以判断Rust宏因为它们名称后面都有一个`!`。你可以在[rust程序设计：宏](https://kaisery.github.io/trpl-zh-cn/ch19-06-macros.html)学到关于宏的内容。而现在，你只需要知道它们是一种特殊的函数，可以在编译期间解析成其他的代码。打印内容输出到屏幕可以变得十分复杂 - 你可能想要表达更多内容而不仅仅是“hello world” - `println!`宏涵盖了大量的格式化的用例。(如果你对C++很熟悉，它相当于`std::fmt`。由于程序员往往不得不输出大量文本，大部分语言都有它们自己的字符串格式化系统!)
6. 最后`}`关闭在`4`上开始的代码块。

继续并输入`cargo run`。在一番编译之后，如果一切正常，你将会在终端上看到“hello world”。

### 有用的`cargo`命令

Cargo是一个相当有用的工具！你可以在[rust程序设计](https://kaisery.github.io/trpl-zh-cn/ch01-03-hello-cargo.html)中学到一些关于它的内容，如果你很感兴趣的话，也可以在[Cargo指南](https://doc.rust-lang.org/cargo/index.html)中学到关于它的所有内容。

当你在用Rust工作时，你会多次与`cargo`进行交互。如果你使用`cargo init`初始化你的项目，你的项目将会是一个cargo *crate*。编译，测试，执行，更新 - Cargo都可以帮助你做到。它甚至可以默认的为你设置`git`。

你可以在`cargo`中发现下面这些比较方便的特性：

* `cargo init` 创建一个新的项目。creates a new project. 那就是你用来创建`hello world`项目的命令。如果你真的想使用`git`，你可以输入`cargo init --vcs none (projectname)`。
* `cargo build` 下载项目中所有的依赖并编译他们，然后再编译你的程序。它不会真的运行你的程序 - 但是这是一种可以快速发现程序中编译错误的方式。
* `cargo update` 将会拉下在`cargo.toml`文件中*crate*清单的最新版本。
* `cargo clean` 可用于删除项目中所有的中间工作文件，从而释放大量磁盘空间。下次你运行/构建项目时，它们会自动下载并重新编译。有时，当项目无法正常工作时，执行`cargo clean`可能会有用 - 尤其是IDE集成开发环境。
* `cargo verify-project` 将会告诉你Cargo的配置是否正确。
* `cargo install` 被用来通过Cargo来安装程序。这对安装一些需要的工具来说十分有用。

Cargo也支持扩展 - 也就是说，插件可以让它做更多的事情。这里有一些插件你可能会觉得很有用：

* Cargo可以格式化你的所有代码，让它看起来像Rust手册中的标准Rust代码一样。你需要输入一次`rustup component add rustfmt`来安装这个工具。 当安装结束之后，你在任何时候都可以输入`cargo fmt`来格式化你的代码。
* 如果你想要使用`mdbook`的格式 - 正如[this book](https://github.com/thebracket/rustrogueliketutorial)所用的一样! - cargo同样可以帮你。你需要执行`cargo install mdbook`来添加该工具到你的系统环境中。然后使用`mdbook build`将会构建一个文档项目，`mdbook init`将会创建一个新的文档项目，`mdbook serve`将会提供一个本地的web服务来展示你的工作! 你可以从[mdbook](https://rust-lang-nursery.github.io/mdBook/cli/index.html)学习相关的内容。
* Cargo也可以集成一个代码校验工具 - 叫做`Clippy`。Clippy有点学术性(就像微软的office中和它同名的工具一样!). 仅需执行`rustup component add clippy`一次，你就可以在任何时候输入`cargo clippy`来查看有关你的代码中可能有什么问题的建议!


### 创建一个新项目

让我们修改新创建的项目以使用[RLTK](https://github.com/thebracket/bracket-lib) - Roguelike工具集.

## 设置Cargo.toml文件

自动生成的Cargo文件开起来像这样：

```toml
[package]
name = "helloworld"
version = "0.1.0"
authors = ["Your name if it knows it"]
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

继续并确定你的名字是正确的！下一步，我们要让Cargo使用RLTK - Roguelike工具集类库。Rust让这个变得非常容易。调整`dependencies`模块，看起来像这样：

```toml
[dependencies]
rltk = { version = "0.8.0" }
```

我们告诉它包的名称叫做`rltk`，可以在Cargo中获取到 - 所以我们只需要提供一个版本号。你可以在任何时候执行`cargo search rltk`来查看最新的版本，或者访问[crate网站](https://crates.io/crates/rltk)。

偶尔运行`cargo update`命令是个好主意 - 它将会更新你的项目中使用的类库。

## Hello Rust - RLTK风格!

继续并替换`src\main.rs`中的内容：

```rust
use rltk::{Rltk, GameState};

struct State {}
impl GameState for State {
    fn tick(&mut self, ctx : &mut Rltk) {
        ctx.cls();
        ctx.print(1, 1, "Hello Rust World");
    }
}

fn main() -> rltk::BError {
    use rltk::RltkBuilder;
    let context = RltkBuilder::simple80x50()
        .with_title("Roguelike Tutorial")
        .build()?;
    let gs = State{ };
    rltk::main_loop(context, gs)
}
```

现在创建一个新的`resources`文件夹，RLTK运行需要一些文件，我们将这些文件放在这个文件夹中。下载[resources.zip](./resources.zip)，并将其解压在该文件夹中。请注意解压后的文件路径是`resources/backing.fs` (等)而不是`resources/resources/backing.fs`。

保存，然后回到终端。输入`cargo run`，你会在终端窗口看到`Hello Rust`字样。

![Screenshot](./img/c1-s1.png)

如果你是Rust新手，你可能会疑惑`Hello Rust`代码干了些什么，和这里为什么要这么写 - 所以我们花一点时间浏览一下代码。

1. 第一行相当于C++中的`#include`或C#中的`using`。它告诉编译器我们需要命名空间`rltk`中的`Rltk`和`GameState`类型。过去这里你需要添加额外的`extern crate`行，但是最新的Rust版本已经可以理解而不需要你手动添加了。
2. 使用`struct State{}`，我们创建了一个新的`结构体`。结构体就像是Pascal中的Record，或者其他语言中的Class：你可以在其中存储一堆数据，你也可以在其中添加一些方法。在这里，我们不需要任何数据 - 我们只需要一个可以附加代码的地方。如果你想要学习更多关于结构体的知识，你可以访问[Rust程序设计：结构体](https://kaisery.github.io/trpl-zh-cn/ch05-00-structs.html)
3. `impl GameState for State`有点拗口！我们告诉Rust我们的`State`结构体需要*实现*`GameState`接口。Trait和其他语言中的接口或基类很像：他们设置了一个结构让你可以实现自己的代码，然后可以与他们提供的库进行交互 - 如果没有trait，类库需要知道关于你代码中的所有信息。在这里，`GameState`是一个RLTK提供的trait。RLTK需要你满足一点 - 它在它的每一帧上调用你的程序。你可以在[rust程序设计：trait](https://kaisery.github.io/trpl-zh-cn/ch10-02-traits.html)学习trait相关的内容。
4. `fn tick(&mut self, ctx : &mut Rltk)`是一个函数定义。这里是实现GameState接口的地方，所以我们需要为这个trait实现这个函数 - 它需要匹配trait提供的类型。函数是构建Rust代码块的基础，我推荐[rust程序设计：函数是如何工作的](https://kaisery.github.io/trpl-zh-cn//ch03-03-how-functions-work.html). 
    1. 在该例中，`fn tick`意思是“创建一个函数，将其命名为tick”(它被称为“tick”是因为它在渲染的每一帧上都要“滴答”一下，在游戏编程中，通常将每次迭代称为tick)。 
    2. 它并没有以`-> type`作为结束，所以它相当于C语言中的`void`函数 - 它一旦被调用不会返回任何值。 该函数的参数也可以从一些解释中收益。
    3. `&mut self`意味着“该函数需要访问结构体State，并且可能会修改他” (`mut`是“mutable”的简写 - 表示它可以修改结构体中的变量 - “state”)。 结构体中的函数也可以只包含`&self` - 意思是，我们可以看到结构体的内容，但是不会修改它。如果你忽略了`&self`，函数将无法查看结构体中的内容 - 但是可以被调用，仿佛结构体就是一个命名空间一样。(你可以经常看到这种名为`new`的函数 - 它们为你提供了该结构体的副本 - `State::new()`).
    4. `ctx: &mut Rltk`意思是“传入了一个名为`ctx`的变量" (`ctx`是“context”的缩写). 冒号表示我们为这个变量指定了类型。
    5. `&`意思是“传入了一个引用” - 这是指向变量现有副本的指针。该变量没有被复制，你正在处理传入的版本；如果你做了修改，你也会修改原件。[rust程序设计：引用和借用](https://kaisery.github.io/trpl-zh-cn/ch04-02-references-and-borrowing.html)解释的比我要好。
    6. `mut`再一次表示它是一个可变引用：你被允许去修改上下文。
    7. 最后`Rltk`是你将接收到的变量类型。在此例中，它是`RLTK`库中定义的结构体，它提供了你可以在屏幕上执行的各种操作。
5. `ctx.cls();`是指“调用变量`ctx`提供的`cls`函数“。`cls`是“clear the screen”的缩写 - 我们告诉上下文应该清空虚拟终端。在某一帧的最开始执行这个方法是个明智的选择，除非你确实不想这么做。
6. `ctx.print(1, 1, "Hello Rust World");`是要求上下文在位置(1,1)处打印“Hello Rust World”。
7. 现在轮到`fn main()`了。*每个*可运行的程序都有一个`main`函数：它告诉操作系统如何启动程序。
8. ```
    use rltk::RltkBuilder;
    let context = RltkBuilder::simple80x50()
        .with_title("Roguelike Tutorial")
        .build()?;
   ```
   是在结构体内调用一个函数的例子 - 该函数并没有携带“self”。在其他语言中，这可能会被称为构造函数。我们将该函数称为`simple80x50` (它是RLTK提供的结构体，可以使终端有80个字节的宽度，50个字节的高度，窗口的标题为“Roguelike Tutorial”。
9. `let gs = State{ };`是一个变量分配的例子(请查看[rust程序设计：变量和可变性](https://kaisery.github.io/trpl-zh-cn/ch03-01-variables-and-mutability.html))。我们创建了一个新的变量叫做`gs`(“game state”的简称)，并将其赋值为上面定义的`State`结构体的副本。
10. `rltk::main_loop(context, gs)`在命令空间`rltk`内被调用，激活了一个叫做`main_loop`的函数。它需要我们之前创建好的`context`和`GameState` - 所以我们将其传过去即可。RLTK尝试去除一些运行GUI/游戏应用程序的某些复杂性，并提供一些包装。现在该函数将接管并控制程序，并在每次程序“滴答”时调用`tick`函数 - 也就是说，一个周期结束将会进入下一个周期，每秒钟可以发生60甚至更多次。

希望这些解释将起到一些用处！

## 跟着教程一起玩游戏

你可能喜欢直接运行教程的代码，而不是一个一个的敲代码！好消息是，代码托管在Github上可以供你细读(perusal)。你需要安装`git`(rustup可以帮到你)。选择一个你将要存放教程代码的目录，并打开终端：

```bash
cd <path to tutorials>
git clone https://github.com/thebracket/rustrogueliketutorial .
```

过了一会儿，它将下载完整的教程代码(包括这本书的源码)。像下面这样展示(并不完全)：

```
───book
├───chapter-01-hellorust
├───chapter-02-helloecs
├───chapter-03-walkmap
├───chapter-04-newmap
├───chapter-05-fov
├───resources
├───src
```

这是哪儿?

* `book`目录存放这个教程的内容。你可以将它忽略，直到你想要检查我的拼写是否有问题！
* 每一章节的代码示例都在`chapter-xy-name`文件夹下；举个例子，`chapter-01-hellorust`。
* `src`文件夹内包含了一些简单的脚本，以便在运行之前提醒你对章节的目录进行修改。
* `resources`有一些你为这个示例下载的ZIP文件。每个章节的目录都已预先配置使用此文件夹。
* `Cargo.toml`设置包含了所有教程的“工作空间” - 他们共享依赖，所以它不会占用你的整个驱动，不会在你每次使用时重新下载所有的东西。

运行这个示例，打开你的终端：

```bash
cd <where you put the tutorials>
cd chapter-01-hellorust
cargo run
```

如果你使用了*Visual Studio Code*，你可以使用*File -> Open Folder*打开你已检查过的整个目录。使用终端内置的命令，你可以`cd`到每个教程中并执行`cargo run`。
## 获取教程源码

你可以在[https://github.com/thebracket/rustrogueliketutorial](https://github.com/thebracket/rustrogueliketutorial)获取所有教程的源码。

## 更新教程

我经常更新这个教程 - 添加新的章节，修复问题等。你将会定期的打开教程的目录，并输入`git pull`。它告诉`git`(源码版本控制)去github仓库中查看是否有新更新的内容。它将会下载所有变更过的内容，然后你就再一次拥有了最新的教程。

## 更新你的项目

你可能会发现`rltk_rs`或其他的包被修改过，然后你想要最新的版本。在项目目录下，你可以输入`cargo update`来更新每一个依赖。你也可以输入`cargo update --dryrun`来查看哪些需要更新，这不会改变任何东西(人们经常会更新他们的crate，所以这是一个很大的清单)。

## 更新Rust

你可以运行`rustup self update`这条命令。我不太建议在`Visual Studio Code`或其他IDE中执行这个命令，但是如果你想要这样做请确保你已经有了最新发布的`Rust`版本(和相关的工具)。这将更新Rust的更新工具(我知道这听起来很递归)。然后你可以输入`rustup update`并下载所有工具的最新版本。

## 获取帮助

这里有很多方式来获取帮助：

* 如果你有任何的问题，提升的建议或者你想要我加的任何东西，可以随时联系我(推特名是`@herberticus`)。
* [/r/rust](https://www.reddit.com/r/rust/)上的好人对解决Rust问题很有帮助。
* [/r/roguelikedev](https://www.reddit.com/r/roguelikedev/)上的杰出人才对Roguelike的问题十分有帮助。他们的Disord也非常活跃。

[在浏览器中使用web assembly运行这些章节的代码(需要WebGL2)](https://bfnightly.bracketproductions.com/rustbook/wasm/chapter-01-hellorust/)

---

Copyright (C) 2019, Herbert Wolverson.

---
