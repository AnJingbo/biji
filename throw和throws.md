

* throw：
表示方法内抛出某种异常对象
如果异常对象是非 RuntimeException 则需要在方法申明时加上该异常的抛出 即需要加上 throws 语句 或者 在方法体内 try catch 处理该异常，否则编译报错
执行到 throw 语句则后面的语句块不再执行
* throws：
方法的定义上使用 throws 表示这个方法可能抛出某种异常
需要由方法的调用者进行异常处理
```java
import java.io.IOException;
 
public class TestThrowsThrow {
 
	public static void main(String[] args) {
		testThrows();
		
		Integer i = null;
		testThrow(i);
		
		String filePath = null;
		try {
			testThrow(filePath);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 测试 throws 关键字
	 * @throws NullPointerException
	 */
	public static void testThrows() throws NullPointerException {
		Integer i = null;
		System.out.println(i + 1);
	}
	
	/**
	 * 测试 throw 关键字抛出 运行时异常
	 * @param i
	 */
	public static void testThrow(Integer i) {
		if (i == null) {
			throw new NullPointerException();//运行时异常不需要在方法上申明
		}
	}
	
	/**
	 * 测试 throw 关键字抛出 非运行时异常，需要方法体需要加 throws 异常抛出申明
	 * @param i
	 */
	public static void testThrow(String filePath) throws IOException {
		if (filePath == null) {
			throw new IOException();//运行时异常不需要在方法上申明
		}
	}
}
 
```