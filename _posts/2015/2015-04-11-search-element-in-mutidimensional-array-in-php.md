---
layout: post
title: PHP中多维数组的搜索 
categories:
- Programming
tags:
- PHP 
- 算法
---

我们在使用php的时候可能会遇到这样的问题，如果想从一个一维数组搜索某个值并返回其对应的key，那么可以使用array_search函数:
{% highlight php %}
//在arr中搜索value
$arr = array("a"=>1,"b"=>2,"c"=>3);
$value = 3;
$key = array_search(3);
{% endhighlight %}
如果是一个多维的数组需要查找值为某个value的key，array_search是不能够完成这个目的的。
