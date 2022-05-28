

```java
/**
 * 1047. 删除字符串中的所有相邻重复项
 *
 * 给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。
 * 在 S 上反复执行重复项删除操作，直到无法继续删除。
 * 在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。
 *
 * 示例：
 * 输入："abbaca"
 * 输出："ca"
 */

import java.util.Stack;
public class Ti28 {
    public static void main(String[] args) {
        String res = removeDuplicates("abbaca");
        System.out.println(res);
    }

    public static String removeDuplicates(String S) {
        Stack<Character> stack = new Stack<>();
        StringBuilder res = new StringBuilder();
        for(int i = 0; i < S.length(); i++){
        //注意：用if-else比只用if好很多
            if(stack.size() >= 1 && S.charAt(i) == stack.peek()){
                stack.pop();
            }else{
                stack.push(S.charAt(i));
            }
        }
        while(stack.size() != 0){
            res = res.insert(0, stack.pop());
        }
        //将StringBuilder转为String类型
        return res.toString();
    }
}
```