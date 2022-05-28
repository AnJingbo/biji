

什么是XML?
xml是可扩展的标记性语言。

xml的作用？
1、用来保存数据，而且这些数据具有自我描述性
2、它还可以作为项目或者模块的配置文件
3、还可以作为网络传输数据的格式(现在以 JSON 为主)

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!--
   上面是xml文件的声明
    version：表示xml的版本
	encoding：表示xml文件本身的编码
-->
<books> <!--books表示多个图书信息-->
    <book sn="SN001"> <!--book表示一个图书信息 sn属性表示图书的序列号-->
	    <name>时间简史</name> <!--书名-->
		<author>霍金</author> <!--书的作者-->
	</book>
	
	<book sn="SN002">
	    <name>java书</name>
		<author>lalala</author>
	</book>
</books>
```
![图示](https://img-blog.csdnimg.cn/20210511152040240.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### XML命名规则：
1、名称可以包含字母、数字以及其它的字符。
2、名称不能以数字或者标点符号开始
3、名称不能以字符"xml"(或者 XML、Xml)开始（其实可以）
4、名称不能包含空格

XML中的元素（标签）也分成单标签和双标签：
单标签格式：<标签名 属性="值" 属性="值"... />
双标签格式：<标签名 属性="值" 属性="值"...>文本数据或者子标签</标签名>

### XML属性
xml 的标签属性和 html 的标签属性是十分类似的，属性可以提供元素的额外信息。
在标签上可以书写属性：
一个标签上可以书写多个属性，**每个属性的值必须用引号括起来，否则会报错**
### 语法规则
1. 所有 XML 元素都必须有关闭标签（也就是闭合）
2. XML 标签对大小写敏感
3. XML必须正确地嵌套
4. XML文档必须有根元素。**根元素是没有父标签的顶级元素，并且只能有唯一一个**
5. XML的属性值须加引号
6. XML中的特殊字符。比如 > 的特殊字符 &gt； < 的特殊字符 &lt；
7. 文本区域（CDATA区）。**CDATA语法可以告诉 XML 解析器，我 CDATA 里的文本内容，不需要 XML 语法解析。即把输入的文本内容原样显示，不会解析XML**
**CDATA 格式：**<![CDATA[文本内容]]>
![图示](https://img-blog.csdnimg.cn/20210511203515123.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![图示](https://img-blog.csdnimg.cn/20210511203602747.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![图示](https://img-blog.csdnimg.cn/20210511205759245.png)
创建一个lib目录，添加 dom4j 的jar包，然后将其添加到类路径
![图示](https://img-blog.csdnimg.cn/20210511223731927.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![图示](https://img-blog.csdnimg.cn/20210511223123578.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.util.List;

public class Dom4jTest {
    public static void main(String[] args) throws DocumentException {
        // 1、读取books.xml文件
        SAXReader reader = new SAXReader();
        Document document = reader.read("xml-test/src/books.xml");

        // 2、通过Document对象获取根元素
        Element rootElement = document.getRootElement();

        // 3、通过根元素获取book标签对象
        List<Element> books = rootElement.elements("book");// element()和elements()：通过标签名查找子元素

        // 4、遍历，处理每个book标签转换为Book类
        for(Element book : books){
            System.out.println(book.asXML());// asXML()：将标签对象转化为标签字符串

            Element nameElement = book.element("name");
            String nameText = nameElement.getText();// getText()：获取标签中的文本内容

            // 直接获取指定标签名的文本内容
            String authorText = book.elementText("author");

            // 获取属性值
            String sn = book.attributeValue("sn");
        }
    }
}
```