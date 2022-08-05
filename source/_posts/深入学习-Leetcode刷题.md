---
title: 深入学习-Leetcode刷题
date: 2021-08-31 15:02:57
tags: 大三自学
---



# 这里是刷LeetCode Top100中所记录的一些问题嗷！！！





## 5. 最长回文子串：

- 强调技巧：

1. 发现动态规划的影子
2. 你必须有意识，动态规划的终点是什么。比如这道题目中，`bp[i][j] = s[i]==s[j] + dp[i+1][j-1]`，那么这个条件能够执行下去的条件是什么，对于我这一层来讲，必须`j-1 >= i+1`，那反过来呢？反过来就不能够递推，也就有可能达到了重点。
3. Length + Begin的方式遍历，成功实现了一层层把房子搭起来嗷！！！
4. 像这一道题目，重点还在于，搭建的时候，我们必须意识到。递推这个过程，是从下往上进行的，所以我们写代码的时候，也需要从下往上构建。所谓的从下往上构建，本质上就是把这一张二维表不断填满的过程，但是我们使用的不是递归，是动态规划，因此这个时候，就不是自己调用自己，看返回值了。而是从下往上，把整个表给构建好嗷！！！



- 题目：

![image-20210901212053052](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210901212053.png)



- 代码：

```java
public String longestPalindrome(String s) {
        char[] array = s.toCharArray();
        int len = s.length();
        boolean[][] dp = new boolean[len][len];
        int begin = 0;
        int maxLen = 1;

        if (len < 2){
            return s;
        }

        for (int i = 0; i < len; i++) {
            dp[i][i] = true;
        }
        for (int length = 1;length <= len ;length ++){
            for (int i=0;i<len;i++){
                int j = i+length;
                if (j >= len){
                    break;
                }else {
                    if (array[i]==array[j]){
                        if (j-i == 1){
                            dp[i][j] = true;
                        }else {
                            dp[i][j] = dp[i+1][j-1];
                        }
                    }else {
                        dp[i][j] = false;
                    }
                }

                if (dp[i][j] && j - i + 1 > maxLen) {
                    maxLen = j - i + 1;
                    begin = i;
                }
            }

        }

        return s.substring(begin, begin + maxLen);
}
```







## 19. 删除链表的倒数第N个结点

- 强调技巧：
  1. 哑结点的加入（便于链表操作的一致性，非常有用嗷！！！）
  2. 例如提供链表表头，获得链表长度这种简单的代码应该形成肌肉记忆和反射。不要写成段的代码，要封装成函数嗷！！！
- 题目：

![image-20210831172416608](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210831172423.png)

- 答案：

  - 解法一：

  遍历链表两次，第一次获得链表长度，第二次删除对应的结点：

  ```java
  class Solution {
      public ListNode removeNthFromEnd(ListNode head, int n) {
          ListNode dummy = new ListNode(0, head);
          int length = getLength(head);
          ListNode cur = dummy;
          for (int i = 1; i < length - n + 1; ++i) {
              cur = cur.next;
          }
          cur.next = cur.next.next;
          ListNode ans = dummy.next;
          return ans;
      }
  
      public int getLength(ListNode head) {
          int length = 0;
          while (head != null) {
              ++length;
              head = head.next;
          }
          return length;
      }
  }
  ```

  - 解法二：

  使用栈FILO的特性，将每一个结点入栈，出栈的时候删掉对应位置的结点：

  ```java
  public ListNode19 removeNthFromEnd2(ListNode19 head, int n){
          ListNode19 dummy = new ListNode19(0,head);
        ListNode19 newHead = dummy;
          LinkedList<ListNode19> linkedList = new LinkedList<ListNode19>();
        while (dummy!=null){
              linkedList.addLast(dummy);
              dummy = dummy.next;
          }
  
          ListNode19 listNode19 = null;
  
          while (n >= 0){
              listNode19 = linkedList.removeLast();
              n--;
          }
  
          listNode19.next = listNode19.next.next;
  
          return newHead.next;
  }
  ```
  
  - 解法三：
  
  双指针，两者相隔为n，这样前面的结点到达最后一个元素的时候，后面一个结点恰好位置为要删除元素的位置嗷！！！







## 70. 爬楼梯

1. 题目：

![image-20210902092955558](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210902092955.png)

2. 答案：

```java
public int minCostClimbingStairs(int[] cost) {
        //记得这里下标减去1嗷！！！
        int len = cost.length;
        int[] mem = new int[len + 1];

        if (len == 1){
            return 0;
        }

        if (len == 2){
            return 0;
        }

        mem[0] = 0;
        mem[1] = 0;

        for (int i = 2; i <= len; i++) {
            mem[i] = Math.min(mem[i-1]+cost[i-1],mem[i-2]+cost[i-2]);
        }

        return mem[len];
}
```

- 这里很多细节哈，前两个阶梯默认开销都是0，如果你不往上走的话，默认开销都是零。在这里，三阶以上的阶梯，只能从下一阶梯或者下两阶梯跳上来。所以这一阶梯的最小值就等于下一阶梯的最小值加上下一阶梯的开销，或者是下两阶梯的最小值，加上下两阶梯的开销嗷！！！







## 509. 斐波那契数列：

1. 做法一：动态规划：

```java
public int fib1(int n) {
    if (n == 0) {
        return 0;
    }

    if (n == 1) {
        return 1;
    }

    int[] temp = new int[n + 1];
    temp[0] = 0;
    temp[1] = 1;

    for (int i = 2; i <= n; i++) {
        temp[i] = temp[i - 1] + temp[i - 2];
    }

    return temp[n];
}
```

- 明显看出来，要计算n，那就必须建立出来n-1和n-2，n-1和n-2到了最基层，就是0和1。由于动态规划是用之前的结果推导出现在的结果，我们要从2开始，一直推导到n才能结束嗷！！！（从底层往上层推导），典型的以空间换时间，保存每一次的执行结果，供下一次循环的时候调用。特点：For从小到大，构造数组存储中间过程数据，以推导出最后的结果，公式中间用到了递推关系嗷！！！



2. 做法二：递归：

```java
public int fib(int n){
    if (n == 0){
        return 0;
    }

    if (n == 1){
        return 1;
    }

    return(fib2(n-1) + fib2(n-2));
}
```

- 和上面相比，一样的，用到了递推的想法，不一样的地方在于。递归并没有存储中间的数据，而是单纯关系每一层之间的关系，并将底层关系理清除了而已嗷！！！
- 由于反复入栈出栈，递归的效率并不高，但是和上面相比，没有用到额外内存，内存消耗较低。



## 1137. 第 N 个泰波那契数

![image-20210901221638884](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210901221638.png)

```java
class Solution1137 {
    public int tribonacci1(int n) {
        if (n == 0){
            return 0;
        }
        if (n == 1){
            return 1;
        }
        if (n == 2){
            return 1;
        }
        int[] ints = new int[n+1];
        ints[0] = 0;
        ints[1] = 1;
        ints[2] = 1;

        for (int i = 3; i <= n; i++) {
            ints[i] = ints[i-1] + ints[i-2] + ints[i-3];
        }

        return ints[n];

    }

    public int tribonacci2(int n) {
        if (n == 0){
            return 0;
        }
        if (n == 1){
            return 1;
        }
        if (n == 2){
            return 1;
        }

        return tribonacci2(n-1) + tribonacci2(n-2) + tribonacci2(n-3);

    }
}
```

和509没啥本质区别嗷！！！

- 但是问题凸显十分明显：

![image-20210901221750238](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210901221750.png)

递归时间太长了，动规刚刚好嗷！！！



















# 方法论：

## KMP：

- 解决的是字符串的匹配问题嗷！！！
- 暴力匹配

## 二叉树：

### 递归遍历：

- 递归三部曲：
  1. 确定递归参数的参数和返回值
  2. 确定终止条件
  3. **单层**递归逻辑
- 传入二叉树的时候，传入根节点就可以了嗷！！！
- 主要是单层逻辑的编写还有终止条件的确定嗷！！！确定不好的话，可能就会一入递归深似海嗷！！！
- 上面这个递归的方法，对于所有的递归都适用的嗷！！！
- 这里适用三部曲进行一个分析：
  1. 确定递归的参数和返回值：二叉树的递归的参数比较少。例如这里递归遍历，传入的参数就是二叉树的头指针和记录结果的一个数据结果。
  2. 终止条件：什么时候终止，二叉树的特殊节点显然为终止节点，就是为NULL的时候哇！！！
  3. 单层逻辑：前序遍历，本层逻辑，应该先遍历自己，再遍历左子树，再遍历右子树嗷！！！
- 栈 ---> push root ---> 读取一个元素 ---> push right ---> push left ---> 循环...



### 非递归遍历：

- 又称为迭代法嗷！！！
- 递归逻辑本质上是适用栈这种结构来模拟和实现的嗷！！！
- 前序和后续都可以使用栈来解决哦！！！（可以通过颠倒顺序来解决嗷！！！）



## 回溯算法：

- 回溯和递归是相辅相成的嗷！！！回溯算法在递归下面嗷！！！

- 回溯本质是一个纯暴力的解法嗷！！！

- 目标问题：

  - 组合问题
  - 排列问题
  - 切割问题
  - 子集问题
  - 棋盘问题

- 如何理解回溯法：

  - 可以抽象为一个树形结构（N叉树）

  - ```java
    void backtracing(params){
        if(终止条件){
    		//收集结果
            return;
        }
        //单层搜索逻辑
        for(集合的元素集){
    	    //处理节点
            ...
            //递归
            ...
            //回溯操作
        }
        return;
    }
    ```



### 组合问题：

- 通过理性的暴力解决不理性的暴力嗷！！！
- 所有可以通过回溯算法解决的问题，都可以抽象为一棵树嗷！！！
- 思路：先把组合对应的树画出来再去coding解决这个问题嗷！！！
- 回溯三部曲：
  1. 确定递归函数的参数和返回值
  2. 确定递归的终止条件
  3. 确定单层递归的逻辑（单层搜索的逻辑）

```java
//二维数组result，放到全局变量而不是参数，可以让代码参数更少，更加简洁嗷！！！
//一维数组path
//n为要处理的数组，k为目标组合中元素的个数嗷！！！
void backtracing(n,k,startIndex){
    //到了叶子节点之后，就到递归终点啦！！！
    if(path.size == k){
        result.push(path);
        return;
    }
    
    for(i=startIndex,i<n;i++){
        //这里是递归操作
        path.push(i);
        backtracing(n,k,i+1);
        //这里是回溯操作
        path.pop();
    }
}
```

- LeetCode第77题的代码：

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class No77Combinations {
    public static void main(String[] args) {
        System.out.println("new Solution77().combine(4,2) = " + new Solution77().combine(4, 2));
    }
}

class Solution77 {
    List<List<Integer>> result = new ArrayList<java.util.List<Integer>>();
    LinkedList<Integer> path = new LinkedList<Integer>();

    public List<List<Integer>> combine(int n, int k) {
        combine2(n,k,1);
        return result;
    }

    void combine2(int n,int k,int startIndex){
        if (path.size() == k){
            result.add((List<Integer>) path.clone());
            return;
        }

        for (int i = startIndex; i <= n; i++) {
            path.addLast(i);
            combine2(n,k,i+1);
            path.removeLast();
        }

        return;
    }
}
```



### 组合问题的减枝操作：

- 减少时间和空间的浪费嗷！！！
- 这里主要是对于单层搜索逻辑优化：
- 剪枝一般的都是在遍历的for循环中做文章嗷！！！





## 快速排序：

- 每次排序，将一个元素排到对应的位置上嗷！！！

```java
public class QuickSort {
    @Test
    public void test() {
        int[] temp = {2,3,6,1,2,2,2,2,2};
        quickSort(temp,0,temp.length-1);
        for (int i = 0; i < temp.length; i++) {
            System.out.println("temp[i] = " + temp[i]);
        }
    }

    public static void quickSort(int[] array, int left, int right) {
        /*left -- 数组的左边界(例如，从起始位置开始排序，则l=0)
         *right -- 数组的右边界(例如，排序截至到数组末尾，则r=a.length-1)
         *array -- 待排序的数组
         */
        if (left < right) {
            int cur = partition(array, left, right);
            quickSort(array, left, cur-1); //递归
            quickSort(array, cur+1, right);
        }else {
            return;
        }
    }

    public static int partition(int[] arr, int left ,int right){
        int base = arr[left];
        while (left < right){
            while (left < right && arr[left] < base){
                left ++;
            }

            while (left < right && arr[right] > base){
                right --;
            }

            if (left < right && arr[left] == arr[right]){
                left ++;
            }else {
                swap(arr,left,right);
            }
        }

        return left;
    }

    public static void swap(int[] arr,int left , int right){
        int temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;
    }
}
```





## 动态规划：

- 动态规划五部曲：
  - dp数组（一维数组或者二维数组）以及下标的含义
  - 递推公式
  - dp数组如何初始化
  - **遍历顺序**
  - 打印dp数组
- 注意，这里的定义以及下标的含义会直接影响到dp数组的size，这个很重要，dp[m]和dp[m+1]有的时候，`dp[m+1]`更加好用嗷！！！下标就对上了物理含义嗷！！！



### 背包问题：

- 01背包：n种物品，每种物品只有一个
- 完全背包：n种物品，每种物品有无限个
- n重背包：n种物品，每种物品个数各不相同
- 例题：

![image-20210914112235066](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914112235.png)

暴力解法：

每个物品取或者不取，可以采用回溯进行暴力搜索嗷！！！说白了就是每种情况遍历，每个物品取或者不取有两种状态，因此时间复杂度为($2^{n}$)的复杂度嗷！！！

- 五部曲：

  1. 二维dp数组：`dp[i][j]`，i表示，任取下标为[0,i]的物品，放入容量为j的背包里面的最大价值。

  2. 放与不放物品i就是这个状态之前的状态嗷！！！

     `dp[i][j] = dp[i-1][j]` --- 不放物品i

     `dp[i-1][j-weight[i]]+ value[i]` --- 放入物品i

     `dp[i][j] = Math.max{上面两个嗷！！！}`

  3. 画表看情况先嗷！！！（初始化非常重要嗷！！！）

  4. 遍历（两层for循环嗷！！！）

  两层循环是可以颠倒的，可以通过元素推导的顺序推导出来

  5. 打印出dp数组最后的那个值就是答案嗷！！！




## 单调栈：

1. 单调栈里存放的元素是什么？

单调栈里只需要存放元素的**下标i**就可以了，如果需要使用对应的元素，直接T[i]就可以获取。

2. 单调栈里元素是递增呢？ 还是递减呢？

**注意一下顺序为 从栈头到栈底的顺序**，因为单纯的说从左到右或者从前到后，不说栈头朝哪个方向的话，大家一定会越看越懵。



- 使用单调栈主要有三个判断条件：
  1. 当前遍历的元素T[i]小于栈顶元素T[st.top()]的情况
  2. 当前遍历的元素T[i]等于栈顶元素T[st.top()]的情况
  3. 当前遍历的元素T[i]大于栈顶元素T[st.top()]的情况

```java
public int[] dailyTemperatures(int[] temperatures) {
    Stack<Integer> stack = new Stack<>();
    int[] result = new int[temperatures.length];
    if (temperatures.length == 1){
        return result;
    }

    for (int i = 0; i < temperatures.length; i++) {
        while (stack.size() > 0 && temperatures[i] > temperatures[stack.peek()]){
            Integer top = stack.pop();
            result[top] = i - top;
        }

        stack.push(i);
    }

    return result;
}
```









