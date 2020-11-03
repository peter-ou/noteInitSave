## LeetCode算法题

#### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)【简单】考题

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

**示例:**

```
给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

思路：一边遍历，一边放入哈希表，如果target-nums[i]在哈希表中，就返回

```java
		Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            }
            map.put(nums[i], i);
        }
```

#### [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)【中等】考题

给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例：**

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

思路：就相加就行，但是别忘了进位，最后别忘了可能会因为进位多出来一个节点

#### [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)【中等】考题

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

思路：滑动窗口，将字符放到哈希表中，当前字符在哈希表中的时候更新起始位置，

```java
		Map<Character,Integer> map = new HashMap<>();int start = -1,ans = 0;
		for(int i = 0;i<s.length();i++){
			char c = s.charAt(i);
			if(map.containsKey(c) && map.get(c)>start){start = map.get(c);}
			else{ans = Math.max(ans,i-start);}
			map.put(c,i);
		}
		return ans;
```

#### [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)【中等】考题

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

**示例 1：**

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

思路：中心扩散法，以每一个元素，或每两个元素为中心扩散，找到最大的回文子串并且更新start和end标记

```java
		int start = 0,end = 0;
        for(int i = 0;i<s.length();i++){
            int len1 = lengthOfPalindrome(s,i,i),len2 = lengthOfPalindrome(s,i,i+1);
            int len = Math.max(len1,len2);
            if(len>end-start+1){
                if(len%2==1){start = i-len/2;end = i+len/2;}//根据长度的奇偶确定新的起始位置
                else{start = i-len/2+1;end = i+len/2;}
            }
        }
        return s.substring(start,end+1);
    int lengthOfPalindrome(String s,int left,int right){
        while(left>=0&&right<s.length()&&s.charAt(left)==s.charAt(right)){
            left--;right++;
        }
        return right - left-1;
    }
```

#### [7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)【简单】考题

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

注意：假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

**示例 1:**

```
输入: 123
输出: 321
```

思路：拿到末尾数字，然后乘以10+下一个末尾数字就可以了，要注意判断溢出问题

```java
		int res = 0;
        while(x!=0) {
            int tmp = x%10;//每次取末尾数字
            if (res>214748364 || (res==214748364 && tmp>7)) { return 0;}//判断向上溢出
            if (res<-214748364 || (res==-214748364 && tmp<-8)) { return 0;}//判断向下溢出
            res = res*10 + tmp;
            x /= 10;
        }
        return res;
```

#### [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)【简单】考题

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**示例 1:**

```
输入: 121
输出: true
```

**示例 2:**

```
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

思路：所有负数都不可能是回文，除了 `0` 以外，所有个位是 `0` 的数字不可能是回文，反转后半部分的数字，如果后半部分数字和前半部分相同则是回文数字，当反转后的数字大于前半数字时达到一半

```java
		if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }
        int revertedNumber = 0;
        while (x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }
        return x == revertedNumber || x == revertedNumber / 10;
```

#### [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)【中等】考题

给你 *n* 个非负整数 *a*1，*a*2，...，*a*n，每个数代表坐标中的一个点 (*i*, *ai*) 。在坐标内画 *n* 条垂直线，垂直线 *i* 的两个端点分别为 (*i*, *ai*) 和 (*i*, 0)。找出其中的两条线，使得它们与 *x* 轴共同构成的容器可以容纳最多的水。

**示例：**

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49
```

思路：双指针，一个指向开头，一个指向结尾，如果当前高度比较矮，移动指针

```java
		int left = 0,right = height.length-1;
        int area = 0;
        while(left<right){
            area = Math.max(area,(right-left)*Math.min(height[left],height[right]));
            if(height[left]<height[right])left++;
            else right--;
        }
        return area;
```

#### [14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)【简单】考题

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

示例 1:

```
输入: ["flower","flow","flight"]
输出: "fl"
```


示例 2:

```
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```

思路：两两比较最长公共前缀，如果为空串记得提前退出，也可以纵向比较

```java
		String prefix = strs[0];int count = strs.length;
        for (int i = 1; i < count; i++) {
            prefix = longestCommonPrefix(prefix, strs[i]);//
            if (prefix.length() == 0) {break;}
        }
        return prefix;
    public String longestCommonPrefix(String str1, String str2) {
        int length = Math.min(str1.length(), str2.length()),index = 0;
        while (index < length && str1.charAt(index) == str2.charAt(index)) {
            index++;
        }
        return str1.substring(0, index);
    }
```

#### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)【中等】考题

给你一个包含 *n* 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？请你找出所有满足条件且**不重复**的三元组。

**注意：**答案中不可以包含重复的三元组。

**示例：**

```
给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

思路：排序，然后遍历数组，双指针分别指向下一个和最后一个，记得跳过重复情况

```java
		Arrays.sort(nums); // O(nlogn)
        for (int i = 0; i < nums.length - 2; i++) { // O(n^2)
            if (nums[i] > 0) break; // 第一个数大于 0，后面的数都比它大，肯定不成立了
            if (i > 0 && nums[i] == nums[i - 1]) continue; // 去掉重复情况
            int target = -nums[i];
            int left = i + 1, right = nums.length - 1;
            while (left < right) {
                if (nums[left] + nums[right] == target) {
                    ans.add(new ArrayList<>(Arrays.asList(nums[i], nums[left], nums[right])));
                    left++; right--; // 首先无论如何先要进行加减操作
                    while (left < right && nums[left] == nums[left-1]) left++;// 去掉重复情况
                    while (left < right && nums[right] == nums[right+1]) right--;// 去掉重复情况
                } else if (nums[left] + nums[right] < target) {left++;}
                  else {right--;}
            }
        }
        return ans;
```

#### [16. 最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/)【中等

给定一个包括 *n* 个整数的数组 `nums` 和 一个目标值 `target`。找出 `nums` 中的三个整数，使得它们的和与 `target` 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

**示例：**

```
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
```

思路：和上一题三数之和类似，排序+双指针

#### [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)【中等

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/original_images/17_telephone_keypad.png" alt="img" style="zoom: 33%;" />

**示例:**

```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**说明:**
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

思路：回溯

```java
	Map<Character, String> phoneMap = new HashMap<Character, String>() {{
            put('2', "abc");put('3', "def");put('4', "ghi");put('5', "jkl");
            put('6', "mno");put('7', "pqrs");put('8', "tuv");put('9', "wxyz");
        }};
        backtrack(combinations, phoneMap, digits, 0, new StringBuffer());
        return combinations;
    }
    public void backtrack(List<String> combinations, Map<Character, String> phoneMap, 
    						String digits, int index, StringBuffer combination) {
        if (index == digits.length()) {
            combinations.add(combination.toString());
        } else {
            char digit = digits.charAt(index);
            String letters = phoneMap.get(digit);
            for (int i = 0; i < letters.length(); i++) {//多叉树
                combination.append(letters.charAt(i));
                backtrack(combinations, phoneMap, digits, index + 1, combination);
                combination.deleteCharAt(index);
            }
        }
    }
```

#### [18. 四数之和](https://leetcode-cn.com/problems/4sum/)【中等

给定一个包含 *n* 个整数的数组 `nums` 和一个目标值 `target`，判断 `nums` 中是否存在四个元素 *a，**b，c* 和 *d* ，使得 *a* + *b* + *c* + *d* 的值与 `target` 相等？找出所有满足条件且不重复的四元组。

**注意：**答案中不可以包含重复的四元组。

**示例：**

```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。
满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

思路：和三数之和一样，排序+双指针，前面两层遍历

#### [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)【中等】考题

给定一个链表，删除链表的倒数第 *n* 个节点，并且返回链表的头结点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

**说明：**给定的 *n* 保证是有效的。

**进阶：**你能尝试使用一趟扫描实现吗？

思路：一趟扫描，双指针，第一个指针先移动n步，然后两个一起移动，记得加上哨兵节点简化代码

```java
	ListNode dummy = new ListNode(0);dummy.next = head;
    ListNode first = dummy,second = dummy;
    for (int i = 1; i <= n + 1; i++) {
        first = first.next;
    }
    while (first != null) {
        first = first.next;
        second = second.next;
    }
    second.next = second.next.next;
    return dummy.next;
```

#### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)【简单】考题

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串，判断字符串是否有效。有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

**示例 4:**

```
输入: "([)]"
输出: false
```

**示例 5:**

```
输入: "{[]}"
输出: true
```

思路：左括号入栈，遇到右括号出栈，判断是否匹配/栈是否空

```java
		Deque<Character> stack = new LinkedList<>();
		Map<Character,Character> map = new HashMap<>();
		map.put('(',')');map.put('[',']');map.put('{','}');
		for(char c:s.toCharArray()){
			if(map.containsKey(c)) stack.push(c);
			else{if(stack.isEmpty() || c != map.get(stack.pop())) return false;}
		}
		return stack.isEmpty();
```

#### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)【简单】考题

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例：**

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

思路：构造哨兵节点，然后比较节点大小，移动节点

```java
		ListNode prehead = new ListNode(0),prev = prehead;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                prev.next = l1;
                l1 = l1.next;
            } else {
                prev.next = l2;
                l2 = l2.next;
            }
            prev = prev.next;
        }
        prev.next = l1 == null ? l2 : l1;
        return prehead.next;
```

#### [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)【中等】考题

数字 *n* 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

**示例：**

```
输入：n = 3
输出：["((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"]
```

思路：回溯法

```java
 		List<String> ans = new ArrayList<String>();
        backtrack(ans, new StringBuilder(), 0, 0, n);
        return ans;
    public void backtrack(List<String> ans, StringBuilder cur, int open, int close, int max) {
        if (cur.length() == max * 2) {
            ans.add(cur.toString());
            return;
        }
        if (open < max) {
            cur.append('(');
            backtrack(ans, cur, open + 1, close, max);
            cur.deleteCharAt(cur.length() - 1);
        }
        if (close < open) {
            cur.append(')');
            backtrack(ans, cur, open, close + 1, max);
            cur.deleteCharAt(cur.length() - 1);
        }
    }
```

#### [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)【中等

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

**你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。

**示例:**

```
给定 1->2->3->4, 你应该返回 2->1->4->3.
```

思路：就是常规的两个一反转，记得需要哨兵节点，和记住前一节点

```java
		ListNode dummy = new ListNode(0);dummy.next = head;
        ListNode prevNode = dummy;
        while ((head != null) && (head.next != null)) {
            ListNode firstNode = head;
            ListNode secondNode = head.next;

            prevNode.next = secondNode;//交换
            firstNode.next = secondNode.next;
            secondNode.next = firstNode;

            prevNode = firstNode;
            head = firstNode.next; // jump
        }
        return dummy.next;
```

#### [26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)【简单

给定一个排序数组，你需要在**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

思路：双指针，只有两个指针对应元素不相等时才将前面的复制后面的，并且移动双指针，否则只移动后面的指针

```java
	int i = 0;
    for (int j = 1; j < nums.length; j++) {
        if (nums[j] != nums[i]) {
            i++;
            nums[i] = nums[j];
        }
    }
    return i + 1;
```

#### [31. 下一个排列](https://leetcode-cn.com/problems/next-permutation/)【中等

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须**[原地](https://baike.baidu.com/item/原地算法)**修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
`1,2,3` → `1,3,2`
`3,2,1` → `1,2,3`
`1,1,5` → `1,5,1`

思路：从后往前找到第一个非升序的数，然后从后往前找到第一个大于前面找到的数的位置，交换这两个数，交换之和将后面的数反转。

```java
		int i = nums.length - 2;
        while (i >= 0 && nums[i + 1] <= nums[i]) {
            i--;
        }
        if (i >= 0) {
            int j = nums.length - 1;
            while (j >= 0 && nums[j] <= nums[i]) {
                j--;
            }
            swap(nums, i, j);//交换元素
        }
        reverse(nums, i + 1);//反转后面的部分
```

#### [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)【困难】考题

给定一个只包含 `'('` 和 `')'` 的字符串，找出最长的包含有效括号的子串的长度。

**示例 1:**

```
输入: "(()"
输出: 2
解释: 最长有效括号子串为 "()"
```

思路：栈，栈中保存左括号下标

```java
		public int longestValidParentheses(String s) {
        int maxans = 0;
        Stack<Integer> stack = new Stack<>();
        stack.push(-1);
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else {
                stack.pop();
                if (stack.empty()) {
                    stack.push(i);
                } else {
                    maxans = Math.max(maxans, i - stack.peek());
                }
            }
        }
        retrn maxans;
    }
```

#### [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)【中等】考题

假设按照升序排序的数组在预先未知的某个点上进行了旋转。( 例如，数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` )。搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 `-1` 。

你可以假设数组中不存在重复的元素。你的算法时间复杂度必须是 *O*(log *n*) 级别。

**示例 1:**

```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
```

思路：二分

<img src="https://assets.leetcode-cn.com/solution-static/33/33_fig1.png" alt="fig1" style="zoom: 50%;" />

```java
		int left = 0, right = n - 1;
        while (left <= right) {
            int mid = left+(right-left)/2;
            if (nums[mid] == target) {return mid;}
            if (nums[0] <= nums[mid]) {
                if (nums[0] <= target && target < nums[mid]) {right = mid - 1;} 
                else { left = mid + 1;}
            } else {
                if (nums[mid] < target && target <= nums[n - 1]) {left = mid + 1;} 
                else { right = mid - 1;}
            }
        }
```

#### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)【中等】考题

给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。你的算法时间复杂度必须是 *O*(log *n*) 级别。如果数组中不存在目标值，返回 `[-1, -1]`。

**示例 1:**

```
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
```

思路：二分变体

查找第一个目标值位置

```java
		int left = 0,right = nums.length-1;
        while(left<=right){
            int mid = left+(right-left)/2;
            if(nums[mid]>=target){right = mid-1;}//相等或大于，向左收缩
            else{left = mid+1;}
        }
        if(left>nums.length-1||nums[left]!=target) {return -1;}//返回左边元素
        return left;
```

查找最后一个目标值位置

```java
		int left = 0,right = nums.length-1;
        while(left<=right){
            int mid = left+(right-left)/2;
            if(nums[mid]<=target){left = mid+1;}//向右收缩
            else{right = mid-1;}
        }
        if(right<0||nums[right]!=target) {return -1;}//返回右边元素
        return right;
```

#### [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)【简单

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。你可以假设数组中无重复元素。

**示例 1:**

```
输入: [1,3,5,6], 5
输出: 2
```

思路：每次向左移动的时候记录下当前mid的值，初始化结构最后一个位置

```java
		int left = 0,right = nums.length-1;
        int result = nums.length;
        while(left<=right){
            int mid = (right-left)/2+left;
            if(nums[mid]>=target){
                result = mid;
                right = mid-1;
            }else{left = mid+1;} 
        }
        return result;
```

#### [38. 外观数列](https://leetcode-cn.com/problems/count-and-say/)【简单

给定一个正整数 *n*（1 ≤ *n* ≤ 30），输出外观数列的第 *n* 项。

注意：整数序列中的每一项将表示为一个字符串。

「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。前五项如下：

```
1.     1
2.     11     “一个 1 
3.     21     “两个 1 ” 
4.     1211   “一个 2 一个 1 ” 
5.     111221 “一个 1 一个 2 两个 1 ” 
```

思路：就是按照步骤生成就行

```
def countAndSay(self, n: int) -> str:
    res = '1'
    for _ in range(n-1):  # 控制循环次数
        i, tmp = 0, ''
        for j, c in enumerate(res):
            if c != res[i]:
                tmp += str(j-i) + res[i]
                i = j
        res = tmp + str(len(res) - i) + res[-1]
    return res
```

#### [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)【中等】考题

给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的数字可以无限制重复被选取。**说明：**

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7,
所求解集为：
[
  [7],
  [2,2,3]
]
```

思路：回溯+剪枝

<img src="https://pic.leetcode-cn.com/1598091943-GPoHAJ-file_1598091940246" alt="img" style="zoom: 25%;" />

```java
	public List<List<Integer>> combinationSum(int[] candidates, int target) {
        int len = candidates.length;
        List<List<Integer>> res = new ArrayList<>();
        if (len == 0) {return res;}
        // 排序是剪枝的前提
        Arrays.sort(candidates);
        Deque<Integer> path = new ArrayDeque<>();
        dfs(candidates, 0, len, target, path, res);
        return res;
    }

    private void dfs(int[] candidates, int begin, int len, int target, Deque<Integer> path,
    				List<List<Integer>> res) {
        // 由于进入更深层的时候，小于 0 的部分被剪枝，因此递归终止条件值只判断等于 0 的情况
        if (target == 0) {res.add(new ArrayList<>(path)); return;}
        for (int i = begin; i < len; i++) {
            if (target - candidates[i] < 0) {break;}// 重点理解这里剪枝，前提是候选数组已经有序，
            path.addLast(candidates[i]);
            dfs(candidates, i, len, target - candidates[i], path, res);
            path.removeLast();
        }
    }
```

什么时候使用 used 数组，什么时候使用 begin 变量
有些朋友可能会疑惑什么时候使用 used 数组，什么时候使用 begin 变量。这里为大家简单总结一下：

排列问题，讲究顺序（即 [2, 2, 3] 与 [2, 3, 2] 视为不同列表时），需要记录哪些数字已经使用过，此时用 used 数组；
组合问题，不讲究顺序（即 [2, 2, 3] 与 [2, 3, 2] 视为相同列表时），需要按照某种顺序搜索，此时使用 begin 变量。

#### [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)【中等

给定一个数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用一次。

**说明：**

- 所有数字（包括目标数）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

思路：和上一题一样，回溯+剪枝，只是把dfs里面的i改为i+1即可，因为每个组合只能用一个

#### [46. 全排列](https://leetcode-cn.com/problems/permutations/)【中等】考题

给定一个 **没有重复** 数字的序列，返回其所有可能的全排列。

思路：回溯

```java
	public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        List<Integer> output = new ArrayList<Integer>();
        for (int num : nums) {output.add(num);}
        int n = nums.length;
        backtrack(n, output, res, 0);
        return res;
    }

    public void backtrack(int n, List<Integer> output, List<List<Integer>> res, int first) {
        if (first == n) {// 所有数都填完了
            res.add(new ArrayList<Integer>(output));
        }
        for (int i = first; i < n; i++) {
            Collections.swap(output, first, i); // 动态维护数组
            backtrack(n, output, res, first + 1);  // 继续递归填下一个数
            Collections.swap(output, first, i); // 撤销操作
        }
    }
```

#### [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)【中等】考题

给定一个 *n* × *n* 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

**说明：**

你必须在**[原地](https://baike.baidu.com/item/原地算法)**旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要**使用另一个矩阵来旋转图像。

思路：旋转二维数组，先对矩阵进行转置(行列互换)，再反转每一行

```java
 // transpose matrix
    for (int i = 0; i < n; i++) {
      for (int j = i; j < n; j++) {
        int tmp = matrix[j][i];
        matrix[j][i] = matrix[i][j];
        matrix[i][j] = tmp;
      }
    }
    // reverse each row
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < n / 2; j++) {
        int tmp = matrix[i][j];
        matrix[i][j] = matrix[i][n - j - 1];
        matrix[i][n - j - 1] = tmp;
      }
    }
```

#### [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)【中等

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

**示例:**

```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**说明：**

- 所有输入均为小写字母。
- 不考虑答案输出的顺序。

思路：按计数分类，当且仅当它们的字符计数（每个字符的出现次数）相同时，两个字符串是字母异位词。

将计数字符串相同的放在一起

#### [50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)【中等

实现 [pow(*x*, *n*)](https://www.cplusplus.com/reference/valarray/pow/) ，即计算 x 的 n 次幂函数。

**示例 1:**

```
输入: 2.00000, 10
输出: 1024.00000
```

思路：快速幂

```java
	double quickMul(double x, long N) {
        double ans = 1.0;
        double x_contribute = x; // 贡献的初始值为 x
        while (N > 0) {// 在对 N 进行二进制拆分的同时计算答案
            if (N % 2 == 1) {
                ans *= x_contribute; // 如果 N 二进制表示的最低位为 1，那么需要计入贡献
            }         
            x_contribute *= x_contribute; // 将贡献不断地平方
            N /= 2;// 舍弃 N 二进制表示的最低位，这样我们每次只要判断最低位即可
        }
        return ans;
    }
    public double myPow(double x, int n) {
        long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }
```

#### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)【简单】考题

给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

```
输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```


进阶:

如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

思路：动态规划，f(i)代表以第i个数结尾的连续子数组的最大和，那么我们求的就是`max{f(i)}`，`f(i)=max{f(i-1)+ai,ai}`，

```java
	public int maxSubArray(int[] nums) {
        int pre = 0, maxAns = nums[0];
        for (int x : nums) {
            pre = Math.max(pre + x, x);
            maxAns = Math.max(maxAns, pre);
        }
        return maxAns;
    }
```

#### [54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)【中等】考题

给定一个包含 *m* x *n* 个元素的矩阵（*m* 行, *n* 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

**示例 1:**

```
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
```

思路：按层模拟

```java
		int level = 0;
        while(true){
            int top = level,left = level;
            int right = matrix[0].length-1-level;
            int down  = matrix.length-1-level;
            for(int j = left ;j<=right;j++){
                result.add(matrix[top][j]);nums--;
            } 
            if(nums<=0) break;
            for(int i = top+1;i<=down-1;i++) {
                result.add(matrix[i][right]);nums--;
            }
            if(nums<=0) break;
            for(int j = right;j>=left;j--){
                result.add(matrix[down][j]);nums--;
            }
            if(nums<=0) break;
            for(int i = down-1;i>=top+1;i--){
                result.add(matrix[i][left]);nums--;
            }
            if(nums<=0) break;
            level++;
        }
        return result;
```

#### [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)【中等

给定一个非负整数数组，你最初位于数组的第一个位置。数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

**示例 1:**

```
输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
```

思路：贪心

```java
		int rightmost = 0;
        for (int i = 0; i < n; ++i) {
            if (i <= rightmost) {
                rightmost = Math.max(rightmost, i + nums[i]);
                if (rightmost >= n - 1) {
                    return true;//当最远距离可以到达最后的时候直接返回
                }
            }
        }
```

#### [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)【中等】考题

给出一个区间的集合，请合并所有重叠的区间。

示例 1:

```
输入: intervals = [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

思路：按照左端点排序，然后依次判断能否合并

```java
		Arrays.sort(intervals, new Comparator<int[]>() {
            public int compare(int[] interval1, int[] interval2) {
                return interval1[0] - interval2[0];
            }
        });//按照左端点排序
        List<int[]> merged = new ArrayList<int[]>();
        for (int i = 0; i < intervals.length; ++i) {
            int L = intervals[i][0], R = intervals[i][1];
            if (merged.size() == 0 || merged.get(merged.size() - 1)[1] < L) {
                merged.add(new int[]{L, R});
            } else {
                merged.get(merged.size() - 1)[1] = Math.max(merged.get(merged.size() - 1)[1], R);
            }
        }
        return merged.toArray(new int[merged.size()][]);
```

#### [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)【中等

给定一个链表，旋转链表，将链表每个节点向右移动 *k* 个位置，其中 *k* 是非负数。

**示例 1:**

```
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
```

思路：先将链表合并成环，然后再找到环断开的位置即可

```java
	ListNode old_tail = head;int n;
    for(n = 1; old_tail.next != null; n++)
      old_tail = old_tail.next;
    old_tail.next = head;//合并成环

    ListNode new_tail = head;
    for (int i = 0; i < n - k % n - 1; i++)
      new_tail = new_tail.next;
    ListNode new_head = new_tail.next;
    new_tail.next = null;//断开环

    return new_head;
```

#### [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)【中等

一个机器人位于一个 *m x n* 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

思路一：排列组合直接算，从m+n-2中情况中选出m-1个向下

思路二：动态规划，我们令 `dp[i][j]` 是到达 `i, j` 最多路径。动态方程：`dp[i][j] = dp[i-1][j] + dp[i][j-1]`

```
		int[][] dp = new int[m][n];
        for (int i = 0; i < n; i++) dp[0][i] = 1;
        for (int i = 0; i < m; i++) dp[i][0] = 1;
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];  
```

#### [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)【中等】考题

给定一个包含非负整数的 *m* x *n* 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

思路：dp\[0][0] = grid\[0][0]，

当i>0 且 j=0时，`dp[i][0]=dp[i-1][0]+grid[i][0]`

当 i=0且 j>0 时，`dp[0][j]=dp[0][j−1]+grid[0][j]`

当 i>0且 j>0 时，`dp[i][j]=min(dp[i−1][j],dp[i][j−1])+grid[i][j]`

```java
		int rows = grid.length, columns = grid[0].length;
        int[][] dp = new int[rows][columns];
        dp[0][0] = grid[0][0];
        for (int i = 1; i < rows; i++) {
            dp[i][0] = dp[i - 1][0] + grid[i][0];
        }
        for (int j = 1; j < columns; j++) {
            dp[0][j] = dp[0][j - 1] + grid[0][j];
        }
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < columns; j++) {
                dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }
        return dp[rows - 1][columns - 1];
```

#### [66. 加一](https://leetcode-cn.com/problems/plus-one/)【简单

给定一个由**整数**组成的**非空**数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储**单个**数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

思路：从后往前加一，主要考虑进位问题，以及可能新产生一位的问题

```java
		for (int i = digits.length - 1; i >= 0; i--) {
            digits[i]++;
            digits[i] = digits[i] % 10;
            if (digits[i] != 0) return digits;
        }
        digits = new int[digits.length + 1];
        digits[0] = 1;
        return digits;
```

#### [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)【简单

实现 `int sqrt(int x)` 函数。

计算并返回 *x* 的平方根，其中 *x* 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

思路：二分查找

```java
 		int l = 0, r = x, ans = -1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if ((long) mid * mid <= x) {
                ans = mid;
                l = mid + 1;
            } else {r = mid - 1;}
        }
        return ans;
```

#### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)【简单】考题

假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 *n* 是一个正整数。

**示例 1：**

```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
```

思路：我们用`f(x) `表示爬到第 x 级台阶的方案数，考虑最后一步可能跨了一级台阶，也可能跨了两级台阶，所以我们可以列出如下式子：`f(x) = f(x - 1) + f(x - 2)`

```java
 		int p = 0, q = 0, r = 1;
        for (int i = 1; i <= n; ++i) {
            p = q; 
            q = r; 
            r = p + q;
        }
        return r;
```

#### [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)【中等

给定一个包含红色、白色和蓝色，一共 *n* 个元素的数组，**[原地](https://baike.baidu.com/item/原地算法)**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

**注意:**
不能使用代码库中的排序函数来解决这道题。

**示例:**

```
输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
```

思路：荷兰国旗问题，沿着数组移动 `curr` 指针，若`nums[curr] = 0`，则将其与 `nums[p0]`互换；若 `nums[curr] = 2` ，则与 `nums[p2]`互换。

```java
 	int p0 = 0, curr = 0;
    int p2 = nums.length - 1;
    int tmp;
    while (curr <= p2) {
      if (nums[curr] == 0) { // 交换第 p0个和第curr个元素
        tmp = nums[p0];nums[p0++] = nums[curr];nums[curr++] = tmp;
      }
      else if (nums[curr] == 2) {交换第p2个和第curr个元素
        tmp = nums[curr];nums[curr] = nums[p2];nums[p2--] = tmp;
      }
      else curr++;
    }
```

#### [78. 子集](https://leetcode-cn.com/problems/subsets/)【中等】考题

给定一组**不含重复元素**的整数数组 *nums*，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

思路：回溯

```java
 	public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        getAns(nums, 0, new ArrayList<>(), ans);
        return ans;
	}

	private void getAns(int[] nums, int start, 	ArrayList<Integer> temp, List<List<Integer>> ans) { 
        ans.add(new ArrayList<>(temp));
        for (int i = start; i < nums.length; i++) {
            temp.add(nums[i]);
            getAns(nums, i + 1, temp, ans);
            temp.remove(temp.size() - 1);
        }
	}
```

#### [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)【中等

给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

**示例:**

```
board =
[['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']]
给定 word = "ABCCED", 返回 true
给定 word = "SEE", 返回 true
给定 word = "ABCB", 返回 false
```

思路：DFS+回溯

```java
	private boolean[][] marked;
    //        x-1,y
    // x,y-1  x,y    x,y+1
    //        x+1,y
    private int[][] direction = {{-1, 0}, {0, -1}, {0, 1}, {1, 0}};
    private int m; // 盘面上有多少行
    private int n;// 盘面上有多少列
    private String word;
    private char[][] board;
    public boolean exist(char[][] board, String word) {
        m = board.length;
        if (m == 0) {return false;}
        n = board[0].length;
        marked = new boolean[m][n];//标记数组
        this.word = word;this.board = board;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (dfs(i, j, 0)) {
                    return true;
                }
            }
        }
        return false;
    }
    private boolean dfs(int i, int j, int start) {
        if (start == word.length() - 1) {return board[i][j] == word.charAt(start);}//匹配到最后一位
        if (board[i][j] == word.charAt(start)) {
            marked[i][j] = true;
            for (int k = 0; k < 4; k++) {//分别是四个方向
                int newX = i + direction[k][0];
                int newY = j + direction[k][1];
                if (inArea(newX, newY) && !marked[newX][newY]) {//新的坐标在区域内，新的坐标没有走过
                    if (dfs(newX, newY, start + 1)) {//走下一步
                        return true;
                    }
                }
            }
            marked[i][j] = false;//回头
        }
        return false;
    }
    private boolean inArea(int x, int y) {return x >= 0 && x < m && y >= 0 && y < n;}
```

#### [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)【简单

给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

**示例 1:**

```
输入: 1->1->2
输出: 1->2
```

思路：正常操作就行

```
 	ListNode current = head;
    while (current != null && current.next != null) {
        if (current.next.val == current.val) {
            current.next = current.next.next;
        } else {
            current = current.next;
        }
    }
    return head;
```

#### [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/)【中等

给定一个链表和一个特定值 *x*，对链表进行分隔，使得所有小于 *x* 的节点都在大于或等于 *x* 的节点之前。

你应当保留两个分区中每个节点的初始相对位置。

**示例:**

```
输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5
```

思路：造两个哨兵节点，然后分别指向小于或者大于x的节点

```java
 		ListNode before_head = new ListNode(0),before = before_head;
        ListNode after_head = new ListNode(0),after = after_head;
        while (head != null) {
            if (head.val < x) {
                before.next = head;
                before = before.next;
            } else {
                after.next = head;
                after = after.next;
            }
            head = head.next;
        }
        after.next = null;//大于x的尾部置空
        before.next = after_head.next;//小于x的尾部指向大于x的头部
        return before_head.next;
```

#### [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)【简单】考题

给你两个有序整数数组 *nums1* 和 *nums2*，请你将 *nums2* 合并到 *nums1* 中*，*使 *nums1* 成为一个有序数组。

**说明:**

- 初始化 *nums1* 和 *nums2* 的元素数量分别为 *m* 和 *n* 。
- 你可以假设 *nums1* 有足够的空间（空间大小大于或等于 *m + n*）来保存 *nums2* 中的元素。

**示例:**

```
输入:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3
输出: [1,2,2,3,5,6]
```

思路：双指针，从后往前

```java
	int p1 = m - 1;//指向第一个数组
    int p2 = n - 1;//指向第二个数组
    int p = m + n - 1;//指向需要存放数的地方

    while ((p1 >= 0) && (p2 >= 0))
      nums1[p--] = (nums1[p1] < nums2[p2]) ? nums2[p2--] : nums1[p1--];
    System.arraycopy(nums2, 0, nums1, 0, p2 + 1);//比较长的数组剩余部分直接拷贝
```

#### [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/)【中等

给定一个可能包含重复元素的整数数组 ***nums***，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

**示例:**

```
输入: [1,2,2]
输出:
[[2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []]
```

思路：和78题一样，只是要跳过相同元素，排序，然后回溯

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    List<List<Integer>> ans = new ArrayList<>();
    Arrays.sort(nums); //排序
    getAns(nums, 0, new ArrayList<>(), ans);
    return ans;
}
private void getAns(int[] nums, int start, ArrayList<Integer> temp, List<List<Integer>> ans) {
    ans.add(new ArrayList<>(temp));
    for (int i = start; i < nums.length; i++) {
        if (i > start && nums[i] == nums[i - 1]) { continue;}  //和上个数字相等就跳过
        temp.add(nums[i]);
        getAns(nums, i + 1, temp, ans);
        temp.remove(temp.size() - 1);
    }
}
```

#### [91. 解码方法](https://leetcode-cn.com/problems/decode-ways/)【中等

一条包含字母 `A-Z` 的消息通过以下方式进行了编码：

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

给定一个只包含数字的**非空**字符串，请计算解码方法的总数。题目数据保证答案肯定是一个 32 位的整数。

**示例 1：**

```
输入："12"
输出：2
解释：它可以解码为 "AB"（1 2）或者 "L"（12）。
```

思路：动态规划，dp[i] 以 s[i] 结尾的前缀子串有多少种解码方法，

`dp[i] = dp[i - 1] * 1 if s[i] != '0'`，`dp[i] += dp[i - 2] * 1 if  10 <= int(s[i - 1..i]) <= 26`

```java
		int[] dp = new int[len];
		dp[0] = 1;
        char[] charArray = s.toCharArray();
       
        for (int i = 1; i < len; i++) {
            if (charArray[i] != '0') {
                dp[i] = dp[i - 1];
            }
            int num = 10 * (charArray[i - 1] - '0') + (charArray[i] - '0');
            if (num >= 10 && num <= 26) {
                if (i == 1) {dp[i]++;} 
                else {dp[i] += dp[i - 2];}
            }
        }
        return dp[len - 1];
    }
```

#### [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)【中等】考题

反转从位置 *m* 到 *n* 的链表。请使用一趟扫描完成反转。

**说明:**
1 ≤ *m* ≤ *n* ≤ 链表长度。

**示例:**

```
输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL
```

思路：反转就行

```java
 		while(cur!=nNext){//找到前一个节点，后一个节点，然后反转就行
            ListNode next = cur.next;//后一个节点
            cur.next = pre;//当前节点指向前一个节点
            pre = cur;//前一个节点移动到当前节点
            cur = next;//当前节点移动到后一个节点
        }
```

#### [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)【中等】考题

给定一个二叉树，返回它的*中序* 遍历。

**示例:**

```
输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```

**进阶:** 递归算法很简单，你可以通过迭代算法完成吗？

思路：因为，中序遍历顺序是左中右，因此需要用栈保存

```java
		List<Integer> list = new ArrayList<>();
		Deque<TreeNode> stack = new LinkedList<>();
		TreeNode cur = root;
		while(cur != null || !stack.isEmpty()) {
			if (cur != null) {
				stack.push(cur);
				cur = cur.left;
			} else {
				cur = stack.pop();
				list.add(cur.val);
				cur = cur.right;
			}
		}
		return list;
```

#### [95. 不同的二叉搜索树 II](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)【中等，

给定一个整数 *n*，生成所有由 1 ... *n* 为节点所组成的 **二叉搜索树** 。

**示例：**

```
输入：3
输出：
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
解释：
以上的输出对应以下 5 种不同结构的二叉搜索树：

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

思路：递归

```java
	public List<TreeNode> generateTrees(int n) {
        if (n == 0) { return new LinkedList<TreeNode>();}
        return generateTrees(1, n);
    }
    public List<TreeNode> generateTrees(int start, int end) {
        List<TreeNode> allTrees = new LinkedList<TreeNode>();
        if (start > end) {
            allTrees.add(null);
            return allTrees;
        }
        for (int i = start; i <= end; i++) {  // 枚举可行根节点
            List<TreeNode> leftTrees = generateTrees(start, i - 1); // 获得所有可行的左子树集合
            List<TreeNode> rightTrees = generateTrees(i + 1, end); // 获得所有可行的右子树集合
            // 从左子树集合中选出一棵左子树，从右子树集合中选出一棵右子树，拼接到根节点上
            for (TreeNode left : leftTrees) {
                for (TreeNode right : rightTrees) {
                    TreeNode currTree = new TreeNode(i);
                    currTree.left = left;
                    currTree.right = right;
                    allTrees.add(currTree);
                }
            }
        }
        return allTrees;
    }
```

#### [96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)【中等

给定一个整数 *n*，求以 1 ... *n* 为节点组成的二叉搜索树有多少种？

示例:

```
输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

思路：动态规划

题目要求是计算不同二叉搜索树的个数。为此，我们可以定义两个函数：

G(n)): 长度为 n 的序列能构成的不同二叉搜索树的个数。

F(i, n): 以 i 为根、序列长度为 n 的不同二叉搜索树个数(1≤i≤n)。

*G*(*n*)=∑F(i,n)，F(i,n)=G(i−1)⋅G(n−i)，解得G(n)=∑G(i−1)⋅G(n−i)

```java
		int[] dp = new int[n+1];
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
```

#### [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)【中等】考题

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

- 节点的左子树只包含**小于**当前节点的数。
- 节点的右子树只包含**大于**当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

思路一：递归

```java
	public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean isValidBST(TreeNode node, long lower, long upper){
        if(node == null){
            return true;
        }
        if(node.val <= lower || node.val >= upper){
            return false;
        }
        return isValidBST(node.left, lower, node.val) && isValidBST(node.right, node.val, upper);
    }
```

思路二：迭代，中序遍历，比较前后节点

```java
 		Deque<TreeNode> stack = new LinkedList<TreeNode>();
        double inorder = -Double.MAX_VALUE;
        while (!stack.isEmpty() || root != null) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
              // 如果中序遍历得到的节点的值小于等于前一个 inorder，说明不是二叉搜索树
            if (root.val <= inorder) {
                return false;
            }
            inorder = root.val;
            root = root.right;
        }
        return true;
```

#### [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)【简单】考题

给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

思路一：递归，分别左右遍历看是否相等

```java
 	return check(root, root);
    public boolean check(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }
        if (p == null || q == null) {
            return false;
        }
        return p.val == q.val && check(p.left, q.right) && check(p.right, q.left);
    }
```

思路二：迭代，从左右分别层次遍历

```java
 	return check(root, root);
	public boolean check(TreeNode u, TreeNode v) {
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        q.offer(u);q.offer(v);
        while (!q.isEmpty()) {
            u = q.poll();v = q.poll();
            if (u == null && v == null) {continue;}
            if ((u == null || v == null) || (u.val != v.val)) {return false;}
            q.offer(u.left);q.offer(v.right);
            q.offer(u.right);q.offer(v.left);
        }
        return true;
    }
```

#### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)【中等】考题

给你一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）。

 思路：队列

```java
		Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            List<Integer> level = new ArrayList<Integer>();
            int currentLevelSize = queue.size();
            for (int i = 1; i <= currentLevelSize; ++i) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            ret.add(level);
        }
        
        return ret;
```

#### [103. 二叉树的锯齿形层次遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)【中等】考题

给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。例如：
给定二叉树 `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回锯齿形层次遍历如下：

```java
[[3],
  [20,9],
  [15,7]]
```

思路：和上一题一样，只不过偶数行要先进栈，再出栈

#### [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)【简单】考题

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。**说明:** 叶子节点是指没有子节点的节点。

思路：DFS+递归

```java
 	public int maxDepth(TreeNode root) {
        if(root==null) return 0;
        int left = maxDepth(root.left);//左子树高度
        int right = maxDepth(root.right);//右子树高度
        return Math.max(left,right)+1;//当前节点为根节点子树高度
    }
```

#### [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)【中等】考题

根据一棵树的前序遍历与中序遍历构造二叉树。

**注意:**你可以假设树中没有重复的元素。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

思路：递归

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    return buildTreeHelper(preorder, 0, preorder.length, inorder, 0, inorder.length);
}

private TreeNode buildTreeHelper(int[] preorder, int p_start, int p_end, int[] inorder, int i_start, int i_end) {
    // preorder 为空，直接返回 null
    if (p_start == p_end) {
        return null;
    }
    int root_val = preorder[p_start];//前序遍历的起始点就是根节点
    TreeNode root = new TreeNode(root_val);
    //在中序遍历中找到根节点的位置
    int i_root_index = 0;
    for (int i = i_start; i < i_end; i++) {
        if (root_val == inorder[i]) {
            i_root_index = i;
            break;
        }
    }
    int leftNum = i_root_index - i_start;
    //递归的构造左子树
    root.left = buildTreeHelper(preorder, p_start+ 1, p_start + leftNum + 1, inorder, i_start, i_root_index);
    //递归的构造右子树
    root.right = buildTreeHelper(preorder, p_start + leftNum + 1, p_end, inorder, i_root_index + 1, i_end);
    return root;
}
```

#### [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)【简单

将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1。

思路：中序遍历，选择中间偏左的节点作为根节点，即(left+right)/2

```java
	public TreeNode sortedArrayToBST(int[] nums) {
        return helper(nums, 0, nums.length - 1);
    }
    public TreeNode helper(int[] nums, int left, int right) {
        if (left > right) {
            return null;
        }
        int mid = (left + right) / 2;// 总是选择中间位置左边的数字作为根节点
        TreeNode root = new TreeNode(nums[mid]);
        root.left = helper(nums, left, mid - 1);
        root.right = helper(nums, mid + 1, right);
        return root;
    }
```

#### [109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)【中等

给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1。

思路：中序遍历，用一个全局指针保存当前节点，当中序遍历到它的时候才填根节点的值

```java
	ListNode globalHead;
    public TreeNode sortedListToBST(ListNode head) {
        globalHead = head;
        int length = getLength(head);
        return buildTree(0, length - 1);
    }
    public int getLength(ListNode head) {//测链表长度
        int ret = 0;
        while (head != null) {++ret;head = head.next;}
        return ret;
    }
    public TreeNode buildTree(int left, int right) {
        if (left > right) {return null;}
        int mid = (left + right + 1) / 2;
        TreeNode root = new TreeNode();
        root.left = buildTree(left, mid - 1);
        root.val = globalHead.val;
        globalHead = globalHead.next;
        root.right = buildTree(mid + 1, right);
        return root;
    }
```

#### [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)【中等】考题

给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。

**说明:** 叶子节点是指没有子节点的节点。

思路：DFS前序遍历，记得路径要回溯

```java
	List<List<Integer>> ret = new LinkedList<List<Integer>>();
    Deque<Integer> path = new LinkedList<Integer>();
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        dfs(root, sum);
        return ret;
    }
    public void dfs(TreeNode root, int sum) {
        if (root == null) {return;}
        path.offerLast(root.val);//保存沿途路径
        sum -= root.val;
        if (root.left == null && root.right == null && sum == 0) {
            ret.add(new LinkedList<Integer>(path));
        }
        dfs(root.left, sum);
        dfs(root.right, sum);
        path.pollLast();//回溯
    }
```

#### [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)【中等

给定一个二叉树，[原地](https://baike.baidu.com/item/原地算法/8010757)将它展开为一个单链表。

思路：前序遍历保存节点，然后使用right指针展开成链表

下面是迭代方法

```java
		List<TreeNode> list = new ArrayList<TreeNode>();
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        TreeNode node = root;
        while (node != null || !stack.isEmpty()) {
            while (node != null) {
                list.add(node);
                stack.push(node);
                node = node.left;
            }
            node = stack.pop();
            node = node.right;
        }
        int size = list.size();
        for (int i = 1; i < size; i++) {
            TreeNode prev = list.get(i - 1), curr = list.get(i);
            prev.left = null;
            prev.right = curr;
        }
```

#### [118. 杨辉三角](https://leetcode-cn.com/problems/pascals-triangle/)【简单

给定一个非负整数 *numRows，*生成杨辉三角的前 *numRows* 行。

思路：就是正常按照规律写

```java
  		List<List<Integer>> triangle = new ArrayList<List<Integer>>();
        triangle.add(new ArrayList<>());
        triangle.get(0).add(1);//第一行是1
        for (int rowNum = 1; rowNum < numRows; rowNum++) {
            List<Integer> row = new ArrayList<>();
            List<Integer> prevRow = triangle.get(rowNum-1);//上一行
            row.add(1);//每一行的第一个数都是1
            for (int j = 1; j < rowNum; j++) {
                row.add(prevRow.get(j-1) + prevRow.get(j));//等于上一行两列相加
            }
            row.add(1);//每一行的最后一个数都是1
            triangle.add(row);
        }
        return triangle;
```

#### [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)【简单】考题

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票。

思路：一个变量维护最小价格，一个变量维护最大利润

```java
		int minprice = Integer.MAX_VALUE;//记录最小价格
        int maxprofit = 0;//记录最大利润
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < minprice)
                minprice = prices[i];
            else if (prices[i] - minprice > maxprofit)
                maxprofit = prices[i] - minprice;
        }
        return maxprofit;
```

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 5
```

#### [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)【简单】考题

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```

思路：贪心，每一次的峰谷都计算进去，得到的结果就是最大值

```java
		int i = 0;
        int valley = prices[0];//谷
        int peak = prices[0];//峰
        int maxprofit = 0;//最大利润
        while (i < prices.length - 1) {
            while (i < prices.length - 1 && prices[i] >= prices[i + 1])
                i++;
            valley = prices[i];//找到谷
            while (i < prices.length - 1 && prices[i] <= prices[i + 1])
                i++;
            peak = prices[i];找到峰
            maxprofit += peak - valley;计算峰谷差值加入到利润中
        }
        return maxprofit;
```

#### [125. 验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)【简单

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

**说明：**本题中，我们将空字符串定义为有效的回文串。

**示例 1:**

```
输入: "A man, a plan, a canal: Panama"
输出: true
```

思路：双指针，跳过非字母数字，比较遇到的值是否相等

```
		int n = s.length();
        int left = 0, right = n - 1;
        while (left < right) {
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                ++left;
            }
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                --right;
            }
            if (left < right) {
                if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
                    return false;
                }
                ++left;
                --right;
            }
        }
        return true;
```

#### [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)【困难】考题

给定一个未排序的整数数组，找出最长连续序列的长度。

要求算法的时间复杂度为 *O(n)*。

**示例:**

```
输入: [100, 4, 200, 1, 3, 2]
输出: 4
解释: 最长连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

思路：哈希表

```java
	public int longestConsecutive(int[] nums) {
        Set<Integer> num_set = new HashSet<Integer>();
        for (int num : nums) {num_set.add(num);}//去重
        int longestStreak = 0;
        for (int num : num_set) {
            if (!num_set.contains(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                while (num_set.contains(currentNum + 1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }

                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }
        return longestStreak;
    }
```

#### [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)【简单】考题

给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

**说明：**

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

思路：位运算，任何数和 0 做异或运算，结果仍然是原来的数，即 a⊕0=a。任何数和其自身做异或运算，结果是 0，即 a⊕a=0。

因此将所有数作异或运算最后的结果就是只出现一个的那个数

```java
		int single = 0;
        for (int num : nums) {
            single ^= num;
        }
        return single;
```

#### [137. 只出现一次的数字 II](https://leetcode-cn.com/problems/single-number-ii/)【中等，基本不考

给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现了三次。找出那个只出现了一次的元素。

思路：

```java
	int seenOnce = 0, seenTwice = 0;
    for (int num : nums) {
      seenOnce = ~seenTwice & (seenOnce ^ num);
      seenTwice = ~seenOnce & (seenTwice ^ num);
    }
    return seenOnce;
```

#### [139. 单词拆分](https://leetcode-cn.com/problems/word-break/)【中等

给定一个**非空**字符串 *s* 和一个包含**非空**单词的列表 *wordDict*，判定 *s* 是否可以被空格拆分为一个或多个在字典中出现的单词。

**说明：**

- 拆分时可以重复使用字典中的单词。
- 你可以假设字典中没有重复的单词。

思路：动态规划，dp[i] 表示字符串 s 前 i个字符组成的字符串 s[0..i-1]是否能被空格拆分成若干个字典中出现的单词。

```java
		Set<String> wordDictSet = new HashSet(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && wordDictSet.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;//只要有满足的就跳出循环
                }
            }
        }
        return dp[s.length()];
```

#### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)【简单】考题

给定一个链表，判断链表中是否有环。

思路：快慢指针，如果两个指针能相遇则一定有环，否则一定无环

```java
	ListNode slow = head;
    ListNode fast = head.next;
    while (slow != fast) {
        if (fast == null || fast.next == null) {
            return false;
        }
        slow = slow.next;
        fast = fast.next.next;
    }
    return true;
```

#### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)【中等】考题

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。

思路：先用快慢指针找到相遇点，然后两个指针一个从头节点出发一个从相遇点出发，最后相遇的地方就是环入口

<img src="https://pic.leetcode-cn.com/99987d4e679fdfbcfd206a4429d9b076b46ad09bd2670f886703fb35ef130635-image.png" alt="image.png" style="zoom: 33%;" />

2(F+a)=F+a+b+a，F=b，因此相遇点在环入口

```java
	private ListNode getIntersect(ListNode head) {
        ListNode tortoise = head;
        ListNode hare = head;
        while (hare != null && hare.next != null) {
            tortoise = tortoise.next;
            hare = hare.next.next;
            if (tortoise == hare) {
                return tortoise;
            }
        }
        return null;
	}

    public ListNode detectCycle(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode intersect = getIntersect(head);
        if (intersect == null) {return null;}
        ListNode ptr1 = head;
        ListNode ptr2 = intersect;
        while (ptr1 != ptr2) {
            ptr1 = ptr1.next;
            ptr2 = ptr2.next;
        }
        return ptr1;
    }
```

#### [143. 重排链表](https://leetcode-cn.com/problems/reorder-list/)【中等】考题

给定一个单链表 *L*：*L*0→*L*1→…→*L**n*-1→*L*n ，
将其重新排列后变为： *L*0→*L**n*→*L*1→*L**n*-1→*L*2→*L**n*-2→…

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

**示例 1:**

```
给定链表 1->2->3->4, 重新排列为 1->4->2->3.
```

思路：快慢指针找中点，反转后半部分链表，然后轮流插入

```java
public void reorderList(ListNode head) {
    if (head == null || head.next == null || head.next.next == null) {
        return;
    }
    //找中点，链表分成两个
    ListNode slow = head;ListNode fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    ListNode newHead = slow.next;
    slow.next = null;
    newHead = reverseList(newHead);//第二个链表倒置
    while (newHead != null) { //链表节点依次连接
        ListNode temp = newHead.next;
        newHead.next = head.next;
        head.next = newHead;
        head = newHead.next;
        newHead = temp;
    }
}

private ListNode reverseList(ListNode head) {
    if (head == null) {return null;}
    ListNode tail = head;
    head = head.next;
    tail.next = null;
    while (head != null) {
        ListNode temp = head.next;
        head.next = tail;
        tail = head;
        head = temp;
    }
    return tail;
}
```

#### [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)【中等】考题

给定一个二叉树，返回它的 *前序* 遍历。

 思路：迭代

```java
		List<Integer> list = new ArrayList<>();
        Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode node = stack.pop();
            list.add(node.val);
            if(node.right!=null)stack.push(node.right);
            if(node.left!=null)stack.push(node.left);
        }
        return list;
```

#### [146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)【中等】考题

运用你所掌握的数据结构，设计和实现一个 [LRU (最近最少使用) 缓存机制](https://baike.baidu.com/item/LRU)。它应该支持以下操作： 获取数据 `get` 和 写入数据 `put` 。

获取数据 `get(key)` - 如果关键字 (key) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。
写入数据 `put(key, value)` - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

思路：哈希表+双端链表，哈希是用来加速读取的

```java
public class LRUCache {
    class DLinkedNode {
        int key;
        int value;
        DLinkedNode prev;
        DLinkedNode next;
        public DLinkedNode() {}
        public DLinkedNode(int _key, int _value) {key = _key; value = _value;}
    }
    private Map<Integer, DLinkedNode> cache = new HashMap<Integer, DLinkedNode>();
    private int size;
    private int capacity;
    private DLinkedNode head, tail;

    public LRUCache(int capacity) {
        this.size = 0;
        this.capacity = capacity;
        head = new DLinkedNode();// 头部和尾部都使用一个虚拟节点
        tail = new DLinkedNode();
        head.next = tail;
        tail.prev = head;
    }
    public int get(int key) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            return -1;
        }
        // 如果 key 存在，先通过哈希表定位，再移到头部
        moveToHead(node);
        return node.value;
    }
    public void put(int key, int value) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            DLinkedNode newNode = new DLinkedNode(key, value);// 如果 key 不存在，创建一个新的节点
            cache.put(key, newNode);// 添加进哈希表
            addToHead(newNode);// 添加至双向链表的头部
            ++size;
            if (size > capacity) {
                DLinkedNode tail = removeTail();// 如果超出容量，删除双向链表的尾部节点
                // 删除哈希表中对应的项
                cache.remove(tail.key);
                --size;
            }
        }
        else {
            node.value = value;
            moveToHead(node);// 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
        }
    }
    private void addToHead(DLinkedNode node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }
    private void removeNode(DLinkedNode node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
    private void moveToHead(DLinkedNode node) {
        removeNode(node);
        addToHead(node);
    }
    private DLinkedNode removeTail() {
        DLinkedNode res = tail.prev;
        removeNode(res);
        return res;
    }
}
```

#### [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)【中等，可能小

在 *O*(*n* log *n*) 时间复杂度和常数级空间复杂度下，对链表进行排序。

思路：从底至上归并

```java
class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        int len = listNodeLength(head); // 获取链表长度
        ListNode sentry = new ListNode(-1);// 哨兵节点，也有叫傀儡节点（处理链表问题的一般技巧）
        sentry.next = head;
        // 循环 log n 次
        for (int i = 1; i < len; i <<= 1) {
            ListNode prev = sentry;
            ListNode curr = sentry.next;
            // 循环 n 次
            while (curr != null) {
                ListNode left = curr;
                ListNode right = split(left, i);
                curr = split(right, i);
                prev.next = mergeTwoLists(left, right);

                while (prev.next != null) {
                    prev = prev.next;
                }
            }
        }

        return sentry.next;
    }

    // 根据步长分隔链表
    private ListNode split(ListNode head, int step) {
        if (head == null) return null;

        for (int i = 1; head.next != null && i < step; i++) {
            head = head.next;
        }

        ListNode right = head.next;
        head.next = null;
        return right;
    }

    // 获取链表的长度
    private int listNodeLength(ListNode head) {
        int length = 0;
        ListNode curr = head;

        while (curr != null) {
            length++;
            curr = curr.next;
        }

        return length;
    }

    // 合并两个有序链表
    private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode sentry = new ListNode(-1);
        ListNode curr = sentry;

        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                curr.next = l1;
                l1 = l1.next;
            } else {
                curr.next = l2;
                l2 = l2.next;
            }

            curr = curr.next;
        }

        curr.next = l1 != null ? l1 : l2;
        return sentry.next;
    }
}
```

#### [150. 逆波兰表达式求值](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)【中等，可能小

根据[ 逆波兰表示法](https://baike.baidu.com/item/逆波兰式/128437)，求表达式的值。

有效的运算符包括 `+`, `-`, `*`, `/` 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

**说明：**

- 整数除法只保留整数部分。
- 给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。

思路：遇数字入栈，遇到运算符，弹出栈顶两个数将计算结果也入栈

#### [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)【中等

给你一个整数数组 `nums` ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

**示例 1:**

```
输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

思路：动态规划，考虑正负值情况，设Fmax(i)是以第i个元素结尾的乘积最大子数组二点乘积

Fmax(i) = max(Fmax(i-1)\*ai，Fmin(i-1)\*ai，ai)，Fmax(i) = min(Fmax(i-1)\*ai，Fmin(i-1)\*ai，ai)

```java
		public int maxProduct(int[] nums) {
        int length = nums.length;
        int[] maxF = new int[length];
        int[] minF = new int[length];
        System.arraycopy(nums, 0, maxF, 0, length);
        System.arraycopy(nums, 0, minF, 0, length);
        for (int i = 1; i < length; ++i) {
            maxF[i] = Math.max(maxF[i - 1] * nums[i], Math.max(nums[i], minF[i - 1] * nums[i]));
            minF[i] = Math.min(minF[i - 1] * nums[i], Math.min(nums[i], maxF[i - 1] * nums[i]));
        }
        int ans = maxF[0];
        for (int i = 1; i < length; ++i) {
            ans = Math.max(ans, maxF[i]);
        }
        return ans;
    }
```

#### [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)【简单】考题

设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

- `push(x)` —— 将元素 x 推入栈中。
- `pop()` —— 删除栈顶的元素。
- `top()` —— 获取栈顶元素。
- `getMin()` —— 检索栈中的最小元素。



```java
class MinStack {
    Deque<Integer> xStack;
    Deque<Integer> minStack;

    public MinStack() {
        xStack = new LinkedList<Integer>();
        minStack = new LinkedList<Integer>();
        minStack.push(Integer.MAX_VALUE);
    }
    public void push(int x) {
        xStack.push(x);
        minStack.push(Math.min(minStack.peek(), x));
    }
    public void pop() {
        xStack.pop();
        minStack.pop();
    }
    public int top() {
        return xStack.peek();
    }
    public int getMin() {
        return minStack.peek();
    }
}
```

#### [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)【简单

编写一个程序，找到两个单链表相交的起始节点。

思路：先求出两个链表的长度，然后长的先跑长度差，然后两个一起跑，当节点相同时就是相交的点

#### [162. 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)【中等

峰值元素是指其值大于左右相邻值的元素。

给定一个输入数组 `nums`，其中 `nums[i] ≠ nums[i+1]`，找到峰值元素并返回其索引。

数组可能包含多个峰值，在这种情况下，返回任何一个峰值所在位置即可。

你可以假设 `nums[-1] = nums[n] = -∞`。

思路：二分查找，只需要找到nums[i]>nums[i-1]的那个拐点就是峰值，因为能走到这

```java
	public int findPeakElement(int[] nums) {
        int l = 0, r = nums.length - 1;
        while (l < r) {
            int mid = (l + r) / 2;
            if (nums[mid] > nums[mid + 1])
                r = mid;
            else
                l = mid + 1;
        }
        return l;
    }
```

#### [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)【简单】考题

给定一个大小为 *n* 的数组，找到其中的多数元素。多数元素是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

思路一：哈希表保存

思路二：

```java
		int count = 0;
        Integer candidate = null;
        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            count += (num == candidate) ? 1 : -1;
        }

        return candidate;
```

#### [171. Excel表列序号](https://leetcode-cn.com/problems/excel-sheet-column-number/)【简单

给定一个Excel表格中的列名称，返回其相应的列序号。

思路：26进制数

```
 		int ans = 0;
        for(int i=0;i<s.length();i++) {
            int num = s.charAt(i) - 'A' + 1;
            ans = ans * 26 + num;
        }
        return ans;
```

#### [172. 阶乘后的零](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)【简单，可能小

给定一个整数 *n*，返回 *n*! 结果尾数中零的数量。

**示例 1:**

```
输入: 3
输出: 0
解释: 3! = 6, 尾数中没有零。
```

思路：数学，

```java
public int trailingZeroes(int n) {
    int zeroCount = 0;
    long currentMultiple = 5;
    while (n > 0) {
        n /= 5;
        zeroCount += n;
    }
    return zeroCount;
}
```

#### [179. 最大数](https://leetcode-cn.com/problems/largest-number/)【中等】考题

给定一组非负整数 `nums`，重新排列它们每位数字的顺序使之组成一个最大的整数。

**注意：**输出结果可能非常大，所以你需要返回一个字符串而不是整数。

思路：将数组变成字符串，然后全部排序，最后再拼接起来

#### [187. 重复的DNA序列](https://leetcode-cn.com/problems/repeated-dna-sequences/)【中等

所有 DNA 都由一系列缩写为 A，C，G 和 T 的核苷酸组成，例如：“ACGAATTCCG”。在研究 DNA 时，识别 DNA 中的重复序列有时会对研究非常有帮助。

编写一个函数来查找目标子串，目标子串的长度为 10，且在 DNA 字符串 `s` 中出现次数超过一次。

**示例：**

```
输入：s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
输出：["AAAAACCCCC", "CCCCCAAAAA"]
```

思路：线性时间窗口+HashSet

<img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWMubGVldGNvZGUtY24uY29tL0ZpZ3VyZXMvMTg3L2hhc2hlczIucG5n?x-oss-process=image/format,png" alt="在这里插入图片描述" style="zoom: 33%;" />

```
	int L = 10, n = s.length();
    HashSet<String> seen = new HashSet(), output = new HashSet();

    // iterate over all sequences of length L
    for (int start = 0; start < n - L + 1; ++start) {
      String tmp = s.substring(start, start + L);
      if (seen.contains(tmp)) output.add(tmp);
      seen.add(tmp);
    }
    return new ArrayList<String>(output);
```

#### [189. 旋转数组](https://leetcode-cn.com/problems/rotate-array/)【简单】考题

给定一个数组，将数组中的元素向右移动 *k* 个位置，其中 *k* 是非负数。

思路：当我们旋转数组 k 次， k%n 个尾部元素会被移动到头部，剩下的元素会被向后移动。

在这个方法中，我们首先将所有元素反转。然后反转前 k 个元素，再反转后面 n−k 个元素，就能得到想要的结果。

#### [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)【简单

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。

**示例 1：**

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

思路：动态规划，dp[i]指第i个元素位置偷到的最多钱，dp[i] = max{dp[i-2]+nums[i],dp[i-1]}，dp[0] = nums[0],dp[1]=max{nums[0],nums[1]}，dp[n-1]即为所求结果

```java
		int length = nums.length;
        if (length == 1) {
            return nums[0];
        }
        int[] dp = new int[length];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        for (int i = 2; i < length; i++) {
            dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[length - 1];
```

#### [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)【中等

给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

思路：BFS，每一层最后一个节点

```java
		List<Integer> res = new ArrayList<>();
		Queue<TreeNode> queue = new LinkedList<>();
		if(root == null) return res;
		queue.offer(root);
		while(!queue.isEmpty()){
			int len = queue.size();
			for (int i = 0; i < len; i++) {
				TreeNode node = queue.poll();
				if(node.left!=null)queue.offer(node.left);
				if(node.right!=null) queue.offer(node.right);
				if(i == len-1) res.add(node.val);
			}
		}
		return res;
```

#### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)【中等】考题

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

思路：深度优先遍历，如果一个位置为 11，则以其为起始节点开始进行深度优先搜索。在深度优先搜索的过程中，每个搜索到的 11 都会被重新标记为 00。最终岛屿的数量就是我们进行深度优先搜索的次数。

```java
void dfs(char[][] grid, int r, int c) {
        int nr = grid.length;
        int nc = grid[0].length;
        if (r < 0 || c < 0 || r >= nr || c >= nc || grid[r][c] == '0') {return;}
        grid[r][c] = '0';//将1置为0
        dfs(grid, r - 1, c);
        dfs(grid, r + 1, c);
        dfs(grid, r, c - 1);
        dfs(grid, r, c + 1);
    }

    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {return 0;}
        int nr = grid.length;
        int nc = grid[0].length;
        int num_islands = 0;
        for (int r = 0; r < nr; ++r) {
            for (int c = 0; c < nc; ++c) {
                if (grid[r][c] == '1') {
                    ++num_islands;
                    dfs(grid, r, c);
                }
            }
        }
        return num_islands;
    }
```

#### [202. 快乐数](https://leetcode-cn.com/problems/happy-number/)【简单

编写一个算法来判断一个数 `n` 是不是快乐数。

「快乐数」定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是 **无限循环** 但始终变不到 1。如果 **可以变为** 1，那么这个数就是快乐数。

思路：模拟+HashSet保存防止形成一个环

```java
	private int getNext(int n) {
        int totalSum = 0;
        while (n > 0) {
            int d = n % 10;
            n = n / 10;
            totalSum += d * d;
        }
        return totalSum;
    }

    public boolean isHappy(int n) {
        Set<Integer> seen = new HashSet<>();
        while (n != 1 && !seen.contains(n)) {
            seen.add(n);
            n = getNext(n);
        }
        return n == 1;
    }
```

#### [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)【简单

删除链表中等于给定值 ***val\*** 的所有节点。

思路：加一个哨兵节点，然后每次要保留前一个节点为了删除

```java
	ListNode sentinel = new ListNode(0);
    sentinel.next = head;
    ListNode prev = sentinel, curr = head;
    while (curr != null) {
      if (curr.val == val) prev.next = curr.next;
      else prev = curr;
      curr = curr.next;
    }
    return sentinel.next;
```

#### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)【简单】考题

反转一个单链表。

```java
		ListNode pre = null,curr = head,next = null;
        while(curr != null){
            next = curr.next;
            curr.next = pre;
            pre = curr;
            curr = next;
        }
        return pre;
```

#### [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)【中等】考题

在未排序的数组中找到第 **k** 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

思路一：快速排序的划分过程，当某次划分的q为倒数第k个元素的时候就可以了

```java
Random random = new Random();

    public int findKthLargest(int[] nums, int k) {
        return quickSelect(nums, 0, nums.length - 1, nums.length - k);
    }
    public int quickSelect(int[] a, int l, int r, int index) {
        int q = randomPartition(a, l, r);
        if (q == index) {
            return a[q];
        } else {
            return q < index ? quickSelect(a, q + 1, r, index) : quickSelect(a, l, q - 1, index);
        }
    }
    public int randomPartition(int[] a, int l, int r) {
        int i = random.nextInt(r - l + 1) + l;
        swap(a, i, r);
        return partition(a, l, r);
    }
    public int partition(int[] a, int l, int r) {//划分成两块
        int x = a[r], i = l - 1;
        for (int j = l; j < r; ++j) {
            if (a[j] <= x) {
                swap(a, ++i, j);
            }
        }
        swap(a, i + 1, r);
        return i + 1;
    }
    public void swap(int[] a, int i, int j) {int temp = a[i];a[i] = a[j];a[j] = temp;}
```

思路二：堆排序，删除k-1个大堆顶得到第k大元素

```java
	public int findKthLargest(int[] nums, int k) {
        int heapSize = nums.length;
        buildMaxHeap(nums, heapSize);
        for (int i = nums.length - 1; i >= nums.length - k + 1; --i) {//拿掉堆顶重新建堆，k次之后堆顶就是结果
            swap(nums, 0, i);
            --heapSize;
            maxHeapify(nums, 0, heapSize);
        }
        return nums[0];
    }
    public void buildMaxHeap(int[] a, int heapSize) {//从后向前建堆
        for (int i = heapSize / 2; i >= 0; --i) {
            maxHeapify(a, i, heapSize);
        } 
    }
    public void maxHeapify(int[] a, int i, int heapSize) {//递归建堆
        int l = i * 2 + 1, r = i * 2 + 2, largest = i;
        if (l < heapSize && a[l] > a[largest]) {largest = l;} 
        if (r < heapSize && a[r] > a[largest]) {largest = r;}
        if (largest != i) {
            swap(a, i, largest);
            maxHeapify(a, largest, heapSize);
        }
    }
    public void swap(int[] a, int i, int j) {int temp = a[i];a[i] = a[j];a[j] = temp;}
```

#### [217. 存在重复元素](https://leetcode-cn.com/problems/contains-duplicate/)【简单

给定一个整数数组，判断是否存在重复元素。

如果任意一值在数组中出现至少两次，函数返回 `true` 。如果数组中每个元素都不相同，则返回 `false` 。

思路一：排序，时间O(nlogn)

思路二：哈希表，时间O(n)，空间O(n)

#### [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)【中等】考题

在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

思路：动态规划，`dp[i][j]`为以(i,j)为右下角的正方形边长最大值

nums(i,j)==1，dp(i,j) = min(dp(i-1,j),dp(i,j-1),dp(i-1,j-1))+1

nums(i,j)==0，dp(i,j) = 0，

```java
	public int maximalSquare(char[][] matrix) {
        int maxSide = 0;
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return maxSide;
        }
        int rows = matrix.length, columns = matrix[0].length;
        int[][] dp = new int[rows][columns];//默认都是0
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < columns; j++) {
                if (matrix[i][j] == '1') {
                    if (i == 0 || j == 0) {
                        dp[i][j] = 1;
                    } else {
                        dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
                    }
                    maxSide = Math.max(maxSide, dp[i][j]);
                }
            }
        }
        int maxSquare = maxSide * maxSide;
        return maxSquare;
    }
```

#### [225. 用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)【简单

使用队列实现栈的下列操作：

- push(x) -- 元素 x 入栈
- pop() -- 移除栈顶元素
- top() -- 获取栈顶元素
- empty() -- 返回栈是否为空

思路：两个队列

```java
class MyStack {
    Queue<Integer> queue1;
    Queue<Integer> queue2;
    public MyStack() {
        queue1 = new LinkedList<Integer>();
        queue2 = new LinkedList<Integer>();
    }
    public void push(int x) {
        queue2.offer(x);
        while (!queue1.isEmpty()) {
            queue2.offer(queue1.poll());
        }
        Queue<Integer> temp = queue1;
        queue1 = queue2;
        queue2 = temp;
    }
    public int pop() {
        return queue1.poll();
    }
    public int top() {
        return queue1.peek();
    }
    public boolean empty() {
        return queue1.isEmpty();
    }
}
```

#### [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)【简单

翻转一棵二叉树。

思路：递归

```java
	public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        root.left = right;
        root.right = left;
        return root;
    }
```

#### [227. 基本计算器 II](https://leetcode-cn.com/problems/basic-calculator-ii/)【中等

实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式仅包含非负整数，`+`， `-` ，`*`，`/` 四种运算符和空格 ` `。 整数除法仅保留整数部分。

思路：用栈，遇加减号存数，遇乘除号取栈顶计算结果入栈

#### [230. 二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)【中等】考题

给定一个二叉搜索树，编写一个函数 `kthSmallest` 来查找其中第 **k** 个最小的元素。

**说明：**
你可以假设 k 总是有效的，1 ≤ k ≤ 二叉搜索树元素个数。

思路：中序遍历迭代，可以提前找到停下

```java
 	LinkedList<TreeNode> stack = new LinkedList<TreeNode>();
    while (true) {
      while (root != null) {
        stack.add(root);
        root = root.left;
      }
      root = stack.removeLast();
      if (--k == 0) return root.val;
      root = root.right;
    }
```

#### [232. 用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)【简单】考题

使用栈实现队列的下列操作：

- push(x) -- 将一个元素放入队列的尾部。
- pop() -- 从队列首部移除元素。
- peek() -- 返回队列首部的元素。
- empty() -- 返回队列是否为空。

```java
private Stack<Integer> s1 = new Stack<>();
private Stack<Integer> s2 = new Stack<>();
public void push(int x) {
    if (s1.empty())
        front = x;
    s1.push(x);
}
public void pop() {
    if (s2.isEmpty()) {
        while (!s1.isEmpty())
            s2.push(s1.pop());
    }
    s2.pop();    
}
```

#### [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)【简单】考题

请判断一个链表是否为回文链表。

思路：快慢指针将后半部分反转，然后比较

#### [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)【简单

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

思路：如果当前节点的值大于 p 和 q 的值，说明 p 和 q 应该在当前节点的左子树，因此将当前节点移动到它的左子节点；

如果当前节点的值小于 p 和 q 的值，说明 p 和 q 应该在当前节点的右子树，因此将当前节点移动到它的右子节点；

如果当前节点的值不满足上述两条要求，那么说明当前节点就是「分岔点」。此时，p 和 q 要么在当前节点的不同的子树中，要么其中一个就是当前节点。

```java
	public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode ancestor = root;
        while (true) {
            if (p.val < ancestor.val && q.val < ancestor.val) {
                ancestor = ancestor.left;
            } else if (p.val > ancestor.val && q.val > ancestor.val) {
                ancestor = ancestor.right;
            } else {
                break;
            }
        }
        return ancestor;
    }
```

#### [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)【中等】考题

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

思路：递归,定义fx 表示 x节点的子树中是否包含 p 节点或 q 节点

一般在调用类内函数使用this

```java
	private TreeNode ans;
    private boolean dfs(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return false;
        boolean lson = dfs(root.left, p, q);
        boolean rson = dfs(root.right, p, q);
        if ((lson && rson) || ((root.val == p.val || root.val == q.val) && (lson || rson))) {
            ans = root;
        } 
        return lson || rson || (root.val == p.val || root.val == q.val);
    }
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        this.dfs(root, p, q);
        return this.ans;
    }
```

#### [237. 删除链表中的节点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)【简单

请编写一个函数，使其可以删除某个链表中给定的（非末尾）节点。传入函数的唯一参数为 **要被删除的节点** 。

思路：没给头节点，所以将当前节点值改为下一个节点值，然后删除下一个节点

#### [238. 除自身以外数组的乘积](https://leetcode-cn.com/problems/product-of-array-except-self/)【中等

给你一个长度为 *n* 的整数数组 `nums`，其中 *n* > 1，返回输出数组 `output` ，其中 `output[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积。

```java
public int[] productExceptSelf(int[] nums) {
        int length = nums.length;
        int[] L = new int[length];// L 和 R 分别表示左右两侧的乘积列表
        int[] R = new int[length];
        int[] answer = new int[length];
        
        L[0] = 1;// L[i] 为索引 i 左侧所有元素的乘积，对于索引为 '0' 的元素，因为左侧没有元素，所以 L[0] = 1
        for (int i = 1; i < length; i++) {
            L[i] = nums[i - 1] * L[i - 1];
        }

        R[length - 1] = 1;
        // R[i] 为索引 i 右侧所有元素的乘积,对于索引为 'length-1' 的元素，因为右侧没有元素，所以 R[length-1] = 1
        for (int i = length - 2; i >= 0; i--) {
            R[i] = nums[i + 1] * R[i + 1];
        }
        
        for (int i = 0; i < length; i++) {
            answer[i] = L[i] * R[i];
        }
        return answer;
    }
```

#### [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)【中等】考题

编写一个高效的算法来搜索 *m* x *n* 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

```java
 	public boolean searchMatrix(int[][] matrix, int target) {
        int row = matrix.length-1;//左下角
        int col = 0;
        while (row >= 0 && col < matrix[0].length) {
            if (matrix[row][col] > target) {
                row--;
            } else if (matrix[row][col] < target) {
                col++;
            } else { // found it
                return true;
            }
        }
        return false;
    }
```

#### [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)【简单

给定两个字符串 *s* 和 *t* ，编写一个函数来判断 *t* 是否是 *s* 的字母异位词。

思路：如果不是小写字母的限定，那么不使用数组使用哈希表

```
	if (s.length() != t.length()) {
        return false;
    }
    int[] table = new int[26];
    for (int i = 0; i < s.length(); i++) {
        table[s.charAt(i) - 'a']++;
    }
    for (int i = 0; i < t.length(); i++) {
        table[t.charAt(i) - 'a']--;
        if (table[t.charAt(i) - 'a'] < 0) {
            return false;
        }
    }
    return true;
```

#### [258. 各位相加](https://leetcode-cn.com/problems/add-digits/)【简单

给定一个非负整数 `num`，反复将各个位上的数字相加，直到结果为一位数。

思路：直接写，迭代方法

```java
public int addDigits(int num) {
    while (num >= 10) {
        int next = 0;
        while (num != 0) {
            next = next + num % 10;
            num /= 10;
        }
        num = next;
    }
    return num;
}
```

#### [268. 缺失数字](https://leetcode-cn.com/problems/missing-number/)【简单】考题

给定一个包含 `0, 1, 2, ..., n` 中 *n* 个数的序列，找出 0 .. *n* 中没有出现在序列中的那个数。

思路一：异或

思路二：先求0...n的和，再减去数组的和即可

#### [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)【中等

给定正整数 *n*，找到若干个完全平方数（比如 `1, 4, 9, 16, ...`）使得它们的和等于 *n*。你需要让组成和的完全平方数的个数最少。



```java
Set<Integer> square_nums = new HashSet<Integer>();

  protected boolean is_divided_by(int n, int count) {
    if (count == 1) {
      return square_nums.contains(n);
    }

    for (Integer square : square_nums) {
      if (is_divided_by(n - square, count - 1)) {
        return true;
      }
    }
    return false;
  }

  public int numSquares(int n) {
    this.square_nums.clear();

    for (int i = 1; i * i <= n; ++i) {
      this.square_nums.add(i * i);
    }

    int count = 1;
    for (; count <= n; ++count) {
      if (is_divided_by(n, count))
        return count;
    }
    return count;
  }
```

#### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)【简单

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

思路：双指针

```
	for (int lastNonZeroFoundAt = 0, cur = 0; cur < nums.size(); cur++) {
        if (nums[cur] != 0) {
            swap(nums[lastNonZeroFoundAt++], nums[cur]);
        }
    }
```

#### [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)【中等

给定一个包含 *n* + 1 个整数的数组 *nums*，其数字都在 1 到 *n* 之间（包括 1 和 *n*），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

思路：快慢指针，i->nums[i]，因为有重复，因此一定有环。和链表入口环那题思路一致

```java
 public int findDuplicate(int[] nums) {
        int slow = 0, fast = 0;
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
```

#### [292. Nim 游戏](https://leetcode-cn.com/problems/nim-game/)【简单

你和你的朋友，两个人一起玩 [Nim 游戏](https://baike.baidu.com/item/Nim游戏/6737105)：桌子上有一堆石头，每次你们轮流拿掉 1 - 3 块石头。 拿掉最后一块石头的人就是获胜者。你作为先手。你们是聪明人，每一步都是最优解。 编写一个函数，来判断你是否可以在给定石头数量的情况下赢得游戏。

思路：如果堆中石头的数量 n*n* 不能被 44 整除，那么你*总是*可以赢得 Nim 游戏的胜利。

#### [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)【中等】考题

给定一个无序的整数数组，找到其中最长上升子序列的长度。

**示例:**

```
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```

思路：动态规划，dp(i)是以第i个数字结尾的最长上升子序列长度

```java
		int[] dp = new int[nums.length];
		Arrays.fill(dp, 1);
		for (int i = 0; i < nums.length; i++) {
			for (int j = 0; j < i; j++) {
				if (nums[j] < nums[i]) {
					dp[i] = Math.max(dp[i], dp[j] + 1);
				}
			}
		}
		int res = 0;
		for (int i : dp) {//找dp数组中的最大值
			if (i > res) {
				res = i;
			}
		}
		return res;
```

#### [313. 超级丑数](https://leetcode-cn.com/problems/super-ugly-number/)【中等

编写一段程序来查找第 `n` 个超级丑数。

超级丑数是指其所有质因数都是长度为 `k` 的质数列表 `primes` 中的正整数。

#### [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)【中等】考题

给定不同面额的硬币 `coins` 和一个总金额 `amount`。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 `-1`。

你可以认为每种硬币的数量是无限的。

思路：动态规划F(S)：组成金额 S所需的最少硬币数量，F(S) = min(F(S-cj))+1

```java
public int coinChange(int[] coins, int amount) {
    int max = amount + 1;
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, max);//设置成max是为了后面不存在的情况
    dp[0] = 0;
    for (int i = 1; i <= amount; i++) {
      for (int j = 0; j < coins.length; j++) {
        if (coins[j] <= i) {
          dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
        }
      }
    }
    return dp[amount] > amount ? -1 : dp[amount];
  }
```

#### [326. 3的幂](https://leetcode-cn.com/problems/power-of-three/)【简单

给定一个整数，写一个函数来判断它是否是 3 的幂次方。

思路：一直除下去

#### [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)【中等】考题

给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

思路：就是奇偶头节点相连，然后尾部

```java
		ListNode virtual = new ListNode(0);
		virtual.next = head;
		ListNode first = virtual.next;
        if(first == null){
            return null;
        }
		ListNode second = first.next;
		ListNode f = first;
		ListNode s = second;
		while (s != null && s.next != null) {
			f.next = s.next;
			f = f.next;
			s.next = f.next;
			s = s.next;
		}
		f.next = second;
		return virtual.next;
```

#### [334. 递增的三元子序列](https://leetcode-cn.com/problems/increasing-triplet-subsequence/)【中等

给定一个未排序的数组，判断这个数组中是否存在长度为 3 的递增子序列。

数学表达式如下:

> 如果存在这样的 *i, j, k,* 且满足 0 ≤ *i* < *j* < *k* ≤ *n*-1，
> 使得 *arr[i]* < *arr[j]* < *arr[k]* ，返回 true ; 否则返回 false 。

**说明:** 要求算法的时间复杂度为 O(*n*)，空间复杂度为 O(*1*) 。

思路：新建两个变量 small 和 mid ，分别用来保存题目要我们求的长度为 3 的递增子序列的最小值和中间值。接着，我们遍历数组，每遇到一个数字，我们将它和 small 和 mid 相比，若小于等于 small ，则替换 small；否则，若小于等于 mid，则替换 mid；否则，若大于 mid，则说明我们找到了长度为 3 的递增数组！

```java
bool increasingTriplet(vector<int>& nums) {
    int len = nums.size();
    if (len < 3) return false;
    int small = INT_MAX, mid = INT_MAX;
    for (auto num : nums) {
      if (num <= small) {
        small = num;
      } else if (num <= mid) {
        mid = num;
      } 
      else if (num > mid) {
        return true;
      }
    }
    return false;    
  }
```

#### [337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)【中等

在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

思路：

```
	Map<TreeNode, Integer> f = new HashMap<TreeNode, Integer>();
    Map<TreeNode, Integer> g = new HashMap<TreeNode, Integer>();
    public int rob(TreeNode root) {
        dfs(root);
        return Math.max(f.getOrDefault(root, 0), g.getOrDefault(root, 0));
    }

    public void dfs(TreeNode node) {
        if (node == null) {
            return;
        }
        dfs(node.left);
        dfs(node.right);
        f.put(node, node.val + g.getOrDefault(node.left, 0) + g.getOrDefault(node.right, 0));
        g.put(node, Math.max(f.getOrDefault(node.left, 0), g.getOrDefault(node.left, 0)) +			
        			Math.max(f.getOrDefault(node.right, 0), g.getOrDefault(node.right, 0)));
    }
```

#### [338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)【中等

给定一个非负整数 **num**。对于 **0 ≤ i ≤ num** 范围中的每个数字 **i** ，计算其二进制数中的 1 的数目并将它们作为数组返回。

思路：动态规划，*P*(*x*)=*P*(*x*/2)+(*x*mod2)

```
	  int[] ans = new int[num + 1];
      for (int i = 1; i <= num; ++i)
        ans[i] = ans[i >> 1] + (i & 1); 
      return ans;
```

#### [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)【简单】考题

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `char[]` 的形式给出。

不要给另外的数组分配额外的空间，你必须**[原地](https://baike.baidu.com/item/原地算法)修改输入数组**、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 [ASCII](https://baike.baidu.com/item/ASCII) 码表中的可打印字符。

思路：首尾双指针，交换

#### [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)【中等

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

思路：首先遍历整个数组，并使用哈希表记录每个数字出现的次数，并形成一个「出现次数数组」。找出原数组的前 k 个高频元素，就相当于找出「出现次数数组」的前 k 大的值。

在这里，我们可以利用堆的思想：建立一个小顶堆，然后遍历「出现次数数组」：如果堆的元素个数小于 k，就可以直接插入堆中。如果堆的元素个数等于 k，则检查堆顶与当前出现次数的大小。如果堆顶更大，说明至少有 k 个数字的出现次数比当前值大，故舍弃当前值；否则，就弹出堆顶，并将当前值插入堆中。
遍历完成后，堆中的元素就代表了「出现次数数组」中前 k 大的值。

```java
		Map<Integer, Integer> occurrences = new HashMap<Integer, Integer>();
        for (int num : nums) {
            occurrences.put(num, occurrences.getOrDefault(num, 0) + 1);
        }
        // int[] 的第一个元素代表数组的值，第二个元素代表了该值出现的次数
        PriorityQueue<int[]> queue = new PriorityQueue<int[]>(new Comparator<int[]>() {
            public int compare(int[] m, int[] n) {
                return m[1] - n[1];
            }
        });
        for (Map.Entry<Integer, Integer> entry : occurrences.entrySet()) {
            int num = entry.getKey(), count = entry.getValue();
            if (queue.size() == k) {
                if (queue.peek()[1] < count) {
                    queue.poll();
                    queue.offer(new int[]{num, count});
                }
            } else {
                queue.offer(new int[]{num, count});
            }
        }
        int[] ret = new int[k];
        for (int i = 0; i < k; ++i) {
            ret[i] = queue.poll()[0];
        }
        return ret;
    }
```

#### [349. 两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)【简单

给定两个数组，编写一个函数来计算它们的交集。

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

思路：两个栈，然后求栈的交集

#### [371. 两整数之和](https://leetcode-cn.com/problems/sum-of-two-integers/)【简单

**不使用**运算符 `+` 和 `-` ，计算两整数 `a` 、`b` 之和。

思路：位运算

```java
	public int getSum(int a, int b) {
        while(b != 0){
            int temp = a ^ b;
            b = (a & b) << 1;
            a = temp;
        }
        return a;
	}
```

#### [378. 有序矩阵中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)【中等

给定一个 *`n x n`* 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第 `k` 小的元素。
请注意，它是排序后的第 `k` 小元素，而不是第 `k` 个不同的元素。

示例：

```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]],
k = 8,
返回 13。
```

```java
public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int left = matrix[0][0];
        int right = matrix[n - 1][n - 1];
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (check(matrix, mid, k, n)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
    public boolean check(int[][] matrix, int mid, int k, int n) {//判断是不是有k个大于等于mid的数
        int i = n - 1;
        int j = 0;
        int num = 0;
        while (i >= 0 && j < n) {
            if (matrix[i][j] <= mid) {
                num += i + 1;
                j++;
            } else {
                i--;
            }
        }
        return num >= k;
    }
}
```

#### [394. 字符串解码](https://leetcode-cn.com/problems/decode-string/)【中等

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 *encoded_string* 正好重复 *k* 次。注意 *k* 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 *k* ，例如不会出现像 `3a` 或 `2[4]` 的输入。

**示例 1：**

```
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

思路：数字一个栈字母一个栈

#### [402. 移掉K位数字](https://leetcode-cn.com/problems/remove-k-digits/)【中等

给定一个以字符串表示的非负整数 *num*，移除这个数中的 *k* 位数字，使得剩下的数字最小。

**注意:**

- *num* 的长度小于 10002 且 ≥ *k。*
- *num* 不会包含任何前导零。

**示例 1 :**

```
输入: num = "1432219", k = 3
输出: "1219"
解释: 移除掉三个数字 4, 3, 和 2 形成一个新的最小的数字 1219。
```

```
LinkedList<Character> stack = new LinkedList<Character>();
        
    for(char digit : num.toCharArray()) {
      while(stack.size() > 0 && k > 0 && stack.peekLast() > digit) {
        stack.removeLast();
        k -= 1;
      }
      stack.addLast(digit);
    }
        
    /* remove the remaining digits from the tail. */
    for(int i=0; i<k; ++i) {
      stack.removeLast();
    }
        
    // build the final string, while removing the leading zeros.
    StringBuilder ret = new StringBuilder();
    boolean leadingZero = true;
    for(char digit: stack) {
      if(leadingZero && digit == '0') continue;
      leadingZero = false;
      ret.append(digit);
    }
        
    /* return the final string  */
    if (ret.length() == 0) return "0";
    return ret.toString();
  }
```

#### [406. 根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)【中等

假设有打乱顺序的一群人站成一个队列。 每个人由一个整数对`(h, k)`表示，其中`h`是这个人的身高，`k`是排在这个人前面且身高大于或等于`h`的人数。 编写一个算法来重建这个队列。



```java
public int[][] reconstructQueue(int[][] people) {
    Arrays.sort(people, new Comparator<int[]>() {
      @Override
      public int compare(int[] o1, int[] o2) {
        // if the heights are equal, compare k-values
        return o1[0] == o2[0] ? o1[1] - o2[1] : o2[0] - o1[0];
      }
    });

    List<int[]> output = new LinkedList<>();
    for(int[] p : people){
      output.add(p[1], p);
    }

    int n = people.length;
    return output.toArray(new int[n][2]);
  }
```

#### [409. 最长回文串](https://leetcode-cn.com/problems/longest-palindrome/)【简单

给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 `"Aa"` 不能当做一个回文字符串。

**注意:**
假设字符串的长度不会超过 1010。

```
 	public int longestPalindrome(String s) {
        int[] count = new int[128];
        for (char c: s.toCharArray())
            count[c]++;

        int ans = 0;
        for (int v: count) {
            ans += v / 2 * 2;
            if (v % 2 == 1 && ans % 2 == 0)
                ans++;
        }
        return ans;
    }
```

#### [412. Fizz Buzz](https://leetcode-cn.com/problems/fizz-buzz/)【简单

写一个程序，输出从 1 到 *n* 数字的字符串表示。

如果 *n* 是3的倍数，输出“Fizz”；

如果 *n* 是5的倍数，输出“Buzz”；

如果 *n* 同时是3和5的倍数，输出 “FizzBuzz”。

思路：模拟

#### [415. 字符串相加](https://leetcode-cn.com/problems/add-strings/)【简单】考题

给定两个字符串形式的非负整数 `num1` 和`num2` ，计算它们的和。

**提示：**

1. `num1` 和`num2` 的长度都小于 5100
2. `num1` 和`num2` 都只包含数字 `0-9`
3. `num1` 和`num2` 都不包含任何前导零
4. **你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式**

思路：就是从个位开始相加，每次加完变成字符串，然后记得进位，这样可以保证不会溢出

#### [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)【中等

给定一个**只包含正整数**的**非空**数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

**注意:**

1. 每个数组中的元素不会超过 100
2. 数组的大小不会超过 200

思路：动态规划，

`dp[i][j]`表示从数组的 `[0, i]` 这个子区间内挑选一些正整数，每个数只能用一次，使得这些数的和**恰好等于** `j`。

`dp[i][j] = dp[i - 1][j] or dp[i - 1][j - nums[i]]`

```
int sum = 0;
        for (int num : nums) {
            sum += num;
        }

        // 特判：如果是奇数，就不符合要求
        if ((sum & 1) == 1) {
            return false;
        }

        int target = sum / 2;
        // 创建二维状态数组，行：物品索引，列：容量（包括 0）
        boolean[][] dp = new boolean[len][target + 1];

        // 先填表格第 0 行，第 1 个数只能让容积为它自己的背包恰好装满
        if (nums[0] <= target) {
            dp[0][nums[0]] = true;
        }
        // 再填表格后面几行
        for (int i = 1; i < len; i++) {
            for (int j = 0; j <= target; j++) {
                // 直接从上一行先把结果抄下来，然后再修正
                dp[i][j] = dp[i - 1][j];

                if (nums[i] == j) {
                    dp[i][j] = true;
                    continue;
                }
                if (nums[i] < j) {
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i]];
                }
            }
        }
        return dp[len - 1][target];
    }
```

#### [448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)【简单

给定一个范围在 1 ≤ a[i] ≤ *n* ( *n* = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, *n*] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为*O(n)*的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

思路：使用哈希表，

#### [454. 四数相加 II](https://leetcode-cn.com/problems/4sum-ii/)【中等

给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 `(i, j, k, l)` ，使得 `A[i] + B[j] + C[k] + D[l] = 0`。

为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -228 到 228 - 1 之间，最终结果不会超过 231 - 1 。

**例如:**

```
输入:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]
输出:
2
```

#### [455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)【简单

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。对每个孩子 i ，都有一个胃口值 gi ，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j ，都有一个尺寸 sj 。如果 sj >= gi ，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

思路：预排序+双指针

#### [461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)【简单

两个整数之间的[汉明距离](https://baike.baidu.com/item/汉明距离)指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 `x` 和 `y`，计算它们之间的汉明距离。

```
	int xor = x ^ y;
    int distance = 0;
    while (xor != 0) {
      if (xor % 2 == 1)
        distance += 1;
      xor = xor >> 1;
    }
    return distance;
```

#### [476. 数字的补数](https://leetcode-cn.com/problems/number-complement/)【简单

给定一个正整数，输出它的补数。补数是对该数的二进制表示取反。

```
 	public int findComplement(int num) {
        int maxBitNum = 0;
        int tmpNum = num;
        while (tmpNum > 0) {
            maxBitNum += 1;
            tmpNum >>= 1;
        }
        return num ^ ((1 << maxBitNum) - 1);
    }
```

#### [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)【中等

给出二叉 **搜索** 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 `node` 的新值等于原树中大于或等于 `node.val` 的值之和。

思路：反序中序遍历

```
	int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if (root != null) {
            convertBST(root.right);
            sum += root.val;
            root.val = sum;
            convertBST(root.left);
        }
        return root;
    }
```

#### [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)【简单】考题

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

思路：递归

```java
	public int diameterOfBinaryTree(TreeNode root) {
        if(root == null) return 0;
        ans = 0;
        depth(root);
        return ans-1;
    }
    int depth(TreeNode node){
        if(node==null) return 0;
        int left = depth(node.left);
        int right = depth(node.right);
        ans = Math.max(ans, left+right+1);
        return Math.max(left, right)+1;
    }
```

#### [560. 和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)【中等

给定一个整数数组和一个整数 **k，**你需要找到该数组中和为 **k** 的连续的子数组的个数。

**示例 1 :**

```
输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
```

```
	public int subarraySum(int[] nums, int k) {
        int count = 0, pre = 0;
        HashMap < Integer, Integer > mp = new HashMap < > ();
        mp.put(0, 1);
        for (int i = 0; i < nums.length; i++) {
            pre += nums[i];
            if (mp.containsKey(pre - k)) {
                count += mp.get(pre - k);
            }
            mp.put(pre, mp.getOrDefault(pre, 0) + 1);
        }
        return count;
    }
```

#### [575. 分糖果](https://leetcode-cn.com/problems/distribute-candies/)【简单

给定一个**偶数**长度的数组，其中不同的数字代表着不同种类的糖果，每一个数字代表一个糖果。你需要把这些糖果**平均**分给一个弟弟和一个妹妹。返回妹妹可以获得的最大糖果的种类数。

思路：HashSet，min(count，n/2)

```
 	public int distributeCandies(int[] candies) {
        HashSet < Integer > set = new HashSet < > ();
        for (int candy: candies) {
            set.add(candy);
        }
        return Math.min(set.size(), candies.length / 2);
    }
```

#### [581. 最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)【简单

给定一个整数数组，你需要寻找一个**连续的子数组**，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

你找到的子数组应是**最短**的，请输出它的长度。

思路：这个算法背后的思想是无序子数组中最小元素的正确位置可以决定左边界，最大元素的正确位置可以决定右边界。因此，首先我们需要找到原数组在哪个位置开始不是升序的。我们从头开始遍历数组，一旦遇到降序的元素，我们记录最小元素为 min 。类似的，我们逆序扫描数组 nums，当数组出现升序的时候，我们记录最大元素为 max。然后，我们再次遍历 nums 数组并通过与其他元素进行比较，来找到 min 和 max 在原数组中的正确位置。我们只需要从头开始找到第一个大于 min 的元素，从尾开始找到第一个小于 max 的元素，它们之间就是最短无序子数组。

```java
	public int findUnsortedSubarray(int[] nums) {
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        boolean flag = false;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] < nums[i - 1])
                flag = true;
            if (flag)
                min = Math.min(min, nums[i]);
        }
        flag = false;
        for (int i = nums.length - 2; i >= 0; i--) {
            if (nums[i] > nums[i + 1])
                flag = true;
            if (flag)
                max = Math.max(max, nums[i]);
        }
        int l, r;
        for (l = 0; l < nums.length; l++) {
            if (min < nums[l])
                break;
        }
        for (r = nums.length - 1; r >= 0; r--) {
            if (max > nums[r])
                break;
        }
        return r - l < 0 ? 0 : r - l + 1;
    }
```

#### [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)【简单

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则**不为** NULL 的节点将直接作为新二叉树的节点。

思路：DFS合并

```
	public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null) {
            return t2;
        }
        if (t2 == null) {
            return t1;
        }
        TreeNode merged = new TreeNode(t1.val + t2.val);
        merged.left = mergeTrees(t1.left, t2.left);
        merged.right = mergeTrees(t1.right, t2.right);
        return merged;
    }
```

#### [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)【中等

给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

思路：中心扩展

```java
	public int countSubstrings(String s) {
        int n = s.length(), ans = 0;
        for (int i = 0; i < 2 * n - 1; ++i) {
            int l = i / 2, r = i / 2 + i % 2;
            while (l >= 0 && r < n && s.charAt(l) == s.charAt(r)) {
                --l;
                ++r;
                ++ans;
            }
        }
        return ans;
    }
```

#### [654. 最大二叉树](https://leetcode-cn.com/problems/maximum-binary-tree/)【中等

给定一个不含重复元素的整数数组。一个以此数组构建的最大二叉树定义如下：

1. 二叉树的根是数组中的最大元素。
2. 左子树是通过数组中最大值左边部分构造出的最大二叉树。
3. 右子树是通过数组中最大值右边部分构造出的最大二叉树。

通过给定的数组构建最大二叉树，并且输出这个树的根节点。

```java
	public TreeNode constructMaximumBinaryTree(int[] nums) {
        return construct(nums, 0, nums.length);
    }
    public TreeNode construct(int[] nums, int l, int r) {
        if (l == r)
            return null;
        int max_i = max(nums, l, r);
        TreeNode root = new TreeNode(nums[max_i]);
        root.left = construct(nums, l, max_i);
        root.right = construct(nums, max_i + 1, r);
        return root;
    }
    public int max(int[] nums, int l, int r) {
        int max_i = l;
        for (int i = l; i < r; i++) {
            if (nums[max_i] < nums[i])
                max_i = i;
        }
        return max_i;
    }
```

#### [717. 1比特与2比特字符](https://leetcode-cn.com/problems/1-bit-and-2-bit-characters/)【简单

有两种特殊字符。第一种字符可以用一比特`0`来表示。第二种字符可以用两比特(`10` 或 `11`)来表示。

现给一个由若干比特组成的字符串。问最后一个字符是否必定为一个一比特字符。给定的字符串总是由0结束。

```
	public boolean isOneBitCharacter(int[] bits) {
        int i = 0;
        while (i < bits.length - 1) {
            i += bits[i] + 1;
        }
        return i == bits.length - 1;
    }
```

#### [718. 最长重复子数组](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)【中等】考题

给两个整数数组 `A` 和 `B` ，返回两个数组中公共的、长度最长的子数组的长度。

思路：动态规划，这样我们就可以提出动态规划的解法：令 `dp[i][j]` 表示 A[i:] 和 B[j:] 的最长公共前缀，那么答案即为所有` dp[i][j] `中的最大值。如果 A[i] == B[j]，那么` dp[i][j] = dp[i + 1][j + 1] + 1`，否则 `dp[i][j]` = 0。

```
	public int findLength(int[] A, int[] B) {
        int n = A.length, m = B.length;
        int[][] dp = new int[n + 1][m + 1];
        int ans = 0;
        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 0; j--) {
                dp[i][j] = A[i] == B[j] ? dp[i + 1][j + 1] + 1 : 0;
                ans = Math.max(ans, dp[i][j]);
            }
        }
        return ans;
    }
```

#### [728. 自除数](https://leetcode-cn.com/problems/self-dividing-numbers/)【简单

*自除数* 是指可以被它包含的每一位数除尽的数。

例如，128 是一个自除数，因为 `128 % 1 == 0`，`128 % 2 == 0`，`128 % 8 == 0`。

还有，自除数不允许包含 0 。

给定上边界和下边界数字，输出一个列表，列表的元素是边界（含边界）内所有的自除数。

```
	public List<Integer> selfDividingNumbers(int left, int right) {
        List<Integer> ans = new ArrayList();
        for (int n = left; n <= right; ++n) {
            if (selfDividing(n)) ans.add(n);
        }
        return ans;
    }
    public boolean selfDividing(int n) {
        for (char c: String.valueOf(n).toCharArray()) {
            if (c == '0' || (n % (c - '0') > 0))
                return false;
        }
        return true;
    }
```

#### [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)【中等

请根据每日 `气温` 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

例如，给定一个列表 `temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`，你的输出应该是 `[1, 1, 4, 2, 1, 1, 0, 0]`。

**提示：**`气温` 列表长度的范围是 `[1, 30000]`。每个气温的值的均为华氏度，都是在 `[30, 100]` 范围内的整数。

思路：单调栈

```java
	public int[] dailyTemperatures(int[] T) {
        int length = T.length;
        int[] ans = new int[length];
        Deque<Integer> stack = new LinkedList<Integer>();
        for (int i = 0; i < length; i++) {
            int temperature = T[i];
            while (!stack.isEmpty() && temperature > T[stack.peek()]) {
                int prevIndex = stack.pop();
                ans[prevIndex] = i - prevIndex;
            }
            stack.push(i);
        }
        return ans;
    }
```

#### [763. 划分字母区间](https://leetcode-cn.com/problems/partition-labels/)【中等

字符串 `S` 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一个字母只会出现在其中的一个片段。返回一个表示每个字符串片段的长度的列表。

```java
	public List<Integer> partitionLabels(String S) {
        int[] last = new int[26];
        for (int i = 0; i < S.length(); ++i)
            last[S.charAt(i) - 'a'] = i;
        
        int j = 0, anchor = 0;
        List<Integer> ans = new ArrayList();
        for (int i = 0; i < S.length(); ++i) {
            j = Math.max(j, last[S.charAt(i) - 'a']);
            if (i == j) {
                ans.add(i - anchor + 1);
                anchor = i + 1;
            }
        }
        return ans;
    }
```

#### [767. 重构字符串](https://leetcode-cn.com/problems/reorganize-string/)【中等

给定一个字符串`S`，检查是否能重新排布其中的字母，使得两相邻的字符不同。

若可行，输出任意可行的结果。若不可行，返回空字符串。

思路：太难了，除了暴力解没有思路

#### [771. 宝石与石头](https://leetcode-cn.com/problems/jewels-and-stones/)【简单

 给定字符串`J` 代表石头中宝石的类型，和字符串 `S`代表你拥有的石头。 `S` 中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。

`J` 中的字母不重复，`J` 和 `S`中的所有字符都是字母。字母区分大小写，因此`"a"`和`"A"`是不同类型的石头。

```
 	public int numJewelsInStones(String J, String S) {
        int jewelsCount = 0;
        Set<Character> jewelsSet = new HashSet<Character>();
        int jewelsLength = J.length(), stonesLength = S.length();
        for (int i = 0; i < jewelsLength; i++) {
            char jewel = J.charAt(i);
            jewelsSet.add(jewel);
        }
        for (int i = 0; i < stonesLength; i++) {
            char stone = S.charAt(i);
            if (jewelsSet.contains(stone)) {
                jewelsCount++;
            }
        }
        return jewelsCount;
    }
```

#### [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)【中等】考题

给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长公共子序列的长度。

一个字符串的 *子序列* 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。

若这两个字符串没有公共子序列，则返回 0。

**示例 1:**

```
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace"，它的长度为 3。
```

思路：动态规划

`dp[i][j]` 的含义是：对于 `s1[1..i]` 和 `s2[1..j]`，它们的 LCS 长度是 `dp[i][j]`。

```java
public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m + 1][n + 1];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 获取两个串字符
                char c1 = text1.charAt(i), c2 = text2.charAt(j);
                if (c1 == c2) {
                    // 去找它们前面各退一格的值加1即可
                    dp[i + 1][j + 1] = dp[i][j] + 1;
                } else {
                    //要么是text1往前退一格，要么是text2往前退一格，两个的最大值
                    dp[i + 1][j + 1] = Math.max(dp[i + 1][j], dp[i][j + 1]);
                }
            }
        }
        return dp[m][n];
    }
```

#### [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)【简单

找出数组中重复的数字

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

思路：用HashSet保存结果

```java
	public int findRepeatNumber(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        int repeat = -1;
        for (int num : nums) {
            if (!set.add(num)) {
                repeat = num;
                break;
            }
        }
        return repeat;
    }
```

#### [剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)【简单

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

```java
 	public String replaceSpace(String s) {
        int length = s.length();
        char[] array = new char[length * 3];
        int size = 0;
        for (int i = 0; i < length; i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                array[size++] = '%';
                array[size++] = '2';
                array[size++] = '0';
            } else {
                array[size++] = c;
            }
        }
        String newStr = new String(array, 0, size);
        return newStr;
    }
```

#### [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)【简单

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

思路：使用栈

```java
	public int[] reversePrint(ListNode head) {
        Stack<ListNode> stack = new Stack<ListNode>();
        ListNode temp = head;
        while (temp != null) {
            stack.push(temp);
            temp = temp.next;
        }
        int size = stack.size();
        int[] print = new int[size];
        for (int i = 0; i < size; i++) {
            print[i] = stack.pop().val;
        }
        return print;
    }
```

#### [剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)【简单】考题

写一个函数，输入 `n` ，求斐波那契（Fibonacci）数列的第 `n` 项。斐波那契数列的定义如下：

```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
```

斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

```
	public int fib(int n) {
        int a = 0, b = 1, sum;
        for(int i = 0; i < n; i++){
            sum = (a + b) % 1000000007;
            a = b;
            b = sum;
        }
        return a;
    }
```

#### [剑指 Offer 11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)【简单

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 `[3,4,5,1,2]` 为 `[1,2,3,4,5]` 的一个旋转，该数组的最小值为1。 

思路：二分

```java
	public int minArray(int[] numbers) {
        int low = 0;
        int high = numbers.length - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (numbers[mid] < numbers[high]) {
                high = mid;
            } else if (numbers[mid] > numbers[high]) {
                low = mid + 1;
            } else {
                high -= 1;
            }
        }
        return numbers[low];
    }
```

#### [剑指 Offer 14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)【中等

给你一根长度为 `n` 的绳子，请把绳子剪成整数长度的 `m` 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 `k[0],k[1]...k[m-1]` 。请问 `k[0]*k[1]*...*k[m-1]` 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

```
	public int cuttingRope(int n) {
        if(n <= 3) return n - 1;
        int a = n / 3, b = n % 3;
        if(b == 0) return (int)Math.pow(3, a);
        if(b == 1) return (int)Math.pow(3, a - 1) * 4;
        return (int)Math.pow(3, a) * 2;
    }
```

#### [剑指 Offer 15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)【简单】考题

请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

```
	public int hammingWeight(int n) {
        int res = 0;
        while(n != 0) {
            res += n & 1;
            n >>>= 1;
        }
        return res;
    }
```

#### [剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)【简单

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

思路：可以真删除节点，也可以假删除

#### [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)【简单】考题

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

思路：首尾双指针

#### [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)【中等】考题

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

思路：递归

```java
	public boolean isSubStructure(TreeNode A, TreeNode B) {
        return (A != null && B != null) && (recur(A, B) || isSubStructure(A.left, B) || 		
        		isSubStructure(A.right, B));
    }
    boolean recur(TreeNode A, TreeNode B) {
        if(B == null) return true;
        if(A == null || A.val != B.val) return false;
        return recur(A.left, B.left) && recur(A.right, B.right);
    }
```

#### [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)【简单】考题

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

思路：递归

```java
	public TreeNode mirrorTree(TreeNode root) {
        if(root == null) return null;
        TreeNode tmp = root.left;
        root.left = mirrorTree(root.right);
        root.right = mirrorTree(tmp);
        return root;
    }
```

迭代：

```java
public TreeNode mirrorTree(TreeNode root) {
        if(root == null) return null;
        Stack<TreeNode> stack = new Stack<>() {{ add(root); }};
        while(!stack.isEmpty()) {
            TreeNode node = stack.pop();
            if(node.left != null) stack.add(node.left);
            if(node.right != null) stack.add(node.right);
            TreeNode tmp = node.left;
            node.left = node.right;
            node.right = tmp;
        }
        return root;
    }
```

#### [剑指 Offer 31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)【中等

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

思路：用一个栈模拟，如果栈顶元素等于弹出序列当前值，那么弹出栈

```java
	public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> stack = new Stack<>();
        int i = 0;
        for(int num : pushed) {
            stack.push(num); // num 入栈
            while(!stack.isEmpty() && stack.peek() == popped[i]) { // 循环判断与出栈
                stack.pop();
                i++;
            }
        }
        return stack.isEmpty();
    }
```

#### [剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)【中等

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。



```java
 	public boolean verifyPostorder(int[] postorder) {
        return recur(postorder, 0, postorder.length - 1);
    }
    boolean recur(int[] postorder, int i, int j) {
        if(i >= j) return true;
        int p = i;
        while(postorder[p] < postorder[j]) p++;
        int m = p;
        while(postorder[p] > postorder[j]) p++;
        return p == j && recur(postorder, i, m - 1) && recur(postorder, m, j - 1);
    }
```

#### [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)【中等

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

```java
	Node pre, head;
    public Node treeToDoublyList(Node root) {
        if(root == null) return null;
        dfs(root);
        head.left = pre;
        pre.right = head;
        return head;
    }
    void dfs(Node cur) {
        if(cur == null) return;
        dfs(cur.left);
        if(pre != null) pre.right = cur;
        else head = cur;
        cur.left = pre;
        pre = cur;
        dfs(cur.right);
    }
```

#### [剑指 Offer 38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)【中等

输入一个字符串，打印出该字符串中字符的所有排列。

你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。

思路：回溯

```java
List<String> res = new LinkedList<>();
    char[] c;
    public String[] permutation(String s) {
        c = s.toCharArray();
        dfs(0);
        return res.toArray(new String[res.size()]);
    }
    void dfs(int x) {
        if(x == c.length - 1) {
            res.add(String.valueOf(c)); // 添加排列方案
            return;
        }
        HashSet<Character> set = new HashSet<>();
        for(int i = x; i < c.length; i++) {
            if(set.contains(c[i])) continue; // 重复，因此剪枝
            set.add(c[i]);
            swap(i, x); // 交换，将 c[i] 固定在第 x 位 
            dfs(x + 1); // 开启固定第 x + 1 位字符
            swap(i, x); // 恢复交换
        }
    }
    void swap(int a, int b) {char tmp = c[a];c[a] = c[b];c[b] = tmp;}
```

#### [剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)【简单

输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

思路：快排；堆排序

#### [剑指 Offer 41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)【困难】考题

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

- void addNum(int num) - 从数据流中添加一个整数到数据结构中。
- double findMedian() - 返回目前所有元素的中位数。

思路：建立一个 小顶堆 AA 和 大顶堆 BB ，各保存列表的一半元素，且规定：

A 保存 较大 的一半，
B 保存 较小 的一半

```java
class MedianFinder {
    Queue<Integer> A, B;
    public MedianFinder() {
        A = new PriorityQueue<>(); // 小顶堆，保存较大的一半
        B = new PriorityQueue<>((x, y) -> (y - x)); // 大顶堆，保存较小的一半
    }
    public void addNum(int num) {
        if(A.size() != B.size()) {
            A.add(num);
            B.add(A.poll());
        } else {
            B.add(num);
            A.add(B.poll());
        }
    }
    public double findMedian() {
        return A.size() != B.size() ? A.peek() : (A.peek() + B.peek()) / 2.0;
    }
}
```

#### [剑指 Offer 45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)【中等

输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

思路：和前面的最大数字一样，排序，只不过修改排序的方式

```
		String[] strs = new String[nums.length];
        for(int i = 0; i < nums.length; i++) 
            strs[i] = String.valueOf(nums[i]);
        Arrays.sort(strs, (x, y) -> (x + y).compareTo(y + x));
        StringBuilder res = new StringBuilder();
        for(String s : strs)
            res.append(s);
        return res.toString();
```

#### [剑指 Offer 47. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)【中等

在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

思路：和64题最小路径和思路一样，动态规划

```java
		int m = grid.length, n = grid[0].length;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(i == 0 && j == 0) continue;
                if(i == 0) grid[i][j] += grid[i][j - 1] ;
                else if(j == 0) grid[i][j] += grid[i - 1][j];
                else grid[i][j] += Math.max(grid[i][j - 1], grid[i - 1][j]);
            }
        }
        return grid[m - 1][n - 1];
```

#### [剑指 Offer 49. 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)【中等

我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

思路：动态规划

<img src="https://pic.leetcode-cn.com/837411664f096417badf857fa51e77fd30cb1309a5637c37d24d8a4a48a42b03-Picture1.png" alt="Picture1.png" style="zoom: 40%;" />

```java
		public int nthUglyNumber(int n) {
        int a = 0, b = 0, c = 0;
        int[] dp = new int[n];
        dp[0] = 1;
        for(int i = 1; i < n; i++) {
            int n2 = dp[a] * 2, n3 = dp[b] * 3, n5 = dp[c] * 5;
            dp[i] = Math.min(Math.min(n2, n3), n5);
            if(dp[i] == n2) a++;
            if(dp[i] == n3) b++;
            if(dp[i] == n5) c++;
        }
        return dp[n - 1];
    }
```

#### [剑指 Offer 50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)【简单

在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

思路：哈希表

```
	public char firstUniqChar(String s) {
        HashMap<Character, Boolean> dic = new HashMap<>();
        char[] sc = s.toCharArray();
        for(char c : sc)
            dic.put(c, !dic.containsKey(c));
        for(char c : sc)
            if(dic.get(c)) return c;
        return ' ';
    }
```

#### [剑指 Offer 51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)【困难】考题

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

**示例 1:**

```
输入: [7,5,6,4]
输出: 5
```

思路：归并排序的时候计算

```java
public int reversePairs(int[] nums) {
        int len = nums.length;
        if (len < 2) {return 0;}
        int[] copy = new int[len];
        for (int i = 0; i < len; i++) {
            copy[i] = nums[i];
        }
        int[] temp = new int[len];
        return reversePairs(copy, 0, len - 1, temp);
    }

    private int reversePairs(int[] nums, int left, int right, int[] temp) {
        if (left == right) {return 0;}

        int mid = left + (right - left) / 2;
        int leftPairs = reversePairs(nums, left, mid, temp);
        int rightPairs = reversePairs(nums, mid + 1, right, temp);

        if (nums[mid] <= nums[mid + 1]) {
            return leftPairs + rightPairs;
        }
        int crossPairs = mergeAndCount(nums, left, mid, right, temp);
        return leftPairs + rightPairs + crossPairs;
    }
    private int mergeAndCount(int[] nums, int left, int mid, int right, int[] temp) {
        for (int i = left; i <= right; i++) {
            temp[i] = nums[i];
        }

        int i = left;
        int j = mid + 1;
        int count = 0;
        for (int k = left; k <= right; k++) {
            if (i == mid + 1) {
                nums[k] = temp[j];
                j++;
            } else if (j == right + 1) {
                nums[k] = temp[i];
                i++;
            } else if (temp[i] <= temp[j]) {
                nums[k] = temp[i];
                i++;
            } else {
                nums[k] = temp[j];
                j++;
                count += (mid - i + 1);
            }
        }
        return count;
    }
```

#### [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)【简单

一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

思路：268题

#### [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)【简单

给定一棵二叉搜索树，请找出其中第k大的节点。

思路：反序前序遍历，迭代法可以提前停下，和230题类似

#### [剑指 Offer 55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)【简单】考题

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

思路：递归，深度等于左右最大值+1

```java
	public boolean isBalanced(TreeNode root) {
        if (root == null) return true;
        return Math.abs(depth(root.left) - depth(root.right)) <= 1 && isBalanced(root.left) && 	
        															  isBalanced(root.right);
    }
    private int depth(TreeNode root) {
        if (root == null) return 0;
        return Math.max(depth(root.left), depth(root.right)) + 1;
    }
```

#### [剑指 Offer 56 - I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)【中等

一个整型数组 `nums` 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。

思路：先对所有数字进行一次异或，得到两个出现一次的数字的异或值。在异或结果中找到任意为 1 的位。根据这一位对所有的数字进行分组。在每个组内进行异或操作，得到两个数字。

```java
	public int[] singleNumbers(int[] nums) {
        int ret = 0;
        for (int n : nums) {
            ret ^= n;
        }
        int div = 1;
        while ((div & ret) == 0) {
            div <<= 1;
        }
        int a = 0, b = 0;
        for (int n : nums) {
            if ((div & n) != 0) {
                a ^= n;
            } else {
                b ^= n;
            }
        }
        return new int[]{a, b};
    }
```

#### [剑指 Offer 57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)【简单

输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

思路：排序数组，使用双指针法

```
	public int[] twoSum(int[] nums, int target) {
        int i = 0, j = nums.length - 1;
        while(i < j) {
            int s = nums[i] + nums[j];
            if(s < target) i++;
            else if(s > target) j--;
            else return new int[] { nums[i], nums[j] };
        }
        return new int[0];
    }
```

#### [剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)【简单

输入一个正整数 `target` ，输出所有和为 `target` 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

思考：滑动窗口

#### [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)【简单

输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

思考：按单词变成字符串数组，然后反转

#### [剑指 Offer 58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)【简单

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

思路：和前面的旋转数组类似

#### [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)【简单】考题

给定一个数组 `nums` 和滑动窗口的大小 `k`，请找出所有滑动窗口里的最大值。

思路：就是暴力寻找每个滑动窗口内部的最大值

#### [剑指 Offer 61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)【简单

从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

思路一：Set+遍历

思路二：排序

#### [剑指 Offer 64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)【中等

求 `1+2+...+n` ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

思路：递归

```java
	public int sumNums(int n) {
		if(n==1) return 1;
        n += sumNums(n - 1);
        return n;
    }
```



