

204. 计数质数
统计所有小于非负整数 n 的质数的数量。
示例 ：
输入：n = 10
输出：4
解释：小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
思路：采用暴力算法，容易超时
方法：埃氏筛
(1) 初始化长度为 n 的标记数组，表示这个数组是否为质数。数组初始化所有的数都是质数
(2) 从 2 开始，将每个质数的倍数都标记为合数，标记到 根号n 即可(即代码中 i * i < n)
```java
class Solution {
     public int countPrimes(int n) {
        int num = 0;
        boolean[] isPrime = new boolean[n];// true 表示是质数，false 表示是合数
        Arrays.fill(isPrime, true);//一开始假设所有数都是素数
        for(int i = 2; i * i < n; i++){
            if(isPrime[i]){//如果当前是素数
            // 就把从 i*i 开始，i 的所有倍数都设置为 false。
                for(int j = i * i; j < n; j += i){
                    isPrime[j] = false;;
                }
            }
        }
        //计数
        for(int i = 2; i < n; i++){
            if(isPrime[i]){
                num++;
            }
        }
        return num;
    }
}
```