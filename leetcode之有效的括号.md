

```java
import java.util.Stack;

/**
 * 20. 有效的括号
 * 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
 * 有效字符串需满足：
 *     1、 左括号必须用相同类型的右括号闭合。
 *     2、左括号必须以正确的顺序闭合。
 * 注意空字符串可被认为是有效字符串。
 */
/*
 分析：第一种情况：字符串里左方向的括号多余了，所以不匹配。例：( [ { } ] ( )
      第二种情况：括号没有多余，但是括号的类型没有匹配上。例：( [ { } } }
      第三种情况：字符串里右方向的括号多余了，所以不匹配。例：( [ { } ] ) ) )
 */
public class Ti27 {
    public static void main(String[] args) {
        boolean flag = isValid("([{}]()");
        System.out.println(flag);
    }
    public static boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        int i = 0;
        while(i < s.length()){
            //将字符以相反的字符加进去是为了之后判断每种字符是否匹配时可以统一用【stack.peek() !=(或==) s.charAt(i)】来判断，而不用分开去判断每种字符是否匹配
            if(s.charAt(i) == '('){
                stack.push(')');
            }else if(s.charAt(i) == '['){
                stack.push(']');
            }else if(s.charAt(i) == '{'){
                stack.push('}');
            }else if(stack.isEmpty() || stack.peek() != s.charAt(i)){//能执行到这里，stack.isEmpty()指的就是第三种情况
                return false;
            }else if(stack.peek() == s.charAt(i)){
                stack.pop();
            }
            i++;
        }
        return stack.isEmpty();
    }
}
```