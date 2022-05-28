

860. 柠檬水找零
在柠檬水摊上，每一杯柠檬水的售价为 5 美元。
顾客排队购买你的产品，（按账单 bills 支付的顺序）一次购买一杯。
每位顾客只买一杯柠檬水，然后向你付 5 美元、10 美元或 20 美元。你必须给每个顾客正确找零，也就是说净交易是每位顾客向你支付 5 美元。
注意，一开始你手头没有任何零钱。
如果你能给每位顾客正确找零，返回 true ，否则返回 false 。
示例 1：
输入：[5,5,5,10,20]
输出：true
解释：
前 3 位顾客那里，我们按顺序收取 3 张 5 美元的钞票。
第 4 位顾客那里，我们收取一张 10 美元的钞票，并返还 5 美元。
第 5 位顾客那里，我们找还一张 10 美元的钞票和一张 5 美元的钞票。
由于所有客户都得到了正确的找零，所以我们输出 true。
示例 2：
输入：[10,10]
输出：false

思路：
账单是20的情况，要优先消耗一个10美元和一个5美元，因为10美元只能给账单20美元找零，而5美元可以给10美元和20美元找零，5美元更万能
局部最优：遇到账单20，优先消耗10美元，完成本次找零
全局最优：完成全部账单的找零
//自己写的：
```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int five = 0;
        int ten = 0;
        int twen = 0;
        for(int i = 0; i < bills.length; i++){
            if(bills[i] == 5){
                five++;
                continue;
            }else if(bills[i] == 10){
                ten++;
                if(five >= 1){
                    five--;
                }else{
                    return false;
                }
            }else{
                twen++;
                if(ten >= 1){
                    if(five >= 1){
                        five--;
                        ten--;
                    }else{
                        return false;
                    }
                }else{
                    if(five >= 3){
                        five -= 3;
                    }else{
                        return false;
                    }
                }
            }
        }
        return true;
    }
}
```
//精简后的代码：
```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int five = 0;
        int ten = 0;
        int twen = 0;
        for(int bill : bills){
            if(bill == 5){
                five++;
                continue;
            }else if(bill == 10){
                if(five == 0){
                    return false;
                }
                ten++;
                five--;
            }else{
                //优先消耗10美元，因为5美元的找零用处更大
                if(five > 0 && ten > 0){
                    five--;
                    ten--;
                    twen++;//其实这一行代码可以删了，因为记录20其实没有意义，不会用20来找零
                }else if(five >= 3){
                    five -= 3;
                    twen++;//其实这一行代码可以删了，因为记录20其实没有意义，不会用20来找零
                }else return false;
            }
        }
        return true;
    }
}
```