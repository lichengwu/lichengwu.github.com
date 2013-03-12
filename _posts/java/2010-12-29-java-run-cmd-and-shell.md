---
layout: post
title: "java执行命令(cmd,shell)"
description: ""
category: java
subhead: ''
tags: [java,  linux,  shell,  cmd]

---
一个简单的小工具，用java执行系统命令，并打印输出。

    public class OSExecute {
        /**
         * <b>command。</b>
         * <p>
         * <b>详细说明：</b>
         * </p>
         * <!-- 在此添加详细说明 --> 无。
         * 
         * @param command
         */
        public static void command(String command) {
            try {
                Process process = new ProcessBuilder(Arrays.asList(command.split(" "))).start();
                // 标准输入流
                BufferedReader result = new BufferedReader(new InputStreamReader(
                        process.getInputStream()));
                String s = result.readLine();
                while (s != null) {
                    System.out.println(s);
                    s = result.readLine();
                }
                // 标准错误输入流
                BufferedReader error = new BufferedReader(new InputStreamReader(
                        process.getErrorStream()));
                s = error.readLine();
                while (s != null) {
                    System.err.println(s);
                    s = error.readLine();
                }
            } catch (Exception e) {
                // 纠正
                if (!command.startsWith("CMD /C")) {
                    command("CMD /C " + command);
                } else {
                    throw new RuntimeException(e.getMessage());
                }
            }
        }

        public static void main(String[] args) {
            OSExecute.command("dir");
        }
    }

测试ls（windows下测试）,命令的结果：

    驱动器 E 中的卷是 Doc
    卷的序列号是 B411-2480

    E:/workspace/java/ThinkInJava/book 的目录

    2010/12/28 20:30 <DIR> .
    2010/12/28 20:30 <DIR> ..
    2010/12/28 19:31 518 .classpath
    2010/08/29 08:59 380 .project
    2010/12/28 17:09 <DIR> .settings
    2010/12/28 19:57 <DIR> bin
    2010/12/28 20:32 100 data.txt
    2010/08/29 09:02 <DIR> source
    2010/12/28 19:55 <DIR> src
    2010/08/29 09:14 <DIR> test
    3 个文件 998 字节
    7 个目录 13,018,263,552 可用字节

{% include JB/setup %}