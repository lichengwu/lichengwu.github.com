---
layout: post
title: "iText 解决中文问字体问题 显示中文"
description: ""
category: java
subhead: ''
tags: [java,  iText]

---

总结一下，基本上有三种方法解决iText显示中文问题。
 
#### 方法一：
使用Windows系统字体(TrueType)
 
#### 方法二：
使用iTextAsian.jar中的字体

#### 方法三：
使用资源字体(ClassPath)
由于比较简单，直接上代码了。

    public class PDF2Chinese
    {
        public static void main(String[] args) throws DocumentException, IOException
        {
            Document document = new Document();
            OutputStream os = new FileOutputStream(new File("chinese.pdf"));
            PdfWriter.getInstance(document,os);
            document.open();
            //方法一：使用Windows系统字体(TrueType)    
            BaseFont baseFont = BaseFont.createFont("C:/Windows/Fonts/SIMYOU.TTF",BaseFont.IDENTITY_H,BaseFont.NOT_EMBEDDED);
    
            //方法二：使用iTextAsian.jar中的字体    
            //BaseFont baseFont = BaseFont.createFont("STSong-Light",BaseFont.IDENTITY_H,BaseFont.NOT_EMBEDDED);    
    
            //方法三：使用资源字体(ClassPath)    
            ////BaseFont baseFont = BaseFont.createFont("/SIMYOU.TTF",BaseFont.IDENTITY_H,BaseFont.NOT_EMBEDDED);    
    
            Font font = new Font(baseFont);
            document.add(new Paragraph("解决中文问题了！",font));
            document.close();
        }
    }  
 
好了，现在可以打开生成的chinese.pdf看到中文了！

