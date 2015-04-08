---
layout: post
title: "解决 jQuery 与 prototype冲突 jQuery与easyvalidation冲突"
description: ""
category: fe
subhead: ''
tags: [jquery, prototype, easyvalidation, javascript]

---

看以一篇解决方案，我测试没成功http://www.blogjava.net/beansoft/archive/2008/07/03/212407.html
今天问了老师，得到了正解。
 
注意jQuery与prototype的加载顺序。

其实这两个框架冲突主要是$符号的冲突，在jQuery中将$引用的对象映射回原始的对象就可以解决了
 
导入js
  
    <script type="text/javascript" src="js/jquery.js" mce_src="js/jquery.js"></script>  
    <script type="text/javascript">  
        jQuery.noConflict();  
    </script>  
    <script type="text/javascript" src="js/functions.js" mce_src="js/functions.js"></script>  
    <!-- begin easyvalidation -->  
        <script src="js/easyvalidation/prototype.js" mce_src="js/easyvalidation/prototype.js" type="text/javascript"></script>  
        <script src="js/easyvalidation/effects.js" mce_src="js/easyvalidation/effects.js" type="text/javascript"></script>  
        <script src="js/easyvalidation/validation_cn.js" mce_src="js/easyvalidation/validation_cn.js" type="text/javascript"></script>  
        <!-- 需要的样式文件 -->  
        <link rel="stylesheet" type="text/css" href="css/easyvalidation/style_min.css" mce_href="css/easyvalidation/style_min.css" />   
    <!-- end easyvalidation -->  
 
编写代码，
下面是一个自动绑定下拉列表的函数，测试在有prototype环境中能过正常运行
 
    function selectBind(selectId,valueStr){  
        //解决jQuery与prototype的冲突  
        var count = jQuery("#"+selectId+" option").length;  
        for(var i=0;i<count;i++){  
            if(jQuery("#"+selectId).get(0).options[i].value==valueStr){  
                jQuery("#"+selectId).get(0).options[i].selected=true;  
                break;  
            }  
        }  
    }
      
------
####附录：

`jQuery.noConflict()`

运行这个函数将变量$的控制权让渡给第一个实现它的那个库。

这有助于确保jQuery不会与其他库的$对象发生冲突。

在运行这个函数后，就只能使用jQuery变量访问jQuery对象。例如，在要用到$("div p")的地方，就必须换成jQuery("div p")。

注意: 这个函数必须在你导入jQuery文件之后，并且在导入另一个导致冲突的库之前 使用。当然也应当在其他冲突的库被使用之前，除非jQuery是最后一个导入的。

Run this function to give control of the $ variable back to whichever library first implemented it.

This helps to make sure that jQuery doesn't conflict with the $ object of other libraries.
By using this function, you will only be able to access jQuery using the 'jQuery' variable. For example, where you used to do $("div p"), you now must do jQuery("div p").

**NOTE:** This function must be called after including the jQuery javascript file, but before including any other conflicting library, and also before actually that other conflicting library gets used, in case jQuery is included last.

##### 返回值
jQuery
##### 示例
将$引用的对象映射回原始的对象。

    jQuery.noConflict();
    // 使用 jQuery
    jQuery("div p").hide();
    // 使用其他库的 $()
    $("content").style.display = 'none';
恢复使用别名$，然后创建并执行一个函数，在这个函数的作用域中仍然将$作为jQuery的别名来使用。在这个函数中，原来的$对象是无效的。这个函数对于大多数不依赖于其他库的插件都十分有效。

    jQuery.noConflict();
    (function($) { 
    $(function() {
    // 使用 $ 作为 jQuery 别名的代码
    });
    })(jQuery);
    // 其他用 $ 作为别名的库的代码
创建一个新的别名用以在接下来的库中使用jQuery对象。

    var j = jQuery.noConflict();
    // 基于 jQuery 的代码
    j("div p").hide();
    // 基于其他库的 $() 代码
    $("content").style.display = 'none';


