

```java
/*
501. 二叉搜索树中的众数
给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。
假定 BST 有如下定义：
结点左子树中所含结点的值小于等于当前结点的值
结点右子树中所含结点的值大于等于当前结点的值
左子树和右子树都是二叉搜索树
例如：
给定 BST [1,null,2,2],
             1
              \
               2
              /
            2
返回[2].
*/
class Solution {
    int maxCount = 0;//最大频率
    int count = 0;//统计频率
    TreeNode pre = null;//记录前一个结点
    List<Integer> result = new ArrayList<>();
    public int[] findMode(TreeNode root) {
        traversal(root);
        int[] res = new int[result.size()];
        for(int i = 0; i < result.size(); i++){
            res[i] = result.get(i);
        }
        return res;
    }
    public void traversal(TreeNode cur){
        if(cur == null){
            return;
        }
        traversal(cur.left);
        if(pre == null){//第一个结点
            count = 1;//频率为1
        }else if(pre.val == cur.val){//与前一个结点数值相同
            count++;
        }else{//与前一个结点数值不同
            count = 1;
        }
        pre = cur;//更新上一个结点
        //求最大频率元素的集合：应该先遍历一遍数组，找到最大频率(maxCount)，
        //然后再重新遍历一遍数组把出现频率为maxCount的元素放进集合，这种方式遍历了两遍数组
        //但这里只需要遍历一次就可以找到所有众数
        if(count == maxCount){//如果和最大频率相同，放进result中
            result.add(cur.val);
        }
        //但此时的maxCount还不一定是真正的最大频率，因此下面要做如下操作
        if(count > maxCount){//如果计数大于最大值
            maxCount = count;//更新最大频率
            result.clear();//很关键的一步，清空result，因为之前result里的元素都失效了
            result.add(cur.val);
        }
        traversal(cur.right);
    }
}
//自己写的
class Solution {
    public int[] findMode(TreeNode root) {
        if(root == null){
            return new int[0];
        }
        List<Integer> list = new ArrayList<>();
        traversal(root, list);
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < list.size(); i++){
            if(!map.containsKey(list.get(i))){
                map.put(list.get(i), 1);
            }else{
                map.put(list.get(i), map.get(list.get(i)) + 1);
            }
        }
        //遍历，找到最大频率，记为maxCount
        int maxCount = 0;
        Set<Integer> set = map.keySet();
        for(Integer item : set){
            if(maxCount < map.get(item)){
                maxCount = map.get(item);
            }
        }
        //将出现频率为maxCount的元素放进集合
        List<Integer> temp = new ArrayList<>();
        for(Integer item : set){
            if(maxCount == map.get(item)){
                temp.add(item);
            }
        }
        //将temp集合转换成int数组
        int[] res = new int[temp.size()];
        for(int i = 0; i < temp.size(); i++){
            res[i] = temp.get(i);
        }
        /*
          一开始写的
          //记录出现频率为maxCount的元素的数量
          int num = 0;
          for(Integer item : set){
              if(maxCount == map.get(item)){
                  num++;
              }
          }
          //将出现频率为maxCount的元素放进集合
          int[] res = new int[num];
          int i = 0;
          for(Integer item : set){
              if(maxCount == map.get(item)){
                  res[i++] = item;
              }
          }
        */
        return res;
    }
    public void traversal(TreeNode root, List<Integer> list){
        if(root == null){
            return;
        }
        traversal(root.left, list);
        list.add(root.val);
        traversal(root.right, list);
    }
}
//非递归
class Solution {
    public int[] findMode(TreeNode root) {
        if(root == null){
            return new int[0];
        }
        int maxCount = 0;//最大频率
        int count = 0;//统计频率
        TreeNode pre = null;//记录前一个结点
        List<Integer> result = new ArrayList<>();
        
        TreeNode temp = root;
        Stack<TreeNode> stack = new Stack<>();
        while(!stack.isEmpty() || temp != null){
            if(temp != null){
                stack.push(temp);
                temp = temp.left;
            }else{
                temp = stack.pop();
                if(pre == null){
                    count = 1;
                }else if(pre.val == temp.val){
                    count++;
                }else{
                    count = 1;
                }
                pre = temp;
                if(count == maxCount){
                    result.add(temp.val);
                }
                if(count > maxCount){
                    maxCount = count;
                    result.clear();
                    result.add(temp.val);
                }
                temp = temp.right;
            }
        }
        int[] res = new int[result.size()];
        for(int i = 0; i < result.size(); i++){
            res[i] = result.get(i);
        }
        return res;
    }
}
```