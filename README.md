# Algorithm-Weekly-Summary
This is a good habit of reviewing all the medium and hard stuff 
every week which would definitely helps me get advanced more quickly

06.27.2020 First Week:

06.19 - 06.27 quite a long week
1. Leetcode:
1275
Find winner on TicTacToe
用n替换3（容易cover到follow up），然后检查四个方向；

2. Leetcode:
1179 
reformat department table;
Select ID,
SUM(IF(MONTH = 'JAN', REVERNUE, NULL)) AS JAN_REVENUE;
...
SUM(IF(MONTH = 'DEC', REVERNUE, NULL)) AS DEC_REVENUE;
From
Department(table name)
Group By
ID


3.Leetcode:
617 Merge Two binary Tree

Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.

recursion;
分左边右边然后分别搞；

core code:
if (t1 == null && t2 == null) {
  return null;
} else if (t1 == null) {
  return t2;
} else if (t2 == null) {
  return t1;
}
//要新建一个root的
TreeNode root = new TreeNode(t1.val + t2.val);
t1.left = mergeTree(t1.left, t1.left);
t1.right = mergeTree(t2.right, t2.right);
return root;
}
就检查一下谁是null谁不是null这样，没啥难的；

https://leetcode.com/problems/merge-two-binary-trees/

4. Ransom Node(int[26] 要是有-1的就不对，没有就算了）；
//数组A是否能够装下数组b
//或者反过来也一样
//用一个int[]array就好；

5. Leetcode
1313. Decompress Run-Length Encoded List
We are given a list nums of integers representing a list compressed with run-length encoding.

Consider each adjacent pair of elements [freq, val] = [nums[2*i], nums[2*i+1]] (with i >= 0). 
For each such pair, there are freq elements with value val concatenated in a sublist. 
Concatenate all the sublists from left to right to generate the decompressed list.

Return the decompressed list.

//mindset:
新建一个new array 然后size 是每个偶数位的总和；
然后再根据原来的array来逐个填充新的array的值

6.Leetcode
443 compress string
      8888个d
aaabccd...dddeef

//还是inplace；
//假设原来给的array足够长就好了

//corner case
if (char == null || char.length == 0) {
  return 0;
}
int slow = 0;
int fast = 0;
int index = 0;
int size = char.length - 1;
while (fast < size) {
  while (fast < size && array[slow] == array[fast]) {
    fast++;
  }
  int freq = fast - slow;
  char[index++] = char[slow];
  for (char ch : String.valueOf(freq)) {
    char[index++] = ch;
  }
  slow = fast;
}
return index;
}

7.Leetcode724. Find Pivot Index
Mindset:用prefix sum搞懂它的物理意义再做题 一定能够把题目有信心的做出来；
//先找everything sum（sum)
//然后再用一个leftSum拿到左边的值
//然后linear scan从左往右分步推进；
//在每一步的过程当中，就需要对leftSum/sum分别更新；
//找到相等return i，最终扫描完了都找不到，就return -1；

8.665 Non-Decreasing Array
Mindset：关键是要现在index对应的这个要小于等于前面的；
然后分两种情况讨论；
1.改变现状的，最简单，容易操作的；
2.改变前一个的，其他的情况（更高的风险，特殊情况才需要改变）；
if (nums[i] < nums[i - 1]) {
ct++;
then if ( i < 2 || nums[i - 2] <= nums[i]) {
  nums[i - 1] = nums[i];
} else {
  nums[i] = nums[i - 1];
}
return ct <= 1;
}

9. Bucket Sort
Leetcode:
220. Contains Duplicate III
Given an array of integers, find out whether there are two distinct indices i and j in the array such that the absolute difference between nums[i] and nums[j] is at most t and the absolute difference between i and j is at most k.

Example 1:

Input: nums = [1,2,3,1], k = 3, t = 0
Output: true
Example 2:

Input: nums = [1,0,1,1], k = 1, t = 2
Output: true
Example 3:

Input: nums = [1,5,9,1,5,9], k = 2, t = 3
Output: false

//Mindset:
Use bucket sort;
use long to prevent overflow and defined every element in specific bucket by linear scan the whole array
bucket sort的核心是建立从array[i]到reconduct number的一个映射；
然后这个映射的逻辑和范围都是根据k 和t来进行界定的；
******
public boolean containDuplicates(int[] array, int k, int t) {
  if (array == null || array.length == 0 || k < 1 || t < 0) {
    return false;
  }
  Map<Long, Long> map = new HashMap<>();
  for (int i = 0; i < array.length; i++) {
    int reconductNum = (long)array[i] - Integer.MIN_VALUE;
    int bucket = reconductNum / (long(t) + 1);
    //分情况讨论了：
    //在同一个bucket里面；在（左右）相邻的bucket里面但是差距小于等于t
    if (map.containsKey(bucket) || map.containsKey(bucket - 1) && reconductNum - map.get(bucket - 1) <= t ||
     map.containsKey(bucket + 1) && map.get(bucket + 1) - reconductNum) {
      return true;
    }
    //remove the smallest one if the size of buck has been exceed;
    if (map.entrySet().size() >= k) {
      long lastBucket = ((long)array[i - k] - Integer.MIN_VALUE) / ((long)t + 1);
      map.remove(lastBucket);
    }
    map.put(bucket, reconductNum);
  }
  return true;
}

这个题目没有什么滑头的，用DP的知识来做就是老老实实的得用一个int[][]matrix这样的表格来记录当前的长度和index
10.longest subString
public class Solution {
  public String longestCommon(String source, String target) {
    // Write your solution here
    // 基本的思路就是从头拿一个个的进行比较；这样等于是每个子循环里面就有一个n^2；total就是n^4；
    // 但是这样子效率就非常低了，因为有很多重复的计算（比如说在取substring的时候就特别缓慢）；
    // DP please
    // how? 还是新建一个int[][] matrix 把记录的最长index 摆进去；
    // corner case
    if (source.length() == 0 || target.length() == 0) {
      return "";
    }
    int longest = 0;
    int start = 0;
    int[][] result = new int[source.length()][target.length()];
    for (int i = 0; i < source.length(); i++) {
      for (int j = 0; j < target.length(); j++) {
        //这里的条件就是两个取charAt是相同的；
        if (source.charAt(i) == target.charAt(j)) {
          if (i == 0 || j == 0) {
            result[i][j] = 1;
          } else {
            //这里直接问斜角拿那个数即可；
            result[i][j] = result[i - 1][j - 1] + 1;
          }
          //check longest
          if (longest < result[i][j]) {
            //update longest;
            longest = result[i][j];
            start = i - longest + 1;
          }
        }
      }
    }
    return source.substring(start, start + longest);
  }
}

11.longest subsequence
Mindset:
int[][] result = new int[s.length()][t.length()];
//和上面不同的是，更新的条件不仅仅是他们的char唯一相等，不相等的时候也可以更新的；
//如果不想等就照抄下来前面的数值就好；
//然后index 从1开始的话语，就不能把index 为0这个条件忽略掉；得加上
Core code:
for (int i = 1; i <= s.length(); i++) {
  for (int j = 1; j <= t.length(); j++) {
    if (s.charAt(i - 1) == t.charAt(j - 1)) {
      result[i][j] = result[i - 1][j - 1] + 1;
    } else {
      result[i][j] = Math.max(result[i - 1][j], result[i][j - 1]);
    }
  }
}
return result[s.length()][t.length()];
}
