---  
layout: post
title: Summary of Backtracking Alg
categories: Backtracking
description: just for practice
keywords: Backtracking
---  
# 题目描述  

https://leetcode.com/problems/combination-sum/
https://leetcode.com/problems/combination-sum-ii/
https://leetcode.com/problems/combinations/

https://leetcode.com/problems/permutations/
https://leetcode.com/problems/permutations-ii/

https://leetcode.com/problems/combination-sum/
https://leetcode.com/problems/combination-sum-iii/
https://leetcode.com/problems/combination-sum-iv/

https://leetcode.com/problems/subsets/description/
https://leetcode.com/problems/subsets-ii/

https://leetcode.com/problems/n-queens/

https://www.youtube.com/watch?v=pfiQ_PS1g8E&list=PLot-Xpze53lf5C3HSjCnyFghlW0G1HHXo


具体的算法模版：
~~~
void backtrack(路径, 选择列表起始索引, 其他状态) {
    if (到达终止条件) { 记录结果; return; }
    if (非法状态) { return; }
    
    for (int i = start; i < n; i++) {
        // 剪枝（可选，但注意先排序）
        if (某些条件) break; 或 continue;
        
        做选择;
        backtrack(新索引, 新状态);
        撤销选择;
    }
}
~~~  
第一步：明确定义递归函数的功能
对于 Combination Sum，我们可以定义：
> dfs(start, target)：在 candidates[start..] 范围内，选出若干数字（可重复使用同一个数字），使得它们的和等于 target。当前已选的数字保存在 list 中。  

第二步：统一使用递归基（base case）处理终止条件
永远先将终止条件写在函数最前面，而不是分散在循环内。  
第三步：循环内只做“选择 - 递归 - 撤销”三件事，并适当剪枝  
>  if (某些条件) break; 或 continue;
>
像: leetcode 40. Combination Sum II, 就是在剪枝上做文章，Each number in candidates may only be used once in the combination. 常见的方案是：

~~~java
Arrays.sort(candidates);
//循环迭代的时候
if (i > start && candidates[i] == candidates[i - 1]){
    continue;
}
~~~  


如何写出清晰回溯代码:

第一步：明确递归函数参数及含义
~~~
确定状态变量：当前处理到当前的数据（第几行,第几步、当前棋盘、冲突辅助数组）, 不要引入多余参数
~~~ 


第二步：确定终止条件（写在最前面）
~~~
    通常就是 if (row == n) { 记录结果; return; }
    if (到达终止条件) { 记录结果; return; }
    if (非法状态) { return; }
~~~

第三步：在递归函数内，遍历当前层的所有可能选择
~~~
这一步是递归函数的主要逻辑，主要是处理当前数据，针对数据的选择，进行当前的判断，以及怎么推导到下一步的参数：  

对于 N 皇后，每一层（行）的选择就是所有列 0..n-1。对每个选择，先判断是否合法（能否放置）。

对于排列组合而言，就是挑选当前合适的元素

大体的流程是：：做选择 → 递归 → 撤销选择。
~~~  

第三部中的撤销选择，是一个比较重要的步骤和逻辑：
~~~
需要手动撤销的：那些在递归调用前后被修改的、且是共享的可变状态（例如 used 数组、全局变量、对象字段）。因为递归返回后，后续的兄弟分支需要看到“未被修改”的状态。

不需要手动撤销的：通过值传递（pass-by-value）的基本类型参数（如 int index）或不可变对象，因为每个递归调用都有自己的副本，修改不会影响上一层。
~~~

第四步：合法性判断单独抽取函数（或使用辅助数组）
~~~  
如果使用辅助数组（列、对角线），则判断变成 O(1) 且非常清晰。
如果不用辅助数组，单独写一个 isValid() 函数，扫描之前的行。
~~~   

额外技巧：**画递归树**


