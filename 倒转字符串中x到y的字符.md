

```java
public class Ti12 {
    public static void main(String[] args) {
        String s = "abcdefg";
        String res = reverseStr(s, 3, 7);
        System.out.println(res);
    }

    public static String reverseStr(String s, int x, int y) {
        char[] a = s.toCharArray();
        x--;
        y--;
        for(int i = x; i < x + (y - x + 1) / 2; i++){
            char temp = a[i];
            a[i] = a[y + x - i];
            a[y + x - i] = temp;
        }
        return new String(a);
    }
    public static String reverseStr1(String s, int x, int y) {
        char[] a = s.toCharArray();
        x--;
        y--;
        for(int i = x, j = y; i < x + (j - x + 1) / 2; i++, y--){
            char temp = a[i];
            a[i] = a[y];
            a[y] = temp;
        }
        return new String(a);
    }
    public static String reverseStr2(String s, int x, int y) {
        char[] a = s.toCharArray();
        x--;
        y--;
        //类比倒转全部字符串
        //x + (y - x + 1) / 2
        for(int i = x, j = y; i < (x + y + 1) / 2; i++, j--){
            char temp = a[i];
            a[i] = a[j];
            a[j] = temp;
        }
        return new String(a);
    }
    //倒转整个数组
    public void reverseString(char[] s) {
        char temp;
        for(int i = 0; i < s.length / 2; i++){
            if(s[i] == s[s.length - 1 - i]){
                continue;
            }else{
                temp = s[i];
                s[i] = s[s.length - 1 - i];
                s[s.length - 1 - i] = temp;
            }
        }
    }
    //倒转整个数组 双指针
    public void reverseString(char[] s) {
        char temp;
        for(int i = 0, j = s.length - 1; i < s.length / 2; i++,j--){
            if(s[i] == s[j]){
                continue;
            }else{
                temp = s[i];
                s[i] = s[j];
                s[j] = temp;
            }
        }
    }

}
```