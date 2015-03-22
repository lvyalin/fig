代码风格指南
==================

本手册是基础代码规范([PSR-1][])的继承和扩展。

为了尽可能的提升阅读其他人代码时的效率，下面例举了一系列的通用规则，特别是有关于PHP代码风格的。

各个成员项目间的共性组成了这组代码规范。当开发者们在多个项目中合作时，本指南将会成为所有这些项目中共用的一组代码规范。 因此，本指南的益处不在于这些规则本身，而在于在所有项目中共用这些规则。

[RFC 2119][]中的`必须(MUST)`，`不可(MUST NOT)`，`建议(SHOULD)`，`不建议(SHOULD NOT)`，`可以/可能(MAY)`等关键词将在本节用来做一些解释性的描述。

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/hfcorriez/fig-standards/blob/zh_CN/接受/PSR-0.md
[PSR-1]: https://github.com/hfcorriez/fig-standards/blob/zh_CN/接受/PSR-1-basic-coding-standard.md


1. 概述
-----------

- 代码`必须`遵守 [PSR-1][]。

- 代码`必须`使用4个空格来进行缩进，而不是用制表符。

- 一行代码的长度`不建议`有硬限制；软限制`必须`为120个字符，`建议`每行代码80个字符或者更少。

- 在`命名空间(namespace)`的声明下面`必须`有一行空行，并且在`导入(use)`的声明下面也`必须`有一行空行。

- `类(class)`的左花括号`必须`放到其声明同一行，右花括号则`必须`放到类主体下面自成一行。

- `方法(method)`的左花括号`必须`放到其声明同一行，右花括号则`必须`放到方法主体的下一行。

- 所有的`属性(property)`和`方法(method)` `必须`有可见性声明；`抽象(abstract)`和`终结(final)`声明`必须`在可见性声明之前；而`静态(static)`声明`必须`在可见性声明之后。

- 在控制结构关键字的后面`必须`有一个空格；而`方法(method)`和`函数(function)`的关键字的后面`不可`有空格。

- 控制结构的左花括号`必须`跟其放在同一行，右花括号`必须`放在该控制结构代码主体的下一行。

- 控制结构的左括号之后`不可`有空格，右括号之前也`不可`有空格。

### 1.1. 示例

这个示例中简单展示了上文中提到的一些规则：

```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface {
    public function sampleFunction($a, $b = null) {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar() {
        // 方法主体
    }
}
```

2. 通则
----------

### 2.1 基础代码规范

代码`必须`遵守 [PSR-1][] 中的所有规则。

### 2.2 源文件

所有的PHP源文件`必须`使用Unix LF(换行)作为行结束符。

所有PHP源文件`必须`以一个空行结束。

纯PHP代码源文件的关闭标签`?>` `必须`省略。

### 2.3. 行

行长度`不可`有硬限制。

行长度的软限制`必须`是120个字符；对于软限制，代码风格检查器`必须`警告但`不可`报错。

一行代码的长度`不建议`超过80个字符；较长的行`建议`拆分成多个不超过80个字符的子行。

在非空行后面`不可`有空格。

空行`可以`用来增强可读性和区分相关代码块。

一行`不可`多于一个语句。

### 2.4. 缩进

代码`必须`使用4个空格，且`不可`使用制表符来作为缩进。

> 注意：代码中只使用空格，且不和制表符混合使用，将会对避免代码差异，补丁，历史和注解中的一些问题有帮助。空格的使用还可以使通过调整细微的缩进来改进行间对齐变得更加的简单。

### 2.5. 关键字和 True/False/Null

PHP关键字([keywords][])`必须`使用小写字母。

PHP常量`true`, `false`和`null` `必须`使用小写字母。

[keywords]: http://php.net/manual/en/reserved.keywords.php


3. `命名空间(Namespace)`和`导入(Use)`声明
---------------------------------

`命名空间(namespace)`的声明后面`必须`有一行空行。

所有的`导入(use)`声明`必须`放在`命名空间(namespace)`声明的下面。

一句声明中，`必须`只有一个`导入(use)`关键字。

在`导入(use)`声明代码块后面`必须`有一行空行。

示例：

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... 其它PHP代码 ...

```


4. `类(class)`，`属性(property)`和`方法(method)`
-----------------------------------

术语“类”指所有的`类(class)`，`接口(interface)`和`特性(trait)`。

### 4.1. `扩展(extend)`和`实现(implement)`

一个类的`扩展(extend)`和`实现(implement)`关键词`必须`和`类名(class name)`在同一行。

`类(class)`的左花括号`必须`放在同一行；右花括号必须放在`类(class)`主体的后面自成一行。


```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable {
    // 常量、属性、方法
}
```

### 4.2. `属性(property)`

所有的`属性(property)`都`必须`声明其可见性。

`变量(var)`关键字`不可`用来声明一个`属性(property)`。

一条语句`不可`声明多个`属性(property)`。

一个`属性(property)`声明看起来应该像下面这样。

```php
<?php
namespace Vendor\Package;

class ClassName {
    public $foo = null;
}
```

### 4.3. `方法(method)`

所有的`方法(method)`都`必须`声明其可见性。

`方法名(method name)`其左花括号`必须`放在同一行，且右花括号`必须`放在方法主体的下面自成一行。左括号后面`不可`有空格，且右括号前面也`不可`有空格。

一个`方法(method)`声明看来应该像下面这样。 注意括号，逗号，空格和花括号的位置：

```php
<?php
namespace Vendor\Package;

class ClassName {
    public function fooBarBaz($arg1, &$arg2, $arg3 = []) {
        // 方法主体部分
    }
}
```

### 4.4. `方法(method)`的参数

在参数列表中，逗号之前`不可`有空格，而逗号之后则`必须`要有一个空格。

`方法(method)`中有默认值的参数必须放在参数列表的最后面。

```php
<?php
namespace Vendor\Package;

class ClassName {
    public function foo($arg1, &$arg2, $arg3 = []) {
        // 方法主体部分
    }
}
```

### 4.5. `抽象(abstract)`，`终结(final)`和 `静态(static)`

当用到`抽象(abstract)`和`终结(final)`来做类声明时，它们`必须`放在可见性声明的前面。

而当用到`静态(static)`来做类声明时，则`必须`放在可见性声明的后面。

```php
<?php
namespace Vendor\Package;

abstract class ClassName {
    protected static $foo;

    abstract protected function zim();

    final public static function bar() {
        // 方法主体部分
    }
}
```

### 4.6. 调用方法和函数

调用一个方法或函数时，在方法名或者函数名和左括号之间`不可`有空格，左括号之后`不可`有空格，右括号之前也`不可`有空格。参数列表中，逗号之前`不可`有空格，逗号之后则`必须`有一个空格。

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

5. 控制结构
---------------------

下面是对于控制结构代码风格的概括：

- 控制结构的关键词之后`必须`有一个空格。
- 控制结构的左括号之后`不可`有空格。
- 控制结构的右括号之前`不可`有空格。
- 控制结构的右括号和左花括号之间`必须`有一个空格。
- 控制结构的代码主体`必须`进行一次缩进。
- 控制结构的右花括号`必须`主体的下一行。

每个控制结构的代码主体`必须`被括在花括号里。这样可是使代码看上去更加标准化，并且加入新代码的时候还可以因此而减少引入错误的可能性。

### 5.1. `if`，`elseif`，`else`

下面是一个`if`条件控制结构的示例，注意其中括号，空格和花括号的位置。同时注意`else`和`elseif`要和前一个条件控制结构的右花括号在同一行。

```php
<?php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
```

`推荐`用`elseif`来替代`else if`，以保持所有的条件控制关键字看起来像是一个单词。


### 5.2. `switch`，`case`

下面是一个`switch`条件控制结构的示例，注意其中括号，空格和花括号的位置。`case`语句`必须`要缩进一级，而`break`关键字（或其他中止关键字）`必须`和`case`结构的代码主体在同一个缩进层级。如果一个有主体代码的`case`结构故意的继续向下执行则`必须`要有一个类似于`// no break`的注释。

```php
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```


### 5.3. `while`，`do while`

下面是一个`while`循环控制结构的示例，注意其中括号，空格和花括号的位置。

```php
<?php
while ($expr) {
    // structure body
}
```

下面是一个`do while`循环控制结构的示例，注意其中括号，空格和花括号的位置。

```php
<?php
do {
    // structure body;
} while ($expr);
```

### 5.4. `for`

下面是一个`for`循环控制结构的示例，注意其中括号，空格和花括号的位置。

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

### 5.5. `foreach`

下面是一个`foreach`循环控制结构的示例，注意其中括号，空格和花括号的位置。

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

### 5.6. `try`, `catch`

下面是一个`try catch`异常处理控制结构的示例，注意其中括号，空格和花括号的位置。

```php
<?php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```