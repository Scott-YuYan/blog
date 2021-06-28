* 1.DENSE_RANK() 函数

  DENSE_RANK()是一个窗口函数，它为分区或结果集中的每一行分配排名，而排名值没有间隙。
  
  用法：
  ```
  DENSE_RANK() OVER (
      PARTITION BY <expression>[{,<expression>...}]
      ORDER BY <expression> [ASC|DESC], [{,<expression>...}]
  ) 
  ```
  
  举例：
  ```
  select `score`,dense_rank() over(order by `score` desc) as 'Rank' from `Scores`;
  ```
   参见<a href="https://leetcode-cn.com/problems/rank-scores/comments/">Leetcode 178.分数排名</a> 以及<a href="https://www.begtut.com/mysql/mysql-dense_rank-function.html#">MySQL DENSE_RANK() 函数</a>
