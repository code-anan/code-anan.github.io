title: Excel文件的导入和导出
author: lwl
cover: https://fastly.jsdelivr.net/gh/code-anan/image/20220222164321.png
tags:
  - Excel
  - Hutool
categories:
  - Hutool
my: post/excelUtil
---

# 前言

作为java开发，excel文件的上传和导出应该说是必须要掌握的技能了，但是如果之前没做过的话还是会有一定的难度，这里我记录一下这两个工具的轮子方便以后直接拿过来用

# Excel文件上传和读取

这里我是默认使用springboot项目进行演示，因为springboot方便，如果不是springboot项目也只需要稍作修改即可

## 必要的依赖导入

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.7.21</version>
        </dependency>

        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
            <version>4.1.2</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.7</version>
        </dependency>
```

都是常规的依赖 没什么好说的了

## 前端页面

这里我们就简单写一个input组件和导出的超链接模拟就行

```html
<!DOCTYPE html>
<html >
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
<form action="/excel" method="post" enctype="multipart/form-data">
    <input type="file" id="file" name="file">
    <button type="submit">
        上传
    </button>
</form>

<a href="/export">导出excel</a>
</body>
<script type="text/javascript">
</script>
</html>
```

需要注意的是我们上传文件中的form标签必须要有`enctype="multipart/form-data"`属性，这个作用是设置请求的`content-type`;并且请求方式为post否则后端无法正常接收

## Controller代码

```java
@Autowired
    private ExcelService excelService;

    @RequestMapping(value = "/excel",method = RequestMethod.POST)
    public Map<String, Object> getexcel(@RequestParam("file") MultipartFile file){
        return excelService.readExcelByHutool(file);
        //return excelService.importProfession(file);
    }
```

下面被注释掉的代码是另外一种读取excel文件的方式，两种都可以看看，上面的是使用`Hutool`工具类的方式

## service代码

```java
@Service
public class ExcelService {
    //读取Excel文件(第二种方式)
    public Map<String, Object> importProfession(MultipartFile file) {
        Map<String,Object> map = new HashMap<>();
        //获取文件的名称
        String fileName = file.getOriginalFilename();
        System.out.println(fileName);
        //获取文件的后缀名
        String pattern = fileName.substring(fileName.lastIndexOf(".")+1);
        System.out.println(pattern);
        List<List<String>> listContent = new ArrayList<>();
        String message="导入成功";
        try {
            if (file != null) {
                //文件类型判断
                if (!OtherExcelUtil.isEXCEL(file)) {
                    message = "该文件不是Excel文件";
                } else {
                    listContent = OtherExcelUtil.readExcelContents(file,pattern);
                    //文件内容判断
                    if(listContent.isEmpty()){
                        message="表格内容为空";
                    }else {
                        //循环遍历
                        for (int i = 0; i<listContent.size(); i++){

                            //读取excel表格中的内容
                            String theme  = listContent.get(i).get(0);
                            String predicate  = listContent.get(i).get(1);
                            String properties  = listContent.get(i).get(2);
                            //赋值
                            System.out.println(theme);
                            System.out.println(predicate);
                            System.out.println(properties);
                            //插入数据

                        }
                    }
                }
            }else {
                message = "未选择文件";
            }
        }catch (Exception e){
            e.printStackTrace();
        }
        map.put("code",200);
        map.put("msg",message);
        map.put("data",fileName);

        return map;
    }


    //读取excel文件使用Hutool工具类
    public Map<String, Object> readExcelByHutool(MultipartFile file) {
        InputStream inputStream=null;
        try{
            inputStream = file.getInputStream();
        }catch (Exception e){
            e.printStackTrace();
        }
        ExcelReader excelReader = ExcelUtil.getReader(inputStream);
        List<List<Object>> read = excelReader.read(1, excelReader.getRowCount());
        ArrayList<Student> studentArrayList = new ArrayList<>();
        int count=read.size();
        for (int i = 0; i <count ; i++) {
            Student student = new Student();
            student.setName((String) read.get(i).get(0));
            Object o =read.get(i).get(1);
            student.setAge(Integer.valueOf(String.valueOf(o)));
            student.setEmail((String) read.get(i).get(2));
            studentArrayList.add(student);
            //也可以在这里执行插入数据库的操作 这里我们只简单输出一下
        }
        System.out.println(studentArrayList);
        Map<String,Object> map = new HashMap<>();
        map.put("code",200);
        map.put("msg","导入成功");
        map.put("data",file.getOriginalFilename());
        return map;
    }
}
```

需要注意的是，如果你使用的`excelService.importProfession(file);`这种方式需要另外添加一个工具类（下方的Util工具类）

## Util工具类

```java
public class OtherExcelUtil {

    public static Workbook readExcel(MultipartFile file, String pattern) {
        //文档对象
        Workbook workbook = null;
        if (file != null) {
            try {
                //获取输入流
                InputStream is = file.getInputStream();
                if ("xls".equals(pattern)) {
                    //2003版格式 -xls
                    return workbook = new HSSFWorkbook(is);
                } else if ("xlsx".equals(pattern)) {
                    //2007及以上版本 -xlsx
                    return workbook = new XSSFWorkbook(is);
                } else
                    return null;
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return workbook;
    }

    public static Boolean isEXCEL(MultipartFile file) {
        if (file != null) {
            //文件名
            String fileName = file.getOriginalFilename();
            //文件后缀
            String suffix = fileName.substring(fileName.lastIndexOf(".") + 1);
            //转小写
            suffix = suffix.toLowerCase();
            if ("xls".equals(suffix) || "xlsx".equals(suffix)) {
                return true;
            }
        }
        return false;
    }


    public static List<List<String>> readExcelContents(MultipartFile file, String pattern) {
        List<List<String>> listRow = new ArrayList<>();
        //文档对象
        Workbook workbook = null;
        //表格对象
        Sheet sheet = null;
        //非空和文件格式判断
        if (isEXCEL(file)) {
            workbook = readExcel(file, pattern);
        }
        if (workbook != null) {
            //获取文档首个表格
            sheet = workbook.getSheetAt(0);
            //获取最大行数
            int rowNum = sheet.getPhysicalNumberOfRows();
            //行对象
            Row row = null;
            //单元格数据
            String cellData = null;
            //跳过第一行标题栏
            for (int i = 1; i < rowNum; i++) {
                row = sheet.getRow(i);
                List<String> listCell = new ArrayList<>();
                if (StringUtils.isBlank(row.getCell(0).toString())) {
                    break;
                }
                for (int j = 0; j < 3; j++) {
                    cellData = row.getCell(j).toString();
                    listCell.add(cellData);
                }
                listRow.add(listCell);
            }
        }
        return listRow;
    }
}
```

这个工具类是使用第二种方式所必须的，如果使用第一种方式我们有我们引入的hutool依赖就足够了

## model实体类

我们习惯把每一行的数据封装成一个实体类，这样读取数据之后方便赋值观察上面代码可以看到我使用的例子为student类，以后使用的话要看你需要什么类

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Student {
    private String name;
    private Integer age;
    private String email;
}
```

## 演示

效果演示，这里我就使用第一种方式（当然结果都是一样的）![](https://fastly.jsdelivr.net/gh/code-anan/image/20220222144431.png)

点击上传可以看到控制台输出了结果`[Student(name=皮卡丘, age=4, email=719603766@qq.com), Student(name=妙蛙种子, age=3, email=123445@qq.com)]`

# Excel文件导出

代码不多，我们这里直接就写在controller中即可

```java
@RequestMapping(value = "/export",method = RequestMethod.GET)
    @ResponseBody
    public void export(HttpServletResponse response){

        List<Student> list = new ArrayList<>();
        list.add(new Student("张三",14,"719603766@qq.com"));
        list.add(new Student("张四",14,"719603766@qq.com"));
        list.add(new Student("张五",15,"719603766@qq.com"));
        list.add(new Student("张六",16,"719603766@qq.com"));
        list.add(new Student("张七",17,"719603766@qq.com"));

        // 通过工具类创建writer，默认创建xls格式
        ExcelWriter writer = ExcelUtil.getWriter();
        //自定义标题别名
        writer.addHeaderAlias("name", "姓名");
        writer.addHeaderAlias("age", "年龄");
        writer.addHeaderAlias("email", "邮件");
        // 合并单元格后的标题行，使用默认标题样式
        writer.merge(2, "学生信息单");
        // 一次性写出内容，使用默认样式，强制输出标题
        writer.write(list, true);
        //out为OutputStream，需要写出到的目标流
        //response为HttpServletResponse对象
        response.setContentType("application/vnd.ms-excel;charset=utf-8");
        //test.xls是弹出下载对话框的文件名，不能为中文，中文请自行编码
        SimpleDateFormat format = new SimpleDateFormat("YYYY_mm_dd");
        String name ="student_"+format.format(new Date());
        response.setHeader("Content-Disposition","attachment;filename="+name+".xls");
        ServletOutputStream out= null;
        try {
            out = response.getOutputStream();
            writer.flush(out, true);
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            // 关闭writer，释放内存
            writer.close();
        }
        //此处记得关闭输出Servlet流
        IoUtil.close(out);
    }
```

这里我们模拟了数据，实际开发一般会在数据库中获取；注释非常详细了，ok大功告成以后可以直接拿来用了(*^▽^*)