---
layout: post
catalog: true
title: "利用Flying Saucer 和 iText 实现HTMl转PDF(java)"
description: ""
category: java
subhead: ''
tags: [java, iText, pdf, html]

---

Flying Saucer(或者叫xhtmlrender project on java.net)是一个基于iText的开源java库，能够轻松的将html(带css2.1)生成pdf。

iText是一个生成PDF文档的开源java库，能够动态从XML或者数据库生成PDF，同时它具备PDF文档的绝大多数属性(比如加密……)，支持java，C\#等。

### 生成简单的PDF

下面我们首先创建一个简单的带css的html，代码如下：


    <?xml version="1.0" encoding="UTF-8"?>

    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml">
        <head>
            <title>My First Document</title>
            <style type="text/css"> b { color: green; } </style>
        </head>
        <body>
            <p>
                <b>Greetings Earthlings!</b>
                We've come for your Java.
            </p>
        </body>
    </html>

接下来生成一个pdf


    public class FirstDoc
    {
        public static void main(String[] args) throws DocumentException, IOException
        {
            String path = System.getProperty("user.dir") + "/src/";
            String inputFile = path + "samples/firstdoc.html";
            String url = new File(inputFile).toURI().toURL().toString();
            String outputFile = path + "outputs/fistdoc.pdf";
            OutputStream os = new FileOutputStream(outputFile);
            ITextRenderer render = new ITextRenderer();
            render.setDocument(url);
            render.layout();
            render.createPDF(os);
            os.close();
        }
    }

结果如下:

![](/images/java/1_zps6a7d60b1.png)


### 用Fly生成内容

下面的我们先用StringBuilder生成一个HTML的字符串，然后用DOM解析，生成PDF。

    
    public class OneHundredBottles{
        public static void main(String[] args) throws Exception
        {
            String path =System.getProperty("user.dir")+"/src/";
            StringBuffer buf = new StringBuffer();
            buf.append("<html>");
            // put in some style
            buf.append("<head><style language='text/css'>");
            buf.append("h3 { border: 1px solid #aaaaff; background: #ccccff; ");
            buf.append("padding: 1em; text-transform: capitalize; font-family: sansserif; font-weight: normal;}");
            buf.append("p { margin: 1em 1em 4em 3em; } p:first-letter { color: red; font-size: 150%; }");
            buf.append("h2 { background: #5555ff; color: white; border: 10px solid black; padding: 3em; font-size: 200%; }");
            buf.append("</style></head>");
            // generate the body
            buf.append("<body>");
            buf.append("<p><img src='"+path+"samples/100bottles.jpg'/></p>");
            for(int i=99; i>0; i--) {
                buf.append("<h3>"+i+" bottles of beer on the wall, "
                        + i + " bottles of beer!</h3>");
                buf.append("<p>Take one down and pass it around, "
                        + (i-1) + " bottles of beer on the wall</p>/n");
            }
            buf.append("<h2>No more bottles of beer on the wall, no more bottles of beer. ");
            buf.append("Go to the store and buy some more, 99 bottles of beer on the wall.</h2>");
            buf.append("</body>");
            buf.append("</html>");
            DocumentBuilder builder = DocumentBuilderFactory.newInstance().newDocumentBuilder();
            Document doc = builder.parse(new StringBufferInputStream(buf.toString()));
            ITextRenderer renderer = new ITextRenderer();
            renderer.setDocument(doc, null);
            String outputFile = path+"outputs/100bottles.pdf";
            OutputStream os = new FileOutputStream(outputFile);
            renderer.layout();
            renderer.createPDF(os);
            os.close();
        }
    }


 效果如下：

![](/images/java/2_zpsf3bd60e6.png)

### 在服务端创建PDF

下面展示在servlet里面创建pdf

    public class PDFServlet extends HttpServlet {
        protected void processRequest(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            response.setContentType("application/pdf");
            StringBuffer buf = newStringBuffer();
            buf.append("<html>");
            String css = getServletContext().getRealPath("/PDFservlet.css");
            // put in some style
            buf.append("<head><link rel='stylesheet' type='text/css' "+"href='"+css+"' media='print'/></head>");
            ... //generate the rest of the HTML
            // parse our markup into an xml Document
            try{
                DocumentBuilder builder = DocumentBuilderFactory.newInstance().newDocumentBuilder();
                Document doc = builder.parse(new StringBufferInputStream(buf.toString()));
                ITextRenderer renderer = new ITextRenderer();
                renderer.setDocument(doc, null);
                renderer.layout();
                OutputStream os = response.getOutputStream();
                renderer.createPDF(os);
                os.close();
            } catch(Exception ex) {
                ex.printStackTrace();
            }
        }
    }


 原文地址:[http://today.java.net/pub/a/today/2007/06/26/generating-pdfs-with-flying-saucer-and-itext.html](http://today.java.net/pub/a/today/2007/06/26/generating-pdfs-with-flying-saucer-and-itext.html "http://today.java.net/pub/a/today/2007/06/26/generating-pdfs-with-flying-saucer-and-itext.html")

