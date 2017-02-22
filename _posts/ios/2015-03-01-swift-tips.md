---
layout: post
catalog: true
title: "Swift技巧，持续更新"
description: "汇总一下初学swift遇到的问题"
category: ios
subhead: '汇总一下初学swift遇到的问题'
tags: [swift, ios]

---

<br >

#### 1.NSURL含有中文，会导致创建NSURL的结果是nil，把字符串用UTF8转码一下就好了:

    var url = NSURL(string: "http://wthrcdn.etouch.cn/weather_mini?city=北京".stringByAddingPercentEscapesUsingEncoding(NSUTF8StringEncoding)!)

#### 2.iOS7/8中用swift方法定位服务(CLLocationManager)无法获取定位信息：
初始化CLLocationManager

        locationMgr = CLLocationManager()
        locationMgr.delegate = self
        locationMgr.desiredAccuracy = kCLLocationAccuracyBest
        locationMgr.distanceFilter = 1000.0
        locationMgr.startUpdatingLocation()
后发现，应用启动没有询问是否允许定位服务，也无法获取位置信息。

在代码中加入Requests permission方法，这个是为了兼容iOS7和iOS8的代码，加入了一个判断。在iOS8中需要询问用户是否同意使用位置信息，否则的话该功能不可用。

        if self.locationMgr.respondsToSelector("requestAlwaysAuthorization") {
            self.locationMgr.requestAlwaysAuthorization()
        }   
        
依然无法成功定位，原因在于requestAlwaysAuthorization()方法需要在plist加入`NSLocationAlwaysUsageDescription`key才能正确执行，value可以是任意值。

PS：requestWhenInUseAuthorization()在plist加入对应`NSLocationWhenInUseUsageDescription`的key。      
        


