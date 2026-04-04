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

