### dbf文件相关属性

> DBFField.FIELD_TYPE_D 表示Date型   
>
> DBFField.FIELD_TYPE_L 表示布尔型
>
> DBFField.FIELD_TYPE_N 表示数字       
>
> DBFField.FIELD_TYPE_C 表示字符串
>
> DBFField.FIELD_TYPE_F 表示浮点型   
>
> DBFField.FIELD_TYPE_M 不晓得,欢迎补充

<!-- more -->

jar自取
```
https://pan.baidu.com/s/15a1JxP90qNo4LwX7OHLBJQ 
提取码: 6aom
```

### 1. 定义DBF文件字段

```java
// 字段数组
DBFField[] fields = new DBFField[列的个数];
// 表头---数组
String[] strutName = new String[列的个数]
// 数据类型 -- 数组
byte[] strutType = new byte[列的个数];
// 设置名称
fields[i].setName(strutName[i]);
// 设置类型
fields[i].setDataType(strutType[i]);

// 根据数据类型和数据本身指定数据长度(日期类型一般不指定长度)
fields[i].setFieldLength(15);
// 如果数据类型是浮点型 需要制定小数点位数 2 -->  0.00
fields[i].setDecimalCount(2);
// 如果数据类型是数值型不需要指定小数点位数, 会直接四舍五入
```

### 2. 逐条的写入记录

   ```java
Object[] rowData = new Object[列的个数];
// 如果要插入的数据是数字 但是为null  需要指定0 不可以使用空字符串和null
rowData[z] = Double.parseDouble("0");
   ```

### 3. 输出文件

  ```java
// 定义输出流，并关联的一个文件
// 这里需要注意的是,输出dbf是在已经存在的dbf文件中覆盖写入数据.所以在定义输出流的时候必须要指定一个存在dbf文件的路径并且指定名称.
File rootFile = new File(Play.applicationPath, "/public/download");
OutputStream fos = new FileOutputStream(rootFile.getPath()+文件名称.dbf);

// 定义 DBF输出对象
DBFWriter writer = new DBFWriter();
writer.setFields(fields);
writer.addRecord(rowData);
writer.write(fos);
  ```

### 4. 代码参考

```java
// 以下为系统中的代码, 仅供参考
public boolean outputDBF(List<PropertyFilter> propertyFilterList) {
        OutputStream fos = null;
        try {
            List<Object> query = departinvoicesManagerDao.queryDBFinfo(propertyFilterList);
            String dbfName = new String("结算单管理.dbf");
            StringBuffer sb = new StringBuffer();
            String[] strutName = sb.append("JSRS,JSRQ,JSDH,GSDM,DWDM,DWMC,CH,JPSR,JSSR,XLF,TPF,CC,FJF,GLQF,RYFJF")
                    .toString().split(",");
            byte[] strutType = new byte[15];
            strutType[0] = DBFField.FIELD_TYPE_N;// JSRS
            strutType[1] = DBFField.FIELD_TYPE_D;// JSRQ
            strutType[2] = DBFField.FIELD_TYPE_C;// JSDH
            strutType[3] = DBFField.FIELD_TYPE_C;// GSDM
            strutType[4] = DBFField.FIELD_TYPE_C;// DWDM
            strutType[5] = DBFField.FIELD_TYPE_C;// DWMC
            strutType[6] = DBFField.FIELD_TYPE_C;// CH
            strutType[7] = DBFField.FIELD_TYPE_F;// JPSR
            strutType[8] = DBFField.FIELD_TYPE_F;// JSSR
            strutType[9] = DBFField.FIELD_TYPE_F;// XLF
            strutType[10] = DBFField.FIELD_TYPE_F;// TPF
            strutType[11] = DBFField.FIELD_TYPE_C;// CC
            strutType[12] = DBFField.FIELD_TYPE_F;// FJF
            strutType[13] = DBFField.FIELD_TYPE_F;// GLQF
            strutType[14] = DBFField.FIELD_TYPE_F;// RYFJF
            // 定义DBF文件字段
            DBFField[] fields = new DBFField[15];

            for (int i = 0; i <= 14; i++) {
                fields[i] = new DBFField();
                fields[i].setName(strutName[i]);
                fields[i].setDataType(strutType[i]);
                // Date类型不能设置字段长度，这里没有处理其它没有字段长度的类型
                if (strutType[i] == DBFField.FIELD_TYPE_C) {
                    fields[i].setFieldLength(20);
                } else if (strutType[i] == DBFField.FIELD_TYPE_N) {
                    fields[i].setFieldLength(15);
                } else if (strutType[i] == DBFField.FIELD_TYPE_F) {
                    fields[i].setFieldLength(15);
                    fields[i].setDecimalCount(2);
                }
            }
            DBFWriter writer = new DBFWriter();
            writer.setFields(fields);
            // 一条条的写入记录
            for (int j = 0; j < query.size(); j++) {
                Object[] object = (Object[]) query.get(j);
                Object[] rowData = new Object[15];
                for (int z = 0; z < 15; z++) {
                    if (fields[z].getDataType() == DBFField.FIELD_TYPE_N || fields[z].getDataType() == DBFField.FIELD_TYPE_F) {
                        if (object[z] != null) {
                            rowData[z] = Double.parseDouble(object[z].toString());
                        } else {
                            rowData[z] = Double.parseDouble("0");
                        }
                    } else {
                        rowData[z] = object[z];
                    }
                }
                writer.addRecord(rowData);
            }
            // 定义输出流，并关联的一个文件
            File rootFile = new File(Play.applicationPath, "/public/download");
            fos = new FileOutputStream(rootFile.getPath() + "\\" + dbfName);
            // 写入数据
            writer.write(fos);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                fos.close();
            } catch (Exception e) {
            }
        }
        return true;
    }     
```

