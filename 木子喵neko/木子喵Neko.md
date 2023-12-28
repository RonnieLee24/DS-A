# 木子喵Neko

## 1. 递归与递推

### 递归【未知 ---> 已知】

- 将未知的问题缩小
- 直到已知的问题
- 便开始返回值
- 最后解决我们的问题

### 递推【已知 ---> 未知】

- 由已知的问题
- 向前推位置
- 一步步随算到我们最后要解决的问题上



兔子问题：
假定一对大兔子每月能生对小兔子，每对新生的小兔子

- 经过一个月可以长成一对大兔子具备繁殖能力
- 如果不发生死亡，且每次均生下一雌一雄
  问:我们最初有一对小兔子，一年后共有多少 <font color="yellow"> 对 </font>兔子?

| 月份 | 0（初始态） | 1    | 2    | 3    | 4    | 5    | 6    | 7    | ……   |
| ---- | ----------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 小   | 1           | 0    | 1    | 1    | 2    | 3    | 5    | 8    | ……   |
| 大   | 0           | 1    | 1    | 2    | 3    | 5    | 8    | 13   | ……   |
| 总   | 1           | 1    | 2    | 3    | 5    | 8    | 13   | 21   | ……   |

2个要点：		

1. 经过一个月，上个月的小兔子都成长为大兔子了，所以：当前月的大兔子 = 上个月兔子总和

   ![image-20221211113829102](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212111138224.png)

2. 大兔子经过一个月可以生出一对小兔子，所以：当前月的小兔子 = 上个月的大兔子

   ![image-20221211114155847](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212111141881.png)

### 斐波那契数列（Fibonacci）

$$
f(n)=\left\{\begin{matrix}
1 & n = 0 & \\ 
1 & n = 1 & \\
f(n-1)+f(n-2) & n >1
\end{matrix}\right.
$$

递归

```java
public void Fibonacci(int n){
    //	base case
    if(n == 0 || n == 1){
        return 1;
    }
    return Fibonacci(n-1) + Fibonacci(n-2);
}
```

递推

用已知推未知，所以我们直接定义一个数组，用循环来完成

```java
public class ditui {
    public static void main(String[] args) {
        int[] arr = new int[100];
        arr[0] = 1;
        arr[1] = 1;
        for (int i = 2; i < arr.length; i++) {
            arr[i] = arr[i - 1] + arr[i - 2];
        }
        
        Scanner scanner = new Scanner(System.in);
        System.out.print("输入月份（1-100）：");
        String next = scanner.next();
        int i = Integer.parseInt(next);
        System.out.println("第 " + i + " 月 兔子对数为：" + arr[i]);
    }
}
```

### 三角形最小路径 [120]

1. 顶部到底边
2. 路径上数字之和最大
   - 左下
   - 右下

给定一个三角形 `triangle` ，找出自顶向下的最小路径和。

每一步只能移动到下一行中相邻的结点上。<font color="yellow">**相邻的结点**</font> 在这里指的是 **下标** 与 **上一层结点下标** 相同或者等于 **上一层结点下标 + 1** 的两个结点。也就是说，如果正位于当前行的下标 `i` ，那么下一步可以移动到下一行的下标 <font color="yellow">`i`</font> 或 <font color="yellow">`i + 1`</font> 。

![image-20221211132231774](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212111322981.png)

![image-20221211145603819](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212111456921.png)

![image-20221211150027244](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212111500348.png)

```java
//	思路
//	a(i, j) 第 i 行，第 j 列数
//	s(i, j) 从 a(i, j )到底边的最大路径值
```

$$
S(i,j)=\left\{\begin{matrix}
a(i, j) & i=n & \\ 
a(i,j) + Math.max(S(i+1,j)), S(i+1,j+1)) & i< n  & 
\end{matrix}\right.
$$

但是这种算法：每个节点会大量地重复访问，时间复杂度为 $O(2^{n})$

![23](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212111951860.png)

算法优化：

### 记忆数组

时间复杂度： $O(2^{n})$ --->  $O(n^{2})$【最后复杂度就是遍历 2维数组了】

- 记忆化递归 
  - 每访一个节点，就把它记录下来
  - 新开一个数组 record(i, j) 
    - 如果没有记录，就记录
    - 否则，直接取出

![image-20221211190836037](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212111908125.png)

```java
//leetcode submit region begin(Prohibit modification and deletion)
class Solution {
    //	定义一个 记忆数组 memory【全局变量】
    //	记录 第 i 行 第 j 个节点，到底边的最短距离【避免重复计算】
    Integer[][] memory;
    public int minimumTotal(List<List<Integer>> triangle) {
        memory = new Integer[triangle.size()][triangle.size()];
        //	开始递归（将大三角划分为：顶点 + 2个子三角）
        return dfs(triangle,1, 1);	//	从第一个数，开始迭代 【第一行第一个数】
    }
    
    public int dfs(List<List<Integer>> triangle, int i, int j){
        //	base case
        if (i == triangle.size()){	//	i 从 1开始到达底边的时候
            return triangle.get(i - 1).get(j - 1);
        }
        if (memory[i][j] != null){
            return memory[i][j];
        }
        // 开始递归（将大三角划分为：顶点 + 2个子三角）
        return memory[i][j] = triangle.get(i - 1).get(j - 1) + Math.min(dfs(triangle, i + 1, j), dfs(triangle,i + 1, j + 1));
    }
}
//leetcode submit region end(Prohibit modification and deletion)

}
```



## 2. 动态规划【min / max ：很可能是 dp】

- 每个大问题的子问题都是最优的，所以才可以直接记录下来
- 再下次寻找子问题的最优解时，直接使用

### 2.1 最优子结构

原问题的最优解包含子问题的最优解

### 2.2 状态转移方程

- 写动态规划的程序，一般以递推的方式实现
- 虽然时间复杂度和记忆化搜索一样
- 而且在循环中会计算一些无意义的节点
- 但是递推免去了递归调用时进栈出栈操作，而且避免了在递归层数过多时栈溢出的情况

$$
S(i,j)=\left\{\begin{matrix}
a(i, j) & i=n & \\ 
a(i,j) + Math.max(S(i+1,j)), S(i+1,j+1)) & i< n  & 
\end{matrix}\right.
$$

### 2.3 递归以递推方式实现

三角形第 n 行有 n 个元素

| 实现方式 | 说明                                         |
| -------- | -------------------------------------------- |
| 递归     | 自顶向下（抽象问题到边界）【未知 ---> 已知】 |
| 递推     | 自底向上（边界到抽象）【已知 ---> 未知】     |

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int[][] dp;
        int n = triangle.size();
        dp = new int[n + 1][n + 1];    //	多1个空间是为了让初始值满足状态转移方程（数组默认值为：0）
        //	开始递推（自底向上，由已知推算未知）
        for (int i = n - 1; i >= 0; i--) {    // i：行数 (下标从 0 开始)
            for (int j = 0; j <= i; j++) {	//	j: 统计第 i 行的个数 (i + 1) = (n - 1) + 1 = n
                dp[i][j] = triangle.get(i).get(j) + Math.min(dp[i + 1][j], dp[i + 1][j + 1]);
            }
        }
        return dp[0][0];    //	返回第一行第一个
    }
}
```

### 2.4 手办问题

n个手办

- 每个手办价格为 w[i]
- 喜欢程度为 v[i]

你有 T 块钱，问：如何选择，才能使自己的喜欢值最大。

| 手办名     | 价格 | 喜欢程度 |
| ---------- | ---- | -------- |
| 兵长       | 10   | 24       |
| 蕾姆       | 4    | 9        |
| 小埋       | 4    | 9        |
| 和泉纱雾   | 5    | 10       |
| 空条承太郎 | 3    | 2        |

你现在有13元

#### 贪心算法

- 算出每个物品的性价比
- 优先买性价比高的物品

| 手办名     | 价格                           | 喜欢程度 | 性价比 |
| ---------- | ------------------------------ | -------- | ------ |
| 兵长       | <font color="yellow">10</font> | 24       | 2.4    |
| 蕾姆       | 4                              | 9        | 2.25   |
| 小埋       | 4                              | 9        | 2.25   |
| 和泉纱雾   | 5                              | 10       | 2      |
| 空条承太郎 | <font color="yellow">3</font>  | 2        | 0.67   |

最终的喜欢程度为：24 + 2 = 28

#### 0 / 1 背包问题

手办是一个整体

- 你不能只买🐻和大腿，按照比例给钱，所以不能用贪心来解决

##### 普通递归实现

```bash
f(i, j)：当还剩 j 元钱时，前 i 个物品能达到的最大喜欢值
```

$$
f(i,j)=max\left \{ f(i-1,j) ,f(i-1,j-w(i))+v(i)\right \}
$$

```java
public class jianchi {
    static int[]  price = new int[]{10, 4, 4, 5, 3};
    static int[] value = new int[]{24, 9, 9, 10, 2};
    public static void main(String[] args) {
        System.out.println(f(5, 13));
    }

    public static int f(int i, int j){    // 当还剩 j 元钱时，前 i 个物品能达到的最大喜欢值
        //  baseCase
        if (i == 0){    //  当没有物品，价值为 0
            return 0;
        }
        if (j < price[i - 1]){  //  买不起当前物品，就无需比较
            return f(i - 1, j);
        }
        //  不买第 n 个物品
        //  买第 n 个物品，总钱数就会减少，接下来就用 j - price[i - 1]钱考虑剩下的 i - 1个物品喽
        return Math.max(f(i - 1, j), f(i - 1, j - price[i - 1]) + value[i - 1]);
    }
}
```

那么，这个 0/1 背包问题是否满足<font color="yellow">最优子结构</font>呢???

能不能用<font color="yellow"> 记忆化搜索</font> 来优化这一算法???

| 已知条件                               |                |
| -------------------------------------- | -------------- |
| $n$ 个物品                             | $T$ 元钱       |
| $W_{i}$ 价格                           | $V_{i}$ 喜欢值 |
| $X_{i}\epsilon \left \{ 0,1 \right \}$ | 是否买         |

这样问题转化为：
$$
max\sum_{i=1}^{n}V_{i}X_{i}\left ( \sum_{i=1}^{n}W_{i}X_{i}\leqslant T \right )
$$
证明：上述公式是具有<font color="red">最优子结构</font>的。

- 设 $\left ( y_{1},y_{2}……y_{n} \right )$ 是 $max\sum_{i=1}^{n}V_{i}X_{i}$ 最优解
- 那么 $\left ( y_{2},y_{3}……y_{n} \right )$ 是子问题 $max\sum_{i=2}^{n}V_{i}X_{i}\left ( \sum_{i=2}^{n}W_{i}X_{i}\leqslant T- W_{1}X_{1}\right )$
- 使用<font color="yellow"> 反证法</font>
- 假设子问题存在 $\left ( Z_{2},Z_{3}……Z_{n} \right )$ 是最优解，那么 $\left ( y_{2},y_{3}……y_{n} \right )$ 就不是它的最优解
  - 则：$\sum_{i=2}^{n}V_{i}y_{i}\leqslant \sum_{i=2}^{n}V_{i}Z_{i}$
  - 两边同时加上 $V_{1}y_{1}$
  - $\sum_{i=2}^{n}V_{i}y_{i}+V_{1}y_{1}\leqslant \sum_{i=2}^{n}V_{i}Z_{i}+V_{1}y_{1}$ ===> 得出 $\left ( y_{1},y_{2}……y_{n} \right )$ 并不是大问题的最优解，最优解是 $\left ( y_{1},Z_{2}……Z_{n} \right )$
- 就与我们最开始的设定相矛盾了
- 即：证明了这个问题是有最优子结构的

##### 记忆数组

```java
public class jianchi {
    static int[]  price = new int[]{10, 4, 4, 5, 3};
    static int[] value = new int[]{24, 9, 9, 10, 2};
    //  记忆数组 dp，第一个角标表示物品，第二个角标表示我们有多少钱
    //  防止数组越界，在开数据空间时，增加一个空间
    static int[][] dp = new int[6][14];
    public static void main(String[] args) {
        System.out.println(f(5, 13));
    }

    public static int f(int i, int j){    // 当还剩 j 元钱时，前 i 个物品能达到的最大喜欢值
        //  baseCase
        if (i == 0){    //  当没有物品，价值为 0
            return 0;
        }
        if (dp[i][j] != 0){
            return dp[i][j];
        }
        if (j < price[i - 1]){  //  买不起当前物品，就无需比较
            return dp[i][j] = f(i - 1, j);
        }
        //  不买第 n 个物品
        //  买第 n 个物品，总钱数就会减少，接下来就用 j - price[i - 1]钱考虑剩下的 i - 1个物品喽
        return dp[i][j] = Math.max(f(i - 1, j), f(i - 1, j - price[i - 1]) + value[i - 1]);
    }
}
```

记忆化搜索，时间复杂度：$O(n*t)$

- n：物品个数
- t：拥有的钱

##### 动态规划（递推）

//	从已知 ---> 位置

- 即：物品数从1个开始计算
- 同时利用了数组未赋值时，初始值是0

```java
public class jianchi {
    static int[]  price = new int[]{10, 4, 4, 5, 3};
    static int[] value = new int[]{24, 9, 9, 10, 2};
    static int num = price.length;
    //  记忆数组 dp，第一个角标表示物品，第二个角标表示我们有多少钱
    //  防止数组越界，在开数据空间时，增加一个空间
    static int[][] dp = new int[6][14];
    static int money = dp[0].length;
    public static void main(String[] args) {
        System.out.println(f(5, 13));
    }

    public static int f(int i, int j){    // 当还剩 j 元钱时，前 i 个物品能达到的最大喜欢值
        //  递推实现
        //  遍历记忆数组
        for (int k = 1; k <= i ; k++) {    //  物品个数
            for (int l = j; l >= 0 ; l--) {   //  拥有的钱
                if ( l < price[k - 1]){
                    dp[k][l] = dp[k - 1][l];    //  买不起当前物品 【数组，未赋值时，默认值是 0】
                }else {
                    dp[k][l] = Math.max(dp[k - 1][l], dp[k - 1][l - price[k - 1]] + value[k - 1]);
                }
            }
        }
       return dp[i][j];
    }
}
```

状态转移时，当前状态只与上个状态有关（只在 i - 1 或 i + 1 这一行中取需要的值）

- 空间复杂度：$O(n*t)\rightarrow O(t)$ 【二维数组压缩为1维数组】
- 滚动着覆盖记录

## 3. 栈与队列

栈：FILO

队列：FIFO

### Java 栈和队列

![image-20221214182907034](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212141829178.png)

#### 栈【Stack类】继承 Vector

![image-20221214183026679](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212141830763.png)

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

![栈](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212200928319.png)

##### 应用场景：

当我们要处理的数据只涉及在一段插入和删除数据，并且满足 后进先出（LIFO）特性，我们就可以使用栈这个数据结构

###### 1. 浏览器的回退和前进功能

使用2个栈就可以实现

- 后退：左侧栈顶弹出，压入右侧栈顶
- 前进：右侧栈顶弹出，压入左侧栈顶

![栈实现浏览器倒退和前进](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212200932989.png)

###### 2. 检查符号是否成对出现

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串，判断该字符串是否有效。有效字符串需满足：

- 左括号必须用相同类型的右括号闭合
- 左括号必须以正确的顺序闭合

比如 "()"、"()[]{}"、"{[]}" 都是有效字符串，而 "(]" 、"([)]" 则不是。

这个问题实际是 LeetCode 的一道题目，我们可以利用栈 Stack 来解决这个问题

1. 首先我们将括号间的对应规则放在 Map中
2. 创建一个栈，遍历字符串
   - 如果字符是左括号，就直接加入 stack 中
   - 否则将 stack 的栈顶元素与这个括号做比较，如果不相等就直接返回 false
   - 遍历结束，如果 stack 为空，返回 true

```java
public boolean isValid(String s){
    // 括号之间的对应规则
    HashMap<Character, Character> mappings = new HashMap<Character, Character>();
    mappings.put(')', '(');
    mappings.put('}', '{');
    mappings.put(']', '[');
    Stack<Character> stack = new Stack<Character>();
    char[] chars = s.toCharArray();
    for (int i = 0; i < chars.length; i++) {
        if (mappings.containsKey(chars[i])) {
            char topElement = stack.empty() ? '#' : stack.pop();
            if (topElement != mappings.get(chars[i])) {	//	栈顶 ！= 要加入元素的 value ===> 不是成对出现的！！！
                return false;
            }
        } else {	//	左符号直接入栈
            stack.push(chars[i]);
        }
    }
    return stack.isEmpty();
}
```

###### 3. 反转字符串

将字符串中的每个字符先入栈再出栈就可以了

###### 4. 维护函数调用

最后一个被调用的函数必须先完成执行，符合栈的 后进先出（LIFO）特性

##### 《栈的实现》

栈既可以通过数组实现，也可以通过链表来实现。不管基于数组还是链表，入栈、出栈的事件复杂度都为 $O(1)$

```java
public class MyStack {
    private int[] storage;//存放栈中元素的数组
    private int capacity;//栈的容量
    private int count;//栈中元素数量
    private static final int GROW_FACTOR = 2;

    //不带初始容量的构造方法。默认容量为8
    public MyStack() {
        this.capacity = 8;
        this.storage=new int[8];
        this.count = 0;
    }

    //带初始容量的构造方法
    public MyStack(int initialCapacity) {
        if (initialCapacity < 1)
            throw new IllegalArgumentException("Capacity too small.");

        this.capacity = initialCapacity;
        this.storage = new int[initialCapacity];
        this.count = 0;
    }

    //入栈
    public void push(int value) {
        if (count == capacity) {	//	栈中元素 = 栈的容量（满栈扩容）
            ensureCapacity();
        }
        storage[count++] = value;
    }

    //确保容量大小
    private void ensureCapacity() {
        int newCapacity = capacity * GROW_FACTOR;
        storage = Arrays.copyOf(storage, newCapacity);
        capacity = newCapacity;
    }

    //返回栈顶元素并出栈
    private int pop() {
        if (count == 0)
            throw new IllegalArgumentException("Stack is empty.");
        count--;
        return storage[count];
    }

    //返回栈顶元素不出栈
    private int peek() {
        if (count == 0){
            throw new IllegalArgumentException("Stack is empty.");
        }else {
            return storage[count-1];
        }
    }

    //判断栈是否为空
    private boolean isEmpty() {
        return count == 0;
    }

    //返回栈中元素的个数
    private int size() {
        return count;
    }
}
```

验证：

```java
MyStack myStack = new MyStack(3);
myStack.push(1);
myStack.push(2);
myStack.push(3);
myStack.push(4);
myStack.push(5);
myStack.push(6);
myStack.push(7);
myStack.push(8);
System.out.println(myStack.peek());//8
System.out.println(myStack.size());//8
for (int i = 0; i < 8; i++) {
    System.out.println(myStack.pop());
}
System.out.println(myStack.isEmpty());//true
myStack.pop();//报错：java.lang.IllegalArgumentException: Stack is empty.
```

##### 使用双端队列Deque实现栈的功能

```java
{@code Stack} 类表示对象的后进先出 (LIFO) 堆栈。它使用五个操作扩展类 {@code Vector}，允许将向量视为堆栈。提供了通常的{@code push} 和{@code pop} 操作，以及一种{@code peek} 堆栈顶部项目的方法，一种测试堆栈是否为{@code empty} 的方法，以及一种{@code search} 堆栈中的项目并发现它离顶部有多远的方法。 <p> 首次创建堆栈时，它不包含任何项目。 <p> {@link Deque} 接口及其实现提供了一组更完整和一致的 LIFO 堆栈操作，应优先使用此类。例如：<pre> {@code Deque<Integer> stack = new ArrayDeque<Integer>();<pre> @author Jonathan Payne @since 1.0
```

java已经废弃了类**Stack**, 使用Deque定义了**LIFO（后进先出）**的**栈**操作



#### 队列 【LinkedList 实现 Queue接口】

![image-20221214184133768](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212141841848.png)

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

队列是先进先出（FIFO）的线性表，再具体应用中通常用链表或者数组来实现

- 数组实现：顺序队列
- 链表实现：链式队列

队列只允许再后端（rear）进行插入操作，也就是入队（<font color="yellow">en</font> queue），在前端（front）进行删除操作也就是出队（<font color="yellow"> de</font> queue）

```java
假设队列中有n个元素。
访问：O（n）//最坏情况
插入删除：O（1）//后端插入前端删除元素
```

![队列](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212201045047.png)

##### 队列分类

###### 1. 单队列

单队列就是常见的队列，每次添加元素时候，都是添加队尾

- 顺序队列（数组实现）

  - 假溢出：有位置，却不能添加

    - 假设下图是一个顺序队列，我们将前两个元素 1,2 出队，并入队两个元素 7,8。当进行入队、出队操作的时候，front 和 rear 都会持续往后移动，当 rear 移动到最后的时候,我们无法再往队列中添加数据，即使数组中还有空余空间，这种现象就是 **”假溢出“** 。除了假溢出问题之外，如下图所示，当添加元素 8 的时候，rear 指针移动到数组之外（越界）。

    - 为了避免当只有一个元素的时候，队头和队尾重合使处理变得麻烦，所以引入两个指针，front 指针指向对头元素，<font color="yellow">rear</font> 指针指向队列<font color="yellow"> 最后一个元素的下一个位置</font>，这样当 front 等于 rear 时，此队列不是还剩一个元素，而是空队列。——From 《大话数据结构》

      ![顺序队列假溢出](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212201120204.png)

      

- 链式队列（链表实现）

###### 2. 循环队列

```java
public class MyQueue {
    private Object[] queArray;
    //队列总大小
    private int maxSize;
    //前端
    private int front;
    //后端
    private int rear;
    //队列中元素的实际数目
    private int nItems;
     
    public MyQueue(int s){
        maxSize = s;
        queArray = new Object[maxSize];
        front = 0;
        rear = -1;
        nItems = 0;
    }
     
    //队列中新增数据
    public void insert(int value){
        if(isFull()){
            System.out.println("队列已满！！！");
        }else{
            //如果队列尾部指向顶了，那么循环回来，执行队列的第一个元素
            if(rear == maxSize -1){
                rear = -1;
            }
            //队尾指针加1，然后在队尾指针处插入新的数据
            queArray[++rear] = value;
            nItems++;
        }
    }
     
    //移除数据
    public Object remove(){
        Object removeValue = null ;
        if(!isEmpty()){
            removeValue = queArray[front];
            queArray[front] = null;
            front++;
            if(front == maxSize){
                front = 0;
            }
            nItems--;
            return removeValue;
        }
        return removeValue;
    }
     
    //查看对头数据
    public Object peekFront(){
        return queArray[front];
    }
     
     
    //判断队列是否满了
    public boolean isFull(){
        return (nItems == maxSize);
    }
     
    //判断队列是否为空
    public boolean isEmpty(){
        return (nItems ==0);
    }
     
    //返回队列的大小
    public int getSize(){
        return nItems;
    }
}
```

循环队列可以解决顺序队列的假溢出和越界问题。解决办法就是：从头开始，这样也就会形成头尾相接的循环，这也就是循环队列名字的由来。

还是用上面的图，我们将 rear 指针指向数组下标为 0 的位置就不会有越界问题了。当我们再向队列中添加元素的时候， rear 向后移动。

![循环队列](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212201124834.png)

顺序队列中，我们说 font = rear 时，队列为空，循环队列中则不一样，也可能为满，如上图所示。解决办法有2种：

1. 设置一个标志变量 flag，当 front = rear 并且 flag = 0 的时候队列为空，当 front == rear 并且 flag = 1 的时候队列为满

2. 队列为空的时候就是 front = rear，队列满的时候，我们保证数组还有 <font color="red">一个空闲的位置</font>，如下图所示：那么现在判断队列是否为满的条件就是：

   ```bash
   (rear + 1) % QueueSize = front
   ```

   ![循环队列-队满](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212201159303.png)

##### 《队列实现》

###### ConcurrentLinkedQueue

- ConcurrentLinkedQueue是由链表结构组成的线程安全的先进先出无界队列
- 当多线程要共享访问集合时，ConcurrentLinkedQueue是一个比较好的选择
- 不允许插入null元素
- 支持非阻塞地访问并发安全的队列，不会抛出ConcurrentModifiationException异常。

![image-20221220141410750](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212201414926.png)

|                    | 抛出异常                       | 返回特殊值                    |
| ------------------ | ------------------------------ | ----------------------------- |
| 增加并返回元素     | add()，超出队列容量时抛出异常  | offer()，超出容量返回 false   |
| 删除并返回元素     | remove()，容量为0时，抛出异常  | poll()，容量为0时，返回 false |
| 获取队头元素不删除 | element()，容量为0时，抛出异常 | peek()，容量为0时，返回 null  |

###### Java中的双端队列（Deque）

Deque继承自 Queue 接口，也可以作为单端队列使用

典型代表：LinkedList

![image-20221220142116504](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212201421764.png)

|         | 队头操作（Head）                                             |                                                             | 队尾操作（Tail）                                       |                                                              |
| ------- | ------------------------------------------------------------ | ----------------------------------------------------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
|         | throw exception                                              | return special value                                        | throw exception                                        |                                                              |
| Insert  | addFirst<br />1. 增加元素不能为 null<br />2. 其它异常，比如有界队列 | offerFirst<br />1. 元素不能为 null<br />2. 实现内部调用 add | addLast<br />同addFirst                                | offerLast<br />1. 元素不能为 null<br />2. 实现内部调用 addFirst |
| remove  | removeFirst<br />队列空时：<br />NoSuchElementException<br />也就是说，使用时必须判空 | pollFirst<br />队列空时：return null                        | removeLast<br />队列空时：<br />NoSuchElementException | pollLast<br />队列空时：return null                          |
| examine | getFirst<br />队列空时：<br />NoSuchElementException<br />使用时必须判空 | peekFirst<br />队列空时：return null                        | getLast<br />队列为空时<br />NoSuchElementException    | peekLast<br />队列空时：return null                          |

```java
public boolean offerFirst(E e) {	//	队头入列
    addFirst(e);
    return true;
}
```

Java已经废弃了 Stack，使用 Deque定义了 LIFO（后进先出）的栈操作

| 栈方法 | 内部调用    | 备注                                                         |
| ------ | ----------- | ------------------------------------------------------------ |
| push   | addFirst    | 1. 压入的元素不能为 null<br />2. 可能抛出异常，内部调用的是 addFirst |
| pop    | removeFirst | 队列为空时，会抛出异常 NoSuchElementExcepton                 |
| peek   | peekFirst   | return special value                                         |



##### 应用场景

当我们需要按照 <font color="yellow">一定顺序 </font>来处理数据的时候可以考虑使用队列这个数据结构。

###### 阻塞队列

阻塞队列可以看成在队列基础上加了阻塞操作的队列。当队列为空的时候，出队操作阻塞，当队列满的时候，入队操作阻塞。使用阻塞队列我们可以很容易实现“<font color="yellow"> "生产者 - 消费者“ </font>模型。

###### 线程池中的请求 / 任务队列

线程池中没有空闲线程时，新的任务请求资源时，线程池该如何处理呢？

答：将这些请求放在队列中，当有空闲线程的时候，会循环反复从队列中获取任务来执行。

队列分为：

- 无界队列（基于链表）
  - 可以一直入列，除非系统资源耗尽
  - 比如：FixedThreadPool 使用无界队列 LinkedBlockingQueue
- 有界队列（基于数组）
  - 当队列满的话后面再有任务/请求就会拒绝，在 Java 中的体现就是会抛出`java.util.concurrent.RejectedExecutionException` 异常。

###### Linux 内核进程队列（按优先级排队）

###### 现实生活中的派对，播放器上的播放列表

###### 消息队列



### 中缀表达式

运算符优先级 ---> 让计算机帮我们实现

![image-20221214155935637](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212141559791.png)

我们需要保证在 <font color="yellow">符号栈</font> 中，栈顶到栈底的运算符的 <font color="yellow"> 优先级应该是递减</font> 的

- 这样才能利用栈的特性：先求优先级高的符号
- 但是栈的插入和删除，只能在 <font color="red">栈顶</font> 进行
- 所以我们在符号 <font color="yellow">入栈 </font>时候，需要有一个判断
  - 入栈的符号优先级 > 栈顶，正常入栈
  - 栈顶 >= 待入栈的符号，弹出栈顶的符号，并且弹出数字栈的2个，进行运算，计算出的结果再压入数字栈的栈顶

```java
//	这里只考虑个位数的计算
public class jisuanqi {
    public static void main(String[] args) {
        Stack<String> operation = new Stack<>();
        Stack<Integer> num = new Stack<>();
        int ans;    //  计算弹出变量的计算结果

        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入要计算的表达式");
        String next = scanner.next();
        char[] chars = next.toCharArray();
        for (char aChar : chars) {
            if (aChar >='0' && aChar<='9'){ //  数字栈
                num.push(Integer.parseInt(String.valueOf(aChar)));
            }else{
                //  判断当前栈不为 null，而且当前符号的优先级 <= 栈顶符号的优先级
                //  就将优先级高的弹出计算
                //  一直循环到满足: 当前元素优先级大于栈顶符号，才能将当前符号入栈
                while (!operation.isEmpty() && Integer.parseInt(yx(String.valueOf(aChar))) <= Integer.parseInt(yx(operation.peek()))){
                    Integer peek = num.peek();
                    num.pop();
                    ans = js(num.pop(), peek, operation.pop());  //  计算
                    num.push(ans);
                }
                operation.push(String.valueOf(aChar));  //  运算符入栈
            }
        }
        //  开始出栈了
        while (operation.size() > 0){
            Integer peek = num.peek();
            num.pop();
            ans = js(num.pop(), peek, operation.pop());  //  计算
            num.add(ans);
        }
        System.out.println(num.peek());
    }

    public static String yx(String c){
        switch (c){
            case "+":
            case "-":  //  加减的优先级为：1
                return "1";
            case "*":
            case "/":  //  乘除的优先级为：2
                return "2";
        }
        return null;
    }

    public static int js(int x, int y, String oper){
        switch (oper){
            case "+":
                return x + y;
            case "-":
                return x - y;
            case "*":
                return x * y;
            case "/":
                return x / y;
        }
        return 1;
    }
}
```



加上<font color="red"> 括号</font> 之后，就再加个判断

- 左括号 "("，直接压入符号栈栈顶
- 右括号")"
  - 从 2个栈顶弹出数据进行计算
  - 一直循环，直到栈顶为 "("，再处理下一个字符



注意的是：为了比较符号优先级不出错，左括号 "(" 优先级为最低

- 这样左括号后面的运算符才能入栈

代码实现：

### char ---> int

```java
Integer.parseInt(String.valueOf(aChar));
```



我们之前的乘法口诀是：有括号的话先算括号中的

那么用计算机实现的话：

- 左括号直接入栈
- 左括号（ yx 最低，保证括号内的符号优先级都比它高，可以入栈---> 即：保证先计算括号中的数
- 然后放置后，取出时 ）右括号要一直遍历到 左括号

```java
//	现在只考虑了单括号
package homework;

import java.util.Scanner;
import java.util.Stack;

/**
 * @Author: Ronnie LEE
 * @Date: 2022/12/14 - 12 - 14 - 18:46
 * @Description: homework
 * @version: 1.0
 */
public class jisuanqi {
    public static void main(String[] args) {
        Stack<String> operation = new Stack<>();
        Stack<Integer> num = new Stack<>();
        int ans;    //  计算弹出变量的计算结果


        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入要计算的表达式");
        String next = scanner.next();
        char[] chars = next.toCharArray();
        for (char aChar : chars) {
            if (aChar >='0' && aChar<='9'){ //  数字栈
                num.push(Integer.parseInt(String.valueOf(aChar)));
            }else{
                //  判断当前栈不为 null，而且当前符号的优先级 <= 栈顶符号的优先级
                //  就将优先级高的弹出计算
                //  一直循环到满足: 当前元素优先级大于栈顶符号，才能将当前符号入栈
                while (!operation.isEmpty() && Integer.parseInt(yx(String.valueOf(aChar))) <= Integer.parseInt(yx(operation.peek()))){
                    if ("(".equals(String.valueOf(aChar))){//  加入判断括号的逻辑【如果插入的是 (，就不用判断和操作了，直接插入】
                        break;
                    }
                    if (")".equals(String.valueOf(aChar))){
                        while (!operation.peek().equals("(")){
                            Integer peek = num.peek();
                            num.pop();
                            ans = js(num.pop(), peek, operation.pop());  //  计算
                            num.push(ans);
                        }
                        break;
                    }
                    //=============================
                    Integer peek = num.peek();
                    num.pop();
                    ans = js(num.pop(), peek, operation.pop());  //  计算
                    num.push(ans);
                }
                operation.push(String.valueOf(aChar));  //  运算符入栈
            }
        }
        //  开始出栈了
        while (operation.size() > 0){
            //  增加括号的逻辑由于括号内的已经计算完毕，所以遇到了就直接弹出即可
            if (")".equals(operation.peek()) || "(".equals(operation.peek())){  //  当栈顶是 ）时，它下面必然是 （，所以弹出2次即可
                operation.pop();
            }else {
                Integer peek = num.peek();
                num.pop();
                ans = js(num.pop(), peek, operation.pop());  //  计算
                num.add(ans);
            }
        }
        System.out.println(num.peek());
    }

    public static String yx(String c){
        switch (c){
            case "+":
            case "-":  //  加减的优先级为：1
                return "1";
            case "*":
            case "/":  //  乘除的优先级为：2
                return "2";

            case "(":
            case ")":
                return "0";
        }
        return null;
    }

    public static int js(int x, int y, String oper){
        switch (oper){
            case "+":
                return x + y;
            case "-":
                return x - y;
            case "*":
                return x * y;
            case "/":
                return x / y;
        }
        return 1;
    }

}
```

## 4. 深搜与宽搜

### 深搜（DFS）--- 满足 <font color="yellow">递归 </font>思想

无剪枝 = 暴力搜索

- 遍历整张图
- 用来解决求问题有多少个解，多少条路径，最大路径……等



### 宽搜（BFS）--- 用 <font color="yellow"> 队列</font> 实现

用队列实现

用来解决求 <font color="red"> 无权图的最短路径</font>

- 一层一层地遍历 
- 所以第一次抵达目标结点的路径，就是这个图的最短路径

所以宽搜大部分情况下，不需要遍历完整张图，就能找到答案

#### 8 皇后

| 行数 | 摆法 |
| ---- | ---- |
| 1    | 8    |
| 2    | 8    |
| 3    | 8    |
| 4    | 8    |
| 5    | 8    |
| 6    | 8    |
| 7    | 8    |
| 8    | 8    |

组成了一棵 8 叉树：共有 $8^{8}$ 种方式

限制条件：【这些限制条件在程序中成为 <font color="yellow"> 剪枝</font> 】

- 同行
- 同列
- 对角线

思路如下：

如果在 (i，j) 位置上放置了一个皇后，那么以下几种情况下都不能放置了

1. 第 i 行 （同行） 【逐行放置，这条规则就被规避掉了！！！】
2. 第 j 列 （同列）
3. |a - i| = |b - j|；（a, b）指的是某个位置！！！（对角线）

接下来使用一个一维数组 new int[] record ---> record[i]：表示：第 i 行皇后所在列数

```java
//	皇后位置
[i, record[i]]
```

```java
//  说明:理论上应该创建一个二维数组来表示棋盘，但是实际，上可以通过算法，用一个一维数组即可解决问题
//	arr[8] = {0, 4, 7, 5, 2, 6, 1,3} 
//	对应arr下标表示第几行，即第几个皇后
//	arr[i] = val, val表示
//	第i+1个皇后，放在第i+ 1行的 第val+ 1列

public class MyQueen {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("输入皇后的数目：");
        String next = scanner.next();
        int num = Integer.parseInt(next);
        System.out.println(num + " 皇后共有 " + Queen(num) + " 种排列方法");
    }

    public static int Queen(int n) {
        if (n < 1) {
            return 0;
        }
        int[] record = new int[n];
        return process(0, record, n);	//	从第一行开始添加
    }
    
    public static int process(int i, int[] record, int n) {    //  执行递归
        if (i == n) {   //  Base Case
            for (int k = 0; k < record.length; k++) {
                System.out.print(record[k] + "\t");
            }
            System.out.println();
            return 1;   //  8行都摆满了，排列次数 + 1
        }
        //	每调用一次process，就会产生一个新的栈
        int res = 0;
        for (int j = 0; j < n; j++) {
            if (isValid(record, i, j)) {    //  判断当前点是否可以放入
                record[i] = j;
                res += process(i + 1, record, n);
            }
        }
        return res;
    }

    public static boolean isValid(int[] record, int i, int j) {
        //	当考虑第 i 行，第 j 列的位置时
        //	说明前面的行都已经放置了皇后了【放置的列为：record[0 - (i - 1)]】
        //	第一行不判断，肯定可以放进去
        for (int k = 0; k < i; k++) {
            if (j == record[k] || Math.abs(k - i) == Math.abs(record[k] - j)) {
                return false;
            }
        }
        return true;
    }
}
```



思想：

- 当前行所有列都试过之后，不行的话，要返回上一列可行的地方继续走，依次类推

![3213213232133213未命名绘图](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212161953837.png)

从第二行开始，每一行不管找没找到，都得遍历到最后为止

![未321命名绘图](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212162140455.png)



![未命名绘图7789](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212162152791.png)



![未命名绘图32](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212162208724.png)

对于 res 的理解：

以第一行为基准行，第一行不进行判断，肯定可以放进去

- 共有 4 种类可能
- (i = 0， j = 0)
- (i = 0， j = 1)
- (i = 0,   j = 2)
- (i = 0,   j = 3)
  - i 的范围是 0 - 3，当 i 到达 4 时，越界（base case），说明所有皇后都已经放好
    - return 1;【完成一次排列】

- 将这4种情况下，可以将皇后都放置【n == 4】的次数都加起来就好了

```java
//  说明:理论上应该创建一个二维数组来表示棋盘，但是实际，上可以通过算法，用一个一维数组即可解决问题
//	arr[8] = {0, 4, 7, 5, 2, 6, 1,3} 
//	对应arr下标表示第几行，即第几个皇后
//	arr[i] = val, val表示
//	第i+1个皇后，放在第i+ 1行的 第val+ 1列

public class MyQueen {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("输入皇后的数目：");
        String next = scanner.next();
        int num = Integer.parseInt(next);
        System.out.println(num + " 皇后共有 " + Queen(num) + " 种排列方法");
    }

    public static int Queen(int n) {
        if (n < 1) {
            return 0;
        }
        int[] record = new int[n];
        return process(0, record, n);	//	从第一行开始添加
    }
    
    public static int process(int i, int[] record, int n) {    //  执行递归
        if (i == n) {   //  Base Case
            for (int k = 0; k < record.length; k++) {
                System.out.print(record[k] + "\t");
            }
            System.out.println();
            return 1;   //  8行都摆满了，排列次数 + 1
        }
        //	每调用一次process，就会产生一个新的栈
        int res = 0;
        for (int j = 0; j < n; j++) {
            if (isValid(record, i, j)) {    //  判断当前点是否可以放入
                record[i] = j;
                res += process(i + 1, record, n);
            }
        }
        return res;
    }

    public static boolean isValid(int[] record, int i, int j) {
        //	当考虑第 i 行，第 j 列的位置时
        //	说明前面的行都已经放置了皇后了【放置的列为：record[0 - (i - 1)]】
        //	第一行不判断，肯定可以放进去
        for (int k = 0; k < i; k++) {
            if (j == record[k] || Math.abs(k - i) == Math.abs(record[k] - j)) {
                return false;
            }
        }
        return true;
    }
}
```

#### 奇怪的电梯

大楼每一层都可以停电梯

- 第 $i$ 层楼（$1\leqslant i\leqslant N$）上有一个数字 K($0\leqslant K_{i}\leqslant N)$
- 电梯只有 4 个按钮：开、关、上、下
- 上下的层数 = 当前楼层上的数字【如果不满足要求，相应的按钮就会失灵】

例如：3 3 1 2 5 代表 $K_{i}(K_{1,}=3,K_{2}=3.……)$

在一楼，按“上”可以到4楼，按“下”是不起作用的，因为没有 -2 楼

问：从A层楼到B层楼至少要按几次按钮?

| 输入                  | 输出                |
| --------------------- | ------------------- |
| 【N，A，B】           | 次数（无法到达 -1） |
| N个整数，表示 $K_{i}$ |                     |

![image-20221219233822732](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212192338861.png)

无权有向图

- 无权图的最短路径可以用 宽搜（BFS）来解决

  ![image-20221219234041148](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212192340268.png)

5 1 5 

3 3 1 2 5

https://www.cnblogs.com/dw3306/p/14496979.html

![123未命名绘32图](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212211628339.png)

```java
public class floor {
    //  5 1 5
    static int t;
    static int next;    //  当前楼层可到达的下一个楼层
    static int p1;    //  头
    static int p2;    //  尾

    static int[] upDown = new int[]{1, -1};
    static boolean[] mark = new boolean[6]; //  标记楼层是否到达过
    static int[] k = new int[]{0, 3, 3, 1, 2, 5};
    static int[] pre = new int[6];  //  记录当前结点的上一节点位置（以便查找路径）
    static int[] que = new int[6];  //  队列
    public static void main(String[] args) {
        Arrays.fill(mark, true);    //  初始的时候，所有楼层都可到达
        int n = k.length;
        t = 0;
        p1 = 0;
        p2 = 0;
        que[++p2] = 1;    //  A 楼层入队
        mark[1] = false;
        pre[p2] = -1; //  初始化 pre 数组边界
        while (p1 <= p2){  //  模拟 dfs
            p1++;
            for (int i = 0; i < 2; i++) {
                next = que[p1] + k[que[p1]] * upDown[i];
                if (next > 0 && next < n && mark[next]){
                    que[++p2] = next; //  可达的楼层入队
                    mark[next] = false; //  标记为已到达过
                    pre[p2] = p1;
                    if (next == 5){
                        print(p2);
                        return;
                    }
                }
            }
        }
        System.out.println("无法到达");
    }

    public static void  print(int p2){
        int j = pre[p2];
        while (j != -1){
            t++;    //  路径加1
            j = pre[j];     //  再往前查找路径
            System.out.print("途经：" + j + " ");
        }
        System.out.println();
        for (int i = 0; i < que.length; i++) {
            System.out.println(que[i]);
        }
        System.out.println();
        System.out.println("次数为：" + t);
    }
}
```



## 5. 线段树

线段树是一种二叉树，广义上也被归类为 <font color="yellow">二叉搜索树</font>，是一种工具。

- 对于区间的修改、维护和查询时间复杂度优化为 $log$ 级别

![image-20221221232922686](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212212329800.png)

小区间更新大区间，线段树是<font color="yellow"> 平衡</font> 二叉树

### 局限性

问题需要满足：区间加法

- 对于 $[L,R]$ 的区间，它的答案可以由 $[L,M]$ 和 $[M+1, R]$ 的答案合并求出
- 满足的问题：区间求和，区间 max, min 值等

不满足的问题：

- 区间的众数
- 区间最长连续问题
- 最长不下降问题等

### 解决问题的步骤

#### 1. 建树

![image-20221224001929436](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212240019524.png)

开一个数组，以 <font color="red">堆 </font>的方式存储

- 线段树的数组：一般要开到 $4 * n$【才能确保不会出现越界访问】

举例：

![image-20221224002239411](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212240022497.png)

![546jiu未命名绘图](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212251057863.png)

![未342fw命名绘图](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212251147308.png)

##### 空间大小为 $4*n$



#### 2. 修改

##### 2.1  单点修改

修改数列中下标为 i 的数据

- 从根节点向下 DFS
  - 如果当前节点的左儿子的区间 【L, R】包含了 i，也就是 $L<=i<=R$，就访问左儿子
  - 否则访问右儿子
- 直到 $L=R$，也就是搜到了只包含这个数据的节点，就可以修改它
- 不要忘了将包含此数据的大区间的值更新

![image-20221224003039990](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212240030090.png)

区间修改修改后的查询会用到 <lazy标记>

##### 2.2 区间修改（lazy标记）

如果按照常规思路；向下递归遍历所有节点 一一修改，时间复杂度 $O(n)$ 和暴力处理相差无几

线段树：<font color="yellow">lazy</font> 标记

- 这个区间的值已经更新
- 但是它的子区间却没有更新
- 更新的信息就是标记里存的值

所以修改步骤为：

- 如果 <font color="red">要修改的区间 </font>完全覆盖当前区间
  - 直接更新这个区间，打上 lazy 标记
- 如果没有完全覆盖，且当前区间有 lazy 标记，先下传lazy 标记到子区间，再清除当前区间的 lazy 标记【多次修改，才会有这个<font color="blue">**下传清除**</font>操作】
- 如果修改区间和左儿子有交集，搜索左儿子
- 如果修改区间和右儿子有交集，搜索右儿子
- 最后将当前区间的值更新

![image-20221224010226458](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212240102568.png)

#### 3. 区间查询

##### 3.1 仅有单点修改的区间查询

仅有单点修改的区间查询，不需要处理 lazy 标记

非常简单，步骤为：

- 如果要查询的区间完全覆盖当前区间
  - 直接返回当前区间的值 【baseCase】
- 如果查询区间和左儿子有交集搜索左儿子
- 如果查询区间和右儿子有交集就搜索右儿子
- 最后合并处理2边查询的数据

注意：处理完根节点的左儿子，处理完子问题后，还要回到大问题，看右儿子是否有交集

![image-20221224004253647](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212240042775.png)



##### 3.2 区间修改后的区间查询

步骤为：

- 如果要查询的区间完全覆盖当前区间
  - 直接更新这个区间，打上 lazy 标记
- 如果没有被完全包含，且<font color="yellow">当前区间有 lazy 标记</font>
  - 先下传 lazy 标记到子区间
  - 再清除当前区间的 lazy 标记

- 如果查询区间和左儿子有交集搜索左儿子
- 如果查询区间和右儿子有交集就搜索右儿子
- 最后合并处理2边查询的数据



比如：我们还是要查询$[1,3]$ 区间和

![image-20221224011909039](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212240119139.png)

将 lazy 标记下传给 左右孩子，并清除自身的 lazy 标记

![image-20221224011418257](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212240114364.png)

然后继续看自身的左右儿子

![image-20221224012255806](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212240122905.png)

![image-20221224012339407](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212240123512.png)

### 线段树模板例题

如题：已知一个数列，你需要进行下面 2种操作：

- 将某区间每一个数加上 k 
- 求出某区间所有数的和

| 输入     | 意义                                |
| -------- | ----------------------------------- |
| $n$, $m$ | 数字个数$(1\leq n)$，操作的总个数 m |
| 1 x y k  | 将区间 $ [x,y]$ 内每个数加上 k      |
| 2 x y    | 输出区间 $[x, y]$ 内每个数的和      |

暴力解决：$(m*n)$

线段树：$(m*logn)$

| 输入                              | 计算                                                         | 输出 |
| --------------------------------- | ------------------------------------------------------------ | ---- |
| 一、5 5（n、m）                   |                                                              |      |
| 二、1 5 4 2 3                     |                                                              |      |
| <font color="yellow">2</font> 2 4 | 5 +  4 + 2                                                   | 11   |
| 1 2 3 2                           | 1 <font color="red">7</font> <font color="red">6</font> 2 3  |      |
| <font color="yellow">2</font> 3 4 | 6 + 2                                                        | 8    |
| 1 1 5 1                           | <font color="red">2</font> <font color="red">8 </font><font color="red">7 </font><font color="red">3 </font><font color="red">4</font> |      |
| <font color="yellow">2</font> 1 4 | 2 + 8 + 7 + 3                                                | 20   |

```java
package homework;

/**
 * @Author: Ronnie LEE
 * @Date: 2022/12/24 - 12 - 24 - 18:29
 * @Description: homework
 * @version: 1.0
 */
public class segment_Tree {
    static class Tree{
        int l;
        int r;
        int lazy;   //  lazy 标记
        int res;    //  记录当前节点的区间和

        public Tree() {
        }

        @Override
        public String toString() {
            return "Tree{" +
                    "l=" + l +
                    ", r=" + r +
                    ", lazy=" + lazy +
                    ", res=" + res +
                    '}';
        }
    }

    static int[] data = {0, 1, 5, 4, 2, 3};
    static int[][] operate  = {{5, 5}, {2, 2, 4}, {1, 2, 3, 2}, {2, 3, 4},{1, 1, 5, 1}, {2, 1, 4}};
    static Tree[] tree = new Tree[20];
    //  输入数组【放置越界：4 * n 】

    public static void main(String[] args) {
        for (int i = 0; i < tree.length; i++) {
            tree[i] = new Tree();   //  放置 null 指针，每一个都进行初始化
        }
        build(1, 1, 5); //  建树，从根节点开始，区间为 [1, n]
        for (int i = 1; i < 6; i++) {
            if (operate[i][0] == 1) {   //  修改
                change(1, operate[i][1], operate[i][2], operate[i][3]); //  从根节点 1 开始搜，[x, y] 区间 + k
            }
            if (operate[i][0] == 2){    //  查询
                //  从根节点1 开始搜 [x, y] 区间的和
                System.out.println(query(1, operate[i][1], operate[i][2]));
            }
        }
    }

    public static void build(int i, int l, int r){
        int mid;
        tree[i].l = l;
        tree[i].r = r;  //  节点表示区间范围
        if (l == r) {
            tree[i].res = data[r];
            return; //  处理完边界后一定要退出当前函数
        }
        mid = l + (r - l) / 2;
        build(i * 2, l, mid);
        build(i * 2 + 1, mid + 1, r);   //  建左儿子和右儿子（递归的思想）
        tree[i].res = tree[i * 2].res + tree[i * 2 + 1].res;    //  父节点区间和 res = 左儿子区间和 res + 右儿子区间和 res
    }

    //  区间修改
    public static void change(int i, int l, int r, int k){
        if (tree[i].r <= r && tree[i].l >= l){  //  当前区间被修改区间覆盖
            tree[i].res += k * (tree[i].r - tree[i].l + 1); //  即：该区间的每一个数都 + k
            tree[i].lazy += k;  //  打上 lazy 标记 + k
            return;
        }
        if (tree[i].lazy != 0){ //  如果当前节点有 lazy 标记，下传标记
            downLazy(i);
        }
        if (tree[2 * i].r >= l){   //   如果要修改的区间与左儿子有交集
            change(i * 2, l, r, k);
        }
        if (tree[2 * i + 1].l <= r){    //  如果要修改的区间与左儿子有交集
            change(i * 2 + 1, l, r, k);
        }
        tree[i].res = tree[2 * i].res + tree[2*i + 1].res;  //  更新
    }

    //  区间查询 query
    public static int query(int i, int l, int r){
        int t;  //  用来合并左右区间询问到的值
        if (tree[i].r <= r && tree[i].l >= l){  //  当前区间被修改区间覆盖
            return tree[i].res;
        }
        if (tree[i].lazy != 0){ //  如果当前节点有 lazy 标记，下传标记
            downLazy(i);
        }
        t = 0;
        if (tree[2 * i].r >= l){    //  如果要修改的区间与左儿子有交集
            t += query(i * 2, l, r);
        }

        if (tree[2 * i + 1].l <= r){    //  如果要修改的区间与左儿子有交集
            t += query(i * 2 + 1, l, r);
        }
        return t;
    }

    public static void downLazy(int i){
        int m;
        tree[2 * i].lazy += tree[i].lazy;
        tree[2 * i + 1].lazy += tree[i].lazy;   //  左右儿子继承父亲的 lazy 标记
        m = (tree[i].l + tree[i].r) / 2;
        tree[2 * i].res += tree[i].lazy * (m - tree[i].l + 1);
        tree[2 * i + 1].res += tree[i].lazy * (tree[i].r - m);    //  【r - (mid + 1)】+ 1
        tree[i].lazy = 0;   //  消除父亲的 lazy 标记
    }
}
```

## 6. KMP 算法（字符串搜索算法）

- 时间复杂度：$O(m)预处理$ +$O(n)$ 匹配
- 空间复杂度：$O(m)$

```java
package homework;

import java.util.Scanner;

/**
 * @Author: Ronnie LEE
 * @Date: 2022/12/26 - 12 - 26 - 10:43
 * @Description: homework
 * @version: 1.0
 */
public class KMP {
    static String a;
    static String b;
    public static void main(String[] args) {
        //  这里数组下标是从1 开始的 !!!
        Scanner scanner = new Scanner(System.in);
        System.out.print("请输入字符串 a 的值: ");
        a = scanner.next();
        char[] A = a.toCharArray();
        System.out.print("请输入字符串 b 的值: ");
        b = scanner.next();
        char[] B = b.toCharArray();
        System.out.println("a= " + a + " b= " + b);
        int n = a.length();
        int m = b.length();
        int[] next = new int[m];    //  建立 next 数组，保存部分匹配值
        next[0] = 0;    //  如果只有一个字符，那么没有最长公共前后缀！！！
        //  j 指针回溯的位置，只与 B 串有关，与 A 串无关
        //  next[] 数组构建思路
        //  1. 在 A、B串匹配之前，就通过 B 串，计算回溯的位置（B 子串的最长公共前后缀），存在 next[] 数组中
        //  2. 这个 next 与 B串形成映射数组，存的数据 next[i] 就是 B[1] ~ B[i]
        //  以 递推的方式求出 next 数组
        /**
         *  B 串自己与自己匹配
         *  B[1] ~ B[i] 的前缀与它的后缀匹配
         *  后缀为：主串
         *  前缀为：模式串
         *  以 递推的方式求出 next 数组
         *
         *  目的：对于 i∈[2, m] （个数）
         *       求出 B[1] ~ B[i]
         *       的最长公共前后缀的长度
         *
         *       j：B串前缀的指针，也就是当前字符匹配之前的最长公共前后缀的长度
         */
        for (int i = 1, j = 0; i < m; i++) {   //  构建 next 数组（为了代码美观：将循环改了一下：i j 的映射偏了 1 位置）
            while (B[i] != B[j] && j > 0){  // 匹配失败，需要从 next[j - 1]获取新的 j
                j = next[j - 1];    //  KMP 算法核心点！！！【回溯】
            }
            //  next[i] = j + 1，j 是 B 串前缀的指针，也就是当前字符匹配之前的最长公共前后缀的长度
            //  这里匹配成功了，要加 1
            if (B[i] == B[j]){  //  匹配成功
                j++;
                next[i] = j;    //  i 对应字符串的位置
            }
        }

        //  写出 KMP 匹配步骤
        for (int i = 0, j = 0; i < n; i++) {   //  字符串匹配
            while (A[i] != B[j] && j > 0){
                j = next[j - 1];
            }
            if (A[i] == B[j]){  //  匹配成功
                j++;
            }
            if (j == m){    // 完成匹配
                System.out.println(i - j + 1);  //  输出下标 index
                j = 0;  //  j 重置【当匹配上了之后，j从头开始匹配】
            }
        }
    }
}
```

![fwefasd未命名绘图](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212270030190.png)

https://www.cnblogs.com/zzuuoo666/p/9028287.html

找到 B 在 A 中的位置

![image-20221225202653407](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212252026486.png)

暴力解法：$(n*m)$

- 每次匹配失败后
- i，j 都回溯，从头开始匹配【并没有从上次失败中，获取信息，并总结经验教训】

| KMP算法来源                            |      |
| -------------------------------------- | ---- |
| D.E.<font color="yellow">K</font>unth  | K    |
| J.H.<font color="yellow">M</font>orris | M    |
| V.R.<font color="yellow">P</font>ratt  | P    |

### 6.1 KMP 算法核心

1. 利用匹配失败后的信息
2. 尽量减少模式串（B）与主串（A）的匹配次数
3. 以达到快速匹配的目的
4. 通过一个 <font color="yellow">next 数组</font>，保存模式串（B）中 <font color="yellow">前后最长公共子序列的长度</font>，每次回溯时，通过 next 数组找到，前面匹配过的位置，省去了大量的计算时间

### 6.2 如何减少匹配的次数

#### 字符串的前缀和后缀

比如：字符串 $ababacb$

| 前缀集合                                        | 后缀集合                                        |
| ----------------------------------------------- | ----------------------------------------------- |
| $\left \{ a,ab,aba,abab,ababa,ababac \right \}$ | $\left \{ b,cb,acb,bacb,abacb,babacb \right \}$ |

- 暴力算法中：每次匹配失败后，ij都回溯，从头开始匹配
- KMP算法：不回溯 $i$，只回溯 $ j$ 到指定位置

当我们发现字符匹配失败时，这个失败的信息

- 并不单单只代表失败，同时也代表着

  - 除了当前这 2个 字符不匹配，
  - 前面的 j 个字符都匹配成功了

  ![image-20221225205227708](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212252052796.png)

- 就需要将 B 串后移（这时候有选择地移，移到有可能成功地位置）

  - 从 $i$ 之前开始匹配

    - 需要满足 A后缀 与 B前缀 有交集时

      ![image-20221225231041491](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212252310584.png)

    - 所以 B 子串就可以这样移

      ![image-20221225232012228](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212252320319.png)

      

    - 这时候，只需要将 j 回溯，然后再从 i 处开始匹配

      ![image-20221225232409441](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212252324530.png)

    - 那么，这个 B 串要后移多少位？【j 指针回溯到哪个位置?】

    - 找到 A 字串后缀 与 B字串前缀 的交集里：最长的那个元素。才能使B串后移得最少，且在已知信息中匹配的最多

    - 最长元素的长度：就是 <font color="yellow">j</font> 指针 <font color="yellow">回溯的位置</font>

      - 前面讲过：<font color="red">匹配失败 </font>这个信息，代表着：<font color="yellow">前面的 j 个字符</font>是匹配成功了的，也即是说：<font color="yellow">A 子串与B子串是相同的</font>
      - 那么 “找到 A 子串后缀 与 B子串前缀的交集” <font color="red">等价于</font> “找到 B 子串后缀 与 B子串前缀的交集”
      - j 指针回溯的位置 = B 子串的最长公共前后缀

    - 通过隐藏信息（匹配失败时，A串与B串存在着一段相同的子串），我们可以认为：

    - j 指针回溯的位置，<font color="yellow">只与 B 串有关</font>，与 A串无关

      - 在 A、B字符串匹配之前，就通过 B 串计算回溯位置，存在<font color="yellow">一个数组 next</font> 里【<font color="green">关键</font>】
      - 这个 next 与 B串形成映射数组，存的数据 next[ i] 就是 B[1] ~ B[i] 的最长公共前后缀的长度

  - 从 $i$ 之后开始重新匹配（肯定就不用回溯 $i$ 了 ）

    ![image-20221225210412094](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212252104192.png)

### 6.3 A、B字符串匹配步骤

$i,j$ 初始化为 0

1. 如果 $A[i+1]=B[j+1]$，$i++$，$j++$ 继续匹配

2. 如果 $A[i+1]!=B[j+1]$

   - 回溯 $j$ 到 $next[j]$，直到$A[i+1]=B[j+1]$

   - 这里有一种情况（B串需要移动到 i 后面）

     - $j$ 回溯到 0 时也不能满足使 $A[i+1]=B[j+1]$

     - 当 $j=0$ 时，忽略$j$，增加 $i$，直到 $A[i+1]=B[j+1]$ （注意：只有一个元素时：它没有最长公共前后缀 ---> j = 0）

       ![image-20221226001709959](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212260017055.png)

     - 也就是 j 指向了 a 前面的这个空位置

       ![image-20221226002000365](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212260020455.png)

     - 也就等于 B 串右后移了一位【现在是匹配了，但是如果不匹配的话，忽略j, 增加 i，直到匹配上】

       ![image-20221226002154240](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212260021332.png)

3. 当 j = m （B串完全匹配）时，输出位置，继续匹配（因为后面可能还有匹配的子串）

### 6.4 构建 next 数组

注意：当只有一个元素，它没有最长公共前后缀

![image-20221226144251786](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212261442056.png)

![image-20221226150510303](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212261505421.png)

| 元素          | 最长公共前后缀                            |
| ------------- | ----------------------------------------- |
| A             | 0（只有一个元素时候，没有最长公共前后缀） |
| A B           | 0                                         |
| A B C         | 0                                         |
| A B C D       | 0                                         |
| A B C D A     | 1                                         |
| A B C D A B   | 2                                         |
| A B C D A B D | 0                                         |

B 串自己与自己匹配

$B[1]-B[i]$ 的前缀与后缀匹配

- 后缀为主串
- 前缀为模式串

递推方式求出 next 数组

目的：对于 $i\epsilon [2,m]$，求出 $B[1]-B[i]$ 的最长公共前后缀的长度

1. 如果匹配：
   - $next[i]=j+1$，$j$ 是 B串前缀的指针，也就是当前字符匹配之前的最长公共前后缀的长度，这里匹配成功了，要 + 1
2. 如果不匹配：
   - 回溯 j 指针，$j=next[j]$，直到匹配成功

### 6.5  j 回溯

https://blog.csdn.net/Leycaner/article/details/108301195?utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EOPENSEARCH%7Edefault-1.control&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EOPENSEARCH%7Edefault-1.control

可以看出 KMP 算法的时间复杂度是线性的，即：$O(m+n)$

关于 j 如何回溯，<font color="yellow">模式串（B）整体都是向前移的</font>，只要模式串 <font color="yellow">移到主串（A）的尾部</font>，算法就 <font color="yellow">结束 </font>了

KMP 一次比较只会产生 2种结果：

1. 匹配成功主串的指针（i）向前移动
2. 匹配失败模式串（B）整体向前移动

前面的移动必为 n 次，后面的移动最多 n 次，所以最大的时间复杂度：$2n$

同理也可得，next 数组构建最大时间复杂度为：$2m$

![image-20221227001646687](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212270016814.png)

这种情况下，原来<font color="yellow">绿色部分</font>就<font color="yellow">不能用了</font>，显然，我们想要缩小绿色的部分，还要满足最长公共子串的要求，也就像下面这样：

![新的最长前缀公共子串](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212270019653.png)

这个蓝色的部分，首先要<font color="yellow"> 内容相同</font>，其次还要<font color="yellow">位于深绿色部分的开头和结尾</font>，这不就是**深绿色部分的最长公共前缀子串**吗

即：更新 $j$ $=$ $next[j-1]$



## 7. 并查集【并、查】Disjoint-set data structure

由于支持查询和合并这两种操作，并查集在英文中也被称为

- 联合-查找数据结构（Union-find data structure）
- 合并-查找集合（Merge-find set）

一种树型的数据结构：用于处理一些 <font color="yellow">不相交集合 </font>的 <font color="yellow">合并与查询 </font>问题。

例如：n 个元素，分属不同的 n 个集合

2种操作：

1. 给出 2个元素的关系，合并2个集合（x 与 y 是亲戚，亲戚的亲戚是亲戚），于是【x 所在集合】 与 【y 所在集合】 合并
2. 查询 2个元素是否存在关系（是否在同一个集合）



### 7.1 用数据结构实现并查集

1. 一个集合构建一棵树，任选一个元素作为该集合的根节点

2. 建立 pre 数组记录每个元素的父节点 

   - pre[当前节点] = 父节点
   - 根节点的父节点 = 自己本身

3. 并查集的操作分为2类：“并”和“查”，给出元素关系后，如何进行合并呢？

4. 将一个集合的树变成另一个集合的子树（将一个集合的  root 的父节点改为另一个集合的root），pre[B的根节点] = A的根节点，就可以将 2棵树合并为一棵树

   ![image-20221227083402966](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212270834150.png)

   ![image-20221227083541566](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212270835682.png)

​	 

### 7.2  如何查询 2个元素是否在同一集合？

1. 从该元素开始访问父节点（一般用递归）

2. 直到一步步访问到 root

3. 再对2个元素的 root 进行对比判断

   ![image-20221227084026579](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212270840689.png)

### 7.3 时间复杂度

并查集的“并”与“查”时间复杂度取决于树的深度

避免如下情况：

![image-20221227084156660](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212270841750.png)

### 7.4 路径压缩

- 在查询时，最终目的：找到这个元素所在的这棵树的 root，那么它和其它元素是如何联系的，对我们来说就没有意义了。
- 在每次查询时，对被查询元素到 root 沿途经过的节点顺便进行一步路径压缩
- 将经过节点的父节点都更改为根节点，pre[经过节点]  = root，让树的形状尽量接近：
  ![image-20221227084744293](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212270847402.png)



### 7.5 带权并查集【NOI2001：食物链】

输入

| 输入格式                               | 含义                                       |
| -------------------------------------- | ------------------------------------------ |
| N，M                                   | N个元素，M个操作                           |
| 接下来M行，每行包含三个整数（K，X，Y） |                                            |
| K = 1                                  | 合并 X 与 Y所在的集合                      |
| K = 2                                  | 输出X 与 Y是否在同一集合内【是：Y，否：N】 |

输出

| 输出格式                              |                                   |
| ------------------------------------- | --------------------------------- |
| 对于每一个 K = 2 的操作，都有一行输出 | 每行包含一个大写字母，为 Y 或者 N |

### 7.6 动物王国

![未命321绘图](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212281708550.png)

动物王国有 3 种动物，A吃B，B吃C，C吃A

现在有 N 个动物，以 1 - N 编号，每个动物都是 A，B，C 中的一种，但是我们并不知道它到底是哪一种

有人用 2种 说法对这 N 个动物构成的食物链关系进行描述

| 说法        | 含义          |
| ----------- | ------------- |
| 1    X    Y | X 和 Y 是同类 |
| 2    X    Y | X 吃 Y        |

此人对 N 个动物，用上述 2 种说法，一句接一句地说出 K 句话，这 K 句话有的是真的，有的是假的。

当满足如下3条之一时，就是假话，否则就是真话

- 当前的话与前面某些真的话冲突，就是假话
- 当前的话 X 或 Y 比 N 大，就是假话
- 当前的话表示 X 吃  X，就是假话

你的任务是根据给定的 N $(1\leqslant N\leqslant 50000)$ 和 K句话 $(0\leqslant K\leqslant 100000)$ ，输出假话的总数

输入

| 输入         |                       |
| ------------ | --------------------- |
| N    K       |                       |
| D     X    Y | D = 1,  X 和 Y 是同类 |
|              | D = 2，X 吃 Y         |

输出

只有一个整数，表示假话的数目

### 7.7 分析

基础并查集：亲戚的亲戚是亲戚

而是存在着 3种关系【我的猎物会捕食我的天敌】，是一种<font color="yellow">循环关系</font>！！！

1. 捕食
2. 猎物
3. 天敌

![fsdafe未命名绘图](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212281731065.png)

可以由一些关系退出未知的关系：

- x 和 y 都f捕食 z，那么 xy 肯定是同类
- x 捕食 y，y 捕食 z，那么 z 一定是 x 的天敌

#### 1. 第一种解法：种类并查集（多倍并查集）

这 3 个分类是一种 <font color="yellow">相对的关系</font>

![image_0.8005583959947538](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212281747856.gif)

当 2个 元素要建立练习时候，例如：

x，y 是同类，那这个信息也表示：

- x 的猎物 也是 y 的猎物【x右 = y右】
- x 的天敌 也是 y 的天敌【x左 = y左】



x 吃 y：要将 x 与 y 联系起来，但是它们不再是同类

- y 与 【x的猎物】合并到同一集合 ---> y 和 (x + n) 合并
- 同时我们需要把隐含的信息一起合并
  - y 的天敌是 x 的同类【y左 = x 】
  - y 的猎物是 x 的天敌【y右 = x左】

| 思路                            |                      |
| ------------------------------- | -------------------- |
| [x] 所在集合中的元素都是        | 【x 的同类】         |
| [ x + n ] 所在集合中的元素都是  | 【x 的猎物】x右      |
| [ x + 2n ] 所在集合中的元素都是 | 【x 的天敌】x左      |
| [ x + n] 所在集合中的元素都是   | 【x + 2n 的天敌】x左 |

#### 2. 第二种解法：带权并查集

2个元素建立联系时

- 并不只把它们所在的集合合并
- 还要给它们之间赋一个权值，来表示它们之间的关系

<font color="yellow">难点</font>：路径压缩 / 集合合并时，关系值的改变。

怎么找到当前节点 x 与 根节点 的关系？

数学思维：<font color="yellow">向量 </font>计算

![image-20221228183028277](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212281830387.png)

![image-20221228193340374](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212281933501.png)



