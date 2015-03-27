PHP基础
=======================

传送门
-----------------------
[基础知识]

-[类型][类型]
-[变量]
-[常量]
-[表达式]
-[运算符]
-[流程控制]

[类型]:http://php.net/manual/zh/language.types.php
[变量]:http://php.net/manual/zh/language.variables.php
[常量]:http://php.net/manual/zh/language.constants.php
[表达式]:http://php.net/manual/zh/language.expressions.php
[运算符]:http://php.net/manual/zh/language.operators.php
[流程控制]:http://php.net/manual/zh/language.control-structures.php

编程范式
-----------------------

PHP是一个灵活的动态语言，支持多种编程范式。这些年来一直在不断的进化，重要的里程碑包括PHP 5.0 (2004)增加完善的
面向对象模型、PHP 5.3 (2009)增加匿名函数和命名空间和PHP 5.4 (2012)增加traits. 

### 面向对象编程
PHP具有完整的面向对象编程特性，如类、抽象类、接口、继承、构造函数、克隆和异常等。

* [学习PHP面向对象编程][oop]

### 函数式编程

PHP支持第一类函数(first-class function)，即函数可以赋值给变量，包括用户自定义的函数和内置函数，然后动态调用它。
函数可以作为参数传递给其他函数(即高阶函数)，也可以作为函数返回值返回。

PHP支持函数递归调用，即函数自己调用自己，不过在实际的PHP代码中，我们更喜欢用迭代来代替递归。

* [学习动态调用函数`call_user_func_array`][call-user-func-array]

### 元编程

PHP通过反射API和魔术方法机制，支持多种方式的元编程。开发者通过魔术方法，如`__get()`, `__set()`, `__clone()`, `__toString()`,
`__invoke()`等，可以改变类的行为。Ruby开发者经常说PHP没有`method_missing`方法，实际上通过`__call()`和`__callStatic()`就可以
完成同样的功能。

* [学习魔术方法][magic-methods]
* [学习反射][reflection]

[oop]: http://www.php.net/manual/en/language.oop5.php
[magic-methods]: http://php.net/manual/en/language.oop5.magic.php
[reflection]: http://www.php.net/manual/en/intro.reflection.php
[call-user-func-array]: http://php.net/manual/en/function.call-user-func-array.php


命名空间
----------------------

如前所述，PHP社区的众多开发者已经开发了大量的代码。这意味着一个函数库中的PHP代码可能使用了另外一个库中相同的类名，如果它们共享一个命名空间，则会产生冲突导致异常。

_命名空间_解决了这个问题。如PHP手册里描述的那样，命名空间类似于操作系统中的目录，两个同名文件可以共存于不同的目录。同理，同名的PHP类可以在不同的PHP命名空间下共存，就这么简单。

因而把代码放在自己的命名空间下就显得非常必要，这样其他人就可以放心的使用这些代码，而无需担心与其他函数库的命名冲突。

[PSR-0][psr0] 里提供了命名空间的推荐使用方式, 它试图提供一个标准的文件、类和命名空间的使用惯例，从而让代码做到即插即用。

2013年12月，PHP-FIG发布了新的自动加载标准：[PSR-4][psr4]，将来可能会替换旧的PSR-0标准。PSR-4要求PHP5.3版本以上，而目前很多项目用的都是PHP5.2，
因此当前两个标准都可用，但是对于新应用或者包的话，应优先考虑PSR-4.

* [了解更多命名空间][namespaces]

[namespaces]: http://php.net/manual/en/language.namespaces.php
[psr0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[psr4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md

标准PHP库
-------------------

标准PHP库(SPL)和PHP一起发布，提供了一组类和接口，包括了常用的数据结构如栈，队列和堆等，以及遍历这些数据结构的迭代器，
或者你还可以自己实现SPL接口。

* [学习更多SPL][spl]

[spl]: http://php.net/manual/en/book.spl.php 



命令行接口
------------------

PHP的主要目的是开发Web应用，不过它的命令行脚本接口(CLI)也非常有用。PHP命令行编程可以帮你完成自动化的任务，如测试，部署和
应用管理。

CLI PHP编程非常强大，可以直接调用你自己的app代码而无需创建Web图像界面，需要注意的是不要把CLI PHP脚本放在公开的web目录下！

在命令行下运行PHP:

{% highlight bash %}
> php -i
{% endhighlight %}

选项`-i`将会打印PHP配置，类似于[`phpinfo`][phpinfo]函数。 

选项`-a`提供交互式shell，和ruby的IRB或python的交互式shell相似，此外还有很多其他有用的[命令行选项][cli-options]。

接下来写一个简单的"Hello, $name" CLI程序，先创建名为`hello.php`的脚本：

{% highlight php %}
<?php
if($argc != 2) {
    echo "Usage: php hello.php [name].\n";
    exit(1);
}
$name = $argv[1];
echo "Hello, $name\n";
{% endhighlight %}

PHP会在脚本运行时根据参数创建两个特殊的变量，[`$argc`][argc]是一个整数，表示参数*个数*，[`$argv`][argv]是一个数组变量，包含每个参数的*值*，
它的第一个元素一直是PHP脚本的名字，如本例中为`hello.php`。

命令运行失败时，可以通过`exit()`表达式返回一个非0整数来通知shell，常用的exit返回码可以查看[列表][exit-codes]

运行上面的脚本，在命令行输入：

{% highlight bash %}
> php hello.php
Usage: php hello.php [name]
> php hello.php world
Hello, world
{% endhighlight %}


 * [学习如何在命令行运行PHP][php-cli]
 * [学习如何在Windows环境下运行PHP命令行程序][php-cli-windows]

[phpinfo]: http://php.net/manual/en/function.phpinfo.php
[cli-options]: http://www.php.net/manual/en/features.commandline.options.php
[argc]: http://php.net/manual/en/reserved.variables.argc.php
[argv]: http://php.net/manual/en/reserved.variables.argv.php
[php-cli]: http://php.net/manual/en/features.commandline.php
[php-cli-windows]: http://www.php.net/manual/en/install.windows.commandline.php
[exit-codes]: http://www.gsp.com/cgi-bin/man.cgi?section=3&topic=sysexits



XDebug
-----------------

调试器是软件开发过程中非常重要的一个工具，通过它，可以跟踪代码的执行过程，查看堆栈信息。XDebug是一个PHP调试器，可以集成在常见的IDE中，提供设置断点、
查看堆栈信息等功能，还可以和PHPUnit、KCacheGrind等工具配合，执行代码覆盖率测试和性能调优。

如果你现在还没有使用调试器，仅仅依靠var_dump/print_r调试的话，XDebug就是你的最佳选择。

[安装XDebug][xdebug-install]有点复杂，其中一个重要功能"远程调试"——如果你在本地开发代码，然后在虚拟机或其他主机中测试，那么它对你就非常有用。

通常，你需要修改Apache虚拟主机或者.htaccess配置文件，增加：

    php_value xdebug.remote_host=192.168.?.?
    php_value xdebug.remote_port=9000

"remote host"和"remote port"对应本机IDE的监听地址和端口，然后设置IDE为"等待连接"模式，打开URL:

    http://your-website.example.com/index.php?XDEBUG_SESSION_START=1

这样IDE就会监控脚本的执行，允许用户设置断点和查看内存中的变量值。

图形化调试器使得单步调试、查看变量和现场运行代码变得异常简单，很多IDE都自带或者通过第三方插件支持xdebug的调试，如Mac平台下的开源软件MacGDBp。

 * [了解更多XDebug][xdebug-docs]
 * [了解更多MacGDBp][macgdbp-install]

[xdebug-docs]: http://xdebug.org/docs/
[xdebug-install]: http://xdebug.org/docs/install
[macgdbp-install]: http://www.bluestatic.org/software/macgdbp/
