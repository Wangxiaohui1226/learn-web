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

3.


给定一个字符串 s ，请你找出其中不含有重复字符的 最长 子串 的长度。

 

示例 1:

输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。注意 "bca" 和 "cab" 也是正确答案。
示例 2:

输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。


/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
        let len=0;
        let index=0;
        for(let i=0,j=0;j<s.length;j++) {
            index=s.substring(i,j).indexOf(s[j])
           console.log("index",index);
           if(index!==-1) {
            //满足重复的
            i=index+i+1;
           }
            len=Math.max(len,j-i+1)
        }
        console.log(len)
        return len
      
};


4.给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

 

示例 1:

输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
 示例 2:

输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
 

提示:

1 <= s.length, p.length <= 3 * 104
s 和 p 仅包含小写字母

findAnagrams2 = (s, p) =>{
  console.log("进来了吗")
    let map = {}
    let len = p.length
    let sum = p.length
    let s_len=0
    let res=[]
    for(let i=0;i<len;i++){
        if(map[p[i]]){
            map[p[i]]=map[p[i]]+1
            console.log("111",map[[p[i]]])
        }else{
            map[p[i]]=1
        }
    }
    for(let i=0;i<s.length;i++){
        //如果长度匹配框等于3，那么出最前面那个
        if(s_len==len){
            //如果出的那个是p中的字母，那么map中增加
            if(map[s[i-len]]!== undefined){
                if(map[s[i-len]]>=0){
                    sum++
                }
                map[s[i-len]]=map[s[i-len]]+1
            }
            s_len--
        }
        //如果进的那个是map中的字母
        if(map[s[i]]!== undefined){
            //还有所需
            if(map[s[i]]>0){
                //那么总所需--
                sum--
            }
            map[s[i]]=map[s[i]]-1
        }
        if(sum===0){
            res.push(i-len+1)
        }
        s_len++
    }
    return res
};

5.
给你一个整数数组 nums 和一个整数 k ，请你统计并返回 该数组中和为 k 的子数组的个数 。

子数组是数组中元素的连续非空序列。

 

示例 1：

输入：nums = [1,1,1], k = 2
输出：2
示例 2：

输入：nums = [1,2,3], k = 3
输出：2
 
提示：

1 <= nums.length <= 2 * 104
-1000 <= nums[i] <= 1000
-107 <= k <= 107


var subarraySum = function(nums, k) {
    let preSum=[nums[0]],count=0;
    for(let i=1;i<nums.length;i++){
        preSum[i]=preSum[i-1]+nums[i];
    }
    for(let i=0;i<nums.length;i++){
        for(let j=i;j<nums.length;j++){
            if(i===0){
                if(preSum[j]===k){count++}
            }else{
                if(preSum[j]-preSum[i-1]===k){
                    count++;
                }
            }
        }
    }
    return count;
};
