

![图示](https://img-blog.csdnimg.cn/20210603214652130.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
class Solution {
    public List<Integer> diffWaysToCompute(String expression) {// 计算算式 expression 所有可能的运算结果
        List<Integer> res = new ArrayList<>();
        for(int i = 0; i < expression.length(); i++){
            char c = expression.charAt(i);
            // 扫描算式中的运算符
            if(c == '+' || c == '-' || c == '*'){
                // 以运算符为中心，分割成两个字符串，分别递归计算（分）
                List<Integer> left = diffWaysToCompute(expression.substring(0, i));
                List<Integer> right = diffWaysToCompute(expression.substring(i + 1, expression.length()));
                // 通过子问题的结果，合成原问题的结果（治）
                for(int num1 : left){
                    for(int num2 : right){
                        if(c == '+'){
                            res.add(num1 + num2);
                        }else if(c == '-'){
                            res.add(num1 - num2);
                        }else{
                            res.add(num1 * num2);
                        } 
                    }
                }
            }
        }
        if(res.isEmpty()){// 如果 res 为空，说明传来的字符串只有一个数字，没有运算符
            res.add(Integer.parseInt(expression));
        }
        return res;
    }
}
```