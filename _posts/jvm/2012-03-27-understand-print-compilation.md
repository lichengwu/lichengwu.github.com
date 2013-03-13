---
layout: post
title: "PrintCompilation 参数解释"
description: ""
category: jvm
subhead: ''
tags: [jvm, compile]

---

    b    Blocking compiler (always set for client)
    *    Generating a native wrapper
    %    On stack replacement (where the compiled code is running)
    !    Method has exception handlers
    s    Method declared as synchronized
    n    Method declared as native
    made non entrant    compilation was wrong/incomplete, no future callers will use this version
    made zombie         code is not in use and ready for GC
    
#### "made not entrant" and "made zombie"
>Zombie methods are methods whose code has been made invalid by class loading. Generally the server compiler makes aggressive inlining decisions of non-final methods. As long as the inlined method is never overridden the code is correct. When a subclass is loaded and the method overridden, the compiled code is broken for all future calls to it. The code gets declared "not entrant" (no future callers to the broken code), but sometimes existing callers can keep using the code. In the case of inlining, that's not good enough; existing callers' stack frames are "deoptimized" when they return to the code from nested calls (or just if they are running in the code). When no more stack frames hold PC's into the broken code it's declared a "zombie" - ready for removal once the GC gets around to it.   

原文地址：[http://blog.joda.org/2011/08/printcompilation-jvm-flag.html](http://blog.joda.org/2011/08/printcompilation-jvm-flag.html)

{% include JB/setup %}