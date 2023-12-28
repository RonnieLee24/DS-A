# LeetCode 刷题

## 2248. 多个数组求交集

```java
class Solution {
    public List<Integer> intersection(int[][] nums) {
        int size = nums.length;	//	整体长度
        ArrayList<Integer> list = new ArrayList<>();
        //	利用 hashMap 求出每个数字出现的次数
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < nums[i].length; j++) {
                if (map.containsKey(nums[i][j])) {	//	计算次数
                    map.put(nums[i][j], map.get(nums[i][j]) + 1);
                }else {
                    map.put(nums[i][j], 1);
                }
            }
        }
        for (Integer integer : map.keySet()) {	//	遍历 key
            if (map.get(integer) == size){
                list.add(integer);
            }
        }
        Collections.sort(list);
        return list;
    }
}
```

## 1325. 删除给定值的叶子节点

- 由于我们需要删除所有值为 target 的叶子节点，那么我们的操作顺序应当从二叉树的叶子节点开始，逐步向上直到二叉树的根为止
- 因此我们可以使用递归的方法遍历整颗二叉树，并 <font color="yellow">**在回溯时**</font> 进行删除操作。这样对于二叉树中的每个节点，它的子节点一定先于它被操作。这其实也就是二叉树的 <font color="red">**后序遍历**</font>。

```java
class Solution {
    public TreeNode removeLeafNodes(TreeNode root, int target) {
        if (root == null) return null;

        root.left = removeLeafNodes(root.left, target);	//	一路向左
        root.right = removeLeafNodes(root.right, target);	//	一路向右

        if (root.val == target && root.left == null && root.right == null) return null;	//	删除条件！！！
        return root;
    }
}
```



