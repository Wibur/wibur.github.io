+++
author = "Vancer"
title = "Running Sum of 1d Array"
date = "2020-09-03"
description = "LeetCode"
tags = [
    "LeetCode",
]
categories = [
    "刷題",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
+++

# Running Sum of 1d Array

Given an array nums. We define a running sum of an array as runningSum[i] = sum(nums[0]…nums[i]).

Return the running sum of nums.

## Example1
```
Input: nums = [1,2,3,4]
Output: [1,3,6,10]
Explanation: Running sum is obtained as follows: [1, 1+2, 1+2+3, 1+2+3+4].
```
## Constraints
* 1 <= nums.length <= 1000
* -10^6 <= nums[i] <= 10^6

## Answer
```
硬解
/**
* @param Integer[] $nums
* @return Integer[]
*/
function runningSum($nums) {
    $len = count($nums);
    $output = [];
    for ($i = 0; $i < $len; $i++) {
        $tmp = 0;
        for ($j = 0;$j <= $i; $j++) {
            $tmp += $nums[$j];
        }
        $output[] = $tmp;
    }
    
    return $output;
}
```

```
function runningSum($nums) {
    $last = 0;
    $result = [];

    foreach($nums as $num) {
        $new = $num + $last;
        $last = $new;
        $result[] = $new;
    }

    return $result;
}
```