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

