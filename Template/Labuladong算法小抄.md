# Labuladong算法小抄

## 1. 树

二叉树生成器：https://binary-tree-visualizer.vercel.app/

LeetCode 中输入 root = [5, 4, 6, null, null, 3, 7]，做题时，传入的参数为第一个 5，后面节点间关系，系统已自动生成

| 层数 | 节点个数    |
| ---- | ----------- |
| 1    | 1           |
| 2    | 2           |
| 4    | 4           |
| ……   | ……          |
| n    | 2 ^ (n - 1) |

![image-20221101172126024](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/Tree/202211011721112.png)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
```

```java
//	新建二叉树节点
TreeNode node1 = new TreeNode(2);
TreeNode node2 = new TreeNode(3);
TreeNode node3 = new TreeNode(4);
//	修改节点的值
node1.val = 10;
//	连接节点
nodel.left = node2;
node1.right = node3;
```

## 2. 链表

```java
class ListNode{
    int val;
    ListNode next;
    
    public ListNode(int val){
        this.val = val;
        this.next = null;
    }
}
```

```java
//	新建单链表节点
ListNode node1 = new ListNode(1);
ListNode node2 = new ListNode(3);
ListNode node3 = new ListNode(5);

//	修改节点的值
node1.val = 1;
//	连接节点
node2.next = node3;
node1.next = next2;
```

## 3. 内置数据结构

```java
class Solution{
    public int solutionFunc(String str1, String str2){
        
    }
}
```

```python
class Solution:
    def solutionFunc(self, str1:str, str2:str) -> int
```



### 3.1 数组

```java
int[] nums = new int[n];

boolean[][] visited = new boolean[m][n];

//	类似 C 语言中的数组 ==> 作为形参时
//	非空检查
if(nums.length == 0){
    return;
}
```

### 3.2 字符串

```java
//	不支持 [] 直接访问其中字符，不能直接修改 ===> 转化成 char[]
String s1 = "hello world";
//	（1）获取字符
char c = s1.charAt(2);
//	（2）转成字符数组
char[] chars = s1.toCharArray();
//	（3）字符数组 ===> String
String s = new String(chars);
//	用 equals进行比较
//	String 可以用 "+" 进行拼接，但是效率不高 ===> StringBuilder ===> append（字符、字符串、数字类型）
//	转回去 sb.toString()
```

### 3.3  动态数组 ArrayList

类似 C++ 的 Vector，ArrayList相当于把 Java 内置的数组类型进行了包装

```java
//	存储 String 类型数据的 动态数组
ArrayList<String> strings = new ArrayList<>();

//	存储 int 类型数据的 动态数组
ArrayList<Integer> nums = new ArrayList<>();
```

```java
//	常用方法 [E：代表元素类型]
boolean isEmpty();
int size();
E get(int index);
boolean add(E e);
```

### 3.4  双向链表 LinkedList

ArrayList 底层是 数组，而 LinkedList 底层：双链表

```java
LinkedList<String> strings = new LinkedList<>();
LinkedList<Integer> nums = new LinkedList<>();
```

```java
//	常用方法
boolean isEmpty();
int size();
boolean contains(Object o);     //	时间复杂度：O(N)，必须遍历整个链表才能判断元素是否存在
boolean add(E e);
void addFirst(E e);
E removeFirst();
E removeLast();
    
```

Java Collection 接口里的 add() ==> boolean

作为一个接口来说，任务很大

1. List：元素可以重复，永远返回 true
2. Set：不可重复，对于已经存在的元素，返回 false
3. 不允许插入 null 元素，也会返回 false (NullPointerException

### 3.5  哈希表 HashMap

```java
HashMap<Integer, String> map = new HashMap<>();

HashMap<String, int[]> map = new HashMap<>();
```

```java
//	常用方法 
//	K: 键的类型，V：值的类型
boolean containKey(Object key);
V get(Object key);	//	根据 key, 拿到 value
V put(K key, V value);	//	放入新的 k - v （key 存在，则更新对应的 value）
V remove(K key)
V getOrDefault(Object key, V defaultValue)	//	如果 key 存在，就取出对应的value，否则返回 defaultValue

//	获取哈希表中的 所有 key
Set<K> keySet()
    
//	如果 key 不存在，将 k - v 存入
//	key 存在，什么都不做 （不更新 key 对应的 value）
V putIfAbsent(K key, V value)
```

### 3.6  哈希集合 HashSet

```java
Set<String> set = new HashSet<>();
```

```java
//	常用方法
boolean add(E e)	//	不存在，才添加
boolean contains(Object o)
boolean remove(Object o)
```

### 3.7  队列 Queue（接口 Interface）

```java
Queue<String> q = new LinkedList<>()
```

```java
//	常用方法
boolean isEmpty()
int size()
E peek()
E poll()	//	出队，并返回 队头元素
boolean offer(E e)	//	从 队尾 入队
```

### 3.8 堆栈 Stack

```java
Stack<Integer> s = new Stack<>();
```

```java
//	常用方法
boolean isEmpty()
int size()
E push(E item)	//	压栈
E peek()	//	返回栈顶
E pop()		//	弹栈
```

# SYuki















