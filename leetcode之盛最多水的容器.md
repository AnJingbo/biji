

![图示](https://img-blog.csdnimg.cn/b01715c281dc43cf86fb09d17c8fcebe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 设两指针 i , j ，指向的水槽板高度分别为 height[i] , height[j] ，此状态下水槽面积为 S(i, j) 。由于**可容纳水的高度由两板中的短板决定**，因此可得如下的面积公式 ：S(i, j) = min(height[i], height[j]) × (j - i)。
* 在每个状态下，无论长板或短板向中间收窄一格，都会导致水槽底边宽度 -1 变短：
（1）若向内移动短板 ，水槽的短板 min(height[i], height[j]) 可能变大，因此下个水槽的面积可能增大。
（2）若向内移动长板 ，水槽的短板 min(height[i], height[j]) 不变或变小，因此下个水槽的面积一定变小。
* 因此，**初始化双指针分列水槽左右两端，循环每轮将短板向内移动一格，并更新面积最大值，直到两指针相遇时跳出；即可获得最大面积。**
```java
class Solution {
    public int maxArea(int[] height) {
        int res = 0;
        int i = 0, j = height.length - 1;
        while(i < j){
            if(height[i] < height[j]){
                res = Math.max(res, (j - i) * height[i]);
                i++;
            }else{
                res = Math.max(res, (j - i) * height[j]);
                j--;
            }
        }
        return res;
    }
}
```
* 正确性证明：
假设状态 S(i, j) 下 height[i] < height[j]，在向内移动短板至 S(i+1, j) 时，则相当于消去了 {S(i, j - 1), S(i, j - 2), ... , S(i, i + 1)} 状态集合。而所有消去状态的面积一定都小于当前面积（即小于 S(i, j)），因为这些状态：
（1）短板高度：相比 S(i, j) 相同或更短（ 即小于等于 height[i] ）
（2）底边宽度：相比 S(i, j) 更短
因此，**每轮向内移动短板，所有消去的状态都不会导致面积最大值丢失 。**