

820.单词的压缩编码
单词数组 words 的 有效编码 由任意助记字符串 s 和下标数组 indices 组成，且满足：
words.length == indices.length
助记字符串 s 以 '#' 字符结尾
对于每个下标 indices[i] ，s 的一个从 indices[i] 开始、到下一个 '#' 字符结束（但不包括 '#'）的 子字符串 恰好与 words[i] 相等
给你一个单词数组 words ，返回成功对 words 进行编码的最小助记字符串 s 的长度 。

>思路：
>为什么这题我们需要使用字典树做呢？因为我们需要知道单词列表里，哪些单词是其它某个单词的后缀。既然要求的是后缀，我们只要把单词的倒序插入字典树，再用字典树判断某个单词的逆序是否出现在字典树里就可以了。
>例如：
>1、对于 "abc" 和 "bcd" ———— 虽然后缀有重合，但是依旧需要new出新节点，认为是"新word"，最终字符串只能为"abc#bcd#"
>2、对于 "bc" 和 "abc" ，插入的是逆序："cba" 和 "cb"。因为要的是后缀，而不是前缀(如果按前缀直接顺序插的话，那么结果就是abc#bc#，因为"bc"的首字母和"abc"的首字母就不相等)。并且一定是要先插入长的字符串，再插入短的，否则会有问题。
```java
class Solution {
    public int minimumLengthEncoding(String[] words) {
        Arrays.sort(words, (s1, s2) -> s2.length() - s1.length());// 将words数组按长度从大到小排序
        TrieTree tree = new TrieTree();
        int res = 0;
        for(int i = 0; i < words.length; i++){
            res += tree.insert(words[i]);
        }
        return res;
    }
}
class TrieTree{
    Node root = new Node();
    public int insert(String word){
        Node node = root;
        boolean isNew = false;// 判断是否是一个新字符串
        for(int i = word.length() - 1; i >= 0; i--){// 求后缀，只需要把单词的倒序插入字典树，然后再用字典树判断某个单词的逆序是否出现在字典树里就可以了。
            int index = word.charAt(i) - 'a';
            if(node.children[index] == null){
                node.children[index] = new Node();
                isNew = true;
            }
            node = node.children[index];
        }
        node.isEnd = true;
        return isNew ? word.length() + 1 : 0;
    }
}
class Node{
    boolean isEnd;
    Node[] children = new Node[26];
}
```