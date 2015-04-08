---
layout: post
title: "解决jQuery 1.4 json问题"
description: ""
category: fe
subhead: ''
tags: [jquery, json]

---

最近用jQuery写了一个小例子，
 
    $(function(){  
        $.ajax({  
            url:'server/getColors.jsp',  
            type:"POST",  
            success:function(data){  
                    alert(data);  
                    alert('name:'+data.name);  
                    alert('age:'+data.age);  
                    },  
            dataType:'json',  
            error:function(xhr,status,error){  
                alert('status='+status+',error='+error);  
            }  
        });  
    });  
 
getColors.jsp

    {name:'oliver',age:12} 
     
执行的时候总是不能执行success,
error显示`status=parsererror,error=Invalid JSON: {name:'oliver',age:12}`

但是把jQuery换成1.3版本就可以正常工作！

开始是以为是jQuery的bug，经过求证才知道是jQuery1.4对json数据的校验更加严格了，官方说明如下：

>"json": Evaluates the response as JSON and returns a JavaScript object. In jQuery 1.4 the JSON data is parsed in a strict manner; any malformed JSON is rejected and a parse error is thrown. (See json.org for more information on proper JSON formatting.)

正确的json格式为：

键名称：用双引号 括起

字符串：用使用双引号 括起

数字，布尔类型不需要 使用双引号 括起

所以正确的getColors.jsp

    {"name":"oliver","age":12}  
 
感谢回答问题的 cj205 net_lover showbo

##### 参考资料：

http://www.code-design.cn/article/20100722/jquery-1-4-datatype-is-json-issue.aspx

http://www.code-design.cn/article/20100721/jquery-1-4-2-ajax-plugin-datatype-json-error.aspx

http://topic.csdn.net/u/20101220/20/fce48d0f-067c-44c8-ba9e-5cd1d0968654.html

http://api.jquery.com/jQuery.ajax/

http://forum.jquery.com/topic/jquery-1-4-x-getjson-callback-function-doesn-t-work


