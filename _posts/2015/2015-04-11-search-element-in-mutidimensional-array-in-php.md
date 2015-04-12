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
{% highlight php startinline=true %}
//在arr中搜索value
$arr = array("a"=>1,"b"=>2,"c"=>3);
$value = 3;
$key = array_search(3, $arr);
{% endhighlight %}
如果是一个多维的数组需要查找值为某个value的key，array_search是不能够完成这个目的的。可以将array_search 写成递归的形式:

{% highlight php startinline=true %}
function recursive_array_search($search_value,$arr) {
    foreach($arr as $key => $value) {
        $current_key = $key;
        if($search_value === $value){
			return $current_key;
        } elseif(is_array($value) && recursive_array_search($search_value,$value) !== false) {
           	return $current_key;
		} 
    }
    return false;
}
{% endhighlight %}
该函数只返回最上层的key。更简单的可以使用逻辑运算符代替if，写法更加简单：

{% highlight php startinline=true %}
function recursive_array_search($search_value,$arr) {
    foreach($arr as $key => $value) {
        $current_key = $key;
        if($search_value === $value or (is_array($value) && recursive_array_search($search_value,$value) !== false)) {
           	return $current_key;
		} 
    }
    return false;
}
{% endhighlight %}
>      注意该函数只返回最外层的key，同时return false 只在最后返回(遍历完成之后)
