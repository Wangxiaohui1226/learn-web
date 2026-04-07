leetcode的题目

1.给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

```
/**
 * @param {number[]} height
 * @return {number}
 */
var maxArea = function(height) {
   let left=0;
    let right=height.length-1;;
       let maxdata=''
    while (left < right){
       let h = Math.min(height[left], height[right])
       let w = right - left
        maxdata = Math.max(maxdata, h * w)
        if (height[left] < height[right]){
          left += 1
        }else {
         right -= 1
        }
    
    }
        console.log("max",maxdata)
    return maxdata
};
```

2.给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。





**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

 

```
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
   let len = nums.length; // 定义数组长度
    let ans = []; // 定义结果数组
    // 1.如果数量小于 3，直接返回
    if(len<3){
        return [];
    }
    // 2.排序，从小到大
    nums.sort((a,b)=>a-b);
    console.log(nums)
    // 3.如果排好序的第一个元素大于 0，或最后一个元素小于 0，直接返回
    if(nums[0]>0 || nums[len-1]<0){
        return [];
    }
    // 4.开始遍历寻找三个元素
    // 第一层 for 循环，找到第一个元素
    
    for(let i=0;i<len;i++){// 获取第一个元素
        let first = nums[i];
        // 制造一个 target，变为两数相加，因为  first+second+third=0; 所以 second+third=-fisrt
        let target = -(first);
        // 对第一个元素去重，如果重复直接跳过本次循环
        if(i>0 && nums[i]===nums[i-1]){
            continue;
        }
        let left = i+1;
        let right = len-1;
        while(left<right){
            if((nums[left]+nums[right])===target){
                // 如果刚好满足 target，则 push结果集，并同时回缩 left 和 right
                ans.push([first,nums[left],nums[right]]);
                // 先进行 left 和 right 各自指针的改变
                left++;
                right--;
                // 再判断，避免 left 和 right 重复，如果重复，则跳至下一个
                while(nums[left]==nums[left-1]){
                    left++;
                }
                while(nums[right]==nums[right+1]){
                    right--;
                }
            }else if((nums[left]+nums[right])<target){
                // 如果小于 target，则还需要 left 继续变大才能满足
                left++;
            } else{
                // 如果大于 target，则需要减小 right
                right--;
            }
        }
    }
    return ans;
};
```

