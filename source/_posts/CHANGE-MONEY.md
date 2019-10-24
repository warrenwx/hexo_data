---
title: CHANGE-MONEY
date: 2019-10-24 20:40:18
tags: dp recursively
---

code for change money with golang

```golang
var memo map[int]int

func change(coins []int, kinds int, num int) int {
    if num == 0 {
        return 0
    }
    
    if cost, ok := memo[num]; ok {
        return cost
    }
    
    minCost := 0x7FFFFFFF
    for i := 0; i < kinds; i++ {
        if num - coins[i] >= 0 {
            curCost := change(coins, 3, num - coins[i])
            if curCost < minCost {
                minCost = curCost
            }    
        }
    }

    memo[num] = minCost + 1
    return minCost + 1
}

func main(){
    memo = make(map[int]int)
    coins := []int {1, 5, 10}
    fmt.Println(change(coins, 3, 7000))
}

```
