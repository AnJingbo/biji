

![图示](https://img-blog.csdnimg.cn/a87f119263c6421fb6fdf31da0ac0b78.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 思路：本题需要按列来计算比较容易理解，即求出每一列雨水的高度（因为宽度为 1，所以体积就是高度），然后直接相加即可。
* 按列计算：
![图示](https://img-blog.csdnimg.cn/34c3dd9c7d9a44c7abe7d7f8e6eec78a.jpg)
* 示例：求列 4 的雨水高度
![图示](https://img-blog.csdnimg.cn/354a812b8c444e2a9179f73a1c4d1423.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 列 4 左侧最高的柱子是列 3，高度为 2；
列 4 右侧最高的柱子是列 7，高度为 3；
列 4 柱子的高度为 1；
那么列 4 的雨水高度为：列 3 和列 7 的高度最小值减去列 4 的高度。
列 4 的雨水高度求出来了，宽度为 1，相乘就是列 4 的雨水体积了。
### 双指针法
```java
class Solution {
    public int trap(int[] height) {
        int sum = 0;
        for(int i = 0; i < height.length; i++){
            // 第一个柱子和最后一个柱子不接雨水（下标为0的地方和下标为height.length - 1的地方不会接雨水）
            /*if(i == 0 || i == height.length - 1){
                continue;
            }*/
            int leftMax = 0; // 记录左边柱子的最高高度
            int rightMax = 0; // 记录右边柱子的最高高度
            for(int l = i - 1; l >= 0; l--){
                if(height[l] > leftMax){
                    leftMax = height[l];
                }
            }
            for(int r = i + 1; r < height.length; r++){
                if(height[r] > rightMax){
                    rightMax = height[r];
                }
            }
            int h = Math.min(leftMax, rightMax) - height[i]; // 左侧柱子的最高高度和右侧柱子的最高高度的最小值 - 当前柱子的高度
            if(h > 0){
                sum += h;
            }
        }
        return sum;
    }
}
```
### 动态规划法
```java
class Solution {
    public int trap(int[] height) {
        // 记录当前柱子的左边柱子中的最高高度
        int[] leftMax = new int[height.length];
        // 状态转移方程：dp[i] = Math.max(dp[i - 1], height[i - 1])
        leftMax[0] = 0;
        for(int i = 1; i < height.length; i++){
            leftMax[i] = Math.max(leftMax[i - 1], height[i - 1]);
        }

        // 记录当前柱子的右边柱子中的最高高度
        int[] rightMax = new int[height.length];
        // 状态转移方程：dp[i] = Math.max(dp[i + 1], height[i + 1])
        rightMax[height.length - 1] = 0;
        for(int i = height.length - 2; i >= 0; i--){
            rightMax[i] = Math.max(rightMax[i + 1], height[i + 1]);
        }

        int sum = 0;
        for(int i = 0; i < height.length; i++){
            int h = Math.min(leftMax[i], rightMax[i]) - height[i];
            if(h > 0){
                sum += h;
            }
        }
        return sum;
    }
}
```
### 单调栈
* 单调栈是按行来计算雨水。
* 按行计算：
![图示](https://img-blog.csdnimg.cn/805fcb207b0f41fd81fba803a1aa34cb.png)
* 思路：
先将下标 0 的柱子加入到栈中，然后从下标 1 开始遍历所有的柱子
（1）如果当前遍历的元素（柱子）高度小于栈顶元素，说明这里会有积水，就把这个元素加入栈中。**（从栈头到栈底的顺序应该是从小到大的）**
（2）如果当前遍历的元素（柱子）高度等于栈顶元素，要更新栈顶元素，因为遇到相同高度的柱子，需要使用最右边的柱子来计算宽度。
（3）如果当前遍历的元素（柱子）高度大于栈顶元素，则将栈顶元素弹出，这个就是凹槽的底部，也就是中间位置，下标记为 mid，对应的高度为 height[mid]；
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;此时的栈顶元素 stack.peek()，就是凹槽左边的位置，下标为 stack.peek()，对应的高度为 height[stack.peek()]；
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当前遍历的元素 i，就是凹槽右边的位置，下标为 i，对应的高度为 height[i]；
那么雨水高度是 Math.min(凹槽左边高度, 凹槽右边高度) - 凹槽底部高度，代码为：int h = Math.min(height[stack.peek()], height[i]) - height[mid];
雨水的宽度是 凹槽右边的下标 - 凹槽左边的下标 - 1（因为只求中间宽度），代码为：int w = i - stack.peek() - 1;
当前凹槽雨水的体积就是：h * w
![图示](https://img-blog.csdnimg.cn/12ff46f2677d4b02a549f00e9ff4caa1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_14,color_FFFFFF,t_70,g_se,x_16)
* **栈里存放的是每个元素对应的下标**。宽可以通过柱子之间的下标来计算，通过下标还可以得到的柱子对应的高度，从而计算长
```java
class Solution {
    public int trap(int[] height) {
        Stack<Integer> stack = new Stack<>(); // 存放下标
        stack.push(0);
        int sum = 0;
        for(int i = 1; i < height.length; i++){
            if(height[i] < height[stack.peek()]){
                stack.push(i);
            }else if(height[i] == height[stack.peek()]){
                stack.pop(); // 其实这一句可以不加，效果是一样的
                stack.push(i);
            }else{
                while(!stack.isEmpty() && height[i] > height[stack.peek()]){
                    int mid = stack.pop();
                    if(!stack.isEmpty()){
                        int h = Math.min(height[stack.peek()], height[i]) - height[mid];
                        int w = i - stack.peek() - 1;
                        sum += h * w;
                    }
                }
                stack.push(i);
            }
        } 
        return sum;
    }
}
```