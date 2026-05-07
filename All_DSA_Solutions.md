# Unit 1 - Linked Lists & Stacks

## 1. Palindrome Linked List (LeetCode 234)
**Problem Statement:** Given the `head` of a singly linked list, return `true` if it is a palindrome or `false` otherwise.
**Example Input:** `[1,2,2,1]` -> **Output:** `true`

```java
import java.util.Scanner;

class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}

public class PalindromeLinkedList {
    public static boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) return true;
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode prev = null;
        while (slow != null) {
            ListNode next = slow.next;
            slow.next = prev;
            prev = slow;
            slow = next;
        }
        ListNode left = head, right = prev;
        while (right != null) {
            if (left.val != right.val) return false;
            left = left.next;
            right = right.next;
        }
        return true;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter number of nodes:");
        if (!sc.hasNextInt()) return;
        int n = sc.nextInt();
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        if (n > 0) {
            System.out.println("Enter elements:");
            for (int i = 0; i < n; i++) {
                curr.next = new ListNode(sc.nextInt());
                curr = curr.next;
            }
        }
        System.out.println("Is Palindrome: " + isPalindrome(dummy.next));
    }
}
```

## 2. Merge Two Sorted Lists (LeetCode 21)
**Problem Statement:** You are given the heads of two sorted linked lists `list1` and `list2`. Merge the two lists into one sorted list. Return the head of the merged linked list.
**Example Input:** `list1 = [1,2,4]`, `list2 = [1,3,4]` -> **Output:** `[1,1,2,3,4,4]`

```java
import java.util.Scanner;

public class MergeTwoSortedLists {
    public static ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        while(list1 != null && list2 != null) {
            if(list1.val <= list2.val) {
                curr.next = list1;
                list1 = list1.next;
            } else {
                curr.next = list2;
                list2 = list2.next;
            }
            curr = curr.next;
        }
        curr.next = (list1 != null) ? list1 : list2;
        return dummy.next;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter number of nodes in list 1:");
        int n1 = sc.nextInt();
        ListNode dummy1 = new ListNode(0);
        ListNode curr1 = dummy1;
        if (n1 > 0) System.out.println("Enter elements for list 1:");
        for(int i=0; i<n1; i++) {
            curr1.next = new ListNode(sc.nextInt());
            curr1 = curr1.next;
        }

        System.out.println("Enter number of nodes in list 2:");
        int n2 = sc.nextInt();
        ListNode dummy2 = new ListNode(0);
        ListNode curr2 = dummy2;
        if (n2 > 0) System.out.println("Enter elements for list 2:");
        for(int i=0; i<n2; i++) {
            curr2.next = new ListNode(sc.nextInt());
            curr2 = curr2.next;
        }

        ListNode merged = mergeTwoLists(dummy1.next, dummy2.next);
        System.out.println("Merged list:");
        while(merged != null) {
            System.out.print(merged.val + " ");
            merged = merged.next;
        }
    }
}
```

## 3. Valid Parentheses (LeetCode 20)
**Problem Statement:** Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.
**Example Input:** `s = "()[]{}"` -> **Output:** `true`

```java
import java.util.Scanner;
import java.util.Stack;

public class ValidParentheses {
    public static boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for(char c : s.toCharArray()) {
            if(c == '(') stack.push(')');
            else if(c == '{') stack.push('}');
            else if(c == '[') stack.push(']');
            else if(stack.isEmpty() || stack.pop() != c) return false;
        }
        return stack.isEmpty();
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter string:");
        String s = sc.next();
        System.out.println("Is Valid: " + isValid(s));
    }
}
```

## 4. Reverse Linked List (LeetCode 206)
**Problem Statement:** Given the `head` of a singly linked list, reverse the list, and return the reversed list.
**Example Input:** `head = [1,2,3,4,5]` -> **Output:** `[5,4,3,2,1]`

```java
import java.util.Scanner;

public class ReverseLinkedList {
    public static ListNode reverseList(ListNode head) {
        ListNode prev = null, curr = head;
        while(curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter number of nodes:");
        if (!sc.hasNextInt()) return;
        int n = sc.nextInt();
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        if(n > 0) {
            System.out.println("Enter elements:");
            for(int i=0; i<n; i++) {
                curr.next = new ListNode(sc.nextInt());
                curr = curr.next;
            }
        }
        ListNode reversed = reverseList(dummy.next);
        System.out.println("Reversed list:");
        while(reversed != null) {
            System.out.print(reversed.val + " ");
            reversed = reversed.next;
        }
    }
}
```

## 5. Reverse Nodes in k-Group (LeetCode 25)
**Problem Statement:** Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return the modified list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.
**Example Input:** `head = [1,2,3,4,5]`, `k = 2` -> **Output:** `[2,1,4,3,5]`

```java
import java.util.Scanner;

public class ReverseNodesInKGroup {
    public static ListNode reverseKGroup(ListNode head, int k) {
        ListNode curr = head;
        int count = 0;
        while(curr != null && count != k) {
            curr = curr.next;
            count++;
        }
        if(count == k) {
            curr = reverseKGroup(curr, k);
            while(count-- > 0) {
                ListNode next = head.next;
                head.next = curr;
                curr = head;
                head = next;
            }
            head = curr;
        }
        return head;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter number of nodes:");
        int n = sc.nextInt();
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        if(n > 0) {
            System.out.println("Enter elements:");
            for(int i=0; i<n; i++) {
                curr.next = new ListNode(sc.nextInt());
                curr = curr.next;
            }
        }
        System.out.println("Enter k:");
        int k = sc.nextInt();

        ListNode res = reverseKGroup(dummy.next, k);
        System.out.println("Result:");
        while(res != null) {
            System.out.print(res.val + " ");
            res = res.next;
        }
    }
}
```


# Unit 2 - Stacks, Queues & Arrays

## 6. Next Greater Element I (LeetCode 496)
**Problem Statement:** The next greater element of some element `x` in an array is the first greater element that is to the right of `x` in the same array. Given two distinct 0-indexed integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`, find the next greater element.
**Example Input:** `nums1 = [4,1,2]`, `nums2 = [1,3,4,2]` -> **Output:** `[-1,3,-1]`

```java
import java.util.*;

public class NextGreaterElement {
    public static int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map = new HashMap<>();
        Stack<Integer> stack = new Stack<>();
        for (int num : nums2) {
            while (!stack.isEmpty() && stack.peek() < num) {
                map.put(stack.pop(), num);
            }
            stack.push(num);
        }
        int[] res = new int[nums1.length];
        for (int i = 0; i < nums1.length; i++) {
            res[i] = map.getOrDefault(nums1[i], -1);
        }
        return res;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter size of nums1:");
        int n1 = sc.nextInt();
        int[] nums1 = new int[n1];
        System.out.println("Enter elements of nums1:");
        for(int i=0; i<n1; i++) nums1[i] = sc.nextInt();

        System.out.println("Enter size of nums2:");
        int n2 = sc.nextInt();
        int[] nums2 = new int[n2];
        System.out.println("Enter elements of nums2:");
        for(int i=0; i<n2; i++) nums2[i] = sc.nextInt();

        int[] res = nextGreaterElement(nums1, nums2);
        System.out.println("Result: " + Arrays.toString(res));
    }
}
```

## 7. Final Prices With a Special Discount in a Shop (LeetCode 1475)
**Problem Statement:** Given an integer array `prices` where `prices[i]` is the price of the `ith` item. You will receive a discount equivalent to `prices[j]` where `j` is the minimum index such that `j > i` and `prices[j] <= prices[i]`. Return the final prices.
**Example Input:** `prices = [8,4,6,2,3]` -> **Output:** `[4,2,4,2,3]`

```java
import java.util.*;

public class FinalPrices {
    public static int[] finalPrices(int[] prices) {
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < prices.length; i++) {
            while (!stack.isEmpty() && prices[stack.peek()] >= prices[i]) {
                prices[stack.pop()] -= prices[i];
            }
            stack.push(i);
        }
        return prices;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter number of items:");
        int n = sc.nextInt();
        int[] prices = new int[n];
        System.out.println("Enter prices:");
        for(int i=0; i<n; i++) prices[i] = sc.nextInt();
        System.out.println("Final prices: " + Arrays.toString(finalPrices(prices)));
    }
}
```

## 8. First Unique Character in a String (LeetCode 387)
**Problem Statement:** Given a string `s`, find the first non-repeating character in it and return its index. If it does not exist, return `-1`.
**Example Input:** `s = "leetcode"` -> **Output:** `0`

```java
import java.util.Scanner;

public class FirstUniqueCharacter {
    public static int firstUniqChar(String s) {
        int[] freq = new int[26];
        for(char c : s.toCharArray()) freq[c - 'a']++;
        for(int i = 0; i < s.length(); i++) {
            if(freq[s.charAt(i) - 'a'] == 1) return i;
        }
        return -1;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter string:");
        String s = sc.next();
        System.out.println("First unique character index: " + firstUniqChar(s));
    }
}
```

## 9. Daily Temperatures (LeetCode 739)
**Problem Statement:** Given an array of integers `temperatures` represents the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the `ith` day to get a warmer temperature.
**Example Input:** `temperatures = [73,74,75,71,69,72,76,73]` -> **Output:** `[1,1,4,2,1,1,0,0]`

```java
import java.util.*;

public class DailyTemperatures {
    public static int[] dailyTemperatures(int[] temperatures) {
        int[] res = new int[temperatures.length];
        Stack<Integer> stack = new Stack<>();
        for(int i=0; i<temperatures.length; i++) {
            while(!stack.isEmpty() && temperatures[stack.peek()] < temperatures[i]) {
                int idx = stack.pop();
                res[idx] = i - idx;
            }
            stack.push(i);
        }
        return res;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter number of days:");
        int n = sc.nextInt();
        int[] temps = new int[n];
        System.out.println("Enter temperatures:");
        for(int i=0; i<n; i++) temps[i] = sc.nextInt();
        System.out.println("Result: " + Arrays.toString(dailyTemperatures(temps)));
    }
}
```

## 10. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit (LeetCode 1438)
**Problem Statement:** Given an array of integers `nums` and an integer `limit`, return the size of the longest non-empty subarray such that the absolute difference between any two elements of this subarray is less than or equal to `limit`.
**Example Input:** `nums = [8,2,4,7], limit = 4` -> **Output:** `2`

```java
import java.util.*;

public class LongestSubarrayLimit {
    public static int longestSubarray(int[] nums, int limit) {
        Deque<Integer> maxD = new LinkedList<>();
        Deque<Integer> minD = new LinkedList<>();
        int i = 0, j;
        for (j = 0; j < nums.length; j++) {
            while (!maxD.isEmpty() && nums[j] > nums[maxD.peekLast()]) maxD.pollLast();
            while (!minD.isEmpty() && nums[j] < nums[minD.peekLast()]) minD.pollLast();
            maxD.addLast(j);
            minD.addLast(j);
            while (nums[maxD.peekFirst()] - nums[minD.peekFirst()] > limit) {
                if (maxD.peekFirst() == i) maxD.pollFirst();
                if (minD.peekFirst() == i) minD.pollFirst();
                i++;
            }
        }
        return j - i;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter array size:");
        int n = sc.nextInt();
        int[] nums = new int[n];
        System.out.println("Enter elements:");
        for(int i=0; i<n; i++) nums[i] = sc.nextInt();
        System.out.println("Enter limit:");
        int limit = sc.nextInt();
        System.out.println("Longest Subarray Length: " + longestSubarray(nums, limit));
    }
}
```


# Unit 3 - Trees & Graphs (Part 1)

## 11. Kth Smallest Element in a BST (LeetCode 230)
**Problem Statement:** Given the `root` of a binary search tree, and an integer `k`, return the `kth` smallest value (1-indexed) of all the values of the nodes in the tree.
**Example Input:** `root = [3,1,4,null,2], k = 1` -> **Output:** `1`

```java
import java.util.*;

class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int x) { val = x; }
}

public class KthSmallestBST {
    static int count = 0;
    static int result = -1;

    public static int kthSmallest(TreeNode root, int k) {
        count = 0;
        inorder(root, k);
        return result;
    }

    private static void inorder(TreeNode node, int k) {
        if (node == null) return;
        inorder(node.left, k);
        count++;
        if (count == k) {
            result = node.val;
            return;
        }
        inorder(node.right, k);
    }

    public static void main(String[] args) {
        // Sample tree: 3 -> left:1, right:4, 1->right:2
        TreeNode root = new TreeNode(3);
        root.left = new TreeNode(1);
        root.right = new TreeNode(4);
        root.left.right = new TreeNode(2);
        
        Scanner sc = new Scanner(System.in);
        System.out.println("Sample BST built. Enter k:");
        int k = sc.nextInt();
        System.out.println("Kth smallest: " + kthSmallest(root, k));
    }
}
```

## 12. Symmetric Tree (LeetCode 101)
**Problem Statement:** Given the `root` of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).
**Example Input:** `root = [1,2,2,3,4,4,3]` -> **Output:** `true`

```java
import java.util.*;

public class SymmetricTree {
    public static boolean isSymmetric(TreeNode root) {
        if(root == null) return true;
        return isMirror(root.left, root.right);
    }

    private static boolean isMirror(TreeNode t1, TreeNode t2) {
        if(t1 == null && t2 == null) return true;
        if(t1 == null || t2 == null) return false;
        return (t1.val == t2.val) && isMirror(t1.left, t2.right) && isMirror(t1.right, t2.left);
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(2);
        root.left.left = new TreeNode(3);
        root.left.right = new TreeNode(4);
        root.right.left = new TreeNode(4);
        root.right.right = new TreeNode(3);

        System.out.println("Is symmetric: " + isSymmetric(root));
    }
}
```

## 13. Binary Tree Inorder Traversal (LeetCode 94)
**Problem Statement:** Given the `root` of a binary tree, return the inorder traversal of its nodes' values.
**Example Input:** `root = [1,null,2,3]` -> **Output:** `[1,3,2]`

```java
import java.util.*;

public class InorderTraversal {
    public static List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        helper(root, res);
        return res;
    }

    private static void helper(TreeNode node, List<Integer> res) {
        if(node != null) {
            helper(node.left, res);
            res.add(node.val);
            helper(node.right, res);
        }
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        root.right = new TreeNode(2);
        root.right.left = new TreeNode(3);

        System.out.println("Inorder Traversal: " + inorderTraversal(root));
    }
}
```

## 14. Path Sum II (LeetCode 113)
**Problem Statement:** Given the `root` of a binary tree and an integer `targetSum`, return all root-to-leaf paths where the sum of the node values in the path equals `targetSum`.
**Example Input:** `root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22` -> **Output:** `[[5,4,11,2],[5,8,4,5]]`

```java
import java.util.*;

public class PathSumII {
    public static List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(root, targetSum, new ArrayList<>(), res);
        return res;
    }

    private static void backtrack(TreeNode node, int targetSum, List<Integer> current, List<List<Integer>> res) {
        if(node == null) return;
        current.add(node.val);
        if(node.left == null && node.right == null && targetSum == node.val) {
            res.add(new ArrayList<>(current));
        } else {
            backtrack(node.left, targetSum - node.val, current, res);
            backtrack(node.right, targetSum - node.val, current, res);
        }
        current.remove(current.size() - 1);
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(5);
        root.left = new TreeNode(4);
        root.right = new TreeNode(8);
        root.left.left = new TreeNode(11);
        root.left.left.left = new TreeNode(7);
        root.left.left.right = new TreeNode(2);
        root.right.left = new TreeNode(13);
        root.right.right = new TreeNode(4);
        root.right.right.left = new TreeNode(5);
        root.right.right.right = new TreeNode(1);

        System.out.println("Paths with sum 22: " + pathSum(root, 22));
    }
}
```

## 15. Rotting Oranges (LeetCode 994)
**Problem Statement:** You are given an `m x n` grid where where each cell can have one of three values: `0` representing an empty cell, `1` representing a fresh orange, or `2` representing a rotten orange. Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten. Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return `-1`.
**Example Input:** `grid = [[2,1,1],[1,1,0],[0,1,1]]` -> **Output:** `4`

```java
import java.util.*;

public class RottingOranges {
    public static int orangesRotting(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        Queue<int[]> q = new LinkedList<>();
        int fresh = 0;
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(grid[i][j] == 2) q.offer(new int[]{i,j});
                if(grid[i][j] == 1) fresh++;
            }
        }
        if(fresh == 0) return 0;
        int dirs[][] = {{-1,0},{1,0},{0,-1},{0,1}};
        int minutes = 0;
        while(!q.isEmpty() && fresh > 0) {
            int size = q.size();
            for(int i=0; i<size; i++) {
                int[] curr = q.poll();
                for(int[] d : dirs) {
                    int r = curr[0] + d[0];
                    int c = curr[1] + d[1];
                    if(r>=0 && r<m && c>=0 && c<n && grid[r][c] == 1) {
                        grid[r][c] = 2;
                        fresh--;
                        q.offer(new int[]{r,c});
                    }
                }
            }
            minutes++;
        }
        return fresh == 0 ? minutes : -1;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter rows and cols:");
        int m = sc.nextInt();
        int n = sc.nextInt();
        int[][] grid = new int[m][n];
        System.out.println("Enter grid (0: empty, 1: fresh, 2: rotten):");
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                grid[i][j] = sc.nextInt();
            }
        }
        System.out.println("Minutes to rot all: " + orangesRotting(grid));
    }
}
```


# Unit 4 - Graphs, DP & Strings

## 16. Find if Path Exists in Graph (LeetCode 1971)
**Problem Statement:** There is a bi-directional graph with `n` vertices, where each vertex is labeled from `0` to `n - 1`. You are given a 2D integer array `edges` and two nodes `source` and `destination`. Return `true` if there is a valid path from `source` to `destination`, or `false` otherwise.
**Example Input:** `n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2` -> **Output:** `true`

```java
import java.util.*;

public class PathExistsInGraph {
    public static boolean validPath(int n, int[][] edges, int source, int destination) {
        List<List<Integer>> adj = new ArrayList<>();
        for(int i=0; i<n; i++) adj.add(new ArrayList<>());
        for(int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }
        boolean[] visited = new boolean[n];
        Queue<Integer> q = new LinkedList<>();
        q.offer(source);
        visited[source] = true;
        
        while(!q.isEmpty()) {
            int curr = q.poll();
            if(curr == destination) return true;
            for(int neighbor : adj.get(curr)) {
                if(!visited[neighbor]) {
                    visited[neighbor] = true;
                    q.offer(neighbor);
                }
            }
        }
        return false;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter n:");
        int n = sc.nextInt();
        System.out.println("Enter number of edges:");
        int e = sc.nextInt();
        int[][] edges = new int[e][2];
        System.out.println("Enter edges (u v):");
        for(int i=0; i<e; i++) {
            edges[i][0] = sc.nextInt();
            edges[i][1] = sc.nextInt();
        }
        System.out.println("Enter source and destination:");
        int src = sc.nextInt();
        int dst = sc.nextInt();
        System.out.println("Path exists: " + validPath(n, edges, src, dst));
    }
}
```

## 17. Max Area of Island (LeetCode 695)
**Problem Statement:** You are given an `m x n` binary matrix `grid`. An island is a group of `1`s (representing land) connected 4-directionally. Return the maximum area of an island in `grid`. If there is no island, return `0`.
**Example Input:** `grid = [[0,1,1,0,1],[1,1,0,0,0],[0,0,0,1,1]]` -> **Output:** `4`

```java
import java.util.*;

public class MaxAreaOfIsland {
    public static int maxAreaOfIsland(int[][] grid) {
        int max = 0;
        for(int i=0; i<grid.length; i++) {
            for(int j=0; j<grid[0].length; j++) {
                if(grid[i][j] == 1) {
                    max = Math.max(max, dfs(grid, i, j));
                }
            }
        }
        return max;
    }

    private static int dfs(int[][] grid, int i, int j) {
        if(i<0 || i>=grid.length || j<0 || j>=grid[0].length || grid[i][j] == 0) return 0;
        grid[i][j] = 0;
        return 1 + dfs(grid, i+1, j) + dfs(grid, i-1, j) + dfs(grid, i, j+1) + dfs(grid, i, j-1);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter rows and cols:");
        int m = sc.nextInt();
        int n = sc.nextInt();
        int[][] grid = new int[m][n];
        System.out.println("Enter grid (0s and 1s):");
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                grid[i][j] = sc.nextInt();
            }
        }
        System.out.println("Max area: " + maxAreaOfIsland(grid));
    }
}
```

## 18. Flood Fill (LeetCode 733)
**Problem Statement:** Given an image represented by an `m x n` integer grid `image` where `image[i][j]` represents the pixel value of the image. You are also given three integers `sr`, `sc`, and `color`. You should perform a flood fill on the image starting from the pixel `image[sr][sc]`. Return the modified image.
**Example Input:** `image = [[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, color = 2` -> **Output:** `[[2,2,2],[2,2,0],[2,0,1]]`

```java
import java.util.*;

public class FloodFill {
    public static int[][] floodFill(int[][] image, int sr, int sc, int color) {
        if(image[sr][sc] != color) dfs(image, sr, sc, image[sr][sc], color);
        return image;
    }

    private static void dfs(int[][] image, int r, int c, int oldColor, int newColor) {
        if(r<0 || r>=image.length || c<0 || c>=image[0].length || image[r][c] != oldColor) return;
        image[r][c] = newColor;
        dfs(image, r+1, c, oldColor, newColor);
        dfs(image, r-1, c, oldColor, newColor);
        dfs(image, r, c+1, oldColor, newColor);
        dfs(image, r, c-1, oldColor, newColor);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter rows and cols:");
        int m = sc.nextInt(), n = sc.nextInt();
        int[][] image = new int[m][n];
        System.out.println("Enter image pixels:");
        for(int i=0; i<m; i++)
            for(int j=0; j<n; j++) image[i][j] = sc.nextInt();
        
        System.out.println("Enter sr, sc, new color:");
        int sr = sc.nextInt(), scCol = sc.nextInt(), color = sc.nextInt();

        int[][] res = floodFill(image, sr, scCol, color);
        System.out.println("Resulting Image:");
        for(int[] row : res) System.out.println(Arrays.toString(row));
    }
}
```

## 19. House Robber (LeetCode 198)
**Problem Statement:** You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected. Return the maximum amount of money you can rob tonight without alerting the police.
**Example Input:** `nums = [1,2,3,1]` -> **Output:** `4`

```java
import java.util.*;

public class HouseRobber {
    public static int rob(int[] nums) {
        if(nums.length == 0) return 0;
        int prev1 = 0, prev2 = 0;
        for(int num : nums) {
            int tmp = prev1;
            prev1 = Math.max(prev2 + num, prev1);
            prev2 = tmp;
        }
        return prev1;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter number of houses:");
        int n = sc.nextInt();
        int[] nums = new int[n];
        System.out.println("Enter money in each house:");
        for(int i=0; i<n; i++) nums[i] = sc.nextInt();
        System.out.println("Maximum money robbed: " + rob(nums));
    }
}
```

## 20. Group Anagrams (LeetCode 49)
**Problem Statement:** Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.
**Example Input:** `strs = ["eat","tea","tan","ate","nat","bat"]` -> **Output:** `[["bat"],["nat","tan"],["ate","eat","tea"]]`

```java
import java.util.*;

public class GroupAnagrams {
    public static List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        for(String s : strs) {
            char[] chars = s.toCharArray();
            Arrays.sort(chars);
            String key = new String(chars);
            map.putIfAbsent(key, new ArrayList<>());
            map.get(key).add(s);
        }
        return new ArrayList<>(map.values());
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter number of strings:");
        int n = sc.nextInt();
        String[] strs = new String[n];
        System.out.println("Enter strings:");
        for(int i=0; i<n; i++) strs[i] = sc.next();
        
        List<List<String>> res = groupAnagrams(strs);
        System.out.println("Grouped Anagrams: " + res);
    }
}
```


# Unit 5 - Arrays, Bits & Backtracking

## 21. Majority Element (LeetCode 169)
**Problem Statement:** Given an array `nums` of size `n`, return the majority element. The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.
**Example Input:** `nums = [2,2,1,1,1,2,2]` -> **Output:** `2`

```java
import java.util.*;

public class MajorityElement {
    public static int majorityElement(int[] nums) {
        int count = 0, candidate = 0;
        for (int num : nums) {
            if (count == 0) candidate = num;
            count += (num == candidate) ? 1 : -1;
        }
        return candidate;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter array size:");
        int n = sc.nextInt();
        int[] nums = new int[n];
        System.out.println("Enter elements:");
        for(int i=0; i<n; i++) nums[i] = sc.nextInt();
        System.out.println("Majority Element: " + majorityElement(nums));
    }
}
```

## 22. Reverse Bits (LeetCode 190)
**Problem Statement:** Reverse bits of a given 32 bits unsigned integer.
**Example Input:** `n = 00000010100101000001111010011100` -> **Output:** `964176192 (00111001011110000010100101000000)`

```java
import java.util.Scanner;

public class ReverseBits {
    public static int reverseBits(int n) {
        int res = 0;
        for (int i = 0; i < 32; i++) {
            res <<= 1;
            res |= (n & 1);
            n >>>= 1;
        }
        return res;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter an integer to reverse its bits:");
        int n = sc.nextInt();
        System.out.println("Reversed bits (integer): " + reverseBits(n));
    }
}
```

## 23. Ones and Zeroes (LeetCode 474)
**Problem Statement:** You are given an array of binary strings `strs` and two integers `m` and `n`. Return the size of the largest subset of `strs` such that there are at most `m` 0's and `n` 1's in the subset.
**Example Input:** `strs = ["10","0001","111001","1","0"], m = 5, n = 3` -> **Output:** `4`

```java
import java.util.Scanner;

public class OnesAndZeroes {
    public static int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];
        for (String s : strs) {
            int zeros = 0, ones = 0;
            for (char c : s.toCharArray()) {
                if (c == '0') zeros++;
                else ones++;
            }
            for (int i = m; i >= zeros; i--) {
                for (int j = n; j >= ones; j--) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - zeros][j - ones] + 1);
                }
            }
        }
        return dp[m][n];
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter number of strings:");
        int size = sc.nextInt();
        String[] strs = new String[size];
        System.out.println("Enter strings:");
        for(int i=0; i<size; i++) strs[i] = sc.next();
        System.out.println("Enter m (max zeros) and n (max ones):");
        int m = sc.nextInt();
        int n = sc.nextInt();
        System.out.println("Max subset size: " + findMaxForm(strs, m, n));
    }
}
```

## 24. Subsets (LeetCode 78)
**Problem Statement:** Given an integer array `nums` of unique elements, return all possible subsets (the power set). The solution set must not contain duplicate subsets. Return the solution in any order.
**Example Input:** `nums = [1,2,3]` -> **Output:** `[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]`

```java
import java.util.*;

public class Subsets {
    public static List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(res, new ArrayList<>(), nums, 0);
        return res;
    }

    private static void backtrack(List<List<Integer>> res, List<Integer> curr, int[] nums, int start) {
        res.add(new ArrayList<>(curr));
        for (int i = start; i < nums.length; i++) {
            curr.add(nums[i]);
            backtrack(res, curr, nums, i + 1);
            curr.remove(curr.size() - 1);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter number of elements:");
        int n = sc.nextInt();
        int[] nums = new int[n];
        System.out.println("Enter elements:");
        for(int i=0; i<n; i++) nums[i] = sc.nextInt();
        System.out.println("Subsets: " + subsets(nums));
    }
}
```

## 25. Permutations (LeetCode 46)
**Problem Statement:** Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in any order.
**Example Input:** `nums = [1,2,3]` -> **Output:** `[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`

```java
import java.util.*;

public class Permutations {
    public static List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(res, new ArrayList<>(), nums, new boolean[nums.length]);
        return res;
    }

    private static void backtrack(List<List<Integer>> res, List<Integer> curr, int[] nums, boolean[] used) {
        if (curr.size() == nums.length) {
            res.add(new ArrayList<>(curr));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            used[i] = true;
            curr.add(nums[i]);
            backtrack(res, curr, nums, used);
            curr.remove(curr.size() - 1);
            used[i] = false;
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter number of elements:");
        int n = sc.nextInt();
        int[] nums = new int[n];
        System.out.println("Enter elements:");
        for(int i=0; i<n; i++) nums[i] = sc.nextInt();
        System.out.println("Permutations: " + permute(nums));
    }
}
```

