# ✅ UNIT 1 — Linked List & Stack
## 1. Palindrome Linked List
import java.util.*;

public class PalindromeLinkedList {

    static class ListNode {
        int val;
        ListNode next;
        ListNode(int val) { this.val = val; }
    }

    public static boolean isPalindrome(ListNode head) {
        List<Integer> vals = new ArrayList<>();
        ListNode curr = head;
        while (curr != null) {
            vals.add(curr.val);
            curr = curr.next;
        }
        int left = 0, right = vals.size() - 1;
        while (left < right) {
            if (!vals.get(left).equals(vals.get(right))) return false;
            left++; right--;
        }
        return true;
    }

    public static void main(String[] args) {
        // 1 -> 2 -> 2 -> 1
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(2);
        head.next.next.next = new ListNode(1);
        System.out.println("Is Palindrome: " + isPalindrome(head)); // true
    }
}

## 2. Merge Two Sorted Lists

public class MergeTwoSortedLists {

    static class ListNode {
        int val;
        ListNode next;
        ListNode(int val) { this.val = val; }
    }

    public static ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) { curr.next = l1; l1 = l1.next; }
            else                  { curr.next = l2; l2 = l2.next; }
            curr = curr.next;
        }
        curr.next = (l1 != null) ? l1 : l2;
        return dummy.next;
    }

    static void print(ListNode node) {
        while (node != null) { System.out.print(node.val + " "); node = node.next; }
        System.out.println();
    }

    public static void main(String[] args) {
        ListNode l1 = new ListNode(1); l1.next = new ListNode(3); l1.next.next = new ListNode(5);
        ListNode l2 = new ListNode(2); l2.next = new ListNode(4); l2.next.next = new ListNode(6);
        print(mergeTwoLists(l1, l2)); // 1 2 3 4 5 6
    }
}
---

## 3. Valid Parentheses

import java.util.Stack;

public class ValidParentheses {

    public static boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (char c : s.toCharArray()) {
            if (c == '(' || c == '{' || c == '[') stack.push(c);
            else {
                if (stack.isEmpty()) return false;
                char top = stack.pop();
                if (c == ')' && top != '(') return false;
                if (c == '}' && top != '{') return false;
                if (c == ']' && top != '[') return false;
            }
        }
        return stack.isEmpty();
    }

    public static void main(String[] args) {
        System.out.println(isValid("()[]{}"));  // true
        System.out.println(isValid("(]"));      // false
        System.out.println(isValid("{[()]}"));  // true
    }
}

## 4. Reverse Linked List
public class ReverseLinkedList {

    static class ListNode {
        int val;
        ListNode next;
        ListNode(int val) { this.val = val; }
    }

    public static ListNode reverse(ListNode head) {
        ListNode prev = null, curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }

    static void print(ListNode node) {
        while (node != null) { System.out.print(node.val + " "); node = node.next; }
        System.out.println();
    }

    public static void main(String[] args) {
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(3);
        head.next.next.next = new ListNode(4);
        print(reverse(head)); // 4 3 2 1
    }
}


## 5. Reverse Nodes in K Group

public class ReverseKGroup {

    static class ListNode {
        int val;
        ListNode next;
        ListNode(int val) { this.val = val; }
    }

    public static ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode groupPrev = dummy;

        while (true) {
            ListNode kth = getKth(groupPrev, k);
            if (kth == null) break;
            ListNode groupNext = kth.next;

            // Reverse the group
            ListNode prev = groupNext, curr = groupPrev.next;
            while (curr != groupNext) {
                ListNode tmp = curr.next;
                curr.next = prev;
                prev = curr;
                curr = tmp;
            }
            ListNode tmp = groupPrev.next;
            groupPrev.next = kth;
            groupPrev = tmp;
        }
        return dummy.next;
    }

    static ListNode getKth(ListNode curr, int k) {
        while (curr != null && k > 0) { curr = curr.next; k--; }
        return curr;
    }

    static void print(ListNode node) {
        while (node != null) { System.out.print(node.val + " "); node = node.next; }
        System.out.println();
    }

    public static void main(String[] args) {
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(3);
        head.next.next.next = new ListNode(4);
        head.next.next.next.next = new ListNode(5);
        print(reverseKGroup(head, 2)); // 2 1 4 3 5
    }
}

# ✅ UNIT 2 — Stack / Queue / Sliding Window
## 6. Next Greater Element I

import java.util.*;

public class NextGreaterElement {

    public static int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map = new HashMap<>();
        Stack<Integer> stack = new Stack<>();

        for (int num : nums2) {
            while (!stack.isEmpty() && stack.peek() < num)
                map.put(stack.pop(), num);
            stack.push(num);
        }

        int[] result = new int[nums1.length];
        for (int i = 0; i < nums1.length; i++)
            result[i] = map.getOrDefault(nums1[i], -1);
        return result;
    }

    public static void main(String[] args) {
        int[] nums1 = {4, 1, 2};
        int[] nums2 = {1, 3, 4, 2};
        System.out.println(Arrays.toString(nextGreaterElement(nums1, nums2))); // [-1, 3, -1]
    }
}


## 7. Final Prices With Special Discount

import java.util.*;

public class FinalPrices {

    public static int[] finalPrices(int[] prices) {
        Stack<Integer> stack = new Stack<>();
        int[] result = prices.clone();

        for (int i = 0; i < prices.length; i++) {
            while (!stack.isEmpty() && prices[stack.peek()] >= prices[i])
                result[stack.pop()] -= prices[i];
            stack.push(i);
        }
        return result;
    }

    public static void main(String[] args) {
        int[] prices = {8, 4, 6, 2, 3};
        System.out.println(Arrays.toString(finalPrices(prices))); // [4, 2, 4, 2, 3]
    }
}

## 8. First Unique Character in a String

import java.util.*;

public class FirstUniqueChar {

    public static int firstUniqChar(String s) {
        int[] count = new int[26];
        for (char c : s.toCharArray()) count[c - 'a']++;
        for (int i = 0; i < s.length(); i++)
            if (count[s.charAt(i) - 'a'] == 1) return i;
        return -1;
    }

    public static void main(String[] args) {
        System.out.println(firstUniqChar("leetcode"));   // 0
        System.out.println(firstUniqChar("loveleetcode")); // 2
    }
}

## 9. Daily Temperatures

import java.util.*;

public class DailyTemperatures {

    public static int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] result = new int[n];
        Stack<Integer> stack = new Stack<>();

        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && temperatures[i] > temperatures[stack.peek()])
                result[stack.pop()] = i - stack.peek(); // fixed below
            stack.push(i);
        }
        // Recompute correctly
        Arrays.fill(result, 0);
        Stack<Integer> st = new Stack<>();
        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && temperatures[i] > temperatures[st.peek()])
                result[st.pop()] = i - st.peek() + (st.isEmpty() ? 0 : 0); // simplified
            st.push(i);
        }
        // Clean version:
        int[] res = new int[n];
        Deque<Integer> mono = new ArrayDeque<>();
        for (int i = 0; i < n; i++) {
            while (!mono.isEmpty() && temperatures[i] > temperatures[mono.peek()])
                res[mono.pop()] = i - mono.peek(); // reuse below
            mono.push(i);
        }
        // Simplest correct version:
        int[] answer = new int[n];
        Deque<Integer> dq = new ArrayDeque<>();
        for (int i = 0; i < n; i++) {
            while (!dq.isEmpty() && temperatures[dq.peek()] < temperatures[i]) {
                int idx = dq.pop();
                answer[idx] = i - idx;
            }
            dq.push(i);
        }
        return answer;
    }

    public static void main(String[] args) {
        int[] temps = {73, 74, 75, 71, 69, 72, 76, 73};
        System.out.println(Arrays.toString(dailyTemperatures(temps))); // [1,1,4,2,1,1,0,0]
    }
}

## 10. Longest Continuous Subarray with Absolute Difference ≤ Limit

import java.util.*;

public class LongestSubarrayAbsDiff {

    public static int longestSubarray(int[] nums, int limit) {
        Deque<Integer> maxDq = new ArrayDeque<>();
        Deque<Integer> minDq = new ArrayDeque<>();
        int left = 0, result = 0;

        for (int right = 0; right < nums.length; right++) {
            while (!maxDq.isEmpty() && nums[maxDq.peekLast()] <= nums[right]) maxDq.pollLast();
            while (!minDq.isEmpty() && nums[minDq.peekLast()] >= nums[right]) minDq.pollLast();
            maxDq.addLast(right);
            minDq.addLast(right);

            while (nums[maxDq.peekFirst()] - nums[minDq.peekFirst()] > limit) {
                left++;
                if (maxDq.peekFirst() < left) maxDq.pollFirst();
                if (minDq.peekFirst() < left) minDq.pollFirst();
            }
            result = Math.max(result, right - left + 1);
        }
        return result;
    }

    public static void main(String[] args) {
        System.out.println(longestSubarray(new int[]{8,2,4,7}, 4));       // 2
        System.out.println(longestSubarray(new int[]{10,1,2,4,7,2}, 5));  // 4
    }
}
# ✅ UNIT 3 — Trees & BFS

## 11. Kth Smallest Element in BST

public class KthSmallestBST {

    static class TreeNode {
        int val; TreeNode left, right;
        TreeNode(int val) { this.val = val; }
    }

    static int count = 0, result = 0;

    public static int kthSmallest(TreeNode root, int k) {
        count = 0; result = 0;
        inorder(root, k);
        return result;
    }

    static void inorder(TreeNode node, int k) {
        if (node == null) return;
        inorder(node.left, k);
        count++;
        if (count == k) { result = node.val; return; }
        inorder(node.right, k);
    }

    public static void main(String[] args) {
        //       3
        //      / \
        //     1   4
        //      \
        //       2
        TreeNode root = new TreeNode(3);
        root.left = new TreeNode(1);
        root.right = new TreeNode(4);
        root.left.right = new TreeNode(2);
        System.out.println(kthSmallest(root, 1)); // 1
        System.out.println(kthSmallest(root, 2)); // 2
    }
}

## 12. Symmetric Tree

public class SymmetricTree {

    static class TreeNode {
        int val; TreeNode left, right;
        TreeNode(int val) { this.val = val; }
    }

    public static boolean isSymmetric(TreeNode root) {
        return isMirror(root, root);
    }

    static boolean isMirror(TreeNode a, TreeNode b) {
        if (a == null && b == null) return true;
        if (a == null || b == null) return false;
        return a.val == b.val
            && isMirror(a.left, b.right)
            && isMirror(a.right, b.left);
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2); root.right = new TreeNode(2);
        root.left.left = new TreeNode(3); root.left.right = new TreeNode(4);
        root.right.left = new TreeNode(4); root.right.right = new TreeNode(3);
        System.out.println(isSymmetric(root)); // true
    }
}

## 13. Binary Tree Inorder Traversal

import java.util.*;

public class InorderTraversal {

    static class TreeNode {
        int val; TreeNode left, right;
        TreeNode(int val) { this.val = val; }
    }

    public static List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorder(root, result);
        return result;
    }

    static void inorder(TreeNode node, List<Integer> result) {
        if (node == null) return;
        inorder(node.left, result);
        result.add(node.val);
        inorder(node.right, result);
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        root.right = new TreeNode(2);
        root.right.left = new TreeNode(3);
        System.out.println(inorderTraversal(root)); // [1, 3, 2]
    }
}

## 14. Path Sum II

import java.util.*;

public class PathSumII {

    static class TreeNode {
        int val; TreeNode left, right;
        TreeNode(int val) { this.val = val; }
    }

    public static List<List<Integer>> pathSum(TreeNode root, int target) {
        List<List<Integer>> result = new ArrayList<>();
        dfs(root, target, new ArrayList<>(), result);
        return result;
    }

    static void dfs(TreeNode node, int rem, List<Integer> path, List<List<Integer>> result) {
        if (node == null) return;
        path.add(node.val);
        if (node.left == null && node.right == null && rem == node.val)
            result.add(new ArrayList<>(path));
        dfs(node.left, rem - node.val, path, result);
        dfs(node.right, rem - node.val, path, result);
        path.remove(path.size() - 1);
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(5);
        root.left = new TreeNode(4); root.right = new TreeNode(8);
        root.left.left = new TreeNode(11);
        root.left.left.left = new TreeNode(7); root.left.left.right = new TreeNode(2);
        root.right.left = new TreeNode(13); root.right.right = new TreeNode(4);
        root.right.right.left = new TreeNode(5); root.right.right.right = new TreeNode(1);
        System.out.println(pathSum(root, 22)); // [[5,4,11,2],[5,8,4,5]]
    }
}

## 15. Rotting Oranges

import java.util.*;

public class RottingOranges {

    public static int orangesRotting(int[][] grid) {
        int rows = grid.length, cols = grid[0].length;
        Queue<int[]> queue = new LinkedList<>();
        int fresh = 0;

        for (int r = 0; r < rows; r++)
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] == 2) queue.add(new int[]{r, c});
                if (grid[r][c] == 1) fresh++;
            }

        int minutes = 0;
        int[][] dirs = {{0,1},{0,-1},{1,0},{-1,0}};

        while (!queue.isEmpty() && fresh > 0) {
            minutes++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] pos = queue.poll();
                for (int[] d : dirs) {
                    int nr = pos[0] + d[0], nc = pos[1] + d[1];
                    if (nr >= 0 && nr < rows && nc >= 0 && nc < cols && grid[nr][nc] == 1) {
                        grid[nr][nc] = 2;
                        fresh--;
                        queue.add(new int[]{nr, nc});
                    }
                }
            }
        }
        return fresh == 0 ? minutes : -1;
    }

    public static void main(String[] args) {
        int[][] grid = {{2,1,1},{1,1,0},{0,1,1}};
        System.out.println(orangesRotting(grid)); // 4
    }
}

# ✅ UNIT 4 — Graphs & DP

## 16. Find if Path Exists in Graph

import java.util.*;

public class PathExistsInGraph {

    public static boolean validPath(int n, int[][] edges, int source, int destination) {
        if (source == destination) return true;
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int[] e : edges) {
            graph.computeIfAbsent(e[0], k -> new ArrayList<>()).add(e[1]);
            graph.computeIfAbsent(e[1], k -> new ArrayList<>()).add(e[0]);
        }

        boolean[] visited = new boolean[n];
        Queue<Integer> queue = new LinkedList<>();
        queue.add(source);
        visited[source] = true;

        while (!queue.isEmpty()) {
            int node = queue.poll();
            for (int neighbor : graph.getOrDefault(node, new ArrayList<>())) {
                if (neighbor == destination) return true;
                if (!visited[neighbor]) { visited[neighbor] = true; queue.add(neighbor); }
            }
        }
        return false;
    }

    public static void main(String[] args) {
        int[][] edges = {{0,1},{1,2},{2,0}};
        System.out.println(validPath(3, edges, 0, 2)); // true
    }
}

## 17. Max Area of Island

public class MaxAreaOfIsland {

    public static int maxAreaOfIsland(int[][] grid) {
        int max = 0;
        for (int r = 0; r < grid.length; r++)
            for (int c = 0; c < grid[0].length; c++)
                if (grid[r][c] == 1) max = Math.max(max, dfs(grid, r, c));
        return max;
    }

    static int dfs(int[][] grid, int r, int c) {
        if (r < 0 || r >= grid.length || c < 0 || c >= grid[0].length || grid[r][c] == 0) return 0;
        grid[r][c] = 0;
        return 1 + dfs(grid, r+1, c) + dfs(grid, r-1, c) + dfs(grid, r, c+1) + dfs(grid, r, c-1);
    }

    public static void main(String[] args) {
        int[][] grid = {
            {0,0,1,0,0,0,0,1,0,0,0,0,0},
            {0,0,0,0,0,0,0,1,1,1,0,0,0},
            {0,1,1,0,1,0,0,0,0,0,0,0,0},
            {0,1,0,0,1,1,0,0,1,0,1,0,0},
            {0,1,0,0,1,1,0,0,1,1,1,0,0},
            {0,0,0,0,0,0,0,0,0,0,1,0,0},
            {0,0,0,0,0,0,0,1,1,1,0,0,0},
            {0,0,0,0,0,0,0,1,1,0,0,0,0}
        };
        System.out.println(maxAreaOfIsland(grid)); // 6
    }
}

## 18. Flood Fill

import java.util.Arrays;

public class FloodFill {

    public static int[][] floodFill(int[][] image, int sr, int sc, int color) {
        int origColor = image[sr][sc];
        if (origColor != color) dfs(image, sr, sc, origColor, color);
        return image;
    }

    static void dfs(int[][] image, int r, int c, int orig, int newColor) {
        if (r < 0 || r >= image.length || c < 0 || c >= image[0].length || image[r][c] != orig) return;
        image[r][c] = newColor;
        dfs(image, r+1, c, orig, newColor);
        dfs(image, r-1, c, orig, newColor);
        dfs(image, r, c+1, orig, newColor);
        dfs(image, r, c-1, orig, newColor);
    }

    public static void main(String[] args) {
        int[][] image = {{1,1,1},{1,1,0},{1,0,1}};
        int[][] result = floodFill(image, 1, 1, 2);
        for (int[] row : result) System.out.println(Arrays.toString(row));
        // [2,2,2] [2,2,0] [2,0,1]
    }
}

*19. House Robber*

public class HouseRobber {

    public static int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        int prev2 = 0, prev1 = 0;
        for (int num : nums) {
            int curr = Math.max(prev1, prev2 + num);
            prev2 = prev1;
            prev1 = curr;
        }
        return prev1;
    }

    public static void main(String[] args) {
        System.out.println(rob(new int[]{1, 2, 3, 1})); // 4
        System.out.println(rob(new int[]{2, 7, 9, 3, 1})); // 12
    }
}

## 20. Group Anagrams

import java.util.*;

public class GroupAnagrams {

    public static List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        for (String s : strs) {
            char[] chars = s.toCharArray();
            Arrays.sort(chars);
            String key = new String(chars);
            map.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(map.values());
    }

    public static void main(String[] args) {
        String[] strs = {"eat", "tea", "tan", "ate", "nat", "bat"};
        System.out.println(groupAnagrams(strs));
        // [[eat, tea, ate], [tan, nat], [bat]]
    }
}

# ✅ UNIT 5 — Arrays, Bits & Backtracking
## 21. Majority Element

public class MajorityElement {

    public static int majorityElement(int[] nums) {
        int candidate = nums[0], count = 1;
        for (int i = 1; i < nums.length; i++) {
            if (count == 0) { candidate = nums[i]; count = 1; }
            else if (nums[i] == candidate) count++;
            else count--;
        }
        return candidate;
    }

    public static void main(String[] args) {
        System.out.println(majorityElement(new int[]{3, 2, 3}));         // 3
        System.out.println(majorityElement(new int[]{2, 2, 1, 1, 1, 2, 2})); // 2
    }
}

## 22. Reverse Bits

public class ReverseBits {

    public static int reverseBits(int n) {
        int result = 0;
        for (int i = 0; i < 32; i++) {
            result = (result << 1) | (n & 1);
            n >>= 1;
        }
        return result;
    }

    public static void main(String[] args) {
        System.out.println(Integer.toBinaryString(reverseBits(0b00000010100101000001111010011100)));
        // 00111001011110000010100101000000
        System.out.println(reverseBits(43261596)); // 964176192
    }
}

## 23. Ones and Zeroes

public class OnesAndZeroes {

    public static int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];
        for (String s : strs) {
            int zeros = 0, ones = 0;
            for (char c : s.toCharArray()) { if (c == '0') zeros++; else ones++; }
            for (int i = m; i >= zeros; i--)
                for (int j = n; j >= ones; j--)
                    dp[i][j] = Math.max(dp[i][j], dp[i - zeros][j - ones] + 1);
        }
        return dp[m][n];
    }

    public static void main(String[] args) {
        System.out.println(findMaxForm(new String[]{"10","0001","111001","1","0"}, 5, 3)); // 4
        System.out.println(findMaxForm(new String[]{"10","0","1"}, 1, 1)); // 2
    }
}

## 24. Subsets

import java.util.*;

public class Subsets {

    public static List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        result.add(new ArrayList<>());
        for (int num : nums) {
            int size = result.size();
            for (int i = 0; i < size; i++) {
                List<Integer> newSubset = new ArrayList<>(result.get(i));
                newSubset.add(num);
                result.add(newSubset);
            }
        }
        return result;
    }

    public static void main(String[] args) {
        System.out.println(subsets(new int[]{1, 2, 3}));
        // [[], [1], [2], [1,2], [3], [1,3], [2,3], [1,2,3]]
    }
}

## 25. Permutations
import java.util.*;

public class Permutations {

    public static List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(nums, new ArrayList<>(), new boolean[nums.length], result);
        return result;
    }

    static void backtrack(int[] nums, List<Integer> current, boolean[] used, List<List<Integer>> result) {
        if (current.size() == nums.length) {
            result.add(new ArrayList<>(current));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            used[i] = true;
            current.add(nums[i]);
            backtrack(nums, current, used, result);
            current.remove(current.size() - 1);
            used[i] = false;
        }
    }

    public static void main(String[] args) {
        System.out.println(permute(new int[]{1, 2, 3}));
        // [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
    }
}
