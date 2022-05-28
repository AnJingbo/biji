

## 案例
```java
public class Test1 {
    // 提取文章中的所有英文单词
    public static void main(String[] args) {
        String content = "1998年12月8日，第二代Java平台的企业版J2EE发布。" +
                "1999年6月，Sun公司发布了第二代Java平台的0003个版本";
        // 创建一个 Pattern 模式对象，也可以理解为正则表达式对象
        Pattern pattern = Pattern.compile("[a-zA-Z]+");
        // 创建一个匹配器对象。matcher 匹配器按照 pattern 模式的规则到 content 文本中去匹配
        Matcher matcher = pattern.matcher(content);
        // 开始循环匹配
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
        }
    }
}
```
```java
public class Test2 {
    // 提取文章中的所有数字
    public static void main(String[] args) {
        String content = "1998年12月8日，第二代Java平台的企业版J2EE发布。" +
                "1999年6月，Sun公司发布了第二代Java平台的0003个版本";
        // 创建一个 Pattern 模式对象，也可以理解为正则表达式对象
        Pattern pattern = Pattern.compile("[0-9]+");
        // 创建一个匹配器对象。matcher 匹配器按照 pattern 模式的规则到 content 文本中去匹配
        Matcher matcher = pattern.matcher(content);
        // 开始循环匹配
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
        }
    }
}
```
```java
public class Test3 {
    // 提取文章中的所有英文单词和数字
    public static void main(String[] args) {
        String content = "1998年12月8日，第二代Java平台的企业版J2EE发布。" +
                "1999年6月，Sun公司发布了第二代Java平台的0003个版本";
        // 创建一个 Pattern 模式对象，也可以理解为正则表达式对象
        Pattern pattern = Pattern.compile("([a-zA-Z]+)|([0-9]+)");
        // 创建一个匹配器对象。matcher 匹配器按照 pattern 模式的规则到 content 文本中去匹配
        Matcher matcher = pattern.matcher(content);
        // 开始循环匹配
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
        }
    }
}
```
## 原理
```java
public class Test1 {
    // 匹配连续4个数字组成的子串
    public static void main(String[] args) {
        String content = "1998年12月8日，第二代Java平台的企业版J2EE发布。" +
                "1999年6月，Sun公司发布了第二代Java平台的3个版本";
        // \\d 表示一个数字。在 Java 中，两个 \\ 表示一个 \
        Pattern pattern = Pattern.compile("\\d\\d\\d\\d"); // 参数是正则表达式
        // 创建匹配器，按照正则表达式的规则去匹配 content 字符串
        Matcher matcher = pattern.matcher(content);
        /**
         * 本例中 find()：
         *     （1）按照指定的规则，定位满足规则的子字符串（即 1998）
         *     （2）找到满足规则的子字符串后，将该子字符串的开始和结束的索引记录到 matcher 对象
         *         的属性：int[] groups 中：把该子字符串的开始索引记录到：groups[0]=0，把该
         *         子字符串的结束索引+1 记录到：groups[1]=4
         *     （3）同时记录 oldLast 的值为 该子字符串的结束索引+1，即 4。当下次再执行
         *         find() 时，就从 4 开始匹配
         *     （4）按照指定的规则，定位下一个满足规则的子字符串（即 1999）
         *     （5）找到满足规则的子字符串后，将该子字符串的开始和结束的索引记录到 matcher 对象
         *         的属性：int[] groups 中：把该子字符串的开始索引记录到：groups[0]=31，把该
         *         子字符串的结束索引+1记录到：groups[1]=35。
         *     （6）同时记录 oldLast 的值为 该子字符串的结束索引+1，即 35。当下次再执行
         *          find() 时，就从 35 开始匹配
         */
        while(matcher.find()){
            // 截取 content 中 [groups[group * 2], groups[group * 2 + 1]) 的字符串，group 即传进去的参数
            // 截取 content 中 [groups[0], groups[1]) 的字符串
            System.out.println("找到：" + matcher.group(0));
        }
    }
}
```
* 注：在正则表达式中使用 ``()`` 即可实现分组。当使用分组时，除了可以获得整个匹配，还能够获得匹配中的每一个分组
```java
public class Test2 {
    // 匹配连续4个数字组成的子串
    public static void main(String[] args) {
        String content = "1998年12月8日，第二代Java平台的企业版J2EE发布。" +
                "1999年6月，Sun公司发布了第二代Java平台的3个版本";
        // \\d 表示一个数字。在 Java 中，两个 \\ 表示一个 \
        // 分组：正则表达式中 () 表示分组，第一个 () 表示第一组，第二个 () 表示第二组
        Pattern pattern = Pattern.compile("(\\d\\d)(\\d\\d)"); // 参数是正则表达式
        // 创建匹配器，按照正则表达式的规则去匹配 content 字符串
        Matcher matcher = pattern.matcher(content);
        /**
         * 本例中 find()：
         *     （1）按照指定的规则，定位满足规则的子字符串（即 1998）
         *     （2）找到满足规则的子字符串后，将该子字符串的开始和结束的索引记录到 matcher 对象
         *         的属性：int[] groups 中：
         *         (2.1) 把该子字符串的开始索引记录到：groups[0]=0，
         *             把该子字符串的结束索引+1记录到：groups[1]=4
         *         (2.2) 把第一组 () 匹配到的字符串记录到：groups[2]=0，groups[3]=2
         *         (2.3) 把第二组 () 匹配到的字符串记录到：groups[4]=2，groups[5]=4
         *     （3）同时记录 oldLast 的值为 该子字符串的结束索引+1，即 4。当下次再执行
         *         find() 时，就从 4 开始匹配
         *     （4）按照指定的规则，定位下一个满足规则的子字符串（即 1999）
         *     （5）找到满足规则的子字符串后，将该子字符串的开始和结束的索引记录到 matcher 对象
         *         的属性：int[] groups 中：
         *         (5.1) 把该子字符串的开始索引记录到：groups[0]=31，
         *             把该子字符串的结束索引+1记录到：groups[1]=35。
         *         (5.2) 把第一组 () 匹配到的字符串记录到：groups[2]=31，groups[3]=33
         *         (5.3) 把第二组 () 匹配到的字符串记录到：groups[4]=33，groups[5]=35
         *     （6）同时记录 oldLast 的值为 该子字符串的结束索引+1，即 35。当下次再执行
         *          find() 时，就从 35 开始匹配
         */
        while(matcher.find()){
            // 截取 content 中 [groups[group * 2], groups[group * 2 + 1]) 的字符串，group 即传进去的参数

            // 截取 content 中 [groups[0], groups[1]) 的字符串
            System.out.println("找到：" + matcher.group(0));
            // 截取 content 中 [groups[2], groups[3]) 的字符串
            System.out.println("第一组 () 匹配到的字符串：" + matcher.group(1));
            // 截取 content 中 [groups[4], groups[5]) 的字符串
            System.out.println("第二组 () 匹配到的字符串：" + matcher.group(2));
        }
    }
}
```
```java
public class Test3 {
    // 匹配连续4个数字组成的子串
    public static void main(String[] args) {
        String content = "1998年12月8日，第二代Java平台的企业版J2EE发布。" +
                "1999年6月，Sun公司发布了第二代Java平台的3个版本";
        // \\d 表示一个数字。在 Java 中，两个 \\ 表示一个 \
        // 分组：正则表达式中 () 表示分组
        Pattern pattern = Pattern.compile("(\\d\\d\\d\\d)"); // 参数是正则表达式
        // 创建匹配器，按照正则表达式的规则去匹配 content 字符串
        Matcher matcher = pattern.matcher(content);
        /**
         * 本例中 find()：
         *     （1）按照指定的规则，定位满足规则的子字符串（即 1998）
         *     （2）找到满足规则的子字符串后，将该子字符串的开始和结束的索引记录到 matcher 对象
         *         的属性：int[] groups 中：
         *         (2.1) 把该子字符串的开始索引记录到：groups[0]=0，
         *             把该子字符串的结束索引+1记录到：groups[1]=4
         *         (2.2) 把第一组 () 匹配到的字符串记录到：groups[2]=0，groups[3]=4
         *     （3）同时记录 oldLast 的值为 该子字符串的结束索引+1，即 4。当下次再执行
         *         find() 时，就从 4 开始匹配
         *     （4）按照指定的规则，定位下一个满足规则的子字符串（即 1999）
         *     （5）找到满足规则的子字符串后，将该子字符串的开始和结束的索引记录到 matcher 对象
         *         的属性：int[] groups 中：
         *         (5.1) 把该子字符串的开始索引记录到：groups[0]=31，
         *             把该子字符串的结束索引+1记录到：groups[1]=35。
         *         (5.2) 把第一组 () 匹配到的字符串记录到：groups[2]=31，groups[3]=35
         *     （6）同时记录 oldLast 的值为 该子字符串的结束索引+1，即 35。当下次再执行
         *          find() 时，就从 35 开始匹配
         */
        while(matcher.find()){
            // 截取 content 中 [groups[group * 2], groups[group * 2 + 1]) 的字符串，group 即传进去的参数

            // 截取 content 中 [groups[0], groups[1]) 的字符串
            System.out.println("找到：" + matcher.group(0));
            // 截取 content 中 [groups[2], groups[3]) 的字符串
            System.out.println("第一组 () 匹配到的字符串：" + matcher.group(1));
        }
    }
}
```
## 转义字符
* 在我们使用正则表达式去匹配某些特殊字符的时候，需要使用转义字符，否则匹配不到任何字符
* 常见的特殊字符：``. * + () $ / \ ? [] ^ {}``
* **注：在 Java 中，两个 ``\\`` 等同于其它语言中的一个 ``\``**
```java
public class Test1 {
    public static void main(String[] args) {
        // 匹配 (
        String content = "abc.(123(";
        Pattern pattern = Pattern.compile("\\(");
        Matcher matcher = pattern.matcher(content);
        while(matcher.find()){
            System.out.println(matcher.group(0));
        }
    }
}
```
## 字符匹配
| 符号  | 含义                                                         | 示例           | 解释                                                         |
| ----- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| [ ]   | 可接收的字符列表                                             | [abc]          | 匹配 a、b、c 中的任意一个字符                                |
| [^]   | 不接收的字符列表                                             | [\^abc]        | 匹配不在 a、b、c 中的任意一个字符                            |
| -     | 连字符                                                       | [a-z]          | 匹配 a-z 中的任意一个字符                                    |
| **.** | 匹配除了 \n 之外的任意一个字符。如果只匹配 **.** 则需要使用：\\\\. | a\.\.b         | 以 a 开头、b 结尾、中间包括两个任意字符的长度为 4 的字符串   |
| \\\d  | 匹配 0-9 中的任意一个数字，相当于 [0-9]                      | \\\d{3}(\\\d)? | 包含 3 或 4 个数字的字符串                                   |
| \\\D  | 与 \\\d 相反，匹配不在 0-9 中的任意一个数字，相当于 [\^0-9]  | \\\D(\\\d)*    | 以单个非数字字符开头，后接任意个数字的字符串                 |
| \\\w  | 匹配任意一个大小写英文字母、数字、下划线，相当于 [a-zA-Z0-9_] | \\\w{4}        | 匹配连续 4 个字符，这些字符可以是大小写英文字母、数字或者下划线 |
| \\\W  | 与 \\\w 相反，相当于 [\^a-zA-Z0-9_]                          | \\\W{3}        | 匹配连续 3 个字符，这些字符不能是大小写英文字母以及数字以及下划线 |
| \\\s  | 匹配所有空白字符（比如：空格、制表符等）                     | \\\s           | 匹配所有空白字符（比如：空格、制表符等）                     |
| \\\S  | 与 \\\s 相反，匹配所有非空白字符                             | \\\S           | 匹配所有非空白字符                                           |

* ``[a-z]``：匹配 a ~ z 中的任意一个字符
* ``[A-Z]``：匹配 A ~ Z 中的任意一个字符
* ``[0-9]``：匹配 0 ~ 9 中的任意一个字符
* ``[^a-z]``：匹配不在 a ~ z 中的任意一个字符
* ``[^0-9]``：匹配不在 0 ~ 9 中的任意一个字符
* ``[abc]``：匹配 a、b、c 中的任意一个字符
* ``abc``：匹配 abc 这个字符串（区分大小写）
* Java 中的正则表达式默认是区分字母大小写的，怎么设置成不区分大小写：
（1）``(?i)abc``：表示 abc 不区分大小写
（2）``a(?i)bc``：表示 bc 不区分大小写
（3）``a((?i)b)c``：表示只有 b 不区分大小写
（4）``Pattern pattern = Pattern.compile(regixExpression, Pattern.CASE_INSENSITIVE);``
* ``\\d``：匹配 0 ~ 9 中的任意一个数字，相当于 ``[0-9]``
* ``\\D``：与 ``\\d`` 相反，匹配不在 0 ~ 9 中的任意一个数字，相当于 ``[^0-9]``
* ``\\w``：匹配大小写英文字母、数字、下划线，相当于 ``[a-zA-Z0-9_]``
* ``\\W``：与 ``\\w`` 相反，相当于 ``[^a-zA-Z0-9_]``
* ``\\s``：匹配所有空白字符（比如：空格、制表符等）
* ``\\S``：与 ``\\s`` 相反，匹配所有非空白字符
* ``.``：匹配除了 \n 之外的任意一个字符。如果只匹配 **.** 则需要使用：``\\.``
```java
public class Test2 {
    public static void main(String[] args) {
        String content = "abc1234ABCabcd_ #";
        // Pattern pattern = Pattern.compile("\\d\\d\\d"); // 包含连续3个任意数字(0~9)的字符串
        // Pattern pattern = Pattern.compile("\\d{3}"); // 包含连续3个任意数字(0~9)的字符串
        // Pattern pattern = Pattern.compile("[a-z]"); // 匹配 a-z 中的任意一个字符
        // Pattern pattern = Pattern.compile("[A-Z]"); // 匹配 A-Z 中的任意一个字符
        // Pattern pattern = Pattern.compile("abc"); // 匹配 abc 这个字符串（区分大小写）
        // Pattern pattern = Pattern.compile("(?i)abc"); // 匹配 abc 字符串（不区分大小写）
        Pattern pattern = Pattern.compile("abc", Pattern.CASE_INSENSITIVE); // 匹配 abc 字符串（不区分大小写）
        // Pattern pattern = Pattern.compile("[abc]"); // 匹配 a、b、c 中的任意一个字符
        // Pattern pattern = Pattern.compile("[0-9]"); // 匹配 0-9 中的任意一个数字字符
        // Pattern pattern = Pattern.compile("[^a-z]"); // 匹配不在 a-z 中的任意一个字符
        // Pattern pattern = Pattern.compile("[^0-9]"); // 匹配不在 0-9 中的任意一个数字字符
        // Pattern pattern = Pattern.compile("[^abc]"); // 匹配不在 a、b、c 中的任意一个字符
        // Pattern pattern = Pattern.compile("\\d"); // 匹配 0-9 中的任意一个数字字符，相当于[0-9]
        // Pattern pattern = Pattern.compile("\\D"); // 与 \\d 相反，匹配不在 0-9 中的任意一个数字字符，相当于[^0-9]
        // Pattern pattern = Pattern.compile("\\w"); // 匹配任意一个大小写英文字母、数字、下划线，相当于[a-zA-Z0-9_]
        // Pattern pattern = Pattern.compile("\\W"); // 与 \\w 相反，相当于[^a-zA-Z0-9_]
        // Pattern pattern = Pattern.compile("\\s"); // 匹配所有空白字符（比如：空格、制表符等）
        // Pattern pattern = Pattern.compile("\\S"); // 与 \\s 相反，匹配所有非空白字符
        // Pattern pattern = Pattern.compile("."); // 匹配除了 \n 之外的任意一个字符。如果只匹配 . 则需要使用：\\.

        Matcher matcher = pattern.matcher(content);
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
        }
    }
}
```
## 选择匹配符
* 在匹配某个字符串的时候是可选择性的，即：既可以匹配这个，又可以匹配那个。这时你需要使用选择匹配符

| 符号 | 含义                           | 示例   | 解释            |
| ---- | ------------------------------ | ------ | --------------- |
| \|   | 匹配 "\|" 之前或者之后的表达式 | ab\|cd | 匹配 ab 或者 cd |

```java
public class Test3 {
    public static void main(String[] args) {
        String content = "hanshunping 韩&寒冷";
        Pattern pattern = Pattern.compile("han|韩|寒");
        Matcher matcher = pattern.matcher(content);
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
        }
    }
}
```
## 限定符
* 限定符：用于指定其前面的字符或组合项连续出现多少次
* 注：Java 中默认是贪婪匹配，会尽可能匹配多的。比如：``a{3,4}``，会优先匹配 aaaa，如果没有才会去匹配 aaa。如果想要使用非贪婪匹配，则需要将 **?** 字符紧随在其它任何限定符（**\*、+、?、{n}、{n,}、{n,m}**）之后，此时的匹配模式是非贪心的，即会匹配尽可能短的字符。比如：``a{3,4}?`` 会优先匹配 aaa

| 符号  | 含义                                                         | 示例       | 解释                                                         |
| ----- | ------------------------------------------------------------ | ---------- | ------------------------------------------------------------ |
| *     | 指定字符重复 0 次或 n 次                                     | (abc)*     | 包含任意个 abc 的字符串                                      |
| +     | 指定字符重复 1 次或 n 次                                     | m+(abc)*   | 至少一个 m 开头，后接任意个 abc 的字符串                     |
| ?     | 指定字符重复 0 次或 1 次                                     | m+abc?     | 至少一个 m 开头，后接 ab 或 abc 的字符串                     |
| {n}   | 匹配 n 个字符组成的字符串                                    | [abc]{3}   | 由 a、b、c 中的任意一个字符组成的长度为 3 的字符串           |
| {n,}  | 匹配至少 n 个字符组成的字符串                                | [abc]{3,}  | 由 a、b、c 中的任意一个字符组成的长度至少为 3 的字符串       |
| {n,m} | 匹配至少 n 个字符，至多 m 个字符组成的字符串                 | [abc]{3,5} | 由 a、b、c 中的任意一个字符组成的长度不小于 3、不大于 5 的字符串 |
| ?     | 当此字符紧随其它任何限定符（**\*、+、?、{n}、{n,}、{n,m}**）之后时，匹配模式是非贪心的，即会匹配尽可能短的字符串 | a{3,4}?    | 会优先匹配 aaa                                               |

```java
public class Test4 {
    public static void main(String[] args) {
        String content = "137aaa12345aaaaa";
        // Pattern pattern = Pattern.compile("a{3}"); // 表示匹配 aaa
        // Pattern pattern = Pattern.compile("\\d{2}"); // 表示匹配长度为 2 的任意数字字符串

        /* 注：Java 中默认是贪婪匹配，会尽可能匹配长的字符串 */

        // Pattern pattern = Pattern.compile("\\d{2,}"); // 表示匹配长度至少为 2 的任意数字字符串
        // Pattern pattern = Pattern.compile("\\d{2,3}"); // 表示匹配长度至少为 2，至多为 3 的任意数字字符串
        // Pattern pattern = Pattern.compile("a*"); // 表示匹配 0 个或 n 个 a
        // Pattern pattern = Pattern.compile("a+"); // 表示匹配 1 个或 n 个 a
        // Pattern pattern = Pattern.compile("\\d+"); // 表示匹配 1 个或 n 个数字字符
        // Pattern pattern = Pattern.compile("a1?"); // 表示匹配 a 或者 a1

        /* 设置非贪婪匹配，会尽可能匹配短的字符串 */
        Pattern pattern = Pattern.compile("\\d{2,3}?"); // 表示匹配长度至少为 2，至多为 3 的任意数字字符串

        Matcher matcher = pattern.matcher(content);
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
        }
    }
}
```
## 定位符
* 定位符：可以用于指定要匹配的字符串出现的位置，比如在字符串的开始还是结束的位置

| 符号 | 含义                   | 示例            | 解释                                                         |
| ---- | ---------------------- | --------------- | ------------------------------------------------------------ |
| ^    | 指定字符串的起始字符   | \^[0-9]+[a-z]*  | 以至少一个数字开头，后接任意个小写字母的字符串               |
| $    | 指定字符串的结束字符   | \^[0-9]+[a-z]+$ | 以至少一个数字开头，并以至少一个小写字母结尾的字符串         |
| \\\b | 匹配目标字符串的边界   | han\\\b         | 这里说的字符串的边界指的是子串间有空格，或者是目标字符串的结束位置。比如：hanshunping sp**han** nn**han** |
| \\\B | 匹配目标字符串的非边界 | han\\\B         | 和 \\\b 的含义相反。比如：**han**shunping sphan nnhan        |

```java
public class Test5 {
    public static void main(String[] args) {
        String content = "123abc";
        // Pattern pattern = Pattern.compile("^[0-9]+[a-z]*"); // 以至少一个数字开头，后接任意个小写字母的字符串
        Pattern pattern = Pattern.compile("^[0-9]+[a-z]+$"); // 以至少一个数字开头，并以至少一个小写字母结尾的字符串
        Matcher matcher = pattern.matcher(content);
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
        }
    }
}
```
```java
public class Test6 {
    public static void main(String[] args) {
        String content = "hanshunping sphan nnhan";
        // Pattern pattern = Pattern.compile("han\\b");
        Pattern pattern = Pattern.compile("han\\B");
        Matcher matcher = pattern.matcher(content);
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
        }
    }
}
```
```java
public class Test5 {
    public static void main(String[] args) {
        String content = "12";
        // Pattern pattern = Pattern.compile("^[0-9]$");
        Pattern pattern = Pattern.compile("^[0-9][0-9]$");
        Matcher matcher = pattern.matcher(content);
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
        }
    }
}
```
## 分组
* 在正则表达式中使用 ``()`` 即可实现分组。
### 捕获分组
* 捕获分组：可以捕获分组，会将每个分组的起始下标和结束下标+1也放在 groups 数组中

| 常用分组构造形式  | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| (pattern)         | 非命名捕获。编号为 0 的第一个捕获是由整个正则表达式模式匹配的文本，其它捕获结果则根据括号的顺序从 1 开始编号 |
| (?\<name>pattern) | 命名捕获，可以给分组取名。name 字符串不能包含任何标点符号，并且不能以数字开头 |

```java
public class Test7 {
    public static void main(String[] args) {
        String content = "1998hanshuping1997abcd";
        // 非命名分组
        //   matcher.group(0) 得到匹配到的字符串
        //   matcher.group(1) 得到匹配到的字符串的第 1 个分组
        //   matcher.group(2) 得到匹配到的字符串的第 2 个分组
        Pattern pattern = Pattern.compile("(\\d\\d)(\\d\\d)");
        Matcher matcher = pattern.matcher(content);
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
            System.out.println("第 1 个分组的内容：" + matcher.group(1));
            System.out.println("第 2 个分组的内容：" + matcher.group(2));
        }
    }
}
```
```java
public class Test8 {
    public static void main(String[] args) {
        String content = "1998hanshuping1997abcd";
        // 命名分组：可以给分组取名
        Pattern pattern = Pattern.compile("(?<g1>\\d\\d)(?<g2>\\d\\d)");
        Matcher matcher = pattern.matcher(content);
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
            System.out.println("第 1 个分组的内容：" + matcher.group(1));
            System.out.println("第 1 个分组的内容【通过组名】：" + matcher.group("g1"));
            System.out.println("第 2 个分组的内容：" + matcher.group(2));
            System.out.println("第 2 个分组的内容【通过组名】：" + matcher.group("g2"));
        }
    }
}
```
### 非捕获分组
* 非捕获分组：不会捕获分组，即不会将分组的起始下标和结束下标+1放在 groups 数组中，也就是不能通过 matcher.group(1) 等来捕获分组

| 常用分组构造形式 | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| (?:pattern)      | 对于使用 "or" 字符（\|）的情况很有用。比如：`industr(?:y|ies)` 是比 `industry|industries` 更经济的表达式 |
| (?=pattern)      | 比如：`windows (?=95|98|NT|2000)` 匹配 "windows 2000" 中的 "windows"，但不匹配 "windows 3.1" 中的 "windows" |
| (?!pattern)      | 比如：`windows (?!95|98|NT|2000)` 匹配 "windows 3.1" 中的 "windows"，但不匹配 "windows 2000" 中的 "windows" |

```java
public class Test9 {
    public static void main(String[] args) {
        String content = "hello韩顺平教育 韩顺平老师java 12韩顺平同学34";

        /* 找到韩顺平老师、韩顺平教育和韩顺平同学 */
        // Pattern pattern = Pattern.compile("韩顺平教育|韩顺平老师|韩顺平同学");
        // Pattern pattern = Pattern.compile("韩顺平(?:教育|老师|同学)"); // 非捕获分组，不能使用 matcher.group(1) 等来操作

        /* 找到韩顺平这个关键字，但是只要求查找韩顺平教育和韩顺平老师中的韩顺平 */
        // Pattern pattern = Pattern.compile("韩顺平(?=教育|老师)"); // 非捕获分组，不能使用 matcher.group(1) 等来操作

        /* 找到韩顺平这个关键字，但是只要求查找不在韩顺平教育和韩顺平老师中的韩顺平 */
        Pattern pattern = Pattern.compile("韩顺平(?!教育|老师)"); // 非捕获分组，不能使用 matcher.group(1) 等来操作

        Matcher matcher = pattern.matcher(content);
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
        }
    }
}
```
### 反向引用
* 分组：我们可以使用圆括号 `()` 来代表分组，一个 `()` 就代表一个分组
* 捕获：把正则表达式中的分组匹配的内容，保存到内存中以数字编号或者显式命名的组里，方便引用。`()` 从左到右，第一个出现的分组的组号为 1，第二个为 2，以此类推。第 0 组代表的是整个正则表达式匹配的内容
* 反向引用：圆括号的内容被捕获后，可以在这个括号后被使用，从而写出一个比较实用的匹配模式，这个我们称之为反向引用。这种引用既可以是在正则表达式的内部使用（内部反向引用：`\\分组号`），也可以是在正则表达式的外部使用（外部反向引用：`$分组号`）
```java
public class Test11 {
    public static void main(String[] args) {
        String content = "hel1134lo ja1221ck ni3333344ce good";
        /* 匹配两个连续的相同数字 */
        // Pattern pattern = Pattern.compile("(\\d)\\1");
        
        /* 匹配五个连续的相同数字 */
        // Pattern pattern = Pattern.compile("(\\d)\\1{4}");
        
        /* 匹配个位和千位相同，十位和百位相同的数字，比如：1221 */
        Pattern pattern = Pattern.compile("(\\d)(\\d)\\2\\1");
        Matcher matcher = pattern.matcher(content);
        while(matcher.find()){
            System.out.println("找到：" + matcher.group(0));
        }
    }
}
```
* 结巴去重案例：
```java
public class Test12 {
    public static void main(String[] args) {
        /* 通过正则表达式将 content 的内容变为：我要学编程Java*/
        String content = "我....我要....学学学学....编程Java";

        //（1）将 content 中所有的 . 去掉
        String s1 = Pattern.compile("\\.").matcher(content).replaceAll("");
        System.out.println(s1);

        //（2）将 content 中所有连续重复出现的字去掉只剩下一个
        String s2 = Pattern.compile("(.)\\1+").matcher(s1).replaceAll("$1");
        System.out.println(s2);
    }
}
```
## 应用实例
```java
public class Test10 {
    public static void main(String[] args) {
        String content = "韩顺平";
        /* 验证输入的是否是汉字 */
        Pattern pattern = Pattern.compile("^[\u0391-\uffe5]+$");
        
        /* 验证邮政编码。要求：1-9开头，6 位数 */
        // Pattern pattern = Pattern.compile("^[1-9]\\d{5}$");
        
        /* QQ 号码：1-9 开头，5 到 10 位数 */
        // Pattern pattern = Pattern.compile("^[1-9]\\d{4,6}$");
        
        /* 手机号码：13、14、15、18 开头的 11 位数 */
        // Pattern pattern = Pattern.compile("^(13|14|15|18)\\d{9}$");
        
        Matcher matcher = pattern.matcher(content);
        // find() 方法不是整体匹配，只要 content 中有与正则表达式匹配的子串，那么 find() 方法就会返回 true
        // 因此如果要使用 find() 方法去判断整个 content 是否符合正则表达式的要求的话，必须在正则表达式中加上 ^ 和 $
        if(matcher.find()){
            System.out.println("符合要求");
        }else{
            System.out.println("不符合要求");
        }
    }
}
```
```java
public class MatcherTest {
    public static void main(String[] args) {
        String content = "h2 helloh1 jah1ck h1sph2s";

        /* 将字符串 content 中的 h1 和 h2 都变成 h */
        /*Pattern pattern = Pattern.compile("h1|h2");
        Matcher matcher = pattern.matcher(content);
        String temp = matcher.replaceAll("h");*/
        
        // 简写：
        String temp = Pattern.compile("h1|h2").matcher(content).replaceAll("h");

        System.out.println(temp);
        System.out.println(content);
    }
}
```
## String 类中使用正则表达式
```java
public class StringTest {
    public static void main(String[] args) {
        String content = "h2 helloh1 jah1ck h1sph2s";

        /* 将字符串 content 中的 h1 和 h2 都变成 h */

        String temp = content.replaceAll("h1|h2", "h");
        System.out.println(temp);
    }
}
```
```java
public class StringTest1 {
    public static void main(String[] args) {
        String content = "13812345678";
        /* 验证一个手机号，要求必须是 138、139 开头的 */

        // String 的 matches() 方法默认就是整体匹配，它默认就会判断整个 content 字符串是否与正则表达式匹配
        boolean matches = content.matches("1(38|39)\\d{8}");
        if(matches){
            System.out.println("匹配成功");
        }else {
            System.out.println("匹配失败");
        }
    }
}
```
```java
public class StringTest2 {
    public static void main(String[] args) {
        String content = "hello#beijing-jack12danny";

        /* 按照 # 或者 - 或者数字来分割 */
        String[] split = content.split("#|-|\\d+");
        System.out.println(Arrays.toString(split));
    }
}
```
## 示例
```java
public class Test13 {
    public static void main(String[] args) {
        /**
         * 判断字符串是否是一个合格的邮箱
         * 邮箱格式的规则：
         *     1、只能有一个 @
         *     2、@ 前面是用户名，可以是：大小写字母、数字、 _ 和 -
         *     3、@ 后面是域名，并且域名只能是英文字母，比如：sohu.com 或者 hsp.com.cn 等
         */
        String content = "123@qq.com";
        boolean matches = content.matches("[\\w-]+@([a-zA-Z]+\\.)+[a-zA-Z]+");
        if(matches){
            System.out.println("邮箱格式正确");
        }else{
            System.out.println("邮箱的格式不正确！");
        }
    }
}
```
```java
public class Test14 {
    public static void main(String[] args) {
        /**
         * 判断字符串是不是整数或者小数（要考虑正负）
         */
        String content = "10.23";
        /* 可以有前导 0。比如：00123 也算合法的 */
        // boolean matches = content.matches("[-+]?\\d+(\\.\\d+)?");

        /* 不可以有前导 0。比如：00123 不是合法的 */
        boolean matches = content.matches("[-+]?(([1-9]\\d*)|0)(\\.\\d+)?");
        if(matches){
            System.out.println("符合要求");
        }else{
            System.out.println("不符合要求");
        }
    }
}
```
```java
public class Test15 {
    public static void main(String[] args) {
        /**
         * 对一个 url 进行解析
         * 比如：http://www.sohu.com:8080/abc/def/index.htm
         *      协议：http
         *      域名：www.sohu.com
         *      端口号：8080
         *      文件名：index.htm
         */
        String content = "http://www.sohu.com:8080/abc/def/index.htm";
        // 正则表达式是需要根据要求来写的，如果需求要求的话，可以改进正则表达式
        String regex = "^([a-zA-Z]+)://([a-zA-Z\\.]+):(\\d+)[a-zA-Z/]*/([\\w\\.]+)$";
        Pattern pattern = Pattern.compile(regex);
        Matcher matcher = pattern.matcher(content);
        // 整体匹配。如果整体匹配成功，则可以通过 group(n) 来获得对应分组的内容
        if(matcher.find()){
            System.out.println("整体匹配成功：" + matcher.group(0));
            System.out.println("协议：" + matcher.group(1));
            System.out.println("域名：" + matcher.group(2));
            System.out.println("端口号：" + matcher.group(3));
            System.out.println("文件名：" + matcher.group(4));
        }else{
            System.out.println("没有匹配成功");
        }
    }
}
```