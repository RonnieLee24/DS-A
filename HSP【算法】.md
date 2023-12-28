# 	HSP【算法】

## 1. 二分查找（非递归）BS

- 有序
- 运行时间为对数时间  $log_{2}n$

数组：

```bash
{1, 3, 8, 10, 11, 67, 100}
```

```java
package com.alq.binarySerarch;

public class binarySearchDemo {
    public static void main(String[] args) {
        int[] arr = {1, 3, 8, 10, 11, 67, 100};

        int index = binarySearch(arr, 24);
        System.out.println(index);

    }

    public static int binarySearch(int[] arr, int target){
        int l = 0;
        int r = arr.length;
        while (l <= r){
            int mid = l + (r - l) / 2;
            if (arr[mid] < target){
                l = mid+1;
            }else if (arr[mid] > target){
                r = mid - 1;
            }else {
                return mid;
            }
        }

        return -1;
    }
}
```



## 2. 分治算法 (Divide-and-Conquer Algorithm) DAC ---> 汉诺塔

应用场景：

- 快速排序
- 归并排序
- 傅里叶变化
- 二分搜索
- 大整数乘法
- 最接近点对问题
- 循环赛程表
- 汉诺塔

分治法在每一层递归上都有 3 个 步骤：

1. 分解：将原问题分解为若干个规模较小，相互独立，与原问题形式相同的子问题
2. 解决：若子问题规模较小，而容易被解决则直接解决，否则递归地解各个子问题
3. 合并：将各个子问题的解合并为原问题的解

经典案例：汉诺塔（Hanoi）

思路分析：

1. 如果只有一个盘：A --> C

2. 如果 n > 2，我们总是可以看成是 2 个盘子

   1. (n - 1)个盘子【一个整体】

   2. max

   - 将 (n- 1) A --> B

   - 将 max A --> C

   - 将 B 的所有盘：B --> C

```java
package com.alq.divideAndConquer;

public class HanoiDemo {
    public static void main(String[] args) {
        Hanoi(5, 'A', 'B', 'C');
    }
    /**
     * @param n 个数
     * @param a 起点
     * @param b 桥梁
     * @param c 终点
     */
    public static void Hanoi(int n, char a, char b, char c){
        //  1. n = 1, A --> C
        //  2. n > 1, A 中看作是2个盘子：1）上面的 n - 1 个 2）max
        //      2.1 将 (n - 1) A --> B
        //      2.2 将 max A --> C
        //      2.4 (n- 1) B ---> C
        if (n == 1){
            System.out.println(a + "-->" + c);
            return;
        }
        //  2.1
        Hanoi(n - 1, a, c, b);  //  a 通过 c 移动到 b
        //  2.2
        System.out.println(a + "-->" + c);  //  将 max 从 A 移动到 C
        //  2.3
        Hanoi(n - 1, b, a, c);
    }
}
```

## 3. 动态规划 (Dynamic programming)  DP

### 3.1 递归与递推

#### 1. 递归【未知 ---> 已知】

- 将未知的问题缩小
- 直到已知的问题
- 便开始返回值
- 最后解决我们的问题

#### 2. 递推【已知 ---> 未知】

- 由已知的问题
- 向前推位置
- 一步步推算到我们最后要解决的问题上

兔子问题：

假定一对大兔子每月能生对小兔子，每对新生的小兔子

- 经过一个月可以长成一对大兔子具备繁殖能力
- 如果不发生死亡，且每次均生下一雌一雄

问：最初有一对小兔子，1 年后共有多少对兔子？

| 月份 | 0（初始态） | 1    | 2    | 3    | 4    | 5    | 6    | 7    | ……   |
| ---- | ----------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 小   | 1           | 0    | 1    | 1    | 2    | 3    | 5    | 8    | ……   |
| 大   | 0           | 1    | 1    | 2    | 3    | 5    | 8    | 13   | ……   |
| 总   | 1           | 1    | 2    | 3    | 5    | 8    | 13   | 21   | ……   |

2个要点：

![mmq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304241349581.png)

##### Fibonacci数列

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

##### 三角形最小路径和（120）

关注点：

- 顶部到底边
- 路径上数字之和最小
  - 左下
  - 右下
- [i, j] 的下一个相邻节点为：[i + 1, j]，[i + 1, j + 1]

![image-20230424154055818](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304241540874.png)

![image-20230424160135030](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304241601120.png)

![image-20230424160324447](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304241603539.png)

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

![image-20230424160612030](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304241606090.png)

我们使用记忆数组：进行算法优化

##### 记忆数组

时间复杂度： $O(2^{n})$ --->  $O(n^{2})$【最后复杂度就是遍历 2维数组了】

记忆化递归：

- 每访问一个节点，就把它记录下来
- 新开一个数组 record(i, j)
  - 如果没有记录，就记录
  - 否则，直接取出

```java
class Solution {
	//	定义一个 记忆数组 memory
	Integer[][] memory;
    public int minimumTotal(List<List<Integer>> triangle) {
    	memory = new Integer[triangle.size()][triangle.size()];
		//	开始递归（将大三角划分为：顶点 + 2个子三角）
		return dfs(triangle,1, 1);	//	从第一个数，开始迭代

    }
	//	i: 行数 1 -【triangle.size()】
	//	j：列数 1 - [triangle.get[i].size()]
    public int dfs(List<List<Integer>> triangle, int i, int j){
		//	base case
		if (i == triangle.size()){	//	i 从 1 开始到达底边的时候
			return triangle.get(i - 1).get(j - 1);
		}
		if (memory[i][j] != null){
			return memory[i][j];
		}
		// 开始递归（将大三角划分为：顶点 + 2个子三角）
		return memory[i][j]=triangle.get(i - 1).get(j - 1) + Math.min(dfs(triangle, i + 1, j), dfs(triangle,i + 1, j + 1));
	}
}
```

递推方法：

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
		//	开始递推（自低向上，由已知推算未知）
		for (int i = n - 1; i >= 0; i--) {    //	从末行开始 (i, j)代表未知【第 i 行，第 j 列】，取值的时候要取 (i - 1)(j - 1)]
			for (int j = 0; j <= i; j++) {
				dp[i][j] = triangle.get(i).get(j) + Math.min(dp[i + 1][j], dp[i + 1][j + 1]);
			}
		}
		return dp[0][0];    //	返回第一行第一个
    }
}
```

### 3.2 DP

- 每个大问题的子问题都是最优的，所以才可以直接记录下来
- 在下次寻找子问题的最优解时，直接使用

与分治算法不同的是：

- 适合 dp 请求的问题，经分解得到的子问题往往不是互相独立的
- 即下一个子阶段的求解是建立在上一个子阶段的解的基础上，进行进一步求解

#### 1. 最优子结构

原问题的最优解包含子问题的最优解

#### 2. 状态转移方程

- 写状态转移方程的程序，一般以递推的方式实现
- 虽然时间复杂度和记忆化搜索一样
- 而且在循环中会计算一些无意义的节点
- 但是递推免去了调用时进栈出栈的操作，而且避免了在递归时层数过多时栈溢出的情况

$$
f(i,j)=max\left \{ f(i-1,j) ,f(i-1,j-w(i))+v(i)\right \}
$$

#### 3. 手办问题

| 手办名     | 价格 | 喜欢程度 |
| ---------- | ---- | -------- |
| 兵长       | 10   | 24       |
| 蕾姆       | 4    | 9        |
| 小埋       | 4    | 9        |
| 和泉纱雾   | 5    | 10       |
| 空条承太郎 | 3    | 2        |

你现在 有 <font color="red">**13**</font> 元

##### 3.1 贪心算法

- 算出每个物品的性价比
- 优先买性价比高的物品

| 手办名     | 价格                           | 喜欢程度 | 性价比 |
| ---------- | ------------------------------ | -------- | ------ |
| 兵长       | <font color="yellow">10</font> | 24       | 2.4    |
| 蕾姆       | 4                              | 9        | 2.25   |
| 小埋       | 4                              | 9        | 2.25   |
| 和泉纱雾   | 5                              | 10       | 2      |
| 空条承太郎 | <font color="yellow">3</font>  | 2        | 0.67   |

最终的喜欢程度为：24 + 2 = 26

##### 3.2 0 - 1 背包问题

| 背包问题                         | 说明             |
| -------------------------------- | ---------------- |
| 0-1 背包                         | 每个物品最多一个 |
| 完全背包 ==> 可以转化成 0-1 背包 | 每种物品无限件   |

手办是一个整体

- 你不能只买🐻和大腿，按照比例给钱，所以不能用贪心来解决

###### 普通递归实现

```bash
# f(i, j)：当还剩 j 元钱时，前 i 个物品能达到的最大喜欢值

# max【不买这个商品的价值，买这个商品的价值】
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
        return Math.max(f(i - 1, j), f(i - 1, j - price[i - 1]) + value[i - 1]);	//	下标从 0 开始
    }
}
```

###### 递推（记忆化数组）

那么，这个 0/1 背包问题是否满足 <font color="yellow">最优子结构 </font>呢???

能不能用<font color="yellow"> 记忆化搜索</font> 来优化这一算法???

| 已知条件                               |                |
| -------------------------------------- | -------------- |
| $n$ 个物品                             | $T$ 元钱       |
| $W_{i}$ 价格                           | $V_{i}$ 喜欢值 |
| $X_{i}\epsilon \left \{ 0,1 \right \}$ | 是否买         |

这样问题可以转化为：
$$
max\sum_{i=1}^{n}V_{i}X_{i}\left ( \sum_{i=1}^{n}W_{i}X_{i}\leqslant T \right )
$$
证明：上述公式是具有 <font color="red">**最优子结构** </font>的。

- 设 $\left ( y_{1},y_{2}……y_{n} \right )$ 是 $max\sum_{i=1}^{n}V_{i}X_{i}$ 最优解
- 那么 $\left ( y_{2},y_{3}……y_{n} \right )$ 是子问题 $max\sum_{i=2}^{n}V_{i}X_{i}\left ( \sum_{i=2}^{n}W_{i}X_{i}\leqslant T- W_{1}X_{1}\right )$
- 使用<font color="yellow"> **反证法**</font>
  - 假设子问题存在 $\left ( Z_{2},Z_{3}……Z_{n} \right )$ 是最优解，那么 $\left ( y_{2},y_{3}……y_{n} \right )$ 就不是它的最优解
  - 则：$\sum_{i=2}^{n}V_{i}y_{i}\leqslant \sum_{i=2}^{n}V_{i}Z_{i}$
  - 两边同时加上 $V_{1}y_{1}$
  - $\sum_{i=2}^{n}V_{i}y_{i}+V_{1}y_{1}\leqslant \sum_{i=2}^{n}V_{i}Z_{i}+V_{1}y_{1}$ ===> 得出 $\left ( y_{1},y_{2}……y_{n} \right )$ 并不是大问题的最优解，最优解是 $\left ( y_{1},Z_{2}……Z_{n} \right )$
  - 就与我们最开始的设定相矛盾了
- 证明这个问题是有最优子结构的

使用记忆化搜索优化递推：

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

下面用递推实现：

从已知 ---> 未知

- 即：物品数从1个开始计算
- 同时利用了数组未赋值时，初始值是0【我有 13 块钱，但是数组下标是从 0 开始的哦，所以大小要开成 14】

```java
package com.alq.dp;

public class Bag {
    static int[]  price = new int[]{10, 4, 4, 5, 3};
    static int[] value = new int[]{24, 9, 9, 10, 2};
    //  记忆数组 dp，第一个角标表示物品，第二个角标表示我们有多少钱
    //  防止数组越界，在开数据空间时，增加一个空间
    static int[][] dp = new int[6][14];
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
                    //	状态转移方程
                    dp[k][l] = Math.max(dp[k - 1][l], dp[k - 1][l - price[k - 1]] + value[k - 1]);
                }
            }
        }
        return dp[i][j];
    }
}
```

我们发现：当状态转移时

- 当前状态只与上个状态有关
- 只在 i - 1 或 i + 1 这一行中取需要的值
- 再前面状态的值便没有用了

定义一个数组，存放放入物品信息

| 手办名     | 价格 | 喜欢程度 |
| ---------- | ---- | -------- |
| 兵长       | 10   | 24       |
| 蕾姆       | 4    | 9        |
| 小埋       | 4    | 9        |
| 和泉纱雾   | 5    | 10       |
| 空条承太郎 | 3    | 2        |

![image-20230425103347747](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251033061.png)

然后逐步向上查询：

![image-20230425104234188](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251042354.png)

```java
int i = path.length - 1;
int j = path[0].length - 1;
while (i > 0 && j > 0){
    if (path[i][j] == 1){
        System.out.println("第 " + i +  " " + j + "个被选中");
        j -= price[i - 1];
    }
    i--;
}
```



```java
package com.alq.dp;

public class Bag {
    static int[]  price = new int[]{10, 4, 4, 5, 3};
    static int[] value = new int[]{24, 9, 9, 10, 2};
    //  记忆数组 dp，第一个角标表示物品，第二个角标表示我们有多少钱
    //  防止数组越界，在开数据空间时，增加一个空间
    static int[][] dp = new int[6][14];

    static int[][] path = new int[6][14];


    public static void main(String[] args) {
        System.out.println(f(5, 13));
        for (int[] ints : path) {
            for (int anInt : ints) {
                System.out.print(anInt + "\t");
            }
            System.out.println();
        }

        System.out.println("==================");


        for (int[] ints : dp) {
            for (int anInt : ints) {
                System.out.print(anInt+"\t");
            }
            System.out.println();
        }
        //  输出我们最后放入的是那些物品
        int i = path.length - 1;
        int j = path[0].length - 1;
        while (i > 0 && j > 0){
            if (path[i][j] == 1){
                System.out.println("第 " + i +  " " + j + "个被选中");
                j -= price[i - 1];
            }
            i--;
        }
    }

    public static int f(int i, int j){    // 当还剩 j 元钱时，前 i 个物品能达到的最大喜欢值
        //  递推实现
        //  遍历记忆数组
        for (int k = 1; k <= i ; k++) {    //  物品个数
            for (int l = j; l >= 0 ; l--) {   //  拥有的钱
                if ( l < price[k - 1]){
                    dp[k][l] = dp[k - 1][l];    //  买不起当前物品 【数组，未赋值时，默认值是 0】
                }else {
                    //	状态转移方程
                    // dp[k][l] = Math.max(dp[k - 1][l], dp[k - 1][l - price[k - 1]] + value[k - 1]);
                    if (dp[k - 1][l] < dp[k - 1][l - price[k - 1]] + value[k - 1]){ //  买
                        dp[k][l] = dp[k - 1][l - price[k - 1]] + value[k - 1];
                        //  将当前情况记录到 path
                        path[k][l] = 1;
                    }else {
                        dp[k][l] = dp[k - 1][l];
                    }
                }
            }
        }
        return dp[i][j];
    }
}
```



##### 滚动数组

于是空间复杂度可以优化为：

​	$O(n*t)=O(t)$

## 4. KMP算法（字符串搜索算法）

利用之前判定过的信息，通过一个 next 数组，保存模式串前后最长公共子序列长度，每次回溯时，通过 next 数组找到，前面匹配过的位置，省去了大量的计算时间

![image-20230427131240837](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271312942.png)

这种情况下，原来<font color="yellow">绿色部分</font>就<font color="yellow">不能用了</font>，显然，我们想要缩小绿色的部分，还要满足最长公共子串的要求，也就像下面这样：

![image-20230427131259101](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271312200.png)

这个蓝色的部分，首先要<font color="yellow"> 内容相同</font>，其次还要<font color="yellow">位于深绿色部分的开头和结尾</font>，这不就是**深绿色部分的最长公共前缀子串**吗

即：更新 $j$ $=$ $next[j-1]$

- i：next 数组下标，从 1 开始
- j：最长公共前后缀，从 0 开始

```java
package com.kmp;

public class KMP {
    static List<Integer> list = new ArrayList<>();

    public static void main(String[] args) {
        String str1 = "BBC ABCDAB ABCDABCDABDEdsafeABCDABD";
        String str2 = "ABCDABD";

        int[] next = kmpNext(str2);
        kmpSearch(str1, str2, next);
        System.out.println(list);
    }

    public static List<Integer> kmpSearch(String str1, String str2, int[] next){
        int m = str2.length();

        //  遍历
        for (int i = 0, j = 0; i < str1.length(); i++) {
            while (j > 0 && str1.charAt(i) != str2.charAt(j)){  //  如果不等了，就一直向前推进，找到上一个相等的位置
                j = next[j - 1];
            }

            if (str1.charAt(i) == str2.charAt(j)){
                j++;
            }

            if (j == m){
                list.add(i - m + 1);
                j = 0;  //  j 重置【匹配上了之后，j 重头开始匹配】
            }
        }
        return list;
    }

    //  获得一个模式串的 next 数组
    public static int[] kmpNext(String dest) {
        int[] next = new int[dest.length()];
        next[0] = 0;
        for (int i = 1, j = 0; i < dest.length(); i++) {
            while (j > 0 && dest.charAt(i) != dest.charAt(j)){
                j = next[j - 1];
            }

            if (dest.charAt(i) == dest.charAt(j)) {
                next[i] = ++j;
            }
        }
        return next;
    }
}
```



问题引出：找到 B 在 A 中的位置

![image-20230425110232870](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251102958.png)

暴力解法：$(n*m)$

- 每次匹配失败后
- i，j 都回溯，从头开始匹配【并没有从上次失败中，获取信息，并总结经验教训】

| KMP算法来源                                   |      |
| --------------------------------------------- | ---- |
| D.E.<font color="yellow">K</font>unth【美国】 | K    |
| J.H.<font color="yellow">M</font>orris        | M    |
| V.R.<font color="yellow">P</font>ratt         | P    |

### 1. KMP算法核心

1. 利用匹配失败后的信息
2. 尽量减少模式串（B）与主串（A）的匹配次数
3. 以达到快速匹配的目的
4. 通过一个 <font color="yellow">next 数组</font>，保存模式串（B）中 <font color="yellow">前后最长公共子序列的长度</font>，每次回溯时，通过 next 数组找到，前面匹配过的位置，省去了大量的计算时间



### 2. 如何减少匹配次数

字符串的前缀和后缀

比如：字符串 ababacb

| 前缀集合                                        | 后缀集合                                        |
| ----------------------------------------------- | ----------------------------------------------- |
| $\left \{ a,ab,aba,abab,ababa,ababac \right \}$ | $\left \{ b,cb,acb,bacb,abacb,babacb \right \}$ |

- 暴力算法中：每次匹配失败后，ij都回溯，从头开始匹配
- KMP算法：不回溯 $i$，<font color="yellow">**只回溯**</font> $ j$ 到指定位置

当我们发现字符匹配失败时，这个失败的信息

1. 并不单单只代表失败，同时也代表着

   - 除了当前这 2个 字符不匹配

   - 前面的 j 个字符都匹配成功了

     ![image-20230425111737694](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251117784.png)

2. 就需要将 B 串后移（这时候有选择地移，移到有可能成功地位置）

   - B 串移动到 <font color="yellow">i 之前</font>

     - 需要满足 A后缀 与 B前缀 有交集时

       ![image-20230425112456735](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251124829.png)

       ![image-20230425115009063](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251150166.png)

     - 这时候，只需要将 j 回溯，然后再从 i 处开始匹配

       ![image-20230425115233431](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251152538.png)

       那么这个 B 串要后移多少位？【 j 指针回溯到哪个位置 ？】

       1. 找到 A 字串后缀 与 B字串前缀 的交集里：最长的那个元素。才能使B串后移得最少，且在已知信息中匹配的最多
       2. 最长元素的长度：就是 <font color="yellow">j</font> 指针 <font color="yellow">回溯的位置</font>
          - 前面讲过：<font color="red">匹配失败 </font>这个信息，代表着：<font color="yellow">前面的 j 个字符</font>是匹配成功了的，也即是说：<font color="yellow">A 子串与B子串是相同的</font>
          - 那么 “找到 A 子串后缀 与 B子串前缀的交集” <font color="red">等价于</font> “找到 B 子串后缀 与 B子串前缀的交集”
          - j 指针回溯的位置 = B 子串的最长公共前后缀
       3. 通过隐藏信息（匹配失败时，A串与B串存在着一段相同的子串），我们可以认为：
          - j 指针回溯的位置，<font color="yellow">只与 B 串有关</font>，与 A串无关
          - 在 A、B字符串匹配之前，就通过 B 串计算回溯位置，存在<font color="yellow">一个数组 next</font> 里【<font color="green">关键</font>】
          - 这个 next 与 B串形成映射数组，存的数据 next[ i] 就是 B[1] ~ B[i] 的最长公共前后缀的长度

   - B 串移动到 <font color="yellow">i 之后</font> 【即：B 没有最长公共前后缀】，肯定就不用回溯 i 了

### 3.  A【主串】、B【模式串】字符串匹配步骤

$i,j$ 初始化为 0

1. 如果 $A[i+1]=B[j+1]$，$i++$，$j++$ 继续匹配
2. 如果 $A[i+1]!=B[j+1]$
   - 回溯 $j$ 到 $next[j]$，直到$A[i+1]=B[j+1]$
   - 这里有一种情况（B串需要移动到 i 后面）
     - $j$ 回溯到 0 时也不能满足使 $A[i+1]=B[j+1]$
     - 当 $j=0$ 时，忽略$j$，增加 $i$，直到 $A[i+1]=B[j+1]$ （注意：只有一个元素时：它没有最长公共前后缀 ---> j = 0）
3. 当 j = m （B串完全匹配）时，输出位置，继续匹配（因为后面可能还有匹配的子串）

### 4. 构建 next 数组

注意：当只有一个元素，它没有最长公共前后缀

![image-20230425125113313](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251251399.png)

![image-20230425125125654](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304251251748.png)



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

可以看出 KMP 算法的时间复杂度是线性的，即：$O(m+n)$

关于 j 如何回溯，<font color="yellow">模式串（B）整体都是向前移的</font>，只要模式串 <font color="yellow">移到主串（A）的尾部</font>，算法就 <font color="yellow">结束 </font>了

KMP 一次比较只会产生 2种结果：

1. 匹配成功主串的指针（i）向前移动
2. 匹配失败模式串（B）整体向前移动

![image-20230427100927101](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271009265.png)



## 5. 贪心算法（greedy algorithm）

得到的结果不一定是最优的

### 1. 集合覆盖

| 广播台 | 覆盖地区         |
| ------ | ---------------- |
| K1     | 北京、上海、天津 |
| K2     | 广州、北京、深圳 |
| K3     | 成都、上海、杭州 |
| K4     | 上海、天津       |
| K5     | 杭州、大连       |

贪心策略：

1. 遍历所有电台，找到一个覆盖了最多 <font color="red">**未覆盖的地区**</font> 的电台【可能包括一些已覆盖的地区】
2. 将这个电台放入到一个集合中（比如：ArrayList），想办法把该电台覆盖的地区在下次比较时去掉
3. 重复第 1 步直到覆盖了全部的地区

![image-20230427165627223](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271656383.png)

![image-20230427165841072](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271658207.png)

![image-20230427170000829](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271700970.png)

![image-20230427170103726](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304271701854.png)



```java
package com.greedy;

public class Greedy {

    static Set<String> allAreaSet = new HashSet<>();   //  存储剩余的 城市

    static HashMap<String, Set<String>> map = new HashMap<>();

    static List<String> listFinalKey = new ArrayList<>();

    public static void main(String[] args) {

        Set<String> set1 = new HashSet<>();
        set1.add("北京");
        set1.add("上海");
        set1.add("天津");
        Set<String> set2 = new HashSet<>();
        set2.add("广州");
        set2.add("北京");
        set2.add("深圳");
        Set<String> set3 = new HashSet<>();
        set3.add("成都");
        set3.add("上海");
        set3.add("杭州");
        Set<String> set4 = new HashSet<>();
        set4.add("上海");
        set4.add("天津");

        Set<String> set5 = new HashSet<>();
        set5.add("杭州");
        set5.add("大连");

        map.put("K1", set1);
        map.put("K2", set2);
        map.put("K3", set3);
        map.put("K4", set4);
        map.put("K5", set5);

        for (Set<String> value : map.values()) {
            for (String s : value) {
                allAreaSet.add(s);
            }
        }
        System.out.println(allAreaSet);

        List<String> k_list = getK_List(map, allAreaSet);
        System.out.println(k_list);

        System.out.println(allAreaSet);


    }

    public static int getNum(String K, Set<String> set){
        int count = 0;
        Set<String> listK = map.get(K);
        for (String s : listK) {
            if (set.contains(s)){
                count++;
            }
        }

        return count;
    }

    public static void remove(String K, Set<String> allAreaSet){
        Set<String> list_K = map.get(K);

        for (String s : list_K) {
            allAreaSet.remove(s);
        }
    }

    public static List<String> getK_List(Map<String, Set<String>> map, Set<String> allAreaSet) {

        while (allAreaSet.size() != 0){    //  城市集合不为空
            ArrayList<Integer> list = new ArrayList<>();

            Map<String, Integer> countMap = new HashMap<>();

            for (String s : map.keySet()) {
                int num = getNum(s, allAreaSet);
                list.add(num);
                countMap.put(s, num);
            }
            Collection<Integer> values = countMap.values();
            Integer max = Collections.max(values);  //  找到要处理的是哪个 key ！！！

            StringBuilder sb = new StringBuilder();

            for (Map.Entry<String, Integer> stringIntegerEntry : countMap.entrySet()) {
                if (stringIntegerEntry.getValue() == max){
                    sb.append(stringIntegerEntry.getKey());
                    break;
                }
            }

            String area =  sb+"";

            listFinalKey.add(area);

            remove(area, allAreaSet);
        }
        return listFinalKey;
    }
}
```

## 6. 最短路径算法

|              | 时间复杂度                                                   | 适用场景                     | 优化            |
| ------------ | ------------------------------------------------------------ | ---------------------------- | --------------- |
| Dijkstra     | 基于顶点求单源最短路<br />n个顶点、m条边<br />算法时间复杂度为：$O(n^{2})$ | 适合点少边多的无负权边稠密图 | 优先队列-堆优化 |
| Bellman-ford |                                                              |                              | 队列优化-SPFA   |
|              |                                                              |                              |                 |



### 1. Dijkstra：无负权边的单源最短路【BFS搜索思想】

<font color="yellow">**单源最短路**</font>：从某一点出发，到其它所有点的最短路径

利用贪心策略，与 prim 算法异曲同工

实现步骤：

1. 设一个集合：保存已经找到的最短路的顶点，先将起点加入集合

2. 将起点到所有点的距离存在 dis 数组中，不能直达的点存为无穷大

3. 在 dis 数组中找一个最小值，当前值一定是起点到该点的最短路，将该点加入集合

4. 遍历其它点，如果它们通过这个最短的点作为中转，比源点直接到达短，就替换这些顶点到源点的 dis 值（松弛操作）

   ```java
   if dis[i] > dis[min] + map[min, i]{
       dis[i] = dis[min] + map[min, i] 
   }
   ```

5. 又从 dis 中找出最小值，重复上面的操作，直至 T 集合包含所有顶点

![jjii](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305042039264.png)

为什么不能求有负边的图的最短路？？？

![image-20230504204325944](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305042043071.png)

代码实现：

我们需要构建 3 个数组

1. already_arr：已经访问过的顶点
2. dis：出发顶点到其它顶点的距离【最终最短距离会放在这里】
3. pre_visited：前驱顶点

![image-20230505110826277](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305051108491.png)

初始化出发点：

1. 更新出发点到周围节点的距离
2. 更新周围节点的 pre

![image-20230505121338729](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305051213855.png)

注意：在图中只有一个出发节点，其它的都叫做访问节点

![image-20230505121422571](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305051214717.png)

```java
package com.alq.dijkstra;

public class DijkstraAlgorithm {

    public static void main(String[] args) {
        char[] vertex = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        int[][] matrix = new int[vertex.length][vertex.length];
        final int N = 65535;

        matrix[0] = new int[]{N, 5, 7, N, N, N, 2};
        matrix[1] = new int[]{5, N, N, 9, N, N, 3};
        matrix[2] = new int[]{7, N, N, N, 8, N, N};
        matrix[3] = new int[]{N, 9, N, N, N, 4, N};
        matrix[4] = new int[]{N, N, 8, N, N, 5, 4};
        matrix[5] = new int[]{N, N, N, 4, 5, N, 6};
        matrix[6] = new int[]{2, 3, N, N, 4, 6, N};

        Graph graph = new Graph(vertex, matrix);
        graph.showGraph();
        graph.dsj(6);
        int newVertex = graph.visitedVertex.findNewVertex();
        System.out.println("新的访问顶点为：");
        System.out.println(newVertex);
        graph.visitedVertex.show();
        System.out.println();
        graph.visitedVertex.getRoute(6, 3);

    }
}

class Graph {
    char[] vertex;  //  存放节点数据
    int[][] matrix; //  存放边（邻接矩阵）
    VisitedVertex visitedVertex;    //  已经访问的顶点的集合

    public Graph(char[] vertex, int[][] matrix) {
        this.vertex = vertex;
        this.matrix = matrix;
    }

    //  显示图
    public void showGraph() {
        for (int[] ints : matrix) {
            System.out.println(Arrays.toString(ints));
        }
    }

    public void dsj(int index) { //  出发顶点对应下标
        visitedVertex = new VisitedVertex(vertex.length, index);
        update(index);  //  更新 Index 顶点到到周围节点的距离，更新周围节点的前驱

        for (int i = 1; i < vertex.length; i++) {   //  挑选剩余的 n - 1 个顶点
            index = visitedVertex.findNewVertex();
            update(index);  //  再逐个更新 新的访问节点到其余为访问过节点的距离
        }
    }

    //  更新操作
    //  1. 更新 index 下标原点到周围节点的距离
    //  2. 更新周围顶点的 pre
    private void update(int index) {
        int len = 0;
        for (int i = 0; i < matrix[index].length; i++) {
            len = visitedVertex.getDis(index) + matrix[index][i];
            if (!visitedVertex.isVisited(i) && len < visitedVertex.getDis(i)) {  //  dis 初始距离为：65535
                visitedVertex.updateDis(i, len);   //  更新 dis
                visitedVertex.updatePre(i, index);  //  更新 pre
            }
        }
    }

}

class VisitedVertex {
    public int[] dis;
    public int[] already_arr;
    public int[] pre_visited;

    public VisitedVertex(int n, int index) {    //  index：出发点下标
        this.dis = new int[n];
        this.already_arr = new int[n];
        this.pre_visited = new int[n];
        //  初始化 dis 数组
        Arrays.fill(dis, 65535);
        this.dis[index] = 0;    //  设置出发顶点的访问距离为 0
        //  将出发顶点标记为已经访问
        this.already_arr[index] = 1;
    }

    //  判断顶点是否被访问过
    public boolean isVisited(int index) {
        return already_arr[index] == 1;
    }

    //  更新出发顶点到 index 顶点的距离
    public void updateDis(int index, int len) {
        this.dis[index] = len;   //  出发点初始到其它点的距离为：65535
    }

    //  更新前驱
    public void updatePre(int pre, int index) {  //  更新 pre 这个顶点的前驱顶点为 index 节点
        this.pre_visited[pre] = index;
    }

    //  返回出发顶点到 index 顶点的距离
    public int getDis(int index) {
        return dis[index];
    }

    //  选择新的访问节点
    public int findNewVertex() {
        int min = 65535;
        int index = 0;
        for (int i = 0; i < dis.length; i++) {
            if (already_arr[i] == 0 && min > dis[i]) {   //  找出未被访问过的距离最小的节点
                min = dis[i];
                index = i;
            }
        }
        already_arr[index] = 1;
        return index;
    }
    //  显示最后结果
    public void show(){
        System.out.println("==============");
        for (int di : dis) {
            System.out.print(di + "\t");
        }
        System.out.println();
        for (int i : already_arr) {
            System.out.print(i + "\t");
        }
        System.out.println();
        for (int i : pre_visited) {
            System.out.print(i + "\t");
        }
    }

    public void getRoute(int start, int end){
        int index = end;
        StringBuilder sb = new StringBuilder();
        sb.append(end+"");
        while (end != start){   //  根据终点反推到出发点 start，最后反转就是正向顺序了
            sb.append(" " + pre_visited[end]);
            end = pre_visited[end];
        }
        System.out.println("最短路径如下：");
        System.out.println(sb.reverse().toString());
        System.out.println("最短距离为：" + dis[index]);
    }
}
```



### 2. Bellman-ford：可以求带负权边的图的单源最短路

并且而已判断是否存在负环

- 存在负环的图求最短路径是没有意义的
- 因为它的路径权会无限缩小

对边进行松弛操作，一般采用邻接表或是前向星存储，如果是稠密图，还可以改为邻接矩阵，具体还可以看不同的情况

```bash
# [第 i 条边]
e[i].x：边的起点
e[i].y：边的终点
e[i].z：边的权值
```

步骤：

1. 将除起点外所有的顶点到起点的距离数组 dis 初始化为无穷大
2. 迭代：遍历图中的每一条边，对每条边的 2 个顶点进行松弛操作，直到不能再松弛
3. 判断负环：如果迭代超过 n - 1 次后，还能继续松弛，则存在负环        

核心代码如下：

```java
//	一共 n 个顶点
//	如果图中不存在负环，那么某点到另一点的最短路最多有 (n - 1) 条边
//	有负环的图求最短路就无意义了
for k = 1 to n - 1 do	
    for i = 1 to m do	//	枚举 m 条边，对每条边进行松弛操作
        if dis[e[i].y] > dis[e[i].x] + e[i].z
            dis[e[i].y] = dis[e[i].x] + e[i].z
```

时间复杂度：$O(n * m)$

优化：

若在某一次迭代k后，没有点进行松弛操作

则代表已经找出了所有的最短路

迭代结束，直接跳出循环

### 3. Floyd【本质是动态规划】：实现 3层 for 循环！！！

可以求带负权边，但不带负环图的多源最短路

<font color="yellow">**多源最短路**</font>：整个图的每个点到其它所有点的最短路

思路：

任意节点 i --> j 的最短路有 2 种可能：

1. 直接 i 到 j
2. 从 i 经过若干节点 k 到 j

核心代码：

```java
for k = 1 to n do
    for i = 1 to n do
       for j = 1 to n do
          if  f[i, j] > f[i, k] + f[k, j];
			f[i, j] = f[i, k] + f[k, j]
```

时间复杂度：$O^{3}$

```bash
f(k, i, j) # 表示 i -> j 之间可以通过 1...k 顶点的最短路径
# 当 k = 0，原图：f(0, i, j)
```

状态转移方程：
$$
f(k, i,j)=min\left\{\begin{matrix}
 f(k-1,i,j)& i->j 不经过k点 & \\ 
 f(k-1,i,k)+f(k-1,k,j)&i->j经过k点  & 
\end{matrix}\right.
$$
我们发现这个状态转移方程  的 k 这一维空间是可以省略的

因为当前状态 k 只与 k - 1 这一层状态有关，所以可以将空间进行压缩
$$
f(i,j)
$$
我们在求 k 这一层状态之前，一定得把 k - 1 的所有状态全部求出来，才能推导出 k 这一层状态 

![image-20230505193356414](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305051933569.png)

代码实现：

![jjq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305052012533.png)

```java
package com.alq.floyd;

public class FloydAlgorithm {
    static List<Integer> list = new ArrayList<>();

    public static void main(String[] args) {
        char[] vertex = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        int[][] matrix = new int[vertex.length][vertex.length];
        final int N = 65535;
        matrix[0] = new int[]{0, 5, 7, N, N, N, 2};
        matrix[1] = new int[]{5, 0, N, 9, N, N, 3};
        matrix[2] = new int[]{7, N, 0, N, 8, N, N};
        matrix[3] = new int[]{N, 9, N, 0, N, 4, N};
        matrix[4] = new int[]{N, N, 8, N, 0, 5, 4};
        matrix[5] = new int[]{N, N, N, 4, 5, 0, 6};
        matrix[6] = new int[]{2, 3, N, N, 4, 6, 0};

        Graph graph = new Graph(vertex, matrix);
        graph.show();
        System.out.println("===============");
        graph.floyd();
        graph.show();
        List<Integer> path = graph.getPath(0, 3);
        System.out.println(path);

    }
}

class Graph{
    private char[] vertex;
    private int[][] dis;
    private int[][] pre;

    public Graph(char[] vertex, int[][] matrix) {
        this.vertex = vertex;
        this.dis = matrix;
        this.pre = new int[vertex.length][vertex.length];
        //  对 pre 初始化 ---> 初始 pre 都是本身
        for (int i = 0; i < vertex.length; i++) {
            Arrays.fill(pre[i], i);
        }
    }
    //  显示 dis 和 pre
    public void show(){
        for (int[] di : dis) {
            System.out.println(Arrays.toString(di));
        }
        System.out.println("===============");
        for (int[] ints : pre) {
            System.out.println(Arrays.toString(ints));
        }
    }

    public void floyd(){
        int len;
        for (int k = 0; k < vertex.length; k++) {   //  中间
            for (int i = 0; i < vertex.length; i++) {   //  start
                for (int j = 0; j < vertex.length; j++) {   //  end
                    len = dis[i][k] + dis[k][j];
                    if (dis[i][j] > len){   //  如果是直连的话，就不动【2点之间线段最短】
                        dis[i][j] = len;
                        //  pre[k][j] ：从 i 到 j 时，j 的前一步，不能用 k，因为 pre[k][j]不一定等于k
                        //  它等于从 k 到 j 的 j 的前一步，k 到 j 中间可能经过了其它节点
                        pre[i][j] = pre[k][j];  //  更新 pre
                    }
                }
            }
        }
    }

    public List<Integer> getPath(int start, int end){
        List<Integer> list =  new ArrayList<>();
        while (end != start){
            list.add(end);
            end = pre[start][end];
        }
        return list;
    }
}
```

![image-20230506205941035](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305062059225.png)

从这里也可以更好理解：

- 将 j 的前驱更新为 k

```java
 pre[i][j] = pre[k][j];  //  更新 pre
```

画图帮助分析：

![flo](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305071124727.png)

### 4. Dijkstra VS Floyd

|          | 差异                                                         |
| -------- | ------------------------------------------------------------ |
| Dijkstra | 计算某个顶点（<font color="yellow">**出发点**</font>）到其它顶点的最短路径 |
| Floyd    | 计算图中各个顶点之间的最短路径                               |

 

## 7. 骑士周游

![image-20230507115440905](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305071154020.png)

1. DFS的应用
2. 如果使用回溯（就是DFS未能解决），就只能回退，查看其它路径，就在棋盘上不停的回溯

骑士周游问题实现：

1. 创建棋盘 chessBoard，是一个二维数组
2. 将当前位置设为已经访问，然后根据当前位置，计算马儿还能走哪些位置，并放入到一个集合中（ArrayList），最多有 8 个位置，每走一步，就使用 step + 1
3. 遍历 ArrayList 中存放的所有位置，看看哪个可以走通
   - 如果走通，就继续
   - 走不通，就回溯
4. 判断马儿是否完成了任务，使用 step 和应该走的步骤比较，如果没有达到数量，则表示没有完成任务，将棋盘位置置为 0



注意：马儿不同的走法（策略），会得到不同的结果，效率也会有影响【优化！！！】

```java
package com.horse;

public class HorseChessboard {
    //  二维数组【左上角：(0, 0)，右下脚(X - 1，Y - 1)】
    static int X = 8;   //  行
    static int Y = 8;   //  列

    static boolean[][] isVisited= new boolean[X][Y]; //  记录某个位置是否被访问过

    static boolean finished;    // 使用一个属性，标记棋盘的所有位置都被访问

    static int count = 0;   //  同一个位置出发，可以有多个解，但是递归的次序，最后只是最后的答案

    public static void main(String[] args) {
        int row = 0;
        int col = 0;
        int[][] chessBoard = new int[X][Y];
        traversalChessboard(chessBoard, row, col, 1);
        for (int[] ints : chessBoard) {
            System.out.println(Arrays.toString(ints));
        }
    }


    public static ArrayList<Point> getNext(Point curPoint){
        ArrayList<Point> ps = new ArrayList<>();
        Point point = new Point();
        if ((point.x = curPoint.x - 2) >= 0 &&(point.y = curPoint.y - 1) >= 0){   //  5
            ps.add(new Point(point.x, point.y));
        }
        if ((point.x = curPoint.x - 2) >= 0 &&(point.y = curPoint.y + 1) < Y){   //  4
            ps.add(new Point(point.x, point.y));
        }if ((point.x = curPoint.x - 1) >= 0 &&(point.y = curPoint.y - 2) >= 0){   //  6
            ps.add(new Point(point.x, point.y));
        }
        if ((point.x = curPoint.x - 1) >= 0 &&(point.y = curPoint.y + 2) < Y){   //  3
            ps.add(new Point(point.x, point.y));
        }if ((point.x = curPoint.x + 1) < X &&(point.y = curPoint.y - 2) >= 0){   //  7
            ps.add(new Point(point.x, point.y));
        }
        if ((point.x = curPoint.x + 1) < X &&(point.y = curPoint.y + 2) < Y){   //  2
            ps.add(new Point(point.x, point.y));
        }
        if ((point.x = curPoint.x + 2) < X &&(point.y = curPoint.y - 1) >= 0){   //  0
            ps.add(new Point(point.x, point.y));
        }
        if ((point.x = curPoint.x + 2) < X &&(point.y = curPoint.y + 1) < Y){   //  1
            ps.add(new Point(point.x, point.y));
        }
        return ps;
    }

    public static void traversalChessboard(int[][] chessBorad, int row, int col, int step){ //  step 初始 = 1
        chessBorad[row][col] = step;
        isVisited[row][col] = true;
        //  获取当前位置可以走的下一个位置的集合
        ArrayList<Point> next = getNext(new Point(row, col));
        //  遍历
        while (!next.isEmpty()){
            Point p = next.remove(0);   //  取出下一个可以走的位置
            if (!isVisited[p.x][p.y]){
                traversalChessboard(chessBorad, p.x, p.y, ++step);
            }
        }
        //  当该点所有的下一步顶点都走完后

        //  说明：step < X * Y 成立的情况有 2种类
        //  1. 棋盘到目前位置，仍然没有走完
        //  2. 棋盘已经走完了，处于回溯过程
        if (step < X * Y && ! finished){
          chessBorad[row][col]  = 0;
          isVisited[row][col] = false;
        }else { //  棋盘处于一个回溯过程
            finished = true;    //  finished 一旦完成，isVisited 数组就不会再改变了！！！
        }
    }
}

class Point{
    int x;
    int y;

    public Point() {
    }

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

使用贪心算法对原来的算法优化

1. 我们获取当前位置，可以走的下一个位置的集合

   ```java
     ArrayList<Point> ps = new ArrayList<>();
   ```

2. 我们需要对 ps 中所有的 Point 的下一步所有集合的数目，进行非递减（可以有相同的数）排序

```java
//  根据档期那这一步的所有下一步的选择位置，进行非递减排序
public static void sortPs(ArrayList<Point> ps){
    ps.sort(new Comparator<Point>() {
        @Override
        public int compare(Point o1, Point o2) {
            return getNext(o1).size() - getNext(o2).size();
        }
    });
}
```



