PHP基础
=======================

传送门
-----------------------
[基础知识]

- [类型][类型]

- [变量][变量]

- [常量][常量]

- [表达式][表达式]

- [运算符][运算符]

- [流程控制][流程控制]

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

异常
----------------------

异常是大部分流行语言的标准特性，但是PHP开发者却不太重视。其他语言如Ruby极度倚赖异常，在任何错误发生的时候，如HTTP请求失败、DB查询错误，甚至图片资源未找到，都会抛出一个异常，以及时提示那里发生了一个错误。

PHP则对此很宽松，如调用`file_get_contents()`失败，只是返回`FALSE`并提示一个warning信息而已。很多老的PHP框架，如CodeIgniter会返回false，然后在自己的日志里记录一个消息，开发者需要使用如`$this->upload->get_error()`的方式来查看发生了什么错误。这么做需要你自己检查是否有错误，并需要根据不同类调用不同的方法来获取错误消息，而不能让错误明显的显示出来。

这种做法的另外一个弊端是当类自动在屏幕打印一个错误，然后退出进程，阻止了其他开发者动态处理该错误的机会。而异常则是让开发者知道发生了错误，并让他们选择如何处理：

```php
<?php
$email = new Fuel\Email;
$email->subject('My Subject');
$email->body('How the heck are you?');
$email->to('guy@example.com', 'Some Guy');

try
{
    $email->send();
}
catch(Fuel\Email\ValidationFailedException $e)
{
    // The validation failed
}
catch(Fuel\Email\SendingFailedException $e)
{
    // The driver could not send the email
}
finally
{
    // Executed regardless of whether an exception has been thrown, and before normal execution resumes
}
```

### SPL异常

默认的异常类Exception包含的上下文信息很少，对于debug不方便，常见的做法是创建更具体的子类：

```php
<?php
class ValidationException extends Exception {}
```

这使得你可以包含多个catch子句来处理不同的异常，但是这又会导致创建_很多的_自定义异常类，可以用SPL中的异常类来缓解这个问题
[SPL扩展][splext]. 

如使用`__call()`魔术方法，对不存在的方法调用抛出一个`throw new BadFunctionCallException;`，既避免了抛出含义模糊的Exception异常，也省去了自定义异常类的麻烦。

* [学习更多Exceptions][exceptions]
* [了解跟多SPL Exceptions][splexe]
* [PHP中的异常嵌套][nesting-exceptions-in-php]
* [PHP 5.3异常最佳实践][exception-best-practices53]

[exceptions]: http://php.net/manual/en/language.exceptions.php
[splexe]: http://php.net/manual/en/spl.exceptions.php
[splext]: /#standard_php_library
[exception-best-practices53]: http://ralphschindler.com/2010/09/15/exception-best-practices-in-php-5-3
[nesting-exceptions-in-php]: http://www.brandonsavage.net/exceptional-php-nesting-exceptions-in-php/

数据库
----------------------

通常PHP代码使用数据库来持久化存储数据，并有多种方式去连接和操作数据库。在_PHP5.1.0_之前，推荐的方式有[mysql][mysql]、[mysqli][mysqli]和[pgsql][pgsql]等。

如果应用只是使用一个数据库的话，原生驱动就工作的非常好，否则使用MySQL的同时，还需要使用MSSQL或Oracle数据库的话，那么就没有办法只使用一个原生驱动了，只能分别学习各个数据库驱动的API，这非常令人生厌。

另外需要注意，mysql这个原生驱动已经不在活跃开发状态了，从PHP5.4.0开始被标记为不推荐使用，意味着将来版本如PHP 5.6可能会移除这个扩展。如果你正在使用`mysql_connect()`和`mysql_query()`，那么将来可能要重写部分代码，所以最好用mysqli或PDO来代替。<b>如果你正在开发新项目，请不要用mysql扩展，尝试用[MySQLi扩展][mysqli]或PDO来替代</b>

* [PHP: 选择MySQL API](http://php.net/manual/en/mysqlinfo.api.choosing.php)

### PDO

PDO是数据库连接抽象库，从PHP5.1.0开始提供，提供多种数据库的统一的操作接口。PDO不会转化你的SQL查询或者模拟缺失特性；它只是提供统一的API去连接不同的数据库而已。

更重要的是，`PDO`允许你绑定SQL查询语句中的变量，而无需担心SQL注入问题，这主要通过PDO statements和变量绑定来实现。

假设PHP脚本接收一个数字ID作为查询参数，从数据库取回一条记录。下面是一种错误的做法：

```php
$pdo = new PDO('sqlite:users.db');
$pdo->query("SELECT name FROM users WHERE id = " . $_GET['id']); // <-- NO!
```

这是非常糟糕的代码，直接在SQL中插入一个原始输入变量，导致潜在的SQL注入风险。假如黑客构造URL：
`http://domain.com/?id=1%3BDELETE+FROM+users`来传入恶意参数id，则`$_GET['id']`的变量值为`1;DELETE FROM users`，这将删除数据表中的所有用户！因此，你应该使用PDO的绑定参数功能来处理ID输入参数。

```php
$pdo = new PDO('sqlite:users.db');
$stmt = $pdo->prepare('SELECT name FROM users WHERE id = :id');
$stmt->bindParam(':id', $_GET['id'], PDO::PARAM_INT); // <-- Automatically sanitized by PDO
$stmt->execute();
```

这才是正确的代码，在PDO statement中绑定一个参数，使得查询被发给数据库之前，对输入参数进行转义，防止SQL注入攻击。

* [学习PDO][1]

另外一个要注意的问题是，如果数据库连接没有隐式地关闭，那么数据库连接数可能会超过数据库服务器的限制而连接失败，这种错误在其他编程语言中比较常见。PDO对象在销毁的时候会隐式的关闭数据库连接，只要你把指向它的引用全部删除即可，如设置为NULL。如果没有，PHP也会在脚本结束时关闭所有非持久化的数据库连接。

* [了解更多PDO连接][5]


[1]: http://www.php.net/manual/en/book.pdo.php
[5]: http://php.net/manual/en/pdo.connections.php

[mysql]: http://php.net/mysql
[mysqli]: http://php.net/mysqli
[pgsql]: http://php.net/pgsql
[psr0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[psr4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md


缓存
-----------------
### 字节码缓存

在一个PHP文件被执行时，它先被编译为字节码(也称opcode)，然后这些字节码被执行。如果文件没有修改，那么字节码也会保持不变，这意味着编译这一步白白浪费了CPU资源。

这就是引入字节码缓存的原因，通过把字节码保存在内存中来消除冗余的编译，重用它们完成后续的调用。配置字节码缓存非常简单，而且可以极大地提高应用的执行效率，没有理由不使用字节码缓存。

PHP 5.5开始内置字节码缓存组件[OPcache](http://php.net/manual/en/book.opcache.php)，老版本的PHP可以使用第三方的字节码缓存组件，流行的字节码缓存方案有：

* [APC](http://php.net/manual/en/book.apc.php) (PHP 5.4 and earlier)

### 对象缓存

很多时候，在代码中缓存对象可以带来很大的收益，例如获取代价很大的数据和查询结果很少变化的数据库调用。我们可以使用对象缓存系统缓存这些数据，大大加快后续的同类访问请求。如果你在取得这些数据之后，把它们缓存在系统中，在后续对这些数据的请求中，就可以直接使用缓存中的对象，这么做可以很大的提示系统性能，减少服务器的负载。

很多流行的字节码缓存方案也允许你缓存自定义数据，因此我们更应该充分利用对象缓存功能。APCu、XCache和WinCache都提供API，让你把数据缓存在他们的内存cache中。

使用最多的内存对象缓存系统是APCu和redis，APCu是很好的一个对象缓存方案，它提供了简单的API来让你把对象存储在内存中，而且配置和使用都非常容易，它的一个缺点是只能在本机使用。REDIS则是另外一种方式，它是一个单独的服务，可以通过网络访问，这意味着可以在一个地方写入数据，然后在不同的系统中访问这份数据。

Note that when running PHP as a (Fast-)CGI application inside your webserver, every PHP process will have its own cache, i.e. APCu data is not shared between your worker processes. In these cases, you might want to consider using redis instead, as it's not tied to the PHP processes.

在单机性能上，APCu通常比redis更高,但是redis提供了更多高级数据结构和更多得功能

Note that prior to PHP 5.5, APC provides both an object cache and a bytecode cache. APCu is a project to bring APC's object cache to PHP 5.5+, since PHP now has a built-in bytecode cache (OPcache).

学习更多对象缓存系统：

* [APCu](https://github.com/krakjoe/apcu)
* [APC Functions](http://php.net/manual/en/ref.apc.php)
* [Memcached](http://memcached.org/)
* [Redis](http://redis.io/)
* [XCache APIs](http://xcache.lighttpd.net/wiki/XcacheApi)
* [WinCache Functions](http://www.php.net/manual/en/ref.wincache.php)


框架
-------------------
我们使用YAF作为基础框架参考YAF手册
http://www.laruence.com/manual/


安全
-------------------
### Web应用安全

总有一些人会千方百计的想着破坏你的Web应用，提前想办法加强自己的Web应用的安全性非常重要。幸运的是，[The Open Web Application Security Project][owasp] (OWASP) 已经提供详尽的已知安全问题列表和防范对策。每个关注Web安全的开发者都应该仔细阅读该列表。

* [阅读OWASP安全指南][owasp_contents]

[owasp]: https://www.owasp.org/
[owasp_contents]: https://www.owasp.org/index.php/Guide_Table_of_Contents

### 数据过滤

永远不要在PHP代码中信任外部输入，在使用之前一定要先过滤和验证，`filter_var`和`filter_input`函数可以帮助过滤文本和验证文本格式(如邮箱地址)。

外部输入可以是：`$_GET`和`$_POST`表单输入数据、`$_SERVER`超级变量中的某些值和通过`fopen('php://input', 'r')`获取的HTTP请求体。要记住外部输入不仅仅是用户提交的表单数据，还包括上传和下载的文件、session变量、cookie数据和第三方Web服务提供的数据等。

当外部数据被存储合并之后，下次读取时，它们仍然算是外部输入，每次在代码中处理的时候，需要问自己是否已经正确过滤，是否可以信任它们。

数据需要根据不同用处，进行不同的_过滤_，如果把未经过滤的数据输出到HTML页面，它可以在你的网站里执行HTML和JavaScript！即通常说的跨站脚本攻击(XSS)。避免XSS的一个策略就是使用`strip_tags`函数过滤外部输入的所有HTML标签，或者使用`htmlentities`/`htmlspecialchars`转义其中的HTML实体。

另外一个例子是传给命令行命令的选项，这可能非常危险(通常不是一个好主意)，不过你可以用内置的`escapeshellarg`函数过滤命令行的参数。

最后一个例子是根据外部输入来从文件系统中加载文件的操作，可以通过修改路径中的文件名实施攻击。你需要过滤输入中的"/", "../",[null bytes][6], 或其他特殊字符，以防止加载不能公开的包含敏感数据的文件。

* [学习更多数据过滤][1]
* [了解更多`filter_var`][4]
* [了解更多`filter_input`][5]
* [学习更多null字节处理][6]

### 数据清洗

数据清洗就是删除或转义外部输入中的非法或不安全字符。比如把外部数据输出到HTML或插入到SQL语句之前，需要先清洗外部输入。当你通过[PDO](#databases)绑定数据时，它会替你转义输入数据。

有时候需要允许外部数据包含安全的HTML标签，并输出到HTML页面中。这个比较难处理，可以考虑使用其他更严格的格式，如或BBCode，实在不能的话，可以使用[HTML Purifier][html-purifier]库来进行数据清洗。

[了解更多数据清洗过滤器][2]

### 数据验证

数据验证外部输入就是你预期的，如你在处理注册表单时，需要验证email地址、电话号码和年龄等数据

[了解更多数据验证过滤器][3]

[1]: http://www.php.net/manual/en/book.filter.php
[2]: http://www.php.net/manual/en/filter.filters.sanitize.php
[3]: http://www.php.net/manual/en/filter.filters.validate.php
[4]: http://php.net/manual/en/function.filter-var.php
[5]: http://www.php.net/manual/en/function.filter-input.php
[6]: http://php.net/manual/en/security.filesystem.nullbytes.php
[html-purifier]: http://htmlpurifier.org/

### 注册全局变量

**提示:**从PHP 5.4.0开始，`register_globals`配置已经删除，不再生效。保留这个配置，只是提示依赖该配置的应用进行升级。

启用`register_globals`配置后，`$_POST`,`$_GET`和`$_REQUEST`中的变量自动注册为全局变量，使得应用很难辨别变量的确切来源，从而产生安全漏洞。

例如：`$_GET['foo']`将注册变量`$foo`，这会覆盖程序中未声明的同名变量。如果你使用PHP5.4.0之前的版本，请确保已经把`register_globals`设置为**off**。

* [Register_globals in the PHP manual](http://www.php.net/manual/en/security.globals.php)

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

```php
> php -i
```

选项`-i`将会打印PHP配置，类似于[`phpinfo`][phpinfo]函数。 

选项`-a`提供交互式shell，和ruby的IRB或python的交互式shell相似，此外还有很多其他有用的[命令行选项][cli-options]。

接下来写一个简单的"Hello, $name" CLI程序，先创建名为`hello.php`的脚本：

```php
<?php
if($argc != 2) {
    echo "Usage: php hello.php [name].\n";
    exit(1);
}
$name = $argv[1];
echo "Hello, $name\n";
```

PHP会在脚本运行时根据参数创建两个特殊的变量，[`$argc`][argc]是一个整数，表示参数*个数*，[`$argv`][argv]是一个数组变量，包含每个参数的*值*，
它的第一个元素一直是PHP脚本的名字，如本例中为`hello.php`。

命令运行失败时，可以通过`exit()`表达式返回一个非0整数来通知shell，常用的exit返回码可以查看[列表][exit-codes]

运行上面的脚本，在命令行输入：

```php
> php hello.php
Usage: php hello.php [name]
> php hello.php world
Hello, world
```


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
