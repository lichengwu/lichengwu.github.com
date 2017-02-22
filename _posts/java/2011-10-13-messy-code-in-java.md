---
layout: post
catalog: true
title: "Java 乱码总结"
description: ""
category: java
subhead: ''
tags: [java, messy-code]

---

#### 1.URL乱码
有的是，不可避免要在URL上传中文，用一些框架（spring MVC，struts）可以解决。但是我们也可以自己手动解决。
 
    String url = "http://www.softbeta.iteye.com?name=小武";  
    // url编码  
    url = "http://www.softbeta.iteye.com?name=" + java.net.URLEncoder.encode("小武");  
    System.out.println("url:" + url);  
    // url解码  
    url = "http://www.softbeta.iteye.com?name=" + java.net.URLDecoder.decode("小武");  
    System.out.println("url:" + url);  
    
encode(String s) 和 decode(String s)方法会采用系统的默认编码，已标记为@deprecated 取代为带编码的方法encode(String s, String enc),decode(String s, String enc)。 
 
    String url = "http://www.softbeta.iteye.com?name=小武";  
    // url编码  
    url = "http://www.softbeta.iteye.com?name=" + java.net.URLEncoder.encode("小武","utf-8");  
    System.out.println("url:" + url);  
    // url解码  
    url = "http://www.softbeta.iteye.com?name=" + java.net.URLDecoder.decode("小武","utf-8");  
    System.out.println("url:" + url); 
     
url encode和decode在前端js也有很好的支持，这样能方便我们处理请求和响应。

#### 2.JSP乱码

在基于框架开发中，JSP乱码基本上由框架解决，但是有些地方我们的编码需要统一。

在JSP文件头，一般加入：
 
    <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>  

charset=UTF-8的作用是指定JSP向客户端输出的编码方式为"UTF-8"；

pageEncoding="UTF-8"，为了让JSP引擎能正确地解码含有中文字符的JSP页面；

如果对请求编码：
  
    request.setCharacterEncoding("UTF-8");  
    
还有一种是在web容器中设置编码。如Tomcat，如果上面的方法都能解决乱码，可以在server.xml修改URIEncoding：
 
    <Connector port="8088" protocol="HTTP/1.1"   
                   connectionTimeout="20000"   
                   redirectPort="8443" URIEncoding="utf-8"/>  

#### 3.打包文件(zip)乱码 

如果用原生的`java.util.zip.ZipOutputStream`进行文件打包，当文件名出现中文的时候，会出现乱码，目前还没有很好的解决方法，除非修改这个类的代码，然后重新打包。

`org.apache.tools.zip.ZipOutputStream`用这个类进行打包可以设置编码，有效的解决中文问题。

    ZipOutputStream zos = new ZipOutputStream(os);  
    zos.setEncoding("GBK");  
    ...  
    zos.flush();  
    zos.close();  

#### 4.其他情况通用解决

像文件乱码，数据库乱码...等，很多时候是方法没有用对，字符集设置不统一或没有用对。对文件读写，用正确的流，合适的编码，一般不会出问题。数据库也是一样，数据库的编码也后台处理程序编码要统一。但是还是有一些情况我们不能成功解决乱码问题，这是只有强制转码了。 
  
    /** 
     * 转码 
     *  
     * @author oliver 
     * @created 2011-10-13 
     * 
     * @param str 需要转码的字符串 
     * @param originEncode 原字符串编码 
     * @param destEncode 需要转向的编码 
     * @return 
     * @throws UnsupportedEncodingException 
     */  
    public static String transcoding(String str,String originEncode,String destEncode) throws     UnsupportedEncodingException{  
        if(str==null || str.trim()==""){  
            return str;  
        }  
        return new String(str.getBytes(originEncode),destEncode);  
    }  

如果你知道乱码字符串以前用的编码，那么可以用上面的方法解决乱码。

在java.nio包中，也有类似编码解码的方法。
  
    String name="中文";  
    Charset ct = Charset.forName("utf-8");  
    ByteBuffer encode = ct.encode(name);  
    name=ct.decode(encode).toString();
      
总之，乱码问题很多时候是由于我们的方法，工具没有选择正确，或者编码不统一。如果真的没有其他好的办法，可以自己写工具强制转码。
 

