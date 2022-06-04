

### 233. 数字 1 的个数
![ts](https://img-blog.csdnimg.cn/156828f909bd4b1195d39cb3ced2d883.png)
```java
/*
小于等于 n 的所有数字中，个位出现1的次数 + 十位出现1的次数 + ... 最后得到的就是总的出现次数

假设 n = xyzdabc，此时我们求千位是 1 的个数，也就是 d 所在的位置。那么此时有三种情况，
（1）d == 0，那么千位上 1 的个数就是 xyz * 1000
（2）d == 1，那么千位上 1 的个数就是 xyz * 1000 + abc + 1
（3）d > 1，那么千位上 1 的个数就是 xyz * 1000 + 1000

假设 n = 203，那么十位上 1 的个数就是：20
假设 n = 213，那么十位上 1 的个数就是：20 + 4
假设 n = 223，那么十位上 1 的个数就是：20 + 10 
*/
class Solution {
    public int countDigitOne(int n) {
        int res = 0;
        int after = 0; // 当前位后面的数字
        int wei = 1; // 记录当前是个位、十位、百位...
        for(int i = n; i >= 1; i /= 10){
            int cur = i % 10; // 记录当前位的值
            int before = i / 10; // 当前位前面的数字
            if(cur == 0){
                res += before * wei;
            }else if(cur == 1){
                res += before * wei + after + 1;
            }else{
                res += before * wei + wei;
            }
            after += wei * cur;
            wei *= 10;
        }
        return res;
    }
}
```
```java
/*
比如要求 123 这个数含有 1 的个数，其实只要看 12 和 3 含有的 1 相加就可以了，用 dp 方程表示为：dp[i]=dp[i/10]+dp[i%10]
*/
class Solution {
    public int countDigitOne(int n) {
        if(n <= 0) return 0;
        int res = 0;
        int[] dp = new int[n + 1];
        dp[1] = 1;
        for (int i = 1; i <= n; i++) {
            dp[i] = dp[i % 10] + dp[i / 10];
            res += dp[i];
        }
        return res;
    }
}
```