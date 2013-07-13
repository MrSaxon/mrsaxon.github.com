---
layout: post
title: "关于PHP的array_walk方法的一些歧义"
description: ""
category: "PHP"
tags: ["array_walk", "php"]
---
{% include JB/setup %}

最近在开发中由于需要对一个数组进行三层循环处理，介于使用PHP中的foreach或for循环要比PHP本身array处理函数的效率增加40%-100%（此处后面提交文章来解释为什么会增加这么多的效率开销，或者读者可以搜索下，很多人进行了相关测试），所以采用了array_walk方法来满足自己需求上的使用，下面介绍下我的使用过程：

本人有一个待处理数组，结构如下：

{% highlight java %}
    $aNeedToDo = array(
        1 => array(
            array(
                'model' => object,
                'node' => object,
            ),
            ...
        ),
        4 => array(
            array(
                'model' => object,
                'node' => object,
            ),
            ...
        ),
        32 => array(
            array(
                'model' => object,
                'node' => object,
            ),
            ...
        ),
    );
{% endhighlight %}
现在笔者要对其进行处理，最终目的是根据最底下的一维数据对node进行定型且需要对于$aNeedToDo中key值大于1的数组生成一个关联父节点数据，实现关联，并需要将处理结果保存起来最终反馈给用户，于是本人定义了一个回调，用于处理这个数组。

{% highlight java %}
    function _handleTodoList($aTodoListItem, $nNodeCount, &$aReturnData) {
        if($nNodeCount == 1) {//不需要生成父节点
            array_walk($aTodoListItem, '_handleNotParentNode', $aReturnData);
        } else {//需要生成父节点
            $aParentTodoList = array_chunk($aTodoListItem, $aNodeCount);//按照父节点中需要关联的子节点个数对子节点集合进行分组
            array_walk($aParentTodoList'_handleParentNode', $aReturnData);
        }
    }

    function _handleNotParentNode($aNodeItem, $key, &$aReturnData) {
        //do something
        $aReturnData[] = array('status' => 0/1, 'msg' => 'xxx');
    }

    function _handleParentNode($aParentNodeItem, $key, &$aReturnData) {
        //do other something
        $aReturnData[] = array('status' => 0/1, 'msg' => 'xxx');
    }
{% endhighlight %}
实现好以上方法，开始对$aNeedToDo进行处理

    $aReturnData = array();
    array_walk($aNeedToDo, '_handleTodoList', $aReturnData);
按照以往对数组引用的理解此处在array_walk处理后$aReturnData会存在回调函数中保存的处理信息，恰恰相反的时候在array_walk处理过后没有任何结果，于是在array_walk的回调函数内部加上var_dump打印$aReturnData，发现，其实每次调用回调的时候确实$aReturnData是有变化的，那么说明函数声明引用处理是没有问题的，查了一遍PHP手册一直都无解为什么会这样，后来和同事一起研究了下，推断是array_walk在本身接受参数的方式不是引用导致，后来重新找了一份带有评论的PHP手册，大家原来都有对此疑惑的时候，所以想要以引用方式去操作回调的是后上面的代码就要修改一下了。

{% highlight java %}
    function _handleTodoList($aTodoListItem, $nNodeCount, &$aReturnData) {
        if($nNodeCount == 1) {//不需要生成父节点
            array_walk($aTodoListItem, '_handleNotParentNode', &$aReturnData);
        } else {//需要生成父节点
            $aParentTodoList = array_chunk($aTodoListItem, $aNodeCount);//按照父节点中需要关联的子节点个数对子节点集合进行分组
            array_walk($aParentTodoList'_handleParentNode', &$aReturnData);
        }
    }

    function _handleNotParentNode($aNodeItem, $key, &$aReturnData) {
        //do something
        $aReturnData[] = array('status' => 0/1, 'msg' => 'xxx');
    }

    function _handleParentNode($aParentNodeItem, $key, &$aReturnData) {
        //do other something
        $aReturnData[] = array('status' => 0/1, 'msg' => 'xxx');
    }

    $aReturnData = array();
    array_walk($aNeedToDo, '_handleTodoList', &$aReturnData);
{% endhighlight %}
歧义问题迎刃而解！不过这种使用方式和PHP建议的只在方法声明时声明为引用，调用不需要声明违背，还不是很了解为什么这个函数要这样设计，后期可以针对PHP源码加以分析。
