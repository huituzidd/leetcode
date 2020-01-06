**1.两数之和**

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

执行用时 : 36 ms, 在所有 Swift 提交中击败了98.81%的用户
内存消耗 :20.9 MB, 在所有 Swift 提交中击败了5.12%的用户
