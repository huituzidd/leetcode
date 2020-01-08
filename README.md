[1.两数之和]( #1.两数之和)

[2.有效的括号](#2.有效的括号)

[3.最大子序和](#3.最大子序和)

#1.两数之和

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

#2.有效的括号

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

#3.最大子序和

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
