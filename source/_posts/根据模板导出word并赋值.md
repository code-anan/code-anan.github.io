title: 根据模板导出word并赋值
author: lwl
cover: https://gcore.jsdelivr.net/gh/code-anan/image/20220803154716.png

my: post/exportWord

date: 2022-08-03 16:34

categories: 
  - 记录

tags:
  - word
  - Mybatis
  - SpringBoot
------

# 前言

不知道大家有没有这样的需求，就是需要导出一个word按照我们的模板并在相应的地方填上对应的值，正好最近又这样的需求，这里记录一下轮子的使用代码，方便以后使用，下面直接上代码

# 目录结构

这里我使用的还是最普通的SpringBoot项目结构，结构如下

![image-20220803144853321](https://gcore.jsdelivr.net/gh/code-anan/image/20220803144900.png)

模板带参为我们的模板代码，并且使用`{{}}`来占位我们要填充的值，如下![image-20220803145129208](https://gcore.jsdelivr.net/gh/code-anan/image/20220803145129.png)

比如我这里就只是要填这几个值，大家根据自己的需求更改

# 需要的依赖

```xml
<dependency>
            <groupId>cn.afterturn</groupId>
            <artifactId>easypoi-base</artifactId>
            <version>3.0.3</version>
        </dependency>
        <dependency>
            <groupId>cn.afterturn</groupId>
            <artifactId>easypoi-web</artifactId>
            <version>3.0.3</version>
        </dependency>
        <dependency>
            <groupId>cn.afterturn</groupId>
            <artifactId>easypoi-annotation</artifactId>
            <version>3.0.3</version>
        </dependency>

        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.2.0</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
```

这里我使用了mysql数据库和mybatis所以加上了他们的依赖

# 业务代码

## Controller层

```java
@Controller
public class MyController {

    @Autowired
    private MedicalRecordService medicalRecordService;

    @RequestMapping("/download")
    public void downloadMedicalRecord(String fixcode,HttpServletRequest request, HttpServletResponse response) {
        try {
            medicalRecordService.downloadMedicalRecord(fixcode,request,response);
        } catch (Exception e) {
            //这里大家根据自己的需求处理异常
        }
    }
}
```

## service层

定义接口：

```java
public interface MedicalRecordService {
    void downloadMedicalRecord(String fixcode, HttpServletRequest request, HttpServletResponse response) throws IllegalAccessException, IOException;
}
```

实现类：

```java
@Service
public class MedicalRecordServiceImpl implements MedicalRecordService {

    @Autowired
    private MedicalDao medicalDao;
    @Override
    public void downloadMedicalRecord(String fixcode,HttpServletRequest request, HttpServletResponse response){
        Institution record = medicalDao.selectInsByFixCode(fixcode);
        Map<String,Object> params = new HashMap<>();
        params.put("fixedName",record.getFixedName());
        params.put("fixCode",record.getFixCode());
        params.put("appId",record.getAppId());
        params.put("appKey",record.getAppKey());
        params.put("appPrvkey",record.getAppPrvkey());
        params.put("platPubKey",record.getPlatPubKey());
        File file = new File("./模板带参.docx");
        ExportWord.exportWord(file,"C:\\Users\\admin\\Desktop\\","测试信息反馈表（"+record.getFixedName()+").docx",params,request,response);
    }
}
```

## Dao层

接口：

```java
@Mapper
public interface MedicalDao {
    Institution selectInsByFixCode(String fixcode);
}
```

对应的xml(需要在配置文件中设置的路径下)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.basic.dao.MedicalDao">

    <select id="selectInsByFixCode" resultType="Institution">
        select fixCode,fixedName,appId,appKey,appPrvkey,platPubKey from institution where fixCode=#{fixcode}
    </select>
</mapper>
```

## 实体类

```java
public class Institution {
    private String fixedName;
    private String fixCode;
    private String appId;
    private String appKey;
    private String appPrvkey;
    private String platPubKey;

    public String getFixedName() {
        return fixedName;
    }

    public void setFixedName(String fixedName) {
        this.fixedName = fixedName;
    }

    public String getFixCode() {
        return fixCode;
    }

    public void setFixCode(String fixCode) {
        this.fixCode = fixCode;
    }

    public String getAppId() {
        return appId;
    }

    public void setAppId(String appId) {
        this.appId = appId;
    }

    public String getAppKey() {
        return appKey;
    }

    public void setAppKey(String appKey) {
        this.appKey = appKey;
    }

    public String getAppPrvkey() {
        return appPrvkey;
    }

    public void setAppPrvkey(String appPrvkey) {
        this.appPrvkey = appPrvkey;
    }

    public String getPlatPubKey() {
        return platPubKey;
    }

    public void setPlatPubKey(String platPubKey) {
        this.platPubKey = platPubKey;
    }
}
```

实体类的属性一般都和数据库保持一致

## 工具类

```java
import cn.afterturn.easypoi.word.WordExportUtil;
import org.apache.poi.xwpf.usermodel.XWPFDocument;
import org.springframework.util.Assert;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;
import java.net.URLEncoder;
import java.util.Map;
public class ExportWord {
    /**
     * 导出word
     * <p>第一步生成替换后的word文件，只支持docx</p>
     * <p>第二步下载生成的文件</p>
     * <p>第三步删除生成的临时文件</p>
     * 模版变量中变量格式：{{foo}}
     * @param templatePath word模板地址
     * @param temDir 生成临时文件存放地址
     * @param fileName 文件名
     * @param params 替换的参数
     * @param request HttpServletRequest
     * @param response HttpServletResponse
     */
    public static void exportWord(File templatePath, String temDir, String fileName, Map<String, Object> params, HttpServletRequest request, HttpServletResponse response) {
        Assert.notNull(templatePath,"file");
        Assert.notNull(temDir,"static");
        Assert.notNull(fileName,"导出文件名不能为空");
        Assert.isTrue(fileName.endsWith(".docx"),"word导出请使用docx格式");
        if (!temDir.endsWith("/")){
            temDir = temDir + File.separator;
        }
        File dir = new File(temDir);
        if (!dir.exists()) {
            dir.mkdirs();
        }
        try {
            String userAgent = request.getHeader("user-agent").toLowerCase();
            if (userAgent.contains("msie") || userAgent.contains("like gecko")) {
                fileName = URLEncoder.encode(fileName, "UTF-8");
            } else {
                fileName = new String(fileName.getBytes("utf-8"), "ISO-8859-1");
            }
            XWPFDocument doc = WordExportUtil.exportWord07(String.valueOf(templatePath), params);
            String tmpPath = temDir + fileName;
            FileOutputStream fos = new FileOutputStream(tmpPath);
            doc.write(fos);
            // 设置强制下载不打开
            response.setContentType("application/force-download");
            // 设置文件名
            response.addHeader("Content-Disposition", "attachment;fileName=" + fileName);
            OutputStream out = response.getOutputStream();
            doc.write(out);
            out.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        finally {
            delFileWord(temDir,fileName);//这一步看具体需求，要不要删
        }
    }
    /**
     * 删除临时生成的文件
     */
    public static void delFileWord(String filePath, String fileName){
        File file =new File(filePath+fileName);
        File file1 =new File(filePath);
        file.delete();
        file1.delete();
    }
}
```

此功能实现的关键类

## 配置文件

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=233
spring.datasource.url=jdbc:mysql://localhost:3306/test?serverTimezone=UTC
##声明mybatis实体类的别名以及mapper的位置
mybatis.type-aliases-package=com.example.basic.model
mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
```

# 测试

打开http://localhost:8080/download?fixcode=123

得到导出的word如下![](https://gcore.jsdelivr.net/gh/code-anan/image/20220803154253.png)

到这里就大功告成啦(*^▽^*)