---
layout: post
title: "Lucene 实战：快速开始 简单查询"
description: ""
category: java
subhead: ''
tags: [java,  lucene,  search, index]

---

    /**
     *
     * 查询
     *
     * @throws IOException
     * @throws ParseException
     */
    @Test
    public void search() throws IOException, ParseException{
        //创建分词器
        Analyzer analyzer = new StandardAnalyzer(Version.LUCENE_34);
        //索引库
        Directory dir = FSDirectory.open(new File(indexPath));
        //IndexSearcher
        IndexSearcher searcher = new IndexSearcher(dir);
        //查询解析
        QueryParser parser = new QueryParser(Version.LUCENE_34, "content", analyzer);
        Query query = parser.parse("WYSIWYG");
        //查询
        TopDocs topDocs = searcher.search(query, 100);
        System.out.println("查询结果:"+topDocs.totalHits+"条记录");
        //打印结果
        for(ScoreDoc doc : topDocs.scoreDocs){
            Document document = searcher.doc(doc.doc);
            System.out.println(document.get("filepath"));
        }
        searcher.close();
    }

{% include JB/setup %}