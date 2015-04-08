---
layout: post
title: "varnish使用汇总(不断更新)"
description: ""
category: linux
subhead: ''
tags: [varnish, cache, linux]

---

**Q:**如何配置varnish缓存到硬盘？

**A:**[http://blog.lichengwu.cn/linux/2012/09/19/store-varnish-cache-to-disk](http://blog.lichengwu.cn/linux/2012/09/19/store-varnish-cache-to-disk)
 
---- 
 
**Q:**如果debug VCL?

**A:**[http://stackoverflow.com/questions/12576248/how-to-debug-vcl-in-varnish](http://stackoverflow.com/questions/12576248/how-to-debug-vcl-in-varnish)

----
 
**Q:**怎样不重启varnish让新的vcl生效？

**A:**用varnishadm进入管理员页面：
 
    vcl.load <configname> <filename> //加载一个新的vcl配置，configname：给配置起个名字，filename：配置的路径  
    vcl.use <configname> //使用新的配置  
    vcl.discard <configname> // 删除某个配置  
    vcl.list //查看所有加载的配置  
 
---- 
 
**Q:**VCL怎么urlrewrite?

**A:**[https://www.varnish-cache.org/trac/wiki/RedirectsAndRewrites](https://www.varnish-cache.org/trac/wiki/RedirectsAndRewrites)

**PS:**regsub函数支持后向引用(backreferences)。

eg:

    set req.url = regsub(req.url,"/attachment/(.+)(&token=.+)$","/cache/attachment/\1");  
    
----    
    
**Q:**503 service unavailable?  

**A:**503错误，这是因为varnish对后端服务器响应header有限制，默认长度是2048，可将其调大一些

启动参数代码
  
    -p http_resp_hdr_len=8192  
  
VCL官方文档：[https://www.varnish-cache.org/docs/3.0/reference/vcl.html](https://www.varnish-cache.org/docs/3.0/reference/vcl.html)
 

