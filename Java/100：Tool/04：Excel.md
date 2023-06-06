### Excel

------

[TOC]

##### 01：Excel 两种格式

- 文件格式不同。xls 是一个特有的二进制格式，其核心结构是复合文档类型的结构，而 xlsx 的核心结构是 XML 类型的结构，采用的是基于 XML 的压缩方式，使其占用的空间更小。xlsx 中最后一个 x 的意义就在于此；
- 版本不同。xls是excel2003及以前版本生成的文件格式，而xlsx是excel2007及以后版本生成的文件格式；
- 兼容性不同。xlsx格式是向下兼容的，可兼容xls格式；

##### 02：JXL 和 POI两种方式操作Excel

-  在小数据量时jxl快于poi，在大数据量时poi要快于jxl；

##### 03：POI 

1. 导入jar包
   1. 会自动导入

```xml
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>4.1.0</version>
</dependency>

<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>4.1.0</version>
</dependency>
```

2. 相关对象

   - HSSFWorkbook：xls 的 excel文档对象
   - XSSFWorkbook：xlsx 的 excel文档对象

3. 获取Excel对象

   ```java
   public static void processExcel(String path) {
     try {
       // 获取文件流
       InputStream input = new FileInputStream(path);
       // 读取xlsx文件
       Workbook workbook = new HSSFWorkbook(input);
       // 获取第一个工作表信息
       for (int index = 0; index < workbook.getNumberOfSheets(); index++) {
         Sheet sheet = workbook.getSheetAt(index);
         for (int rows = 0; rows < sheet.getPhysicalNumberOfRows(); rows++) {
           Row row = sheet.getRow(rows);
           for (int cols = 0; cols < row.getPhysicalNumberOfCells(); cols++) {
             System.out.print(row.getCell(cols) + ",");
           }
         }
       }
     } catch (IOException e) {
       System.out.println(e.getCause());
     }
   }
   ```
   
4. 新建Excel对象

   ```java
   public static void processExcel(String path) {
       // 创建工作薄对象
       // 这里也可以设置sheet的Name
       Workbook workbook = new XSSFWorkbook();
       // 创建工作表对象
       Sheet sheet = workbook.createSheet("test");
       // 创建工作表的行
       Row row = sheet.createRow(0);
       row.createCell(0).setCellValue("一行三列");
       // 文档输出
       try {
         OutputStream out = new FileOutputStream(path);
         workbook.write(out);
         out.close();
       } catch (IOException e) {
         e.printStackTrace();
       }
   }
   ```

5. ExcelUtils

   ```java
   import java.io.ByteArrayOutputStream;
   import java.io.IOException;
   import java.io.OutputStream;
   import java.util.List;
   
   import org.apache.poi.hssf.usermodel.HSSFCellStyle;
   import org.apache.poi.xssf.usermodel.XSSFCell;
   import org.apache.poi.xssf.usermodel.XSSFCellStyle;
   import org.apache.poi.xssf.usermodel.XSSFRow;
   import org.apache.poi.xssf.usermodel.XSSFSheet;
   import org.apache.poi.xssf.usermodel.XSSFWorkbook;
   
   /**
    * Excel 工具类
    *
    * @author kangkang.li@qunar.com
    * @date 2020-11-12 15:59
    */
   public class ExcelUtils {
       /**
        * 构键文件OutPutStream
        *
        * @param titles
        * @param data Sheets->rows->columns
        * @param sheetName
        * @return
        * @throws IOException
        */
       public static OutputStream buildExcelFileStream(List<List<String>> titles,
                                                       List<List<List<Object>>> data, 
                                                       List<String> sheetName) 
         throws IOException {
           // xlsx 的 excel文档对象
           XSSFWorkbook workbook = new XSSFWorkbook();
           // 创建单元格类型
           XSSFCellStyle style = workbook.createCellStyle();
           // 数据居中
           style.setAlignment(HSSFCellStyle.ALIGN_CENTER);
           for (int index = 0; index < titles.size(); index++) {
               buildExcelSheet(titles.get(index), data.get(index) , workbook, style, sheetName.get(index));
           }
           OutputStream outputStream = new ByteArrayOutputStream();
           workbook.write(outputStream);
           return outputStream;
       }
   
       /**
        * 构键Sheet，方便用于构键多个Sheet
        *
        * @param title
        * @param beans
        * @param workbook
        * @param style
        * @param sheetName
        * @return
        */
       private static void buildExcelSheet(List<String> title,
                                          	List<List<Object>> beans,
                                           XSSFWorkbook workbook,
                                           XSSFCellStyle style, String sheetName) {
           // 创建工作表对象
           XSSFSheet sheet = workbook.createSheet(sheetName);
           // 创建工作表的行
           XSSFRow row = sheet.createRow(0);
   
           // 循环建立表头
           for(int i = 0 ; i < title.size(); i++) {
               // 创建单元格
               XSSFCell cell = row.createCell(i);
               cell.setCellValue(title.get(i));
               cell.setCellStyle(style);
           }
           // 为每一行赋值
           for (int i = 0; i < beans.size(); i++) {
               row = sheet.createRow(i + 1);
   
               List<Object> list = beans.get(i);
               for(int j = 0; j < list.size(); j++) {
                   XSSFCell cell = row.createCell(j);
                   cell.setCellValue(String.valueOf(list.get(j)));
                   cell.setCellStyle(style);
               }
           }
       }
   }
   ```

   

