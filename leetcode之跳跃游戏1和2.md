

![图示](https://img-blog.csdnimg.cn/1fdeee09eaca45caa2aa1a75ae7610bf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public boolean canJump(int[] nums) {
        int farthest = 0;
        for(int i = 0; i < nums.length; i++){
            if(i > farthest){ // 可能碰到了 0，卡住跳不动了
                break;
            }
            farthest = Math.max(farthest, i + nums[i]);
            if(farthest >= nums.length - 1){
                return true;
            }
        }
        return false;
    }
}
```
![图示](https://img-blog.csdnimg.cn/181b665033e14873aa3396cca236f593.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
```java
class Solution {
    public int jump(int[] nums) {
        int res = 0; // 记录跳跃次数
        int farthest = 0; // 记录了在区间 [i, end] 中能够跳到的最远距离
        int end = 0;;
        for(int i = 0; i < nums.length - 1; i++){
            farthest = Math.max(farthest, i + nums[i]);
            if(i == end){
                res++;
                end = farthest;
            }
        }
        return res;
    }
}
```