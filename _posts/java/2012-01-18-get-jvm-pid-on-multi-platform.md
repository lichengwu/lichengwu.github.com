---
layout: post
catalog: true
title: "跨平台获取java进程id(Process ID in Java)"
description: ""
category: java
subhead: ''
tags: [java,  pid,  jvm,  getpids]

---

对于不同平台，获取java进程id有不同的方法，这个做一个总结，写一个工具类。

这个工具主要进行两种尝试来获得pid：

* 从 java.lang.management.RuntimeMXBean获得
* 从操作系统获得
* windows系统
* 非windows系统

工具代码：

    /**
     * Process ID in Java
     *
     * @author lichengwu
     * @created 2012-1-18
     *
     * @version 1.0
     */
    public final class PID {

        private static final Log log = LogFactory.getLog(PID.class);

        /**
         * 私有构造方法
         */
        private PID() {
            super();
        }

        /**
         * 获得java进程id
         *
         * @author lichengwu
         * @created 2012-1-18
         *
         * @return java进程id
         */
        public static final String getPID() {
            String pid = System.getProperty("pid");
            if (pid == null) {
                RuntimeMXBean runtimeMXBean = ManagementFactory.getRuntimeMXBean();
                String processName = runtimeMXBean.getName();
                if (processName.indexOf('@') != -1) {
                    pid = processName.substring(0, processName.indexOf('@'));
                } else {
                    pid = getPIDFromOS();
                }
                System.setProperty("pid", pid);
            }
            return pid;
        }

        /**
         * 从操作系统获得pid
         * <p>
         * 对于windows，请参考:http://www.scheibli.com/projects/getpids/index.html
         *
         * @author lichengwu
         * @created 2012-1-18
         *
         * @return
         */
        private static String getPIDFromOS() {

            String pid = null;

            String[] cmd = null;

            File tempFile = null;

            String osName = ParameterUtil.getParameter(Parameter.OS_NAME);
            // 处理windows
            if (osName.toLowerCase().contains("windows")) {
                FileInputStream fis = null;
                FileOutputStream fos = null;

                try {
                    // 创建临时getpids.exe文件
                    tempFile = File.createTempFile("getpids", ".exe");
                    File getpids = new File(ParameterUtil.getResourcePath("getpids.exe"));
                    fis = new FileInputStream(getpids);
                    fos = new FileOutputStream(tempFile);
                    byte[] buf = new byte[1024];
                    while (fis.read(buf) != -1) {
                        fos.write(buf);
                    }
                    // 获得临时getpids.exe文件路径作为命令
                    cmd = new String[] { tempFile.getAbsolutePath() };
                } catch (FileNotFoundException e) {
                    log.equals(e);
                } catch (IOException e) {
                    log.equals(e);
                } finally {
                    if (tempFile != null) {
                        tempFile.deleteOnExit();
                    }
                    Closer.close(fis, fos);
                }
            }
            // 处理非windows
            else {
                cmd = new String[] { "/bin/sh", "-c", "echo $$ $PPID" };
            }
            InputStream is = null;
            ByteArrayOutputStream baos = null;
            try {
                byte[] buf = new byte[1024];
                Process exec = Runtime.getRuntime().exec(cmd);
                is = exec.getInputStream();
                baos = new ByteArrayOutputStream();
                while (is.read(buf) != -1) {
                    baos.write(buf);
                }
                String ppids = baos.toString();
                // 对于windows参考：http://www.scheibli.com/projects/getpids/index.html
                pid = ppids.split(" ")[1];
            } catch (Exception e) {
                log.error(e);
            } finally {
                if (tempFile != null) {
                    tempFile.deleteOnExit();
                }
                Closer.close(is, baos);
            }
            return pid;
        }
    }

