

辗转相除法：用a除以b（a > b。在程序编程中，求两个数的最大公约数，可以不限a和b的大小，因为如果a < b的话，程序在第一次递归时就会先把a < b变成a > b，即把a和b换一下顺序。可以代入两个数试试，比如4和6）得到结果q和余数r，如果余数r等于0，那么b就是最大公约数；如果余数r不为0，则再用除数b除以余数r再得到一个余数s，如果余数s等于0，那么此时的r就是最大公约数；如果余数s不为0，则再用除数除以余数，…如此循环，直到余数为0，那么此时的除数就是最大公因数。

第一步：用较大的数a除以较小的数b得到一个商q0和一个余数r0；

第二步：若r0＝0，则b为a，b的最大公因数；若r0 ≠ 0，则用除数n除以余数r0得到一个商q1和一个余数r1；

第三步：若r1＝0，则r1为a，b的最大公因数；若r1 ≠ 0，则用除数r0除以余数r1得到一个商q2和一个余数r2；
......
依次计算直至余数等于0，则此时的除数就是所求的最大公因数。

例：用辗转相除法求6/4的最大公约数：

6 / 4 = 1......2
4 / **2** = 2......0

至此，最大公约数为2
以除数和余数反复做除法运算，当余数为 0 时，取当前算式除数为最大公约数

```java
public class GreatestCommonDivisor {
    public static void main(String[] args) {
        int a = gcd(4, 6);
        System.out.println(a);
        int b = lcm(4, 6);
        System.out.println(b);
    }

    public static int gcd(int a, int b) {//求最大公约数
        if (b == 0) {
            return a;
        }
        return gcd(b, a % b);
    }

    public static int lcm(int a, int b) {//求最小公倍数
        return (a * b) / gcd(a, b);
    }
}
```
```java
public static int gcd(int a, int b){// gcd的非递归写法
    while(a % b != 0){
        int temp = a % b;// 记录余数
        a = b;
        b = temp;
    }
    return b;
}
```