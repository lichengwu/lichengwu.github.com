---
layout: post
title: "Lucene 实战：快速开始 创建索引"
description: ""
category: java
subhead: ''
tags: [lucene,  java, index, framework]

---

    import java.io.File;
    import java.io.FileNotFoundException;
    import java.io.FileReader;
    import java.io.IOException;
    import org.apache.lucene.analysis.Analyzer;
    import org.apache.lucene.analysis.standard.StandardAnalyzer;
    import org.apache.lucene.document.Document;
    import org.apache.lucene.document.Field;
    import org.apache.lucene.document.NumericField;
    import org.apache.lucene.index.IndexWriter;
    import org.apache.lucene.index.IndexWriterConfig;
    import org.apache.lucene.store.Directory;
    import org.apache.lucene.store.FSDirectory;
    import org.apache.lucene.util.Version;
    import org.junit.Test;
    /**
     * 创建索引
     *
     * @author Oliver
     *
     */
    public class Index {
        private static String indexPath = System.getProperty("user.dir") + "\\index";
        private static String dataPath = System.getProperty("user.dir") + "\\data";
        @Test
        public void createIndex() throws IOException {
            // 创建分词器
            Analyzer analyzer = new StandardAnalyzer(Version.LUCENE_34);
            // 根据文件路径创建索引库
            Directory dir = FSDirectory.open(new File(indexPath));
            // 创建IndexWriter
            IndexWriterConfig conf = new IndexWriterConfig(Version.LUCENE_34, analyzer);
            IndexWriter writer = new IndexWriter(dir, conf);
            // 数据文件
            File[] dataFile = new File(dataPath).listFiles();
            // 添加文件
            for (File file : dataFile) {
                writer.addDocument(getDocument(file));
            }
            // 关闭
            writer.close();
        }
        /**
         * 添加文件
         *
         * @param file
         * @return
         * @throws FileNotFoundException
         */
        private Document getDocument(File file) throws FileNotFoundException {
            Document doc = new Document();
            //文件内容
            doc.add(new Field("content", new FileReader(file)));
            //文件名字
            doc.add(new Field("filename", file.getName(), Field.Store.YES, Field.Index.NOT_ANALYZED));
            //文件路径
            doc.add(new Field("filepath", file.getAbsolutePath(), Field.Store.YES,
                    Field.Index.NOT_ANALYZED));
            //文件大小
            doc.add( new NumericField("size").setLongValue(file.length()));
            return doc;
        }
    }


{% include JB/setup %}