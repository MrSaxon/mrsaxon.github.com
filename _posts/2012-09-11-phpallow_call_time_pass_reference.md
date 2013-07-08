---
layout: post
title: "关于PHP配置中allow_call_time_pass_reference的说明"
description: ""
category: "PHP"
tags: ["PHP配置", "引用配置"]
---
{% include JB/setup %}

今天在开发的过程中调试代码遇到了一个php warning：Call-time pass-by-reference has been deprecated

几经查找后发现这个错误是通过allow_call_time_pass_reference开关显示的

那这个错误究竟爆出了什么呢

php.ini的原文如下：

{% highlight php %}
; Whether to enable the ability to force arguments to be passed by reference
; at function call time.  This method is deprecated and is likely to be
; unsupported in future versions of PHP/Zend.  The encouraged method of
; specifying which arguments should be passed by reference is in the function
; declaration.  You're encouraged to try and turn this option Off and make
; sure your scripts work properly with it in order to ensure they will work
; with future versions of the language (you will receive a warning each time
; you use this feature, and the argument will be passed by value instead of by
; reference).
{% endhighlight %}
翻译过来的意思如下：

<blockquote>
是否启用在函数调用时强制参数被按照引用传递。此方法已不被赞成并在 PHP/Zend 未来的版本中很可能不再支持。鼓励使用的方法是在函数定义中指定哪些参数应该用引用传递。鼓励大家尝试关闭此选项并确保脚本能够正常运行，以确保该脚本也能在未来的版本中运行（每次使用此特性都会收到一条警告，参数会被按值传递而不是按照引用传递）。
在函数调用时通过引用传递参数是不推荐的，因为它影响到了代码的整洁。如果函数的参数没有声明作为引用传递，函数可以通过未写入文档的方法修改其参数。要避免其副作用，最好仅在函数声明时指定那个参数需要通过引用传递。
</blockquote>
后查看自己的代码传递引用参数风格为：

{% highlight php %}
function exampleFunc($arg1, $arg2) {
    //todo something
}

$array = array(1);
exampleFunc('arg1', &$array);
{% endhighlight %}
而这种使用如上面的翻译所说会影响到整体代码的整洁，没有通过函数的参数格式去控制传入参数的的类型，PHP中不建议这样去写，但是本身不影响程序使用，所以为php的warning

于是乎本人将自己的代码进行了修正，使用了下面的形式接触这种warning：

{% highlight php %}
function exampleFunc($arg1, &$arg2) {
    //todo something
}

$array = array(1);
exampleFunc('arg1', $array);
{% endhighlight %}
建议大家在写代码的时候将allow_call_time_pass_reference设置为Off，提高自己的代码质量！:)
