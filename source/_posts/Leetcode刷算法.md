---
title: Leetcode刷算法
date: 2021-04-05 15:53:08
tags: [算法,Python]
Categories: 算法
---

# 这是蓝桥杯前临阵磨枪的刷Leetcode



<!--more-->



## 数组:

1. 矩阵转置：
   - 首先找到要点，第一行变成了第一列的元素，或者说，每一列的第n个元素变成了第n行，就是靠这个思路去下手的

```python
# 给你一个二维整数数组 matrix， 返回 matrix 的 转置矩阵 。
#
# 矩阵的 转置 是指将矩阵的主对角线翻转，交换矩阵的行索引与列索引。

class Solution:
    def transpose(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[List[int]]
        """
        length = len(matrix[0])
        # print(length)
        result = []
        for i in range(length):
            # print(i)
            temp = []
            for j in matrix:
                temp.append(j[i])
            result.append(temp)
        return result
```

2. 有序数组合并：

   - 题目中有告诉，两个数组本身就是有序的，所以我想到可以拼在一起用冒泡算法等，但是！！！

   - python内自带了排序算法！！！完全可以先拼在一起再去排序，这么说来好像没有用到本身有序这个条件orz。

   - 一些函数的使用：

     - python内置的sort方法，可以对于列表本身进行排序

     - sorted可以对于所有可迭代对象进行排序

     - **sort 与 sorted 区别：**

       sort 是应用在 list 上的方法，sorted 可以对所有可迭代的对象进行排序操作。

       list 的 sort 方法返回的是对已经存在的列表进行操作，无返回值，而内建函数 sorted 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。

     - 从某个位置截取到尾：

       `nums1[m:]`

```python
class Solution:
    def merge(self, nums1, m, nums2, n):
        """
        :type nums1: List[int]
        :type m: int
        :type nums2: List[int]
        :type n: int
        :rtype: None Do not return anything, modify nums1 in-place instead.
        """
        nums1[m:] = nums2
        nums1.sort()

        return nums1
```

3. 寻找两个正序数组中位数：
   - 思路同上，相加之后直接暴力排序

```python
class Solution(object):
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        num = nums1+nums2
        num.sort()
        if len(num) %2 == 1:
            return num[len(num)//2]
        else:
            # print(len(num))
            # print(len(num)/2)
            temp = (num[int(len(num)/2)]+num[int(len(num)/2-1)])/2.0
            return temp
```

















## DP(动态规划)：

- 要解一个给定问题，我们需要解其不同部分（即子问题），再根据子问题的解以得出原问题的解。

- 最优子结构规定的是子问题与原问题的关系

  动态规划要解决的都是一些问题的最优解，即从很多解决问题的方案中找到最优的一个。当我们在求一个问题最优解的时候，如果可以把这个问题分解成多个子问题，然后递归地找到每个子问题的最优解，最后通过一定的数学方法对各个子问题的最优解进行组合得出最终的结果。总结来说就是一个问题的最优解是由它的各个子问题的最优解决定的。

- 递归是实现DP的一种手段吧，感觉上差不太多

- 总结： 解决动态规划问题最难的地方有两点：

  如何定义 f(n)f(n)
  如何通过 f(1)f(1), f(2)f(2), … f(n - 1)f(n−1) 推导出 f(n)f(n)，即状态转移方程


例子：

`给定一个无序的整数数组，找到其中最长上升子序列的长度。`

`示例：输入: [10,9,2,5,3,7,101,18]
输出: 4
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是4。
考虑能否将问题规模减小`



解决：

考虑能否将问题规模减小
将问题规模减小的方式有很多种，一些典型的减小方式是动态规划分类的依据，例如线性，区间，树形等。这里考虑数组上常用的两种思路：

每次减少一半：如果每次将问题规模减少一半，原问题有[10,9,2,5]，和[3,7,101,18]，两个子问题的最优解分别为 [2,5] 和 [3,7,101]，但是找不到好的组合方式将两个子问题最优解组合为原问题最优解 [2,5,7,101]。

每次减少一个：记 f(n)f(n) 为以第 n 个数结尾的最长子序列，每次减少一个，将原问题分为 f(n-1), f(n-2), ..., f(1)，共 n−1 个子问题。n - 1 =7 个子问题以及答案如下：

[10, 9, 2, 5, 3, 7, 101] -> [2, 5, 7, 101]
[10, 9, 2, 5, 3, 7] -> [2, 5, 7]
[10, 9, 2, 5, 3] -> [2, 3]
[10, 9, 2, 5] -> [2, 5]
[10, 9, 2] -> [2]
[10, 9] -> [9]
[10] -> [10]

已经有 7 个子问题的最优解之后，可以发现一种组合方式得到原问题的最优解：f(6)f(6) 的结果 [2,5,7], 7 < 187<18，同时长度也是 f(1)=~f(7= 中，结尾小于 18 的结果中最长的。f(7)f(7) 虽然长度为 4 比 f(6)f(6) 长，但结尾是不小于 18 的，无法组合成原问题的解。

1. 递归：

- 通过状态转移方程实际就可以写出递归

2. 自顶向下（记忆化）

- 在递归地求解子问题 f(1), f(2)... 过程中，将结果保存到一个表里，在后续求解子问题中如果遇到求过结果的子问题，直接查表去得到答案而不计算。

3. 自底向上（迭代）



作者：力扣 (LeetCode)
链接：https://leetcode-cn.com/leetbook/read/dynamic-programming-1-plus/xcos8s/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



`重点：状态转移方程`





1. 给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

```python
# 给定一个整数数组
# nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

class Solution:
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        # 比较显然用动态规划
        # 贪心算法：如果子序列小于0，那么这个子序列就被抛弃了，他对于后面的元素只有负的影响
        # dp：思路类似，只要结果前面序列的结果不为负数，对于后面一个元素的结果是正的，就有意义。需要额外的一个值记录“前面不为0的序列的和”，当它为0，即无法对与后面的元素造成好的影响时，舍弃掉这个值，让它等于下一个元素的值。如果为正的话，证明确实可以起到好的作用，这个时候就加上后面那个值并且存回原来的位置。反复进行上述过程。

        # 我的思考盲区：左边有意义，右边怎么办呢？实际上，从左往右，是以此推导的，从左边开始到右边每个元素（往左的最大加和的结果）。
        # 也就是说，这样覆盖到了每一个元素，思路：从一边下手
        store = 0
        for index in range(len(nums)):
            print(index)
            if store < 0:
                store = nums[index]
            else:
                store += nums[index]
                nums[index] = store
        return max(nums)
```













































## DFS:

- 树的相关概念，python中用数组生成树，树的遍历都很重要嗷

用数组创建树：

```python
class createTree:
    def treeCreate(self,listTree: list,start: int):
        if listTree[start] == None:
            return None


        root = TreeNode(listTree[start])
        lnode = 2 * start + 1
        rnode = 2 * start + 2

        if(lnode> len(listTree) - 1):
            root.left = None
        else:
            root.left = self.treeCreate(listTree,lnode)

        if (rnode > len(listTree) - 1):
            root.right = None
        else:
            root.right = self.treeCreate(listTree,rnode)

        return root
```



树的遍历：

```python
class treeMethod:
    def viewTreeFirst(self,root):
        # 先序遍历
        if root:
            print(root.val)
            self.viewTreeFirst(root.left)
            self.viewTreeFirst(root.right)

    def viewTreeMiddle(self,root):
        # 中序遍历
        if root:
            self.viewTreeMiddle(root.left)
            print(root.val)
            self.viewTreeMiddle(root.right)

    def viewTreeLast(self,root):
        # 后序遍历
        if root:
            self.viewTreeLast(root.left)
            self.viewTreeLast(root.right)
            print(root.val)
```




1. 检查一颗树的平衡性

```python
class Solution:
    def Depth(self,root):
        if root:
            return 1+max(self.Depth(root.left), self.Depth(root.right))
        return 0


    def isBalanced(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root:
            return True
        if abs(self.Depth(root.left)-self.Depth(root.right)) > 1:
            return False
        # 递归对于左右子树进行判断，如果有一个不是平衡二叉树，那么整棵树都不是平衡二叉树
        return self.isBalanced(root.left) and self.isBalanced(root.right)
```

- 题目必须要求我们引入概念：树的深度，树的深度 = 左右子树深度的最大值+1（找到递归条件）
- 若一棵树为平衡二叉树，则其所有的子树都是平衡二叉树（递归条件）





2. 检查两棵树是否相同：

```python
# 给你两棵二叉树的根节点 p 和 q ，编写一个函数来检验这两棵树是否相同。
#
# 如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

# Definition for a binary tree node.
class TreeNode(object):
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
class Solution(object):
    def isSameTree(self, p, q):
        """
        :type p: TreeNode
        :type q: TreeNode
        :rtype: bool
        """
        if p==None and q==None:
            return  True
        elif p==None or q==None:
            return False
        elif p.val != q.val:
            return False
        else:
            return self.isSameTree(p.left,q.left) and self.isSameTree(p.right,q.right)

```



- summary一下树的递归规律哈，上两题都时很典型的树的递归的模式，比如这道题，问树相同吗？每个结点相同吗？这个时候，对于树来说，需要对于每个`相同位置的结点`进行比较。所以就有了这个算法，对于本层比较，再分别递归对于下层比较。对于上面来说，问题则是每一个结点守恒吗？下层的守恒是可以推出上层的守恒的，算法由此而来。





3. 镜像树树的判断：

```python
# 给定一个二叉树，检查它是否是镜像对称的。
#
# 例如，二叉树[1,2,2,3,4,4,3] 是对称的。
#
#     1
#    / \
#   2   2
#  / \ / \
# 3  4 4  3


class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        # 问题转化，两颗树是否是对称的，起始是由左右子树来确定的，左右子树如果是镜像对称的，那它就是镜像对称的，但是这个结论是对于根来说的

        def isJingXiang(p,q):
            if p == None and q==None:
                return True
            elif p==None or q==None:
                return False
            elif p.val != q.val:
                return False
            else:
                return isJingXiang(p.left,q.right) and isJingXiang(p.right and q.left)


        if root == None:
            return True
        else:
            return isJingXiang(root.left,root.right)
```

- 这里其实思路有点不一样，就是递归的方式改变了，要仔细思考，这个递归和上面的不一样嗷
- 1. 首先，根很特别，它只有一个元素，除了根以后，后续的判断就是对于两个结点的判断。
  2. 两个结点的判断你会发现很有意思，右边的往左边走，左边的就要往右边走，这是递归！！！
  3. 只要有一个不符合，整个树就不是我们要的镜像树树嗷！！！
  4. 判断`这两个结点是否成立`，然后基于这一层的判断，引入下一层的判断嗷！！！
- 递归写多了就好，加油加油冲冲冲！！！







## 贪心算法：

- 贪心算法步骤：
  - 步骤1：从某个初始解出发；
  - 步骤2：采用迭代的过程，当可以向目标前进一步时，就根据局部最优策略，得到一部分解，缩小问题规模；
  - 步骤3：将所有解综合起来。
- 其实思维很好理解，在目标前提下，每一步都选择符合最能达成目标的策略即可。



1. 种最多的花的问题：

```python
# 假设有一个很长的花坛，一部分地块种植了花，另一部分却没有。可是，花不能种植在相邻的地块上，它们会争夺水源，两者都会死去。
#
# 给你一个整数数组flowerbed 表示花坛，由若干 0 和 1 组成，其中 0 表示没种植花，1 表示种植了花。
# 另有一个数 ，能否在不打破种植规则的情况下种n朵花？能则返回 true ，不能则返回 false。
class Solution(object):
    def canPlaceFlowers(self, flowerbed, n):
        """
        :type flowerbed: List[int]
        :type n: int
        :rtype: bool
        """

        record = []
        sum = 0
        # 找到为1的两个位置，确定中间能种的花的量
        for index,num in enumerate(flowerbed):
            if num == 1:
                record.append(index)
            else:
                continue

        # print(record)

        if len(record) == 0:
            sum += len(flowerbed)//2 + len(flowerbed)%2
        else:

            for i in range(len(record)-1):
                pre = record[i]
                aft = record[i+1]
                # print(pre,aft)
                sum += (aft-pre)//2 - 1

            print(record[0])
            print(sum)

            sum += record[0]//2 + (len(flowerbed)-1-record[-1])//2

        if sum >= n:
            return True
        else:
            return False
```

- 我们这儿把每种情况分的很细，所谓的贪心，就是要每种情况下尽可能多种花，而不管别的情况。
- 还有一种解法是设置两个指针，一个默认为-1，另外一个在列表中遍历，如果找到一个元素为1，就计算两者之间的种的花花的数量.然后移动指针，pre后移，然后另外一个指针搜索下一个1所在的位置。





2. #### [非递增顺序的最小子序列](https://leetcode-cn.com/problems/minimum-subsequence-in-non-increasing-order/)

```python
# 给你一个数组 nums，请你从中抽取一个子序列，满足该子序列的元素之和 严格 大于未包含在该子序列中的各元素之和。
#
# 如果存在多个解决方案，只需返回 长度最小 的子序列。如果仍然有多个解决方案，则返回 元素之和最大 的子序列。
#
# 与子数组不同的地方在于，「数组的子序列」不强调元素在原数组中的连续性，也就是说，它可以通过从数组中分离一些（也可能不分离）元素得到。
#
# 注意，题目数据保证满足所有约束条件的解决方案是 唯一 的。同时，返回的答案应当按 非递增顺序 排列。
class Solution(object):
    def minSubsequence(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        result = []

        result.append(max(nums))
        nums.remove(max(nums))
        while sum(result) <= sum(nums):
            result.append(max(nums))
            nums.remove(max(nums))

        return result
```

- 这个思路比较简单，每次贪心挑出最容易满足条件的最大值即可，循环，直到某次成功就可







## 二分查找算法：

使用条件：

二分查找的本质是在一组**单调**、**有上下界**、**可索引**的数据中搜索目标值，三大条件缺一不可。

















## 链表：

- 一种数据结构，比起树来说相对简单，里面涉及一些比较简单的基本的算法

用列表创建链表，遍历显示链表。以后链表题目的debug的工具类。

```python
class NodeMethod(object):
    def genListNode(self,list1):
        if len(list1) == 0:
            return None
        else:
            head = ListNode(val=list1[0])
            pre = head
            for i in range(1,len(list1)):
                next = ListNode(list1[i])
                pre.next = next
                pre = pre.next
            return head

    def showListNode(self,head):
        while head.next!=None:
            print(str(head.val)+"->",end="")
            head = head.next
        print(head.val)
```



1. 删除排序链表中的重复元素

- 一个指针的做法：

```python
class Solution:
    def deleteDuplicates(self, head):
        if not head:
            return head

        cur = head
        while cur.next:
            if cur.val == cur.next.val:
                cur.next = cur.next.next
            else:
                cur = cur.next

        return head
# 这里只用了一个指针，当cur有下一个元素的时候，判断这个元素的内容和下一个是否相等，不相等的话就改当前元素和后面的后面连起来。
```

- 两个指针的做法：

```python
class Solution(object):
    def deleteDuplicates(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if head == None:
            return None
        else:
            pre = head
            after = head.next
            while after != None:
                if after.val == pre.val:
                    after = after.next
                else:
                    pre.next = after
                    pre = after
                    after = after.next
            pre.next = after
            return head
```

2. 合并两个有序链表

- 暴力解法：

```python
class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        cur = ListNode(0,None)
        head = cur

        while l1 and l2:
            if l1.val <= l2.val:
                print("l1的值为：" + str(l1.val))
                cur.next = l1
                l1 = l1.next
                cur = cur.next
            else:
                print("l2的值为：" + str(l2.val))
                cur.next = l2
                l2 = l2.next
                cur = cur.next

        if l1:
            cur.next = l1
        if l2:
            cur.next = l2

        return head.next
```

- 递归：

```python
def mergeTwoLists(self, l1, l2):
    if not l1: return l2  # 终止条件，直到两个链表都空
    if not l2: return l1
    if l1.val <= l2.val:  # 递归调用
        l1.next = self.mergeTwoLists(l1.next, l2)
        return l1
    else:
        l2.next = self.mergeTwoLists(l1, l2.next)
        return l2
```



## 双指针：

双指针代表的是 可以作为容器边界的所有位置的范围。在一开始，双指针指向数组的左右边界，表示 数组中所有的位置都可以作为容器的边界，因为我们还没有进行过任何尝试。在这之后，我们每次将 对应的数字较小的那个指针 往 另一个指针 的方向移动一个位置，就表示我们认为 这个指针不可能再作为容器的边界了。

为什么对应的数字较小的那个指针不可能再作为容器的边界了？

在上面的分析部分，我们对这个问题有了一点初步的想法。这里我们定量地进行证明。

考虑第一步，假设当前左指针和右指针指向的数分别为 x 和 y，不失一般性，我们假设 x≤y。同时，两个指针之间的距离为 t。那么，它们组成的容器的容量为：

`min(x, y) * t = x * t`

我们可以断定，如果我们保持左指针的位置不变，那么无论右指针在哪里，这个容器的容量都不会超过 x∗t 了。注意这里右指针只能向左移动，因为 我们考虑的是第一步，也就是指针还指向数组的左右边界的时候。

后面就是一个递归，反复这个过程就OK了嗷！！！

















## 应试小技巧：

- `[int(xs) for xs in input().split(" ")]`，注意一下这个split中间是有个空格的嗷！！！
- 经常会用到指针，python中有一种常用的用法：
  - 指针一般用于指向下标。
  - 一般头指针pre为0或者-1。
  - 通过指针移动能实现很多功能，例如替换原列表的元素来减少使用的空间啊之类的。
- python删除元素：
  - pop , 删除某个位置
  - remove , 删除某个特定元素
  - del ， 删除某个列表中的某个特定元素
  - 可以通过循环删除列表中所有特定元素
  - 注意这个都是在现有list基础上删除的，也就是说，再次使用这个引用对象就是删除后的结果
- enumerate():
  - 同时拿到索引和元素
  - 便于对于特定位置上的值进行修改
- python是append和pop来实现栈的功能
- replace方法：
  - str.replace(old,new[,max])
  - Python中的replace()方法是把字符串中的old（旧字符串）替换成new（新字符串），如果指定第三个参数max，则替换次数不超过max次（将旧的字符串用心的字符串替换不超过max次）。
  - 也可以用循环嘛，如果存在就换，一样可以换完嗷！！！

- 获取列表中的最大值和最大值的下标

```python
test = [1,2,3]
a = max(test)
b = test.index(max(test))
```

- python中的指针可以用数组下标来表示嗷，例如删除一个列表中某个特定元素，可以用“双指针法来解决”（一个快指针，一个慢指针）嗷。
- 注意python语言的一些特性和内置函数，往往非常好用有奇效嗷！！！例如：`给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。`，显然用In和index就可以秒杀。
- python查询列表中某个元素的数量用count函数即可，测试后发现这个函数对于list和str对象都是使用的嗷！！！
- 滑动窗口等字符串比较问题的实现都是使用上面提到的“指针”的方式来实现的嗷。
- startswith和endswith函数可以用于字符串判断前缀后缀。
- //除法和%十分好用嗷！！！
- 字符串截取: `[:2],[-2:]`，第一个是截取前两个，第二个是截取最后两个，`[-1]`表示最后一个
- 链表中，有的时候头指针会比较方便，例如我刚刚做到的两个链表连成一个，若没有头指针，逻辑就容易乱，有头指针会方便一点点嗷！！！（至少逻辑更加清楚了）
- `print("{:.2f}".format(sum(lst)/n))`，这里是format的标准格式，{:.2f} 表示打印输出小数点保留两位小数，{:.0f}表示打印输出不保留小数位。format后面的值会自动填充到前面的字符串中，以两位小数float的形式输出。还有另外一种形式，效果是一样的，最后输出的类型都是str类型，就用这种就可以啦撒，这个可以自动四舍五入的，比较方便。
- `ord`和`chr`能够机智转换`ascii`和字符。
- `upper , isupper , lower , islower , istitle`。
- 很多题目的多位数字可以通过`a*10000+b*100`等方式来实现。
- pow函数可以在python中被用于求幂。

![image-20210417162133128](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210417162133128.png)

- exit可以用于推出程序嗷！！有的时候多层循环找到对应的值了不好退出直接给他来一个exit，欸，舒服了。

- ```python
  y = int(2.5) # y 将是 2
  ```

- 突然想到一种方法能够把字符串中所有相同字符给它替换掉：

  - 要用到count函数，`str.count("a")`
  - `str.replace("a","100",str.count("a"))`

- 列表删除重复元素：`list(set(list))`

- 