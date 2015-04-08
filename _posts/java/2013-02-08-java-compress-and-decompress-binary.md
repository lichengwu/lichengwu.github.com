---
layout: post
title: "Java压缩/解压缩二进制文件"
description: ""
category: java
subhead: ''
tags: [java, decompress, compress]

---

在Java中提供[Deflater](http://docs.oracle.com/javase/7/docs/api/java/util/zip/Deflater.html)和[Inflater](http://docs.oracle.com/javase/7/docs/api/java/util/zip/Inflater.html)工具类来压缩/解压缩数据。
这两个工具类采用[zlib](http://en.wikipedia.org/wiki/Zlib)算法，下面给出一个封装好的工具。

    /**
     * util for compress/decompress data
     * 
     * @author lichengwu
     * @version 1.0
     * @created 2013-02-07 10:14 AM
     */
    public final class CompressionUtil {
    
        private static final int BUFFER_SIZE = 4 * 1024;
    
        /**
         * compress data by {@linkplain Level}
         * 
         * @author lichengwu
         * @created 2013-02-07
         * 
         * @param data
         * @param level
         *            see {@link Level}
         * @return
         * @throws IOException
         */
        public static byte[] compress(byte[] data, Level level) throws IOException {
    
            Assert.notNull(data);
            Assert.notNull(level);
    
            Deflater deflater = new Deflater();
            // set compression level
            deflater.setLevel(level.getLevel());
            deflater.setInput(data);
    
            ByteArrayOutputStream outputStream = new ByteArrayOutputStream(data.length);
    
            deflater.finish();
            byte[] buffer = new byte[BUFFER_SIZE];
            while (!deflater.finished()) {
                int count = deflater.deflate(buffer); // returns the generated
                                                      // code... index
                outputStream.write(buffer, 0, count);
            }
            byte[] output = outputStream.toByteArray();
    		outputStream.close();
            return output;
        }
    
        /**
         * decompress data
         * 
         * @author lichengwu
         * @created 2013-02-07
         * 
         * @param data
         * @return
         * @throws IOException
         * @throws DataFormatException
         */
        public static byte[] decompress(byte[] data) throws IOException, DataFormatException {
    
            Assert.notNull(data);
    
            Inflater inflater = new Inflater();
            inflater.setInput(data);
    
            ByteArrayOutputStream outputStream = new ByteArrayOutputStream(data.length);
            byte[] buffer = new byte[BUFFER_SIZE];
            while (!inflater.finished()) {
                int count = inflater.inflate(buffer);
                outputStream.write(buffer, 0, count);
            }
            byte[] output = outputStream.toByteArray();
    		outputStream.close();
            return output;
        }
    
        /**
         * Compression level
         */
        public static enum Level {
    
            /**
             * Compression level for no compression.
             */
            NO_COMPRESSION(0),
    
            /**
             * Compression level for fastest compression.
             */
            BEST_SPEED(1),
    
            /**
             * Compression level for best compression.
             */
            BEST_COMPRESSION(9),
    
            /**
             * Default compression level.
             */
            DEFAULT_COMPRESSION(-1);
    
            private int level;
    
            Level(
    
            int level) {
                this.level = level;
            }
            public int getLevel() {
                return level;
            }
        }  
    }
    
下面是一个测试：

    @Test
    public void testCompress() throws Exception {

        BufferedInputStream in = new BufferedInputStream(new FileInputStream(
                "/Users/lichengwu/tmp/out_put.txt.bak"));
        ByteArrayOutputStream out = new ByteArrayOutputStream(1024);

        byte[] temp = new byte[1024];
        int size = 0;
        while ((size = in.read(temp)) != -1) {
            out.write(temp, 0, size);
        }
        in.close();
        byte[] data = out.toByteArray();
        byte[] output = CompressionUtil.compress(data, CompressionUtil.Level.BEST_COMPRESSION);
        System.out.println("before : " + (data.length / 1024) + "k");
        System.out.println("after : " + (output.length / 1024) + "k");

        FileOutputStream fos = new FileOutputStream("/Users/lichengwu/tmp/out_put.txt.bak.compress");
        fos.write(output);
        out.close();
        fos.close();
    }

    @Test
    public void testDecompress() throws Exception {

        BufferedInputStream in = new BufferedInputStream(new FileInputStream(
                "/Users/lichengwu/tmp/out_put.txt.bak.compress"));
        ByteArrayOutputStream out = new ByteArrayOutputStream(1024);
        byte[] temp = new byte[1024];
        int size = 0;
        while ((size = in.read(temp)) != -1) {
            out.write(temp, 0, size);
        }
        in.close();
        byte[] data = out.toByteArray();
        byte[] output = CompressionUtil.decompress(data);
        System.out.println("before : " + (data.length / 1024) + "k");
        System.out.println("after : " + (output.length / 1024) + "k");

        FileOutputStream fos = new FileOutputStream("/Users/lichengwu/tmp/out_put.txt.bak.decompress");
        fos.write(output);
        out.close();
        fos.close();
    }
    
关于OutOfMemoryError，请参考：<http://www.devguli.com/blog/eng/java-deflater-and-outofmemoryerror/>    