# 力扣 0421 数组中两个数的最大异或值


[421. 数组中两个数的最大异或值](https://leetcode.cn/problems/maximum-xor-of-two-numbers-in-an-array/)

<!--more-->

```java
class Solution {
    public int findMaximumXOR(int[] nums) {
        int ans = 0;
        Trie trie = new Trie();
        for (int x : nums) {
            trie.add(x);
            ans = Math.max(ans, x ^ trie.find(x));
        }
        return ans;
    }
}

class Trie {
    static class Node {
        Node left, right;
    }

    private Node root = new Node();

    public void add(int x) {
        Node p = root;
        for (int i = 31; i >= 0; i--) {
            boolean left = (x & (1 << i)) > 0;
            if (left) {
                if (p.left == null) p.left = new Node();
                p = p.left;
            } else {
                if (p.right == null) p.right = new Node();
                p = p.right;
            }
        }
    }

    public int find(int x) {
        Node p = root;
        int ans = 0;
        for (int i = 31; i >= 0; i--) {
            boolean left = (x & (1 << i)) > 0;
            if (left && p.right != null || !left && p.left == null) {
                p = p.right;
                ans <<= 1;
            } else {
                p = p.left;
                ans = (ans << 1) | 1;
            }
        }
        return ans;
    }
}
```

