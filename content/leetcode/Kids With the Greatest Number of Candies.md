+++
author = "Vancer"
title = " Kids With the Greatest Number of Candies"
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

Given the array candies and the integer extraCandies, where candies[i] represents the number of candies that the ith kid has.

For each kid check if there is a way to distribute extraCandies among the kids such that he or she can have the greatest number of candies among them. Notice that multiple kids can have the greatest number of candies.

## Example1
```
Input: candies = [4,2,1,1,2], extraCandies = 1
Output: [true,false,false,false,false] 
Explanation: There is only 1 extra candy, therefore only kid 1 will have the greatest number of candies among the kids regardless of who takes the extra candy.
```
## Constraints
* 2 <= candies.length <= 100
* 1 <= candies[i] <= 100
* 1 <= extraCandies <= 50

## Answer
```
($candy + $extraCandies) 括起來也會更快
1跟0 會比true跟false快一些
 /**
     * @param Integer[] $candies
     * @param Integer $extraCandies
     * @return Boolean[]
     */
    function kidsWithCandies($candies, $extraCandies) {
        $max = max($candies);
        $result = [];
        foreach($candies as $candy) {
            if (($candy + $extraCandies) >= $max) {
                $result[] = 1;
            } else {
                $result[] = 0;
            }
        }
        return $result;
    }
```