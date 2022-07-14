title: Java实现pdf与base64编码的互转
author: lwl
cover: https://gcore.jsdelivr.net/gh/code-anan/image/src=http---digittaly.com-wp-content-uploads-2020-06-digifeaturedimg-1.png&refer=http---digittaly.jpg
tags:
  - base64
  - pdf
categories:
  - 记录
my: post/pdfToBase64
---

# 前言

最新刚好有这样的需求，需要实现pdf和base64编码的互转，所以在这里记录一套转换的代码模板方便以后使用，代码主要来源[孙悟空2015](https://blog.csdn.net/fuyuwei2015/article/details/47264007)然后稍微做了一下修改 可以直接使用

# base64编码概述

> Base64是网络上最常见的用于传输8Bit字节码的编码方式之一，Base64就是一种基于64个可打印字符来表示二进制数据的方法。可查看RFC2045～RFC2049，上面有MIME的详细规范。
Base64编码是从二进制到字符的过程，可用于在HTTP环境下传递较长的标识信息。采用Base64编码具有不可读性，需要解码后才能阅读。
Base64由于以上优点被广泛应用于计算机的各个领域，然而由于输出内容中包括两个以上“符号类”字符（+, /, =)，不同的应用场景又分别研制了Base64的各种“变种”。为统一和规范化Base64的输出，Base62x被视为无符号化的改进版本。

好吧 这些概述显然不容易理解 所以我把他简单理解为文件的字符串并且在网络中传输且文件大小大概增加三分之一  这里的文件不局限于pdf文件其他类型的文件也同样适用

#  PDF转base64的方法代码

```java
public static String PDFToBase64(File file) {
        BASE64Encoder encoder = new BASE64Encoder();
        FileInputStream fin =null;
        BufferedInputStream bin =null;
        ByteArrayOutputStream baos = null;
        BufferedOutputStream bout =null;
        try {
            fin = new FileInputStream(file);
            bin = new BufferedInputStream(fin);
            baos = new ByteArrayOutputStream();
            bout = new BufferedOutputStream(baos);
            byte[] buffer = new byte[1024];
            int len = bin.read(buffer);
            while(len != -1){
                bout.write(buffer, 0, len);
                len = bin.read(buffer);
            }
            //刷新此输出流并强制写出所有缓冲的输出字节
            bout.flush();
            byte[] bytes = baos.toByteArray();
            return encoder.encodeBuffer(bytes).trim();

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally{
            try {
                fin.close();
                bin.close();
                bout.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return null;
    }
```

# Base64编码转PDF文件

```java
public static void base64StringToPdf(String base64Content,String filePath){
        BASE64Decoder decoder = new BASE64Decoder();
        BufferedInputStream bis = null;
        FileOutputStream fos = null;
        BufferedOutputStream bos = null;

        try {
            byte[] bytes = decoder.decodeBuffer(base64Content);//base64编码内容转换为字节数组
            ByteArrayInputStream byteInputStream = new ByteArrayInputStream(bytes);
            bis = new BufferedInputStream(byteInputStream);
            File file = new File(filePath);
            File path = file.getParentFile();
            if(!path.exists()){
                path.mkdirs();
            }
            fos = new FileOutputStream(file);
            bos = new BufferedOutputStream(fos);

            byte[] buffer = new byte[1024];
            int length = bis.read(buffer);
            while(length != -1){
                bos.write(buffer, 0, length);
                length = bis.read(buffer);
            }
            bos.flush();
        } catch (Exception e) {
            e.printStackTrace();
        }finally{
            if(bis!=null){
                try {
                    bis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }else if(fos!=null){
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }else if(bos!=null){
                try {
                    bos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

# 示例

```java
public static void main(String[] args) {
        File file = new File("C:\\Users\\admin\\Desktop\\1.pdf");
        String base64 = PDFToBase64(file);
        base64StringToPdf(base64,"C:\\Users\\admin\\Desktop\\2.pdf");
    }
```

先指定一个PDF文件的路径然后将其转换成`base64`字符串，得到字符串之后调用`base64StringToPdf`方法传进去目标路径和base64编码即可得到生成的pdf文件

![](https://gcore.jsdelivr.net/gh/code-anan/image/20211211122515.png) 

![](https://gcore.jsdelivr.net/gh/code-anan/image/20211211122607.png) 

运行完命令 即可看到生成的pdf文件 大功告成

![](https://gcore.jsdelivr.net/gh/code-anan/image/20211211122743.png)
