## 依赖配置 ##

```xml
<repositories>
  <repository>
    <id>lychie-maven-repo</id>
    <url>https://raw.github.com/lychie/maven-repo/master/releases</url>
  </repository>
</repositories>

<dependencies>
  <dependency>
    <groupId>org.lychie</groupId>
    <artifactId>jexcel</artifactId>
    <version>1.0.0</version>
  </dependency>
</dependencies>
```

## 读取EXCEL文档 ##

![persons.xlsx](http://i.imgur.com/tvlJlp9.png)

```java
package org.lychie.jexcel.demo;

import java.util.Date;
import java.util.List;
import org.lychie.jutil.IOUtil;
import org.lychie.jutil.Printer;
import org.lychie.jutil.DateUtil;
import org.lychie.jexcel.ReadableExcel;
import org.lychie.jexcel.validity.BasicValidation;

/**
 * 读取EXCEL文档
 * @date   2015-01-21
 * @author Lychie Fan
 */
public class ReadExcel {
	
	public static void main(String[] args) {
		//创建一个可读的EXCEL对象
		ReadableExcel excel = new ReadableExcel(Person.class);
		//设置POJO属性与EXCEL单元格的映射关系
		excel.setMapper("id", "编号");
		excel.setMapper("age", "年龄");
		excel.setMapper("sex", "性别");
		excel.setMapper("name", "姓名");
		excel.setMapper("date", "生日");
		//载入EXCEL文档
		excel.load(IOUtil.getResourceAsStream("persons.xlsx"));
		try {
			//如果有需要, 通过调这行代码校验EXCEL文档内容的合法性
			excel.validate(new BasicValidation());
		} catch (Throwable e) {
			e.printStackTrace();
			return ;
		}
		//解析EXCEL文档成集合
		List<Person> list = excel.toList();
		//打印输出集合的内容
		Printer.print(list);
	}

	public static class Person {
		
		private int id;
		private int age;
		private String sex;
		private String name;
		private Date date;

		public int getId() {
			return id;
		}

		public int getAge() {
			return age;
		}

		public String getSex() {
			return sex;
		}

		public String getName() {
			return name;
		}

		public void setId(int id) {
			this.id = id;
		}

		public void setAge(int age) {
			this.age = age;
		}

		public void setSex(String sex) {
			this.sex = sex;
		}

		public void setName(String name) {
			this.name = name;
		}

		public Date getDate() {
			return date;
		}

		public void setDate(Date date) {
			this.date = date;
		}

		@Override
		public String toString() {
			return id + "\t" + name + "\t" + sex + "\t" + age + "\t"
					+ DateUtil.format(date);
		}
		
	}
	
}
```

## 写出EXCEL文档 ##

```java
package org.lychie.jexcel.demo;

import java.io.File;
import java.util.Date;
import java.util.List;
import java.util.ArrayList;
import org.lychie.jutil.DateUtil;
import org.lychie.jutil.RandomUtil;
import org.lychie.jexcel.WritableExcel;

/**
 * 写出对象到EXCEL文档
 * @date   2015-1-21
 * @author Lychie Fan
 */
public class WriteExcel {
	
	public static void main(String[] args) {
		//创建一个可写的EXCEL对象
		WritableExcel excel = new WritableExcel(getData());
		//设置POJO属性与EXCEL单元格的映射关系
		excel.setMapper("id", "编号");
		excel.setMapper("age", "年龄");
		excel.setMapper("sex", "性别");
		excel.setMapper("name", "姓名");
		excel.setMapper("date", "生日");
		//将对象内容写出到EXCEL文档
		excel.write(new File("src/main/resources/myfriends.xlsx"));
	}
	
	private static List<Person> getData(){
		List<Person> list = new ArrayList<Person>();
		list.add(new Person("杨忠杰"));
		list.add(new Person("叶水燕"));
		list.add(new Person("杨晓婷"));
		list.add(new Person("叶国珠"));
		list.add(new Person("何国群"));
		return list;
	}

	public static class Person {
		
		private int id;
		private int age;
		private String sex;
		private String name;
		private Date date;
		
		public Person(String name){
			this.name = name;
			this.age = RandomUtil.intSeed(18, 28);
			this.id = RandomUtil.intSeed(1001, 1999);
			this.sex = RandomUtil.boolSeed() ? "男" : "女";
			this.date = DateUtil.parse("1990/09/" + RandomUtil.intSeed(1, 30));
		}

		public int getId() {
			return id;
		}

		public int getAge() {
			return age;
		}

		public String getSex() {
			return sex;
		}

		public String getName() {
			return name;
		}

		public void setId(int id) {
			this.id = id;
		}

		public void setAge(int age) {
			this.age = age;
		}

		public void setSex(String sex) {
			this.sex = sex;
		}

		public void setName(String name) {
			this.name = name;
		}

		public Date getDate() {
			return date;
		}

		public void setDate(Date date) {
			this.date = date;
		}

		@Override
		public String toString() {
			return id + "\t" + name + "\t" + sex + "\t" + age + "\t"
					+ DateUtil.format(date);
		}
		
	}

}
```

结果图：

![myfriends.xlsx](http://i.imgur.com/xoA6rTe.png)