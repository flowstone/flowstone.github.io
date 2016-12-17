---
layout: post
title: 开发自定义模板引擎
date: 2016-08-24
excerpt: "如何快速安装WAMP."
tags: [template]
comments: true
---


### 自定义模板引擎类
**MyTpl.class.php**

{% highlight php %}
<?php
class MyTpl
{
    private $tpl_vars = array();
    //分配
    public function assign($key,$value){
        $this->tpl_vars[$key] = $value;
    }
    public function display($tpl){
        $contents = file_get_contents($tpl);
        foreach ($this->tpl_vars as $k => $v){
        //替换 将{$name} 替换成真实的数据
        $contents = str_replace('{$'."$k".'}',"$v", $contents);
        $compile = './templates_c/'.md5('show.html') . '.php';
        file_put_contents($compile, $contents);
        require $compile;
        }
    }
}
$tpl = new MyTpl;
$tpl-> assign('name','张四');
$tpl-> display('./template/show.html');
{% endhighlight %}

### 自定义视图
**template/show.html**

{% highlight html %}
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    {$name}
</body>
</html>
{% endhighlight %}