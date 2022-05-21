title: 解决MD5遇到的bug
author: lwl
tags:
  - MD5
categories:
  - 加密
my: post/MD5bug
cover: https://fastly.jsdelivr.net/gh/code-anan/image/src=http---qqpublic.qpic.cn-qq_public-0-0-3686373537-EA507FDE9F31C5CF532960603C844A0E-0-fmt=jpg&size=137&h=600&w=900&ppv=1.jpg&refer=http---qqpublic.qpic.jpg
---

# 前言

最近公司做的一个功能有人反馈有问题，仔细看了看代码打断点才发现原来是md5加密的bug后来看了[这篇文章](https://blog.csdn.net/liaoxiaoyi121121/article/details/80408479)才解决，为了更直观我把代码抽离出来方便演示

# 出现bug的原因

```java
    public static void main(String[] args) throws Exception {
        String password="300655";
        String md5 = getMD5(password);
        System.out.println(getMD5(md5));
    }
    public static String getMD5(String str) throws Exception {
        // 生成一个MD5加密计算摘要
        MessageDigest md = MessageDigest.getInstance("MD5");
        // 计算md5函数
        md.update(str.getBytes());

        return new BigInteger(1, md.digest()).toString(16);
    }
```

这种方式应该是常见的写法了，可是问题是计算出md5转化为String类型的时候，如果md5的结果是0开头的，那么你会发现开头的0被抛弃了，那么校验的时候一定会报错，下面我用个例子演示，公司使用md5一般都是两次加密，比如我这里使用`300655`这个值进行加密，在线[加解密网站](https://www.cmd5.com/hash.aspx)上测试的结果如下![](https://fastly.jsdelivr.net/gh/code-anan/image/20220106143551.png)

这个表示加密两次的结果

而用上面的代码测试出来的结果为![](https://fastly.jsdelivr.net/gh/code-anan/image/20220106143804.png)

可以看到结果是不一样的

# 解决方案

使用下面这种方式即可解决

```java
    public static void main(String[] args) throws Exception {
        String password = "300655";
        String md5 = getMD5(password);
        System.out.println(getMD5(md5));
    }

    public static String getMD5(String str) throws Exception {
        // 生成一个MD5加密计算摘要
        MessageDigest md = MessageDigest.getInstance("MD5");
        // 计算md5函数
        md.update(str.getBytes());
        return toHexString(md.digest());
    }

    private static String toHexString(byte[] bytes) {
        Formatter formatter = new Formatter();
        for (byte b : bytes) {
            formatter.format("%02x", b);
        }
        String res = formatter.toString();
        formatter.close();
        return res;
    }
```

原理就是利用`formatter`将字节一个个的转为十六进制的形式即可，可以看到这次的结果为![](https://fastly.jsdelivr.net/gh/code-anan/image/20220106144148.png)

大功告成！(￣▽￣)~*