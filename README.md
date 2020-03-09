[1.两数之和]( #1.两数之和)

[2.有效的括号](#2.有效的括号)

[3.最大子序和](#3.最大子序和)

[4.多数元素](#4.多数元素)

[5.买卖股票的最佳时机](#5.买卖股票的最佳时机)

[6.找到所有数组中消失的数字](#6.找到所有数组中消失的数字)

[7.最短无序连续子数组](#7.最短无序连续子数组)

[8.翻转二叉树](#8.翻转二叉树)

[9.对称二叉树](#9.对称二叉树)

[10.合并二叉树](#10.合并二叉树)

[11.合并排序的数组](#11.合并排序的数组)

# 1.两数之和

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

解题思路：
利用hash的key-value键值对，将target-nums[当前下标]作为key查找，时间复杂度为O（n）

```)
func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var targetNums = [Int : Int]();
            for i in 0..<nums.count {
                targetNums[nums[i]] = i
            }
            for j in 0..<nums.count {
                if targetNums[target - nums[j]] != nil && targetNums[target - nums[j]]! > 0 && j != targetNums[target - nums[j]]  {
                    return [j, targetNums[target - nums[j]]!]
                }
            }    
        return [0 , 0]
}
```

执行用时 : 36 ms, 在所有 Swift 提交中击败了98.81%的用户
内存消耗 :20.9 MB, 在所有 Swift 提交中击败了5.12%的用户

# 2.有效的括号

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
有效字符串需满足：
左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。
示例 1:
输入: "()"
输出: true
示例 2:
输入: "()[]{}"
输出: true
示例 3:
输入: "(]"
输出: false
示例 4:
输入: "([)]"
输出: false
示例 5:
输入: "{[]}"
输出: true

思路：一开始的想法是能不能通过判断个数去解决这个问题，发现这只是其中的一个条件，这个问题最重要的部分在于顺序，所以就想到了利用栈空间的先进后出的特性，利用循环将栈中的最后一个字符与字符串中的字符作比对，匹配到了就出栈，不匹配就入栈。代码如下：
```
class Solution {
    func isValid(_ s: String) -> Bool {
        if s == "" {
            return true
        } else if s.count % 2 != 0 {
            return false
        }
        
        var compareArray = (String(s[s.startIndex]))
        for i in 1..<s.count {
            //排除比较的数组中为空的情况
            if compareArray.count == 0 {
                compareArray.append(String(s[s.index(s.startIndex, offsetBy: i)]))
                continue;
            }
            
            if elemtentValid(String(compareArray.last!), String(s[s.index(s.startIndex, offsetBy: i)])) {
                compareArray.removeLast()
            } else {
                let compareString = String(s[s.index(s.startIndex, offsetBy: i)])
                if compareString == ")" || compareString == "]" || compareString == "}"  {
                    return false
                } else {
                    compareArray.append(compareString)
                }
            }
        }
        
        return compareArray.count == 0 ? true : false
    }
    
    func elemtentValid(_ d: String, _ c: String) -> Bool {
        if d + c == "()" || d + c == "[]" || d + c == "{}"  {
            return true;
        }
        return false;
    }
}
```
后面发现这个方法太繁琐了，因为自己swift语法不熟悉导致了各种判断，执行效率不高，后面看到一位大神的swift代码，觉得很好，代码如下：
```
class Solution {
    func isValid(_ s: String) -> Bool {
        var stack = [Character]()
        
        for (_, char) in s.enumerated() {
            if let topChar = stack.last {
                if  (topChar == ("(") as Character && char == (")") as Character) ||
                    (topChar == ("[") as Character && char == ("]") as Character) ||
                    (topChar == ("{") as Character && char == ("}") as Character){
                    stack.removeLast();
                }
                else {
                    stack.append(char);
                }
            }
            else {
                stack.append(char);
            }
        }
        
        return stack.isEmpty
    }
}
```
思路清晰，简洁。
膜拜。

# 3.最大子序和

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

思路：

for循环遍历，用数组第0个元素与0+1的元素比，取最大值，得到的值与0+1+2的元素再比，以此类推

```
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        var maxNum = nums[0]
        var sum = 0
        for i in 0..<nums.count {
            if sum > 0 {
                sum += nums[i]
            } else {
                sum = nums[i]
            }
            maxNum = max(maxNum, sum)
        }
        return maxNum
    }
}
```

# 4.多数元素

给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

示例 1:

输入: [3,2,3]
输出: 3

示例 2:

输入: [2,2,1,1,1,2,2]
输出: 2

思路：一开始想的是暴力遍历，用每一个元素的count值和[n/2]对比返回，代码如下：

```
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        for num in nums {
            var sum = 0
            for i in nums {
                if num == i {
                    sum += 1
                }
            }
            
            if sum > nums.count / 2 {
                return num
            }
        }
        return -1
    }
}
```

但是执行完以后提交，结果执行结果超出时间限制。。。后面看到了先排序后找众数的想法，很简洁，拿到有序数组的第n/2个元素就是结果。代码如下：

```
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        let array = nums.sorted()
        return array[nums.count / 2]
    }
}
```

然后又看到了摩尔投票法，思路就是如果把该众数记为+1，把其他数记为−1，将它们全部加起来，和是大于 0 的。那么开始定义一个当前的元素tempNum和当前的和count，遍历当前数组，如果tempNum与数组元素相同则加一，不同则减一，如果count等于0，则将数组元素赋值给tempNum，count加一。代码如下：

```
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        var tempNum = 0
        var count = 0
        for num in nums {
            if count == 0 {
                tempNum = num
                count += 1
            } else if tempNum == num {
                count += 1
            } else {
                count -= 1
            }
        }
        return tempNum
    }
}

```

# 5.买卖股票的最佳时机

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

示例 1:

输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
示例 2:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

思路：

其实就是找逆差最大。定义一个最大逆差值reg，一个最小值minPrice,遍历数组元素与minPrice做差值，与reg取最大值，最后得到的数字就是最大利润。代码如下：

```
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        if prices.isEmpty {
            return 0
        }
        var reg = 0
        var minPrice = prices[0]
        for i in 1..<prices.count {
            if minPrice > prices[i] {
                minPrice = prices[i]
            } else {
                reg = max(prices[i] - minPrice, reg)
            }
        }
        return sum
    }
}
```

# 6.找到所有数组中消失的数字

给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, n] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

示例:

输入:
[4,3,2,7,8,2,3,1]

输出:
[5,6]

思路：一开始想的是给数组排序，然后判断nums[i]+1和nums[n+1]是否相等，不相等就是没出现的数字。然后又想到了直接给nums[nums[i] - 1]置为1，然后遍历数组，不是1的就是不存在的。代码如下：

```
class Solution {
    func findDisappearedNumbers(_ nums: [Int]) -> [Int] {
        var array = Array(repeating: 0, count: nums.count)
        var resultArray = [Int]()
        for i in 0..<array.count {
            array[nums[i] - 1] = 1
        }
        for i in 0..<array.count {
            if array[i] != 1 {
                resultArray.append(i+1)
            }
        }
        return resultArray
    }
}
```

# 7.最短无序连续子数组

给定一个整数数组，你需要寻找一个连续的子数组，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

你找到的子数组应是最短的，请输出它的长度。

示例 1:

输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
说明 :

1.输入的数组长度范围在 [1, 10,000]。
2.输入的数组可能包含重复元素 ，所以升序的意思是<=。

思路：一开始的想法是遍历两次，根据元素大小来判断左右边界，最后想了一下，可以把遍历两次变成一次操作来完成，代码如下：

```
class Solution {
    func findUnsortedSubarray(_ nums: [Int]) -> Int {
        var max = nums[0];
        var min = nums[nums.count-1];
        var l = 0, r = -1;
        for i in 0..<nums.count {
            if(max>nums[i]){
                r = i;
            }else{
                max = nums[i];
            }
            if(min<nums[nums.count-i-1]){
                l = nums.count-i-1;
            }else{
                min = nums[nums.count-i-1];
            }
        }
        return r-l+1;
    }
}
```

# 8.翻转二叉树

翻转一棵二叉树。

示例：

输入：

     4
    /   \
    2     7
    / \   / \
    1   3 6   9

输出：

     4
    /   \
    7     2
    / \   / \
    9   6 3   1

备注:
这个问题是受到 Max Howell 的 原问题 启发的 ：
谷歌：我们90％的工程师使用您编写的软件(Homebrew)，但是您却无法在面试时在白板上写出翻转二叉树这道题，这太糟糕了。

经典的反转二叉树来了。。。。

代码如下：

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.left = nil
 *         self.right = nil
 *     }
 * }
 */
class Solution {
    func invertTree(_ root: TreeNode?) -> TreeNode? {
        if root == nil {
            return nil
        }
        let left: TreeNode? = invertTree(root!.left)
        let right: TreeNode? = invertTree(root!.right)
        root!.right = left
        root!.left = right
        return root
    }
}
```

# 9.对称二叉树

给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

```
    1
   / \
  2   2
   \   \
   3    3
```

思路：看到这个就想起直接递归，只要同一层级的左右子树相等，那么就是对称二叉树，代码如下：


```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.left = nil
 *         self.right = nil
 *     }
 * }
 */
class Solution {
    func isSymmetric(_ root: TreeNode?) -> Bool {
        return isMirror(root, root)
    }
    
    func isMirror(_ root1: TreeNode?,_ root2: TreeNode?) -> Bool {
        if root1 == nil && root2 == nil {
            return true
        }
        if root1 == nil || root2 == nil {
            return false
        }
        return root1?.val == root2?.val && isMirror(root1?.left, root2?.right) && isMirror(root1?.right, root2?.left)
    }
}
```


# 10.合并二叉树

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。

示例 1:

输入: 
```
    Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7         
```

输出: 
合并后的树:

```
         3
        / \
       4   5
      / \   \ 
     5   4   7
```

注意: 合并必须从两个树的根节点开始。

思路：看到这个就直接想到递归相加，判断边界条件。代码如下：

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.left = nil
 *         self.right = nil
 *     }
 * }
 */
class Solution {
    func mergeTrees(_ t1: TreeNode?, _ t2: TreeNode?) -> TreeNode? {
        if t1 == nil && t2 == nil {
            return nil
        } else if t1 == nil {
            return t2
        } else if t2 == nil {
            return t1
        }
        let sum = TreeNode(t1!.val + t2!.val)
        sum.left = mergeTrees(t1?.left, t2?.left)
        sum.right = mergeTrees(t1?.right, t2?.right)
        return sum
    }
}
```

# 11.合并排序的数组

给定两个排序后的数组 A 和 B，其中 A 的末端有足够的缓冲空间容纳 B。 编写一个方法，将 B 合并入 A 并排序。

初始化 A 和 B 的元素数量分别为 m 和 n。

示例:

输入:

A = [1,2,3,0,0,0], m = 3

B = [2,5,6],       n = 3

输出: [1,2,2,3,5,6]

说明:

A.length == n + m

思路：由于两个数组已经排序，所以经过遍历，将两个数组最小的值比对，将最小的值放入新数组中。代码如下：

```
class Solution {
    func merge(_ A: inout [Int], _ m: Int, _ B: [Int], _ n: Int) {
        var sort = [Int]()
        var indexa = 0
        var indexb = 0
        while indexa < m || indexb < n {
            if indexa == m {
                sort.append(B[indexb])
                indexb += 1
            } else if indexb == n {
                sort.append(A[indexa])
                indexa += 1
            } else if A[indexa] < B[indexb] {
                sort.append(A[indexa])
                indexa += 1
            } else {
                sort.append(B[indexb])
                indexb += 1
            }
        }
        A = sort
    }
}
```

