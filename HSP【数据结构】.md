# HSP【数据结构】

## 1. 线性结构和非线性结构

### 线性

- 顺序存储结构
- 链式存储结构

### 非线性

- 二维数组
- 多维数组
- 广义表
- 树结构
- 图结构

## 2. 稀疏数组和队列

### 2.1 稀疏数组（sparseArr）

当一个数组中大部分元素为0，或者为同一个值的数组时，可以使用稀疏数组来保存该数组。

稀疏数组的处理方法是：

- 记录数组一共有几行几列，有多少个不同的值
- 把具有不同值的元素的行列及值记录在一个 <font color="yellow">小规模的数组 </font>中，从而缩小程序的规模

![image-20221230234528477](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212302345566.png)

![image-20221230235159411](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212302351471.png)

### 2.2 应用实例

1. 使用稀疏数组，保留二维数组
2. 将稀疏数组存盘，并且可以重新恢复原来的二维数组

![image-20221231000213759](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212310002825.png)

二维数组转成稀疏数组思路：

- 遍历原始二维数组，得到有效数据的个数 sum
- 根据 sum 就可以创建稀疏数组 sparseArr int[sum+1]\[3]
- 将二维数组的有效数据存入到稀疏数组

稀疏数组转回原始二维数组：

- 先读取稀疏数组的第一行，根据第一行的数据创建原始的二维数组，比如 chessArr= new int[11]\[11]
- 再读取稀疏数组后几行的数据，并赋给原始的二维数组即可

```java
public class sparseArr {
    static int[][] chessArr = new int[11][11];
    static int row = chessArr.length;
    static int col = chessArr[0].length;
    static int sum = 0; //  有效数字

    public static void main(String[] args) {
        chessArr[1][2] = 1;
        chessArr[2][3] = 2;
        System.out.println("原始二维数组如下：");
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                System.out.print(chessArr[i][j] + " ");
                if (chessArr[i][j] != 0){
                    sum++;
                }
            }
            System.out.println();
        }
        System.out.println();
        System.out.println("稀疏数组如下：");
        int[][] sparseArr = chessToSparseArr(chessArr);
        printArr(sparseArr);

        System.out.println();
        System.out.println("稀疏数组还原回二维数组：");
        int[][] chess = sparseToChess(sparseArr);
        printArr(chess);
    }

    public static int[][] chessToSparseArr(int[][] chessArr){
        int k = 0;  //  记录有效数个数
        int[][] sparseArr = new int[sum + 1][3];
        sparseArr[0] = new int[]{row, col, sum};    //  一维数组
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (chessArr[i][j] != 0){
                    sparseArr[++k] = new int[]{i, j, chessArr[i][j]};   //  将非 0 值传入到 稀疏数组 sparseArr中
                }
            }
        }
        return sparseArr;
    }

    public static int[][] sparseToChess(int[][] sparseArr){
        int m = sparseArr[0][0];
        int n = sparseArr[0][1];
        int[][] chess1 = new int[m][n];
        for (int i = 1; i < sparseArr.length; i++) {
            chess1[sparseArr[i][0]][sparseArr[i][1]] = sparseArr[i][2];
        }
        return chess1;
    }

    public static void printArr(int[][] arr){
        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < arr[i].length; j++) {
                System.out.print(arr[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```

课后练习：

1. 在前面基础上，将稀疏数组保存到磁盘上，比如：map.data
2. 恢复原来的数组时，读取 map.data 进行恢复

### 2.3 队列（普通）

有序列表

- 可以用数组或链表实现
- 先入先出

数组模拟：

![image-20221231101312425](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212311013527.png)

- 入队：rear++
- 出队：front++

当我们将数据存入队列时称为 ”addQueue“，addQueue的处理需要有2个步骤：

1. 将 rear++，当 front == rear 【空】
2. 若 rear < maxSize，则将数据存入 rear 所指的数组元素中，否则无法存入数据
3. rear == maxSize - 1【队列满】

```java
public class ArrayQueueDemo {
    public static void main(String[] args) {
        ArrayQueue arrayQueue = new ArrayQueue(3);
        Scanner scanner = new Scanner(System.in);
        boolean loop = true;
        //  输出一个菜单
        while (loop){
            System.out.println("s(show): 显示队列");
            System.out.println("e(exit): 退出队列");
            System.out.println("a(add): 添加数据到队列");
            System.out.println("g(get): 从队列取出队列");
            System.out.println("p(peek): 查看队列头的数据");
            String s = scanner.next();
            switch (s){
                case "s":
                    arrayQueue.showQueue();
                    break;
                case "a":
                    System.out.println("输出一个数");
                    int value = scanner.nextInt();
                    arrayQueue.addQueue(value);
                    break;
                case "g":
                    try {
                        int queue = arrayQueue.getQueue();
                        System.out.println("取出的数据是: " + queue);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case "p":
                    try {
                        int peek = arrayQueue.peek();
                        System.out.println("队头的数据是: " + peek);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case "e":
                    scanner.close();
                    loop = false;
                    break;
                default:
                    break;
            }
        }
        System.out.println("程序退出~~");
    }
}

class ArrayQueue{
    //  定义属性
    private final int maxSize;
    private int front;
    private int rear;
    private int[] arr;

    public ArrayQueue(int maxSize) {
        this.maxSize = maxSize;
        arr = new int[maxSize];
        front = -1;
        rear = -1;
    }

    //  判断队列是否满了
    public boolean isFull(){
        return rear == maxSize - 1;
    }

    //  判断队列是否为空
    public boolean isEmpty() {
        return rear == front;
    }

    //  添加数据到队列
    public void addQueue(int n){
        if (isFull()){
            System.out.println("队列已满，无法加入!!!");
            return;
        }
        arr[++rear] = n;
    }

    //  获取队列数据，数据出列
    public int getQueue(){
        if (isEmpty()){
            // 通过抛出异常
            throw new RuntimeException("队列空，不能取数据");
        }
        return arr[++front];
    }

    //  显示队列的头数据，注意不是取出数据
    public int peek(){
        if (isEmpty()){
            throw new RuntimeException("队列空，不能取数据");
        }
        return arr[front + 1];
    }

    //  显示队列所有数据
    public void showQueue(){
        if (isEmpty()){
            System.out.println("队列为空，没有数据~~");
            return;
        }
        System.out.print("当前队列为：");
        for (int i = 0; i < rear + 1; i++) {    //  rear 初始值为 -1
            if (i > front){
                System.out.print(arr[i] + " ");
            }
        }
        System.out.println();
    }
}
```

问题分析并优化：

- 目前数组使用一次就不能用了，没有达到复用效果
- 将这个数组使用算法，改进成一个<font color="yellow">环形的队列</font>，取模：<font color="yellow">%</font>

### 2.4 队列（环形）--- 取模方式实现

思路如下：

1. front 变量的含义做一个调整：front 就指向队列的第一个元素，也就是 <font color="yellow">arr[front]</font> 就是队列的 <font color="yellow">第一个元素</font>

   - front 初始值：0

2. rear 变量的含义也做一个调整：rear 指向队列的最后一个元素的后一个位置，因为希望空出一个空间作为约定

   - rear 的初始值：0

3. 当队列满时，条件是（rear+1）% maxsize == front【满】

4. 当队列为空的条件，rear == front

5. 当我们这样分析，队列中有效的数据的个数 $(rear+maxsize-front)\%maxsize$

   ![312e命名绘图](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301030909708.png)

6. 我们就可以在原来的队列上修改得到一个环形队列

```java
public class CircleArrayQueueDemo {
    public static void main(String[] args) {
        CircleArray circleArray = new CircleArray(4);   //  说明：设置4，其队列的有效数据最大是 3
        Scanner scanner = new Scanner(System.in);
        boolean loop = true;
        //  输出一个菜单
        while (loop){
            System.out.println("s(show): 显示队列");
            System.out.println("e(exit): 退出队列");
            System.out.println("a(add): 添加数据到队列");
            System.out.println("g(get): 从队列取出队列");
            System.out.println("p(peek): 查看队列头的数据");
            String s = scanner.next();
            switch (s){
                case "s":
                    circleArray.showQueue();
                    break;
                case "a":
                    System.out.println("输出一个数");
                    int value = scanner.nextInt();
                    circleArray.addQueue(value);
                    break;
                case "g":
                    try {
                        int queue = circleArray.getQueue();
                        System.out.println("取出的数据是: " + queue);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case "p":
                    try {
                        int peek = circleArray.peek();
                        System.out.println("队头的数据是: " + peek);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case "e":
                    scanner.close();
                    loop = false;
                    break;
                default:
                    break;
            }
        }
        System.out.println("程序退出~~");
    }
}

class CircleArray{
    //  定义属性
    private final int maxSize;
    // front 变量的含义做一个调整：front 就指向队列的第一个元素，也就是arr[front]的第一个元素
    // front 的初始值 = 0
    private int front;
    // rear 变量的含义也做一个调整：rear 指向队列的最后一个元素的后一个位置，因为希望空出一个空间作为约定
    // rear 的初始值 = 0
    private int rear;
    private int[] arr;
    public CircleArray(int maxSize) {
        this.maxSize = maxSize;
        arr = new int[maxSize];
    }
    //  判断队列是否满了
    public boolean isFull(){
        return (rear + 1) % maxSize == front;
    }

    //  判断队列是否为空
    public boolean isEmpty() {
        return rear == front;
    }

    //  添加数据到队列
    public void addQueue(int n){
        if (isFull()){
            System.out.println("队列已满，无法加入!!!");
            return;
        }
        arr[rear] = n;
        rear = (rear + 1) % maxSize;    //  防止越界
    }

    //  获取队列数据，数据出列
    public int getQueue(){
        if (isEmpty()){
            // 通过抛出异常
            throw new RuntimeException("队列空，不能取数据");
        }
        int val  = arr[front];
        front = (front + 1) % maxSize;  //  同样避免越界
        return val;
    }

    //  显示队列的头数据，注意不是取出数据
    public int peek(){
        if (isEmpty()){
            throw new RuntimeException("队列空，不能取数据");
        }
        return arr[front];
    }

    //  显示队列所有数据
    public void showQueue(){
        if (isEmpty()){
            System.out.println("队列为空，没有数据~~");
            return;
        }
        System.out.print("当前队列为：");
        for (int i = front; i < front + size(); i++) {
            System.out.print("arr[" + (i % maxSize) +"]= " + arr[i % maxSize] + " ");
        }
        System.out.println();
    }

    //  求出当前队列有效数据的个数
    public int size(){
        return (rear + maxSize - front) % maxSize;
    }
}
```

## 3. 链表（Linked List）介绍

链表是有序的列表，在内存中存储如下：

![image-20230103095919495](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301030959569.png)

小结：

- 链表是以节点的方式来存储，是链式
- 每个节点包含 data 域、next 域：指向下一个节点
- 如图：发现链表的各个节点不一定是连续存放
- 链表分带头节点的链表和没有头节点的链表，根据实际需求来确

### 3.1 单链表

单链表（带头结点）逻辑结构示意图如下：

![image-20230103100532963](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301031005012.png)

### 3.2 单链表应用实例

#### 1. 普通添加

单链表的创建示意图(<font color="red">**添加**</font>)，显示单向链表的分析

![231未命名绘图](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301031508741.png)

```java

public class singleLinkedListDemo {
    public static void main(String[] args) {
        //  进行测试
        HeroNode hero1 = new HeroNode(1, "宋江", "及时雨");
        HeroNode hero2 = new HeroNode(2, "卢俊义", "玉麒麟");
        HeroNode hero3 = new HeroNode(3, "吴用", "智多星");
        HeroNode hero4 = new HeroNode(4, "林冲", "豹子头");

        //  创建一个链表
        SingleLinkedList singleLinkedList = new SingleLinkedList();
        singleLinkedList.add(hero1);
        singleLinkedList.add(hero2);
        singleLinkedList.add(hero3);
        singleLinkedList.add(hero4);
        singleLinkedList.show();
    }
}

class HeroNode{
    public int no;
    public String name;
    public String nickname;
    public HeroNode next;   //  指向下一个节点

    public HeroNode(int no, String name, String nickname) {
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                ", nickname='" + nickname + '\'' +
                '}';
    }
}

//  定义 SingleLinkedList 管理英雄
class SingleLinkedList{
    //  先初始化头节点，头节点不要动，不存放具体的数据
    private HeroNode head = new HeroNode(0, "", "");

    //  添加节点到单向链表
    //  思路：当不考虑编号顺序时
    //  1. 找到当前链表的最后节点
    //  2. 将最后这个节点的 next 指向 新的节点
    public void add(HeroNode heroNode){
        //  因为 head 节点不能动，因此我们需要一个辅助变量 temp
        HeroNode temp = head;
        //  遍历链表，找到最后
        while (temp.next!=null){
            temp = temp.next;
        }
        temp.next = heroNode;
    }

    //  显示链表【遍历】
    public void show(){
        //  判断链表是否为空
        if (head.next == null){
            System.out.println("链表为空");
            return;
        }
        //  因为头节点，不能动，因此我们需要一个辅助变量来遍历
        HeroNode temp = head;
        while (temp.next!=null){
            temp = temp.next;
            System.out.println(temp);
        }
    }
}
```

 #### 2. 按顺序添加

![image_0.06788895745497969](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301031654921.gif)

```java
public class singleLinkedListDemo {
    public static void main(String[] args) {
        //  进行测试
        HeroNode hero1 = new HeroNode(1, "宋江", "及时雨");
        HeroNode hero2 = new HeroNode(2, "卢俊义", "玉麒麟");
        HeroNode hero3 = new HeroNode(3, "吴用", "智多星");
        HeroNode hero4 = new HeroNode(4, "林冲", "豹子头");

        //  创建一个链表
        SingleLinkedList singleLinkedList = new SingleLinkedList();
        singleLinkedList.addByOrder(hero1);
        singleLinkedList.addByOrder(hero4);
        singleLinkedList.addByOrder(hero2);
        singleLinkedList.addByOrder(hero3);
        singleLinkedList.show();
    }
}

class HeroNode{
    public int no;
    public String name;
    public String nickname;
    public HeroNode next;   //  指向下一个节点

    public HeroNode(int no, String name, String nickname) {
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                ", nickname='" + nickname + '\'' +
                '}';
    }
}

//  定义 SingleLinkedList 管理英雄
class SingleLinkedList{
    //  先初始化头节点，头节点不要动，不存放具体的数据
    private HeroNode head = new HeroNode(0, "", "");

    //  添加节点到单向链表
    //  思路：当不考虑编号顺序时
    //  1. 找到当前链表的最后节点
    //  2. 将最后这个节点的 next 指向 新的节点
    public void add(HeroNode heroNode){
        //  因为 head 节点不能动，因此我们需要一个辅助变量 temp
        HeroNode temp = head;
        //  遍历链表，找到最后
        while (temp.next!=null){
            temp = temp.next;
        }
        temp.next = heroNode;
    }

    public void addByOrder(HeroNode heroNode){
        //  因为 head 节点不能动，因此我们需要一个辅助变量 temp
        //  因为是单链表，所以我们找的 temp 是位于添加位置的前一个节点，否则插入不了
        HeroNode temp = head;
        //  遍历链表，找到最后
        if (temp.next == null){
            temp.next = heroNode;
            return;
        }
        while (temp.next!=null){
            if (heroNode.no == temp.next.no) {
                System.out.println("该节点已经存在，无法插入");
                break;
            }
            if (heroNode.no < temp.next.no){
                heroNode.next = temp.next;
                temp.next = heroNode;
                //  如果在中间被拦截了，在结尾处就不用添加了
                break;
            }
            temp = temp.next;
        }

        if (temp.next == null){ //  遍历到结尾，既没有重复的，而且下标也没有比待加入的no 小的，就直接甩到最后
            temp.next = heroNode;
        }
    }

    //  显示链表【遍历】
    public void show(){
        //  判断链表是否为空
        if (head.next == null){
            System.out.println("链表为空");
            return;
        }
        //  因为头节点，不能动，因此我们需要一个辅助变量来遍历
        HeroNode temp = head;
        while (temp.next!=null){
            temp = temp.next;
            System.out.println(temp);
        }
    }
}
```

#### 3. 单链表节点的修改

```java
public class singleLinkedListDemo {
    public static void main(String[] args) {
        //  进行测试
        HeroNode hero1 = new HeroNode(1, "宋江", "及时雨");
        HeroNode hero2 = new HeroNode(2, "卢俊义", "玉麒麟");
        HeroNode hero3 = new HeroNode(3, "吴用", "智多星");
        HeroNode hero4 = new HeroNode(4, "林冲", "豹子头");

        //  创建一个链表
        SingleLinkedList singleLinkedList = new SingleLinkedList();
        singleLinkedList.addByOrder(hero1);
        singleLinkedList.addByOrder(hero4);
        singleLinkedList.addByOrder(hero2);
        singleLinkedList.addByOrder(hero3);
        singleLinkedList.update(new HeroNode(3, "Kobe", "24"));
        singleLinkedList.show();
    }
}

class HeroNode{
    public int no;
    public String name;
    public String nickname;
    public HeroNode next;   //  指向下一个节点

    public HeroNode(int no, String name, String nickname) {
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                ", nickname='" + nickname + '\'' +
                '}';
    }
}

//  定义 SingleLinkedList 管理英雄
class SingleLinkedList{
    //  先初始化头节点，头节点不要动，不存放具体的数据
    private HeroNode head = new HeroNode(0, "", "");

    //  添加节点到单向链表
    //  思路：当不考虑编号顺序时
    //  1. 找到当前链表的最后节点
    //  2. 将最后这个节点的 next 指向 新的节点
    public void add(HeroNode heroNode){
        //  因为 head 节点不能动，因此我们需要一个辅助变量 temp
        HeroNode temp = head;
        //  遍历链表，找到最后
        while (temp.next!=null){
            temp = temp.next;
        }
        temp.next = heroNode;
    }

    public void addByOrder(HeroNode heroNode){
        //  因为 head 节点不能动，因此我们需要一个辅助变量 temp
        //  因为是单链表，所以我们找的 temp 是位于添加位置的前一个节点，否则插入不了
        HeroNode temp = head;
        //  遍历链表，找到最后
        if (temp.next == null){
            temp.next = heroNode;
            return;
        }
        while (temp.next!=null){
            if (heroNode.no == temp.next.no) {
                System.out.println("该节点已经存在，无法插入");
                break;
            }
            if (heroNode.no < temp.next.no){
                heroNode.next = temp.next;
                temp.next = heroNode;
                //  如果在中间被拦截了，在结尾处就不用添加了
                break;
            }
            temp = temp.next;
        }

        if (temp.next == null){ //  遍历到结尾，既没有重复的，而且下标也没有比待加入的no 小的，就直接甩到最后
            temp.next = heroNode;
        }
    }

    //  显示链表【遍历】
    public void show(){
        //  判断链表是否为空
        if (head.next == null){
            System.out.println("链表为空");
            return;
        }
        //  因为头节点，不能动，因此我们需要一个辅助变量来遍历
        HeroNode temp = head;
        while (temp.next!=null){
            temp = temp.next;
            System.out.println(temp);
        }
    }

    //  修改节点的信息，根据 no 编号来修改，即：no 编号不能改
    public void update(HeroNode newHeroNode){
        HeroNode temp = head;
        while (temp.next!=null){
            if (newHeroNode.no == temp.next.no){
                temp.next.name = newHeroNode.name;	//	只要修改成功，temp.next != null
                temp.next.nickname = newHeroNode.nickname;
                System.out.println("修改成功!");
                break;
            }
            temp = temp.next;
        }

        if (temp.next == null){ //  遍历到结尾了，还没有找到
            System.out.println("你要修改的节点不存在，无法修改");
        }
    }
}
```

#### 4. 单链表节点的删除和小结

![image_0.7012214720823009](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301041430065.gif)

```java
public class singleLinkedListDemo {
    public static void main(String[] args) {
        //  进行测试
        HeroNode hero1 = new HeroNode(1, "宋江", "及时雨");
        HeroNode hero2 = new HeroNode(2, "卢俊义", "玉麒麟");
        HeroNode hero3 = new HeroNode(3, "吴用", "智多星");
        HeroNode hero4 = new HeroNode(4, "林冲", "豹子头");

        //  创建一个链表
        SingleLinkedList singleLinkedList = new SingleLinkedList();
        singleLinkedList.addByOrder(hero1);
        singleLinkedList.addByOrder(hero4);
        singleLinkedList.addByOrder(hero2);
        singleLinkedList.addByOrder(hero3);
        singleLinkedList.show();
        singleLinkedList.update(new HeroNode(3, "Kobe", "24"));
        singleLinkedList.show();
        singleLinkedList.delete(4);
        singleLinkedList.show();

    }
}

class HeroNode{
    public int no;
    public String name;
    public String nickname;
    public HeroNode next;   //  指向下一个节点

    public HeroNode(int no, String name, String nickname) {
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                ", nickname='" + nickname + '\'' +
                '}';
    }
}

//  定义 SingleLinkedList 管理英雄
class SingleLinkedList{
    //  先初始化头节点，头节点不要动，不存放具体的数据
    private HeroNode head = new HeroNode(0, "", "");

    //  添加节点到单向链表
    //  思路：当不考虑编号顺序时
    //  1. 找到当前链表的最后节点
    //  2. 将最后这个节点的 next 指向 新的节点
    public void add(HeroNode heroNode){
        //  因为 head 节点不能动，因此我们需要一个辅助变量 temp
        HeroNode temp = head;
        //  遍历链表，找到最后
        while (temp.next!=null){
            temp = temp.next;
        }
        temp.next = heroNode;
    }

    public void addByOrder(HeroNode heroNode){
        //  因为 head 节点不能动，因此我们需要一个辅助变量 temp
        //  因为是单链表，所以我们找的 temp 是位于添加位置的前一个节点，否则插入不了
        HeroNode temp = head;
        //  遍历链表，找到最后
        if (temp.next == null){
            temp.next = heroNode;
            return;
        }
        while (temp.next!=null){
            if (heroNode.no == temp.next.no) {
                System.out.println("该节点已经存在，无法插入");
                break;
            }
            if (heroNode.no < temp.next.no){
                heroNode.next = temp.next;
                temp.next = heroNode;
                //  如果在中间被拦截了，在结尾处就不用添加了
                break;
            }
            temp = temp.next;
        }

        if (temp.next == null){ //  遍历到结尾，既没有重复的，而且下标也没有比待加入的no 小的，就直接甩到最后
            temp.next = heroNode;
        }
    }

    //  显示链表【遍历】
    public void show(){
        //  判断链表是否为空
        if (head.next == null){
            System.out.println("链表为空");
            return;
        }
        //  因为头节点，不能动，因此我们需要一个辅助变量来遍历
        HeroNode temp = head;
        while (temp.next!=null){
            temp = temp.next;
            System.out.println(temp);
        }
    }

    //  修改节点的信息，根据 no 编号来修改，即：no 编号不能改
    public void update(HeroNode newHeroNode){
        HeroNode temp = head;
        while (temp.next!=null){
            if (newHeroNode.no == temp.next.no){
                temp.next.name = newHeroNode.name;
                temp.next.nickname = newHeroNode.nickname;
                System.out.println("修改成功!");
                break;
            }
            temp = temp.next;
        }

        if (temp.next == null){ //  遍历到结尾了，还没有找到
            System.out.println("你要修改的节点不存在，无法修改");
        }
    }

    //  删除当前节点
    public void delete(int no){
        HeroNode temp = head;
        boolean flag = true;   //  如果在遍历过程中找到了并删除（但是如果到结尾都没找到的话，就输出没找到）
        while (temp.next!=null){
            if (no == temp.next.no){
                temp.next = temp.next.next;	//	temp.next 可能为 Null
                System.out.println("删除成功");
                flag = false;
                break;
            }
            temp = temp.next;
        }
        if (flag){
            System.out.println("该节点不存在，无法删除");
        }
    }
}
```

#### 5. 面试题

1. 有效节点个数

   ```java
   public int size(){
       int count = 0;
       HeroNode cur = head.next;
       while (cur != null){
           count++;
           cur = cur.next;
       }
       return count;
   }
   ```

2. 倒数第 k 个节点

   ```java
   public HeroNode backKthNode(int k){
       if (k > size() || k <= 0){
           System.out.println("越界，无法返回");
           return null;
       }
       HeroNode cur = head.next;   //  cur 为头节点
       int frontOrder = size() - k;    //  头节点移动的次数【 3 - 2 = 1】
   
       while (cur.next != null && frontOrder > 0){
           cur = cur.next;   //  移动了2次
           frontOrder--;
       }
       return cur;
   }
   ```

3. 单链表的反转

   ![image_0.5706003331816261](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301041733807.gif)

    ```java
    public void reverse(){
        HeroNode reverseHead = new HeroNode(0, "", "");
        HeroNode nextNode = null;   //  指向当前节点的下一个节点【保存的是节点】
        HeroNode cur = head.next;
        if (cur == null || size() <= 1){
            System.out.println("无法反转");
            return;
        }
        while (cur != null) {
            //  指针 next 的线会断（移动），所以我们用节点 next来保存
            nextNode = cur.next;    //  先暂时保存当前节点的下一个接待你，因为后面要用【next是指针，而 nextNode为节点】
    
            cur.next = reverseHead.next;    //  节点添加
            reverseHead.next = cur; //  将 cur 连接到新的链表中
            cur = nextNode; //  循环(指向下一个节点)
        }
        head.next = reverseHead.next;
    }
    ```

4. 从头到尾打印单链表

   - 反向遍历（先反转，再打印）

     - 会破坏原来单链表的结构

   - Stack栈【使用双端队列 Deque 实现】

     ```java
     public void printReverse1(){
         Deque<HeroNode> heroNodes = new LinkedList<>(); //  定义一个双端队列（模拟栈）
         HeroNode cur = head.next;
         while (cur != null){
             heroNodes.addFirst(cur);
             cur = cur.next;
         }
         while (heroNodes.size() > 0){
             System.out.println(heroNodes.removeFirst());
         }
     }
     ```

5. 合并2个有序的单链表，合并之后依旧有序、

   - 建一个新链表，哪个小就加进去
   
     ```java
     public void merge(HeroNode heroNode1, HeroNode heroNode2) {
         System.out.println("=====合并 2个 链表====");
         HeroNode mergeHead = new HeroNode(0, "", "");
         HeroNode cur1 = heroNode1.next;
         HeroNode cur2 = heroNode2.next;
         HeroNode temp = mergeHead;  //  添加时候需要找到最后一个节点
     
         while (cur1 != null || cur2 != null) {
             while (cur1 != null && cur2 != null) {  //  当都不为null 时，才可以比较
                 //  储存当前节点的下一个节点
                 while (temp.next != null) {  // 找到末尾节点
                     temp = temp.next;
                 }
                 if (cur1.no < cur2.no) {
                     HeroNode temp33 = cur1.next;  //  储存当前节点的下一个节点
                     cur1.next = null;
                     temp.next = cur1;
                     cur1 = temp33;
                 } else {
                     HeroNode temp44 = cur2.next;  //  储存当前节点的下一个节点
                     cur2.next = null;
                     temp.next = cur2;
                     cur2 = temp44;
                 }
             }
     
             if (cur2 != null) {
                 while (temp.next != null) {  // 找到末尾节点
                     temp = temp.next;
                 }
                 HeroNode temp22 = cur2.next;  //  储存当前节点的下一个节点
                 cur2.next = null;
                 temp.next = cur2;
                 cur2 = temp22;
             }
             
             if (cur1 != null) {
                 while (temp.next != null) {  // 找到末尾节点
                     temp = temp.next;
                 }
                 HeroNode temp11 = cur1.next;  //  储存当前节点的下一个节点
                 cur1.next = null;
                 temp.next = cur1;
                 cur1 = temp11;
             }
         }
         //  输出
         HeroNode cur = mergeHead.next;
         while (cur != null) {
             System.out.println(cur);
             cur = cur.next;
         }
     }
     ```
   
     

### 3.3 双向链表

单向链表缺点分析：

- 查找方向只能是一个方向，而双向链表可以向前或者向后查找
- 单项链表不能自我删除，需要靠辅助节点，而双向链表，则可以 <font color="yellow">自我删除</font>，所以前面我们单链表删除节点时，总是找到temp，temp是待删除节点的前一个结点

双向链表分析：

- 遍历、添加、修改![12342](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301091126024.png)

- 删除（自我删除）

  ![image_0.8755135939787551](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301091136307.gif)

```java
public class doubleLinkedListDemo {
    public static void main(String[] args) {
        //  进行测试
        HeroNode2 hero1 = new HeroNode2(1, "宋江", "及时雨");
        HeroNode2 hero2 = new HeroNode2(2, "卢俊义", "玉麒麟");
        HeroNode2 hero3 = new HeroNode2(3, "吴用", "智多星");
        HeroNode2 hero4 = new HeroNode2(4, "林冲", "豹子头");

        //  创建一个双向链表
        doubleLinkedList dbList = new doubleLinkedList();
        dbList.addLast(hero1);
        dbList.addLast(hero2);
        dbList.addLast(hero3);
        dbList.addLast(hero4);
        dbList.show();

        //  修改链表
        HeroNode2 newHeroNode = new HeroNode2(4, "公孙胜", "入云龙");
        dbList.update(newHeroNode);
        dbList.show();

        // 删除节点
        dbList.delete(3);
        dbList.show();
        
    }
}

class doubleLinkedList{
    public HeroNode2 getHead() {
        return head;
    }

    private final HeroNode2 head = new HeroNode2(0, "", "");

    //  遍历双向链表的方法
    public void show(){
        HeroNode2 cur = head.next;
        while (cur != null){
            System.out.println(cur);
            cur = cur.next;
        }
    }

    //  添加到末尾
    public void addLast(HeroNode2 newHeroNode2){
        HeroNode2 temp = head;
        while (temp.next != null){  //  temp 最终指向最后一个节点
            temp = temp.next;
        }
        temp.next = newHeroNode2;
        newHeroNode2.pre = temp;
    }

    //  修改一个节点的内容
    public void update(HeroNode2 heroNode2){
        HeroNode2 cur = head.next;
        while (cur != null){
            if (cur.no == heroNode2.no){
                cur.name = heroNode2.name;
                cur.nickname = heroNode2.nickname;
                System.out.println("修改成功");
                return;
            }
            cur = cur.next;
        }
        System.out.println("你要修改的节点不存在，请重试！！！");
    }

    //  删除节点
    public void delete(int id){
        HeroNode2 cur = head.next;
        while (cur != null){
            if (cur.no == id){
                cur.pre.next = cur.next;
                if (cur.next != null) {
                    cur.next.pre = cur.pre; //  如果是最后一个节点，就不需要执行这句话 【避免空指针】
                }
                System.out.println("删除成功~");
                return;
            }
            cur = cur.next;
        }
        System.out.println("你要修改的节点不存在，请重试！！！");
    }
}

class HeroNode2 {
    public int no;
    public String name;
    public String nickname;
    public HeroNode2 next;   //  指向下一个节点
    public HeroNode2 pre;   //  指向下一个节点


    public HeroNode2(int no, String name, String nickname) {
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }

    @Override
    public String toString() {
        return "HeroNode2{" +
                "no=" + no +
                ", name='" + name + '\'' +
                ", nickname='" + nickname + '\'' +
                '}';
    }
}
```

#### 1. 普通添加

```java
//  添加到末尾
public void addLast(HeroNode2 newHeroNode2){
    HeroNode2 temp = head;
    while (temp.next != null){  //  temp 最终指向最后一个节点
        temp = temp.next;
    }
    temp.next = newHeroNode2;
    newHeroNode2.pre = temp;
}
```

#### 2. 按顺序添加

```java
//  按照 id 来添加
public void addByOrder(HeroNode2 newHeroNode2){
    HeroNode2 cur = head.next;  //  cur 不为 null
    while (cur != null){
        if (cur.no > newHeroNode2.no){
            HeroNode2 temp = cur.pre;   //  保存前一个节点 [方便处理 4 根线]
            newHeroNode2.next = cur;
            cur.pre = newHeroNode2;
            temp.next = newHeroNode2;
            newHeroNode2.pre = temp;
            System.out.println("插入成功");
            return;
        }
        cur = cur.next;
    }
}
```

#### 3. 修改

```java
//  修改一个节点的内容
public void update(HeroNode2 heroNode2){
    HeroNode2 cur = head.next;
    while (cur != null){
        if (cur.no == heroNode2.no){
            cur.name = heroNode2.name;
            cur.nickname = heroNode2.nickname;
            System.out.println("修改成功");
            return;
        }
        cur = cur.next;
    }
    System.out.println("你要修改的节点不存在，请重试！！！");
}
```

#### 4. 删除（注意空指针）

```java
//  删除节点
public void delete(int id){
    HeroNode2 cur = head.next;
    while (cur != null){
        if (cur.no == id){
            cur.pre.next = cur.next;
            if (cur.next != null) {
                cur.next.pre = cur.pre; //  如果是最后一个节点，就不需要执行这句话 【避免空指针】
            }
            System.out.println("删除成功~");
            return;
        }
        cur = cur.next;
    }
    System.out.println("你要修改的节点不存在，请重试！！！");
}
```

#### 5. 链表反转

```java
class Solution {
    public ListNode reverseList(ListNode head) {
       ListNode pre = null; 
       ListNode cur = head;
       while(cur != null) {
           ListNode temp = cur.next;
           cur.next = pre;
           pre = cur;
           cur = temp;
       }
       return pre;
    }
}
```

![nni](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304252030119.png)



### 3.4 环形链表（Josephu环）

- 编号 1，2，…… n 的 n 个人 围坐一圈
- 约定编号 k$(1\leqslant k\leqslant n)$ 的人从 1 开始报数，数到 m 的那个人又出列
- 以此类推，直到所有人都出列为止
- 由此产生一个出队编号的序列

实现方式：

1. 不带头节点的循环链表来处理 Josephu 问题
2. 先构成一个有 n 个节点的单循环链表
3. 然后从 k 节点起从 1 开始计数，计到 m 时，对应节点从链表中删除
4. 然后再从被删除节点的下一个节点又从 1 开始计数
5. 直到最后一个节点从链表中删除

#### 1. 单向循环链表

- n = 5
- k = 1（从第一个人开始报数）
- m = 2

![image_0.29316039409350214](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301091507189.gif)

根据用户输入，生成一个小孩出圈的顺序：

1. 需要创建一个辅助指针（变量）helper，事先应该指向环形链表的最后这个节点

   ![c1](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301092025081.png)

2. 当小孩报数时，让 first 和 helper 指针同时移动 m - 1次

   ![c2](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301092030231.png)

3. 这时就可以将 first 指向的小孩节点出圈 

   - first = first.next
   - helper.next = first

4. 这样，原来 first 指向的接待你就没有任何引用，就会被 GC 回收

```java
//  根据用户的输入，计算 出圈顺序
/**
	* @param startNo：从第几个小孩开始数数
	* @param countNum：数几下
	* @param nums：最初有多少小孩在圈中
*/
public void countBoy(int startNo, int countNum, int nums){
    //  数据校验
    if (first == null || startNo < 1 || startNo > nums){
        System.out.println("不符合规范！无法输出序列");
        return;
    }
    for (int i = 1; i < startNo; i++) { //  开始位置 startNo
        first = first.getNext();    //  确定起始位置 first
    }

    //  创建一个辅助指针（变量）helper，事先应该指向环形链表的最后这个节点
    Boy helper = first;
    while (helper.getNext() != first){
        helper = helper.getNext();  //  确认初始 helper 位置
    }

    //  开始输出队列：停止条件：helper == first
    while (helper != first){
        for (int i = 1; i <= countNum - 1; i++) {   //  开始数数
            first = first.getNext();    //  当小孩报数时，让 first 和 helper 指针同时移动 m - 1 次
            helper = helper.getNext();  //  （helper 紧紧挨着 first）
        }
        //  这时就可以将 first 指向的小孩节点出圈
        System.out.println(first);

        first = first.getNext();
        helper.setNext(first);
    }
    System.out.println(first);  //  输出最后一个节点
}
```



#### 2. 构建一个单向的环形链表思路：

1. 先创建第一个节点，让 first 指向该节点，并形成环形
1. 后面当我们每创建一个新的节点，就把该节点，加入到已有的环形链表中即可

![image_0.6825705918717684](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301091528051.gif)

遍历环形链表

1. 先让一个辅助指针（变量）curBoy，指向 first 节点
2. 然后通过一个 while 循环遍历该环形链表即可
3. 结束条件 <font color="yellow">**curBoy.next = first**</font>

```java
public class JosePhu {
    public static void main(String[] args) {
        CircleSingleLinkedList list = new CircleSingleLinkedList();
        list.add(25);
        list.show();
        System.out.println("===== 出列顺序 ====");
        list.countBoy(1, 2, 25);
    }
}

//  创建环形单向链表
class CircleSingleLinkedList{
    //  创建一个 first 节点，当前没有编号
    private Boy first = null;
    //  添加小孩节点，构建成也给环形链表

    public void add(int nums){
        if (nums < 1){
            System.out.println("nums 不正确");
            return;
        }
        Boy cur = null; //  辅助指针，帮助构建环形链表
        //  使用 for 循环来创建环形链表
        for (int i = 1; i <= nums; i++) {
            Boy boy = new Boy(i);
            if (i == 1){    //  第一个小孩
                first = boy;
                boy.setNext(first); //  构成环
                cur = first;    //  指向第一个小孩
            }else {
                cur.setNext(boy);   //  改变线
                boy.setNext(first); //  改变线
                cur = boy;  //  最后移动 cur 指针
            }
        }
    }

    public void show(){
        //  因为 first 指针不能动，因此我们仍然使用一个辅助指针完成遍历
        Boy cur = first;
        while (cur.getNext() != first){
            System.out.println(cur);
            cur  = cur.getNext();
        }
        System.out.println(cur);    //  输出末尾节点
    }
}

class Boy{
    private int no;
    private Boy next;

    public Boy(int no) {
        this.no = no;
    }

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public Boy getNext() {
        return next;
    }

    public void setNext(Boy next) {
        this.next = next;
    }

    @Override
    public String toString() {
        return "Boy{" +
                "no=" + no +
                '}';
    }
}
```



## 4. 栈⭐（双端队列模拟栈 Deque）

- Stack

  - 允许变化的一端，称为 栈顶 Top 
  - 另一端为固定的一端，称为栈底 Buttom

- FIFO

- 使用双端队列 Deque 实现

  ```java
  public void printReverse1(){
      Deque<HeroNode> heroNodes = new LinkedList<>(); //  定义一个双端队列（模拟栈）
      HeroNode cur = head.next;
      while (cur != null){
          heroNodes.addFirst(cur);
          cur = cur.next;
      }
      while (heroNodes.size() > 0){
          System.out.println(heroNodes.removeFirst());
      }
  }
  ```

### 4.1 栈的应用场景

1. 子程序调用
2. 处理递归调用
3. 表达式的转换【中缀表达式转后缀表达式】与 求值
4. 二叉树遍历
5. 图形的深度优先【DFS】

### 4.2 用数组模拟栈

由于栈是一种有序列表，当然可以使用数组的结构来存储栈的数据结构

思路分析：

- 使用数组模拟栈
- 定义一个 top 来表示栈顶，初始化为 -1
- 入栈的操作，当有数据加入到栈时，stack[++top] = data;
- 出栈的操作
  - int val = stack[top]
  - top--
  - return val

代码实现：

```java
public class ArrayStackDemo {
    public static void main(String[] args) {

        ArrayStack stack = new ArrayStack(5);
        Scanner in = new Scanner(System.in);
        boolean flag = true;
        while (flag){
            System.out.println("show: 显示栈");
            System.out.println("exit: 退出程序");
            System.out.println("push: 入栈");
            System.out.println("pop: 出栈");
            System.out.println("请输入你的选择");
            String next = in.next();

            switch (next){
                case "show":
                    stack.show();
                    break;
                case "exit":
                    flag = false;
                    break;
                case "push":
                    System.out.println("请输入你要 入栈的数");
                    String next1 = in.next();
                    int i = Integer.parseInt(next1);
                    stack.push(i);
                    break;
                case "pop": //  出栈时可能有运行异常抛出，我们需要 catch 来看具体是什么情况
                    try {
                        int res = stack.pop();
                        System.out.println("出栈的数据是：" + res);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                default:
                    break;
            }

        }
        stack.push(1);
        stack.push(2);
        System.out.println("栈顶元素为：" + stack.peek());
        stack.push(3);
        stack.show();

    }
}

class ArrayStack{
    private final int maxSize;
    private final int[] stack;
    private int top = -1;

    public ArrayStack(int maxSize) {
        this.maxSize = maxSize;
        stack = new int[this.maxSize];
    }

    //  栈满
    public Boolean isFull(){
        return top == maxSize - 1;
    }

    //  栈空
    public boolean isEmpty(){
        return top == -1;
    }

    //  入栈
    public void push(int data){
        if (isFull()){
            System.out.println("栈满了，无法 push");
            return;
        }
        stack[++top] = data;
    }

    //  出栈
    public int pop(){
        if (isEmpty()){
            //  抛出异常，本身就代表终止了，所以就不需要有 return 了
            throw new RuntimeException("栈空，没有数据");
        }
        int val = stack[top];
        top--;
        return val;
    }

    //  栈顶元素
    public int peek(){
        if (isEmpty()){
            System.out.println("栈顶为 null");
            return -1;
        }
        return stack[top];
    }

    //  遍历
    public void show(){
        //  遍历要从栈顶开始遍历
        if (isEmpty()){
            System.out.println("栈空，无法遍历");
            return;
        }

        for (int i = top; i >= 0; i--) {  //  从栈顶开始遍历
            System.out.format("stack[%d] = %d", i, stack[i]);
            System.out.println();
        }
    }
}
```

  ### 4.3 用链表模拟栈

### 4.4 栈实现综合计算器【中缀】

![no1](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301111857747.png)

![image_0.9485540751725847](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301111936409.gif)

代码实现：（Ver1 ）

存在问题：不能有2位数字 --->比如："70+2*6-4";

```java
public class cal {
    public static void main(String[] args) {
        String exp = "7+2*6-4";
        //  创建2个栈

        ArrayStack numStack = new ArrayStack(10);
        ArrayStack operStack = new ArrayStack(10);
        //  定义相关的变量
        int index = 0;
        int num1 = 0;
        int num2 = 0;
        int oper = 0;
        int res = 0;
        char ch = ' ';   //  每次扫描得到的 char 放入 ch 中

        //  开始 while 循环扫描 exp
        while (true){
            ch = exp.substring(index, index + 1).charAt(0);
            if (operStack.isOper(ch)){
                //  判断当前符号栈是否为 null
                if (!operStack.isEmpty()){
                    if (operStack.priority(ch) <= operStack.priority(operStack.peek())){
                        int pop1 = numStack.pop();
                        int pop2 = numStack.pop();
                        int operTop = operStack.pop();
                        int i = operStack.calNum(pop2, pop1, operTop);
                        numStack.push(i);
                        operStack.push(ch);
                    }else { //  当前优先级 > 栈顶
                        operStack.push(ch);
                    }
                }else { //  operStack 不为 null
                    operStack.push(ch);
                }
            }else {
                numStack.push(ch - '0');
            }
            index++;
            if (index == exp.length()){
                break;
            }
        }

        //  现在扫描完毕了，现在处理2个栈中的数据就可以了
        while (true){
            //  终止条件
            if (operStack.isEmpty()){
                break;
            }
            int n1 = numStack.pop();
            int n2 = numStack.pop();
            int op = operStack.pop();
            int i = numStack.calNum(n2, n1, op);
            numStack.push(i);
        }

        res = numStack.peek();
        System.out.println(res);
    }
}

class ArrayStack {  //  需要扩展功能
    private final int maxSize;
    private final int[] stack;
    private int top = -1;

    public ArrayStack(int maxSize) {
        this.maxSize = maxSize;
        stack = new int[this.maxSize];
    }

    //  栈满
    public Boolean isFull() {
        return top == maxSize - 1;
    }

    //  栈空
    public boolean isEmpty() {
        return top == -1;
    }

    //  入栈
    public void push(int data) {
        if (isFull()) {
            System.out.println("栈满了，无法 push");
            return;
        }
        stack[++top] = data;
    }

    //  出栈
    public int pop() {
        if (isEmpty()) {
            //  抛出异常，本身就代表终止了，所以就不需要有 return 了
            throw new RuntimeException("栈空，没有数据");
        }
        int val = stack[top];
        top--;
        return val;
    }

    //  栈顶元素
    public int peek() {
        if (isEmpty()) {
            System.out.println("栈顶为 null");
            return -1;
        }
        return stack[top];
    }

    //  遍历
    public void show() {
        //  遍历要从栈顶开始遍历
        if (isEmpty()) {
            System.out.println("栈空，无法遍历");
            return;
        }

        for (int i = top; i >= 0; i--) {  //  从栈顶开始遍历
            System.out.format("stack[%d] = %d", i, stack[i]);
            System.out.println();
        }
    }

    //  返回运算符的优先级
    public int priority(int oper){
        if (oper == '*' || oper == '/'){
            return 1;
        }else if (oper == '+' || oper == '-'){
            return 0;
        }else {
            return -1;
        }
    }

    //  判断是否是运算符
    public boolean isOper(char val){
        return val == '+' || val == '-' || val == '*' || val == '/';
    }

    //  计算方法
    public int calNum(int num1, int num2, int oper){
        int res = 0;
        switch (oper){
            case '+':
                res = num1 + num2;
                break;
            case '-':
                res = num1 - num2;
                break;
            case '*':
                res = num1 * num2;
                break;
            case '/':
                res = num1 / num2;
                break;
            default:
                break;
        }
        return res;
    }
}
```

解决当处理多位数时，不能发现一个数就立即入栈，因为它可能是多位数

在处理数时，需要向 exp 的 index后再看一位

- 如果是数，继续扫描
- 如果是符号才入栈
- 最后一个数直接入栈

定义一个字符串变量用于拼接

```java
StringBuilder sb = new StringBuilder();
```

```java
public class cal {
    public static void main(String[] args) {
        String exp = "7*2*2-5+1-5+3-4";
        //  创建2个栈

        ArrayStack numStack = new ArrayStack(10);
        ArrayStack operStack = new ArrayStack(10);
        StringBuilder sb = new StringBuilder();


        //  定义相关的变量
        int index = 0;
        int num1 = 0;
        int num2 = 0;
        int oper = 0;
        int res = 0;
        char ch = ' ';   //  每次扫描得到的 char 放入 ch 中

        //  开始 while 循环扫描 exp
        while (true){
            ch = exp.substring(index, index + 1).charAt(0);
            if (operStack.isOper(ch)){
                if (sb.length() != 0) {
                    String s = sb.toString();
                    int i1 = Integer.parseInt(s);
                    numStack.push(i1);
                    sb.delete(0, sb.length());
                }

                //  判断当前符号栈是否为 null
                if (!operStack.isEmpty()){
                    if (operStack.priority(ch) <= operStack.priority(operStack.peek())){
                        int pop1 = numStack.pop();
                        int pop2 = numStack.pop();
                        int operTop = operStack.pop();
                        int i = operStack.calNum(pop2, pop1, operTop);
                        numStack.push(i);
                        operStack.push(ch);
                    }else { //  当前优先级 > 栈顶
                        operStack.push(ch);
                    }
                }else { //  operStack 不为 null
                    operStack.push(ch);
                }
            }else { //  当前指针是 数字
                sb.append(ch - '0'); // 暂时存到 sb中，等待下一个是符号时才压入
            }
            index++;
            if (index == exp.length()){
                numStack.push(Integer.parseInt(sb.toString())); //  处理最后一个数字
                break;
            }
        }

        //  现在扫描完毕了，现在处理2个栈中的数据就可以了
        while (true){
            //  终止条件
            if (operStack.isEmpty()){
                break;
            }
            int n1 = numStack.pop();
            int n2 = numStack.pop();
            int op = operStack.pop();
            int i = numStack.calNum(n2, n1, op);
            numStack.push(i);
        }

        res = numStack.peek();
        System.out.println(res);
    }
}
```

### 4.5 前缀【前缀】、中缀、后缀【逆波兰表达式】表达式规则

#### 前缀（顶 - 次顶）

- 符号在数字前
- (3 + 4) * 5 - 6【中缀】
- \- * + 3 4 5 6【前缀】

计算机使用前缀表达式求值

从 <font color="yellow">右向左 </font>扫描

- 数字 入栈
- 符号
  - 弹出栈顶2个数
  - 用运算符计算
  - 将结果压入栈
- 重复上述步骤直到表达式最左端

(3 + 4) * 5 - 6【中缀】---> - * + 3 4 5 6【前缀】

![image_0.8885328762070734](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301121226649.gif)

#### 中缀（一般会转成后缀表达式）

- 日常写法
  - 需要判断符号优先级
- 对计算机来说不好操作

#### 后缀 (次顶 - 顶)

- 又称为逆波兰表达式，与前缀类似，只是符号在数字之后
- 比如：(3 + 4) * 5 - 6【中缀】
- 转为：3 4 + 5 * 6 - 

| 表达式          | 后缀表达式    |
| --------------- | ------------- |
| a + b           | a b+          |
| a + (b - c)     | a b c - +     |
| a + (b - c) * d | a b c - d * + |
| a + d * (b - c) | a d b c - * + |
| a = 1 + 3       | a 1 3 + =     |

后缀表达式的计算机求值

从 <font color="yellow">左至右 </font>扫描表达式

- 数字，入栈
- 符号
  - 弹出2个数（次顶、顶），并将结果入栈
- 重复上述过程到表达式右端
- 最后得出的值即为表达式的结果

例如：比如：(3 + 4) * 5 - 6【中缀】 ---> 3 4 + 5 * 6 - 【后缀】

![image_0.7283163702907425](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301121447582.gif)

### 4.6 逆波兰计算器（后缀）suffix

- 输入的表达式已经是后缀表达式
- 使用系统栈实现（双端队列）
- 支持小括号和多位整数  

```java
public class polandCal {
    public static void main(String[] args) {
         // 先定义逆波兰表达式
        String suffixExp = "4 5 * 8 - 60 + 8 2 / +";
        ArrayList<String> list = new ArrayList<>();
        String[] s = suffixExp.split(" ");
        for (String s1 : s) {
            list.add(s1);
        }
        System.out.println(list);
        //  定义一个栈

        Deque<Integer> stack = new LinkedList<>();
        //  开始扫描
        for (String s1 : list) {
            char c = s1.charAt(0);
            if (s1.matches("\\d+")){    //  正则表达式【匹配多位数（0个或者）】
                stack.addFirst(Integer.parseInt(s1));
            }else { //  符号
                int num1 = stack.removeFirst();
                int num2 = stack.removeFirst();
                int i = calNum(num2, num1, c);
                stack.addFirst(i);
            }
        }
        System.out.println("res = " + stack.peekFirst());
    }

    public static boolean isOper(char c){
        return c < '0' || c > '9';
    }

    //  计算方法
    public static int calNum(int num1, int num2, int oper){
        int res = 0;
        switch (oper){
            case '+':
                res = num1 + num2;
                break;
            case '-':
                res = num1 - num2;
                break;
            case '*':
                res = num1 * num2;
                break;
            case '/':
                res = num1 / num2;
                break;
            default:
                break;
        }
        return res;
    }
}
```

### 4.7 中缀 ---> 后缀

- 后缀表达式适合计算机运算
- 中缀符合人类常识

具体实现步骤：

1. 初始化2个栈：
   - S1：运算符栈
   - S2：中间结果
2. 从左到右扫描中缀表达式
   - 遇到数字：压入 S2
   - 遇到符号：观察 S1 
     - S1 为空，或 peek() 为 "("，则直接将此运算符入栈
     - S1 不为空，
       - 比栈顶符号的优先级高，压入 S1
       - 比栈顶符号的优先级低或者相同，弹出 peek() 并压入到 s2中。继续与新的栈顶进行比较
   - 遇到括号时：
     - "（"，直接压入 S1
     - "）"：依次弹出 S1 符号，并压入 S2，直到遇到 左括号位置，此时将这一对括号丢弃
3. 扫描完毕后，将 S1 中剩余运算符依次弹出i并压入 S2
4. 依次弹出 S2 中的元素并输出，结果的逆序就是答案

````java
//	中缀 --> 后缀
1 + ((2 + 3) * 4) - 5
````

![动画](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301122117250.gif)

| 扫描到的元素 | 52(栈底->栈顶)                            | s1(栈底->栈顶) | 说明                               |
| ------------ | ----------------------------------------- | -------------- | ---------------------------------- |
| 1            | 1                                         | 空             | 数字，直接入栈                     |
| +            | 1                                         | +              | s1为空，运算符直接入栈             |
| (            | 1                                         | + (            | 左括号，直接入栈                   |
| (            | 1                                         | + ( (          | 同上                               |
| 2            | 1 2                                       | + ( (          | 数字                               |
| +            | 1 2                                       | + ( ( +        | s1栈顶为左括号，运算符直接入栈     |
| 3            | 1 2 3                                     | + ( ( +        | 数字                               |
| )            | 1 2 3 <font color="yellow">+</font>       | + (            | 右括号，弹出运算符直至遇到左括号   |
| x            | 1 2 3 +                                   | + ( ×          | s1栈顶为左括号，运算符直接入栈     |
| 4            | 1 2 3 + 4                                 | + ( ×          | 数字                               |
| )            | 1 2 3 + 4 <font color="yellow">×</font>   | +              | 右括号，弹出运算符直至遇到左括号   |
| -            | 1 2 3 + 4 × <font color="yellow">÷</font> | -              | -与+优先级相同，因此弹出+，再压入- |
| 5            | 1 2 3 + 4 × + 5                           | -              | 数字                               |
| 到达最右端   | 1 2 3 + 4 ÷ + 5 -                         | 空             | s1中剩余的运算符                   |

```java
public class polandCal {
    static Deque<String> S1 = new LinkedList<>();  //  符号栈
    static Deque<String> S2 = new LinkedList<>();  //  数字栈 【由于只有加入的，其实可以用 list 代替】

    public static void main(String[] args) {
         // 先定义逆波兰表达式
        String suffixExp = "4 5 * 8 - 60 + 8 2 / +";
        ArrayList<String> list = new ArrayList<>();
        String[] s = suffixExp.split(" ");

        ArrayList<String> strings1 = new ArrayList<>();
        String str = "1 + ( ( 2 + 3 ) * 4 ) - 5";
        String[] s2 = str.split(" ");
        for (String s1 : s2) {
            strings1.add(s1);
        }
        List<String> strings2 = inSuffixToSuffix(strings1);
        System.out.println(strings2);
        //  定义一个栈

        Deque<Integer> stack = new LinkedList<>();
        //  开始扫描
        for (String s1 : strings2) {
            char c = s1.charAt(0);
            if (s1.matches("\\d+")){    //  正则表达式【匹配多位数（0个或者）】
                stack.addFirst(Integer.parseInt(s1));
            }else { //  符号
                int num1 = stack.removeFirst();
                int num2 = stack.removeFirst();
                int i = calNum(num2, num1, c);
                stack.addFirst(i);
            }
        }
        System.out.println("res = " + stack.peekFirst());
    }

    public static boolean isOper(char c){
        return c < '0' || c > '9';
    }

    //  计算方法
    public static int calNum(int num1, int num2, int oper){
        int res = 0;
        switch (oper){
            case '+':
                res = num1 + num2;
                break;
            case '-':
                res = num1 - num2;
                break;
            case '*':
                res = num1 * num2;
                break;
            case '/':
                res = num1 / num2;
                break;
            default:
                break;
        }
        return res;
    }

    //  计算方法
    public static int operPriority(String str){
        String priority = "";
        switch (str){
            case "+", "-":
                priority = "1";
                break;
            case "*", "/":
                priority = "2";
                break;
            default:
                break;
        }
        return Integer.parseInt(priority);
    }

    //  1 + ((2 + 3) * 4) - 5
    //  中缀 ---> 后缀
    public static List<String> inSuffixToSuffix(List<String> list){
        for (String s : list) {
            if (s.matches("\\d")){  //  数字
                S2.addFirst(s);
            }else if (s.equals("+") || s.equals("-") || s.equals("*") || s.equals("/")){  // 运算符
                if (S1.isEmpty() || S1.peekFirst().equals("(")){  //  观察 S1 是否为空
                    S1.addFirst(s);
                }else { // + - * /
                    if (operPriority(s) > operPriority(S1.peekFirst())){
                        S1.addFirst(s);
                    }else{  //  s 优先级 <= 栈顶
                        while (!S1.isEmpty() && operPriority(s) > operPriority(S1.peekFirst())){
                            S2.addFirst(S1.removeFirst());
                        }
                        S1.addFirst(s);
                    }
                }
            }else if (s.equals("(") || s.equals(")")){ //  左右括号
                if (s.equals("(")){ //  左括号
                    S1.addFirst(s);
                }else { //  右括号
                    while (!S1.peekFirst().equals("(")){
                        S2.addFirst(S1.removeFirst());
                    }
                    S1.removeFirst();   //  将左括号弹出
                }
            }
        }
        //  扫描完毕之后，将 S1 余下的部分放入到 S1 中
        for (String s : S1) {
            S2.addFirst(s);
        }
        List<String> list2 = new ArrayList<>();
        while (!S2.isEmpty()){
            list2.add(S2.removeFirst());
        }
        Collections.reverse(list2);
        return list2;
    }
}
```

## 5. 递归（Recursion）

- 自己调用自己
- 每次调用传入不同的变量

递归有助于解决复杂问题

阶乘：

```java
public class factorial {
    public static void main(String[] args) {
        System.out.println(jieCheng(5));
    }

    public static int jieCheng(int n){
        if (n == 1){
            return 1;
        }else {
            return jieCheng(n - 1) * n;
        }
    }
}
```

### 5.1 递归调用规则

1. 当程序执行到一个方法时，就会开辟一个独立的空间（栈）
2. 每个空间的数据（局部变量），是独立的

### 5.2 递归用于解决什么问题

1. 数学问题：
   - 8 皇后
   - 汉诺塔
   - 阶乘
   - 迷宫
   - 球和篮子
2. 各种算法中也会用到递归
   - 快排
   - 归并排序
   - 二分查找
   - 分治算法
3. 利用栈解决的问题 ---> 递归代码比较简洁

### 5.3 递归遵守重要规则

1. 执行一个方法时，就创建 1 个新的受保护的独立空间(栈空间)
2. 方法的局部变量是独立的，不会相互影响
3. 如果方法中使用的是引用类型变量（比如：数组），就会共享该引用类型的数据
4. 递归必须面退出递归的条件逼近，否则就是无限递归，死龟了:)
5. 当一个方法执行完毕，或者遇到 return，就会返回，遵守谁调用，就将结果返回给谁，同时当方法执行完毕或者返回时，该方法也就执行完毕。

### 5.4 迷宫回溯

使用递归回溯来给小球找路

当 map[i]\[j] 的值为：

- 0：没有走过
- 1：墙体
- 2：通过可以走
- 3：该点已经走过，但是走不通

走迷宫时候，确定策略

1. 下 ---> 右 ---> 上 ---> 左

   ```java
   public class labyrinth {
       public static void main(String[] args) {
           int[][] map = new int[8][7];
           for (int i = 0; i < 7; i++) {
               map[0][i] = 1;
               map[7][i] = 1;
           }
           for (int i = 0; i < 8; i++) {
               map[i][0] = 1;
               map[i][6] = 1;
           }
           //  挡板位置
           map[3][1] = 1;
           map[3][2] = 1;
   
           setWay(map, 1, 1);
           printMap(map);
       }
   
       public static boolean setWay(int[][] map, int i, int j){
           if (map[6][5] == 2){
               return true;
           }else {
               //  base case;
               if (map[i][j] == 0) {   //  利用墙体来防止越界
                   map[i][j] = 2;  //  假定该点可以走通
                   if (setWay(map, i + 1, j)){
                       return true;
                   }else if (setWay(map, i, j + 1)){
                       return true;
                   }else if (setWay(map, i - 1, j)){
                       return true;
                   }else if(setWay(map, i, j - 1)){
                       return true;
                   }else {
                       //  说明该点是走不通的，是死路
                       map[i][j] = 3;
                       return false;
                   }
               }
           }
           return false;
       }
   
       public static void printMap(int[][] arr){
           for (int i = 0; i < arr.length; i++) {
               for (int j = 0; j < arr[i].length; j++) {
                   System.out.print(arr[i][j] + " ");
               }
               System.out.println();
           }
       }
   }
   ```

   ![image-20230128162655573](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301281627713.png)

   可以看到，这种走法没有体现回溯，我们构造一种回溯情景

   ```java
   //  构建回溯
   map[1][2] = 1;
   map[2][2] = 1;
   ```

   ![image-20230128162859934](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301281629027.png)

   分析：只要点的值不是0，就免谈，根本走不到这个点上！！！

   ![image_0.9170755621480102](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301281653868.gif)

2. 小球路径，和设计的路径有关系！！！---> 策略改为上右下左

   ```java
   public static boolean setWay(int[][] map, int i, int j){
       if (map[6][5] == 2){
           return true;
       }else {
           //  base case;
           if (map[i][j] == 0) {   //  利用墙体来防止越界，并且如果走过了设定为2
               map[i][j] = 2;  //  标记该点可以走通，并走过！！！
               if (setWay(map, i - 1, j)){
                   return true;
               }else if (setWay(map, i, j + 1)){
                   return true;
               }else if (setWay(map, i + 1, j)){
                   return true;
               }else if(setWay(map, i, j - 1)){
                   return true;
               }else {
                   //  说明该点是走不通的，是死路
                   map[i][j] = 3;
                   return false;
               }
           }
       }
       return false;
   }
   ```

   

   ![image-20230128170104229](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301281701329.png)

### 5.5 8 皇后（回溯）$O(n!)$

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
        if (i == n) {   //  Base Case 【n 从0 开始】---> 所有皇后已经放置完毕
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
                res += process(i + 1, record, n);	//	当前行可以的话，就去判断下一行了
            }
        }
        return res;
    }
	
    //	(i, j) ---> 待放入的位置
    //	(k, record[k] ---> 已经放入棋盘的皇后 
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

回溯思路：

思想：

- 当前行所有列都试过之后，不行的话，要返回上一列可行的地方继续走，依次类推

![3213213232133213未命名绘图](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212161953837.png)

从第二行开始，每一行不管找没找到，都得遍历到最后为止

![未321命名绘图](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212162140455.png)

![未命名绘图7789](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301291646284.png)

![未命名绘图32](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202212162208724.png)

对于 res 的理解：

以第一行为基准行，第一行不进行判断，肯定可以放进去，共有 4种可能：

- (i = 0， j = 0)
- (i = 0， j = 1)
- (i = 0,   j = 2)
- (i = 0,   j = 3)

i 的范围是 0 - 3，当 i 到达 4 时，越界（base case），说明所有皇后都已经放好

- return 1;【完成一次排列】

- 将这4种情况下，可以将皇后都放置【n == 4】的次数都加起来就好了



## 6. 排序算法

### 6.1 排序分类

#### 1. 内部排序

将需要处理的所有数据都加载到内存中进行排序

#### 2. 外部排序

数据量过大，无法全部加载到内存中，需要协助外部存储（文件等）进行排序

#### 3. 常见的排序分类

![33231](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301291749915.png)

### 6.2 算法时间复杂度

#### 1. 时间频度

一个算法花费的时间与算法中<font color="yellow">语句的执行次数</font>成正比，一个算法中的语句执行次数称为语句频度或时间频度，记为：$T(n)$

- 常数项忽略
- 低次项忽略
- 系数可以忽略

常见的时间复杂度：

![1024px-Comparison_computational_complexity.svg](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301291814804.png)

常见的算法的时间复杂度由小到大依次为：

$O(1)< O(log_{2}n)<O(n)<O(nlog_{2}n)<O(n^{k})<O(2^{n})$

随着问题规模 n 的不断增大，上述事件复杂度不断增大，算法执行效率越低

对数阶：$log_{2}n$

```java
//	log2(1024)
int i = 1;
while(i < n){
    i = i * 2;
}
```

线性对数阶：$nlog_{2}n$

- 将时间复杂度为：$log_{2}n$ 的代码执行了 N 遍 

#### 2. 平均、最坏事件复杂度

| 徘序法\|       | 平均时间      | <font color="yellow">最差 </font>情形 | 稳定度 | 额外空间  | 备注                                |
| -------------- | ------------- | ------------------------------------- | ------ | --------- | ----------------------------------- |
| 冒泡           | $O(n^{2})$    | $O(n^{2})$                            | 稳定   | $O(1)$    | n小时较好                           |
| 交换           | $O(n^{2})$    | $O(n^{2})$                            | 不稳定 | $O(1)$    | n小时较好                           |
| 选择           | $O(n^{2})$    | $O(n^{2})$                            | 不稳定 | $O(1)$    | n小时较好                           |
| 插入           | $O(n^{2})$    | $O(n^{2})$                            | 稳定   | $O(1)$    | 大部分已排序时较好                  |
| 基数【桶排序】 | $O(log_{R}B)$ | $O(log_{R}B)$                         | 稳定   | $O(n)$    | B是真数(0-9)，<br />R是基数(个十百) |
| Shell          | $O(nlogn)$    | $O(n^{s})$ 1<s<2                      | 不稳定 | $O(1)$    | s是所选分组                         |
| 快速           | $O(nlogn)$    | $O(n^{2})$                            | 不稳定 | $O(logn)$ | n大时较好                           |
| 归并           | $O(nlogn)$    | $O(nlogn)$                            | 稳定   | $O(n)$    | n大时较好                           |
| 堆             | $O(nlogn)$    | $O(nlogn)$                            | 不稳定 | $O(1)$    | n大时较好                           |

   

#### 3. 空间复杂度

一个算法在运行过程中临时占用存储空间大小的量度

- 有的算法需要占用的临时工作单元数与解决问题的规模 n 有关
  - 它随着 n 的增大而增大，当 n 较大时，将占用较多的存储单元
  - 例如快速排序，和归并排序算法就属于这种情况

从用户体验看，更看重的是程序执行的速度，一些缓存产品（<font color="yellow">redis</font>、memcache）和算法（基数排序）本质就是用 <font color="yellow">**空间换时间**</font>

### 6.3 冒泡（Bubble）

```java
public class Bubble {
    public static void main(String[] args) {
        int[] arr = {2, 32, 231, 232, 2321};
        BubbleSort(arr);
        printArr(arr);
    }

    public static void BubbleSort(int[] arr){
        for (int i = arr.length - 1; i > = 1; i--) {  //  每次循环都将最大的值放在最后！！！【尾部指针】
            for (int j = 0; j < i; j++) {
                if (arr[j] > arr[j + 1]){
                    swap(arr, j, j + 1);
                }
            }
        }
    }

    public static void printArr(int[] arr){
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }

    public static void swap(int[] arr, int i, int j){
        arr[i] = arr[i]^arr[j];
        arr[j] = arr[i]^arr[j];
        arr[i] = arr[i]^arr[j];
    }
}
```

如果我们发现在某趟排序中，一次交换也没有，可以进行 <font color="yellow">**优化 **</font>！！！---> 提前结束冒泡排序

```java
static boolean flag = false;

public static void BubbleSort(int[] arr){
    for (int i = arr.length - 1; i > 0; i--) {  //  每次循环都将最大的值放在最后！！！【尾部指针】
        for (int j = 0; j < i; j++) {
            if (arr[j] > arr[j + 1]){
                swap(arr, j, j + 1);
                flag = true;    //  只要进行了操作，就说明进行了排序
            }
        }
        if (!flag){ //  flag为false：说明这趟排序中，没有进行 swap，说明已经排好序了，提前结束冒泡！！！
            return;
        }
        flag = false; // 重置 flag 为false，用于下一趟排序判断！！！
    }
}
```

测试时间代码：

```java
//	测试 80000个数据，进行排序！！!
//	9739 ms
int[] test = new int[80000];
for (int i = 0; i < 80000; i++) {
    test[i] = (int) (Math.random() * 80000);
}
long start = System.currentTimeMillis();
BubbleSort(test);
long end = System.currentTimeMillis();
System.out.println("耗时：" + (end - start));
```

### 6.4 选择（Select）

思路：【每一轮只会交换一次】

1. 第一次从 arr[0] ~ arr[n - 1] 中选取最小值，与 arr[0] 交换
2. 第二次从 arr[1] ~ arr[n - 1] 中选取最小值，与 arr[1] 交换
3. 以此类推
4. 总共交换 n - 1次

![jjiijj](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301301945485.png)

 代码思路：

- n - 1 轮排序
- 每一轮排序，又是一个循环
  - 先假定当前值为最小值
  - 然后和后面的值进行比较
    - 如果发现有比 min 小的值，则重新确定 min，并得到下标
    - 当遍历到数组最后时，就得到 min 和 下标
    - swap

```java
public class Select {

    static int min_index = 0;	//	记录最小值的下标

    public static void main(String[] args) {
        int[] arr = {2, 32, 231, 232, 2321, 88, 343, 22, 1, 34};
        selectSort(arr);
        printArr(arr);

        int[] test = new int[80000];
        for (int i = 0; i < 80000; i++) {
            test[i] = (int) (Math.random() * 80000);
        }
        long start = System.currentTimeMillis();
        selectSort(test);
        long end = System.currentTimeMillis();
        System.out.println("耗时：" + (end - start));
    }

    public static void selectSort(int[] arr){
        //	n - 1 轮排序 【（n - 2）- 0 + 1 = n - 1】
        for (int i = 0; i < arr.length - 1; i++) {
            arr[min_index] = arr[i];   //  记录最小值下标
            for (int j = i + 1; j < arr.length; j++) {  //  j：最小值的下标
                if (arr[min_index] > arr[j]){
                    min_index = j;
                }
            }
            swap(arr, i, min_index);	//	如果 min_index 就是 i 的话（假设成功，就不用交换了）
        }
    }

    public static void printArr(int[] arr){
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }

    public static void swap(int[] arr, int i, int j){
        arr[i] = arr[i]^arr[j];
        arr[j] = arr[i]^arr[j];
        arr[i] = arr[i]^arr[j];
    }
}
```

```java
//	优化
if (min_index != i) {
    swap(arr, i, min_index);
}
```

注意：选择排序不是稳定的

比如：

<font color="yellow">**6**</font>、7、6、<font color="red">**2**</font>、8 ---> 第一次交换时，就破坏了稳定性



### 6.5 插入（Insert）--- 扑克牌

将 n 个待排序的元素看作：

- 一个有序表
- 一个无序表
  - 开始时，有序表中只有一个元素，无序表中有 n - 1 个元素	
  - 排序过程中，每次从无序表中取出第一个元素，放入到有序表中，使其成为新的有序表

![GHR](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301311204218.png)

```java
public class Insert {

    static int insertValue = 0;
    static int insert_index = 0;

    public static void main(String[] args) {
        int[] arr = {2, 32, 231, 232, 2321, 88, 343, 22, 1, 34, -1 , -10};
        InsertSort(arr);
        printArr(arr);

        int[] test = new int[80000];
        for (int i = 0; i < 80000; i++) {
            test[i] = (int) (Math.random() * 80000);
        }
        long start = System.currentTimeMillis();
        InsertSort(test);
        long end = System.currentTimeMillis();
        System.out.println("耗时：" + (end - start));
    }

    public static void InsertSort(int[] arr){
        //  第一个数不用比较，所以一共进行 n - 1轮 【(n - 1) - 1 + 1 = n - 1】
        for (int i = 1; i < arr.length; i++) {  //  无序表的第一个元素
            for (int j = 0; j <= i - 1; j++) {   //  有序表
                if (arr[i] < arr[j]){
                    insertValue = arr[i];
                    insert_index = j;  //  待插入位置
                    for (int k = i; k > j ; k--) {
                        arr[k] = arr[k - 1];    //  1. 后移
                    }
                    arr[insert_index] = insertValue;   //  2. 插入
                }
            }
        }
    }

    public static void printArr(int[] arr){
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

### 6.6 希尔排序（Shell）：分组实现

引出：

简单插入算法可能存在的问题：

数组 [2, 3, 4, 5, 6]，此时要插入1 的话，<font color="yellow">**后移次数明显增多**</font>，对效率有影响

我们考虑算法时，要考虑它最差的情况

Donal Shell 于 1959 年提出的一种排序算法，优化的插入排序，<font color="yellow">**缩小增量排序**</font>

基本思想：

- 把记录按下标的一定增量 <font color="red">**分组**</font>
- 对每组使用直接插入算法排序
- 随着增量逐渐减少 

![shell](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301311243946.png)

#### 交换式实现

```java
for (int i = increase; i < arr.length; i++) {
    for (int j = i - increase; j >= 0; j -= increase) {   //    有序表
        if (arr[j] > arr[j + increase]){
            swap(arr, j, j + increase);
        }
    }
}
```

#### 移位式实现（⭐）

```java
public static void InsertSort(int[] arr){
    int gap = arr.length / 2;
    while (gap != 0){
        //  从第increase 个元素进行直接插入
        //  第一个数默认有序
        for (int i = gap; i < arr.length; i++) {    //  无序列表的第一个
            insert_index = i;
            insertValue = arr[i];   //  待插入的值
            if (arr[i] < arr[i - gap]){
                while (insert_index - gap >= 0 && insertValue < arr[insert_index - gap]){
                    //  移动
                    arr[insert_index] = arr[insert_index- gap];    //  右移
                    insert_index -= gap;    //  向前继续确定位置
                }
                //  当退出while循环后，就找到了插入的位置 insert_index
                arr[insert_index] = insertValue;
            }
        }
        gap /= 2;
    }
}
```

关于 gap 的理解图示：

![332211](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301312014142.png)



### 6.7 快速排序（Quick）--- partation 划分区域

做不到稳定

每次都要记录断点线的位置，断点这个空间一定省不掉，类似于二分（可能中间位置挑一点 $O(logN)$），最差$O(n)$ ---> 拿 <font color="yellow">**划分值** </font>划分的过程

![223311](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302011025558.png)

整个过程中，t1、t2、t3 这几个变量是可以复用的，所以深度决定了额外空间

#### 经典快排

最后一个数作划分值

- 小于等于这个数的都放在左边（可以无序）
- 大于这个数的都放在右边

【该过程：时间复杂度：O(N)，空间复杂度：O(1)】

![aer](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302011147491.png)

思路：

- cur <= 划分值
  - cur 与 <= 区的下一个数交换
  - <= 区向右扩一个位置
- 直接往下走

```java
public static int partition(int[] arr, int l ,int r){
    int partittion_num = arr[r];		//	最后一个数作为划分
    int less = l - 1;	//	小于等于区域右边界
    for (int i = l; i <= r; i++){
        if(arr[i] <= partition_num){
            //	1. cur 与 <=区的下一个位置交换
            //	2. <= 区扩一位
            swap(arr, i, ++less);	
        }
    }
    //	返回 <= 区域的最后一个位置
    return less;
}
```

#### 荷兰国旗（三种颜色）

![Flag_of_the_Netherlands.svg](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302011149208.png)

分为3种情形：

1. cur < 划分值
   - 调整完之后，下标是跳的
2. cur = 划分值
   - 无任何操作，下标直接跳
3. cur > 划分值
   - 交换之后，<font color="yellow">下标不跳</font>，继续判定！！！

![qqeeww](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302011240854.png)

```java
public static int[] partition(int[] arr, int l ,int r){
    int less = l - 1;	//	< 区
    int more = r;	//	> 区（默认有一个值）---》最后再调整
	while (l < more){
        if(arr[l] < arr[r]){	//	利用 l 变量开始遍历
            swap(arr, l++, ++less)
        }else if(arr[l] > arr[r]){
            swap(arr, l, --more)	//	cur > 划分值，swap后，当前数下标仍留在原地
        }else{
            l++;
        }
    }
    //	最后处理下大于区域的最后一个数 more
    swap(arr, more, r);
    return new int[] {less + 1, more}; //	返回的是 =区 范围
}
```

#### 随机快排（⭐：工程常用）

随机选择一个数，和最后一个数字交换，当做划分值。后续流程和经典快排一致 【而经典是每次都选择最后一个数进行划分】

![image-20230201153014105](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302011530207.png)

```java
public class Quick {
    public static void main(String[] args) {
        int[] arr = {2, 32, 231, 232, 2321, 11};
        quickSort(arr, 0, arr.length - 1);
        printArr(arr);
        int[] test = new int[80000];
        for (int i = 0; i < 80000; i++) {
            test[i] = (int) (Math.random() * 80000);
        }
        long start = System.currentTimeMillis();
        quickSort(test, 0, test.length - 1);
        long end = System.currentTimeMillis();
        System.out.println("耗时：" + (end - start));

    }

    public static void quickSort(int[] arr, int l, int r) {
        if (l < r) {
            //	随机选择一个数，和最后一个数字交换，当做划分值。后续流程和经典快排一致
            swap(arr, l + (int) (Math.random() * (r - l + 1)), r);
            int[] p = partition(arr, l, r);	//	中间的相等部分
            quickSort(arr, l, p[0] - 1);	//  [l, 相等区间左侧前一个位置]
            quickSort(arr, p[1] + 1, r);	//	[相等区间右侧后一个位置, r]
        }
    }

    public static int[] partition(int[] arr, int l, int r) {
        int less = l - 1;
        int more = r;
        while (l < more) {  //  下标 < 大于区的边界
            if (arr[l] < arr[r]) {
                swap(arr, ++less, l++);
            } else if (arr[l] > arr[r]) {
                swap(arr, --more, l);
            } else {
                l++;
            }
        }
        swap(arr, more, r);
        return new int[] { less + 1, more };
    }

    public static void swap(int[] arr, int i, int j){
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
        //  注意：这里不能用与的方法交换，因为一开始是自己和自己交换
        //  2 个 相同的数 与的话，结果是 0
    }

    public static void printArr(int[] arr){
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

![image-20230201160308144](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302011603264.png)

### 6.8 归并排序（merge）---  master 公式

```java
package recursion;

public class getMax {
    static int maxLeft = 0;
    static int maxRight = 0;
    static int mid = 0; //  中点

    public static void main(String[] args) {
        int[] arr = {2, 32, 231, 232, 2321};
        System.out.println(max(arr, 0, arr.length - 1));
    }

    public static int max(int[] arr, int l, int r){
        //  base case
        if (l == r){
            return arr[l];
        }
        mid = l + (r - l) / 2;
        maxLeft = max(arr, l, mid);
        maxRight = max(arr, mid + 1, r);
        return Math.max(maxLeft, maxRight);
    }
}
```

```java
public class merge {

    public static void main(String[] args) {
        int[] arr = {2, 32, 231, 232, 2321, 11};
        mergeSort(arr, 0, arr.length - 1);
        printArr(arr);
    }

    public static void mergeSort(int[] arr, int l, int r) {
        //	base case
        if(l == r){
            return;
        }
        int mid = l + ((r - l) >> 1);
        mergeSort(arr, l, mid);
        mergeSort(arr, mid + 1, r);
        merge(arr, l, mid, r);	//	左右都排好之后，统一外排
    }

    //	左右两边排好之后，左右各有一个指针，申请一个额外空间，合成一个整体有序的东西
    public static void merge(int[] arr, int l, int m, int r) {
        int[] help = new int[r - l + 1];	//	额外空间
        int i = 0;
        int p1 = l;
        int p2 = m + 1;
        //	两边都有数的情况
        while (p1 <= m && p2 <= r) {
            help[i++] = arr[p1] <= arr[p2] ? arr[p1++] : arr[p2++];
        }
        //	右侧已经没了【这个和下面2个while，虽然是顺序结构，但是只会执行一个】
        while (p1 <= m) {
            help[i++] = arr[p1++];
        }
        //	左侧已经没了
        while (p2 <= r) {
            help[i++] = arr[p2++];
        }
        //	将排好序的数组 help 重新赋给原数组 arr
        for (i = 0; i < help.length; i++) {
            arr[l + i] = help[i];
        }
    }

    public static void printArr(int[] arr){
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

时间复杂度：$T(n)=T(\frac{N}{2})+T(\frac{N}{2})+O(N)$

#### master 公式：

$T(n)=aT(\frac{n}{b})+O(N^{d})$
$$
\left\{\begin{matrix}
logb^{a}>d &O(Nlogb^{a} )& \\ 
 d>logb^{a}&  O(N^{d})& \\
d=logb^{a}& O(N^{d}logN) &\\
\end{matrix}\right.
$$
所以，此时：b = 2，a = 2，d = 1，所以时间复杂度为：$O(nlogn)$

补充阅读：[算法的复杂度与 Master 定理 · GoCalf Blog](https://blog.gocalf.com/algorithm-complexity-and-master-theorem)

#### 应用：求数组小和（在 merge中计算）

在 merge 过程中，如果左侧值 < 右侧值，产生小和

- arr[index_left] * (r - index_right + 1) 
- 对应每个数，都是看它的右侧有多少个数比它大
- 每次都是 <font color="yellow">**组间产生小和**</font>，组内不产生小和

```java
import java.util.Scanner;

public class Main {
    static long sum = 0;	//	防止越界

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String size = in.nextLine();
        String s = in.nextLine();
        String[] s1 = s.split(" ");

        int[] arr = new int[s1.length];
        for (int i = 0; i < s1.length; i++) {
            arr[i] = Integer.parseInt(s1[i]);
        }

        mergeSort(arr, 0, arr.length - 1);
        System.out.println(sum);
    }

    public static void mergeSort(int[] arr, int l, int r) {
        //	base case
        if(l == r){
            return;
        }
        int mid = l + ((r - l) >> 1);
        mergeSort(arr, l, mid);
        mergeSort(arr, mid + 1, r);
        merge(arr, l, mid, r);	//	左右都排好之后，统一外排
    }

    //	左右两边排好之后，左右各有一个指针，申请一个额外空间，合成一个整体有序的东西
    public static void merge(int[] arr, int l, int m, int r) {
        int[] help = new int[r - l + 1];	//	额外空间
        int i = 0;
        int p1 = l; //  左侧指针
        int p2 = m + 1; //  右侧指针
        //	两边都有数的情况
        while (p1 <= m && p2 <= r) {    //  组内没有小和，组间才有小和
            if (arr[p1] <= arr[p2]){
                sum += arr[p1] * (r - p2 + 1);
                help[i++] = arr[p1++];
            }else {
                help[i++] = arr[p2++];
            }
        }
        //	右侧已经没了【这个和下面2个while，虽然是顺序结构，但是只会执行一个】
        while (p1 <= m) {
            help[i++] = arr[p1++];
        }
        //	左侧已经没了
        while (p2 <= r) {
            help[i++] = arr[p2++];
        }
        //	将排好序的数组 help 重新赋给原数组 arr
        for (i = 0; i < help.length; i++) {
            arr[l + i] = help[i];
        }
    }
}
```

#### 应用：求降序对

整体类似，即：求出右边有多少个数 比当前小

### 6.9 堆排序（Heap）--- 完全二叉树

堆结构：完全二叉树结构

- 满二叉树，或通向满二叉树的路上
- 每一层节点都是从左向右依次填好
- 落地结构：数组

![image-20230213162926723](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302131629875.png)

脑补结构如下：

![image-20230213163141629](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302131631741.png)

生成规则：

- 当前节点 i
  - 左孩子：2 i + 1
  - 右孩子：2 i + 2
  - 父节点：(i - 1) / 2

#### 大根堆（单独用也很好用）

- 头部最大
- 左右无要求

![image-20230213163455648](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302131634760.png)

构建方式：

1. 拿到一个数，都和自己的父比较
2. 如果能交换就交换了
3. 来到新位置后，再看父节点，如果能交换则继续交换
4. 当添加完最后一个数后，数组调完之后的样子就是大根堆

![223311](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302131712178.png)

##### heapInsert【从下至上】$O(N)$

```java
for (int i = 0; i < arr.length; i++) {
    heapInsert(arr, i);
}

public static void heapInsert(int[] arr, int index) {
    //	注意：-1 / 2 = 0
    while (arr[index] > arr[(index - 1) / 2]) {	//	插入的值 > 父节点【一直比较】
        swap(arr, index, (index - 1) / 2);
        index = (index - 1) / 2;	//	更新下标
    }
}
```

##### heapify【从上至下】调整成大根堆

- 将堆顶与最后一个交换
- 从上至下
- 比 2个儿子小，与儿子中最大值交换
- 一直到没有儿子比自己大

```java
public static void heapify(int[] arr, int index, int heapSize) {
    int left = index * 2 + 1;	//	左孩子
    while (left < heapSize) {	//	左孩子不越界
        int largest = left + 1 < heapSize && arr[left + 1] > arr[left] ? left + 1 : left;	//	右孩子不越界	[largest：孩子中最大值下标]
        largest = arr[largest] > arr[index] ? largest : index;	//	我 和 我 2个孩子中，最大值下标
        if (largest == index) {
            break;	//	我就是最大的，无需交换了
        }
        swap(arr, largest, index);
        index = largest;
        left = index * 2 + 1;
    }
}
```

##### 堆排序整体代码

```java
package heap;

public class bigHeap {
    public static void main(String[] args) {
        int[] arr = {5, 7, 0 , 6, 8};
        for (int i = 0; i < arr.length; i++) {
            heapInsert(arr, i);
        }
        int heapSize = arr.length;
        swap(arr, 0, --heapSize);	//	0 位置和最后一个位置交换，并且堆的大小 - 1
        while (heapSize > 0) {
            heapify(arr, 0, heapSize);
            swap(arr, 0, --heapSize);
        }
        printArr(arr);
    }

    public static void heapInsert(int[] arr, int index) {
        while (arr[index] > arr[(index - 1) / 2]) {
            swap(arr, index, (index - 1) / 2);
            index = (index - 1) / 2;
        }
    }
    
    public static void heapify(int[] arr, int index, int heapSize) {
        int left = index * 2 + 1;	//	左孩子
        while (left < heapSize) {	//	左孩子不越界
            int largest = left + 1 < heapSize && arr[left + 1] > arr[left] ? left + 1 : left;	//	右孩子不越界	[largest：孩子中最大值下标]
            largest = arr[largest] > arr[index] ? largest : index;	//	我 和 我 2 个孩子中，最大值下标
            if (largest == index) {
                break;	//	我就是最大的，无需交换了
            }
            swap(arr, largest, index);
            index = largest;
            left = index * 2 + 1;
        }
    }

    public static void swap(int[] arr, int i, int j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }

    public static void printArr(int[] arr){
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

### 6.10 Arrays.sort 系统排序

- size < 60 ：Insertion
- size > 60
  - merge【非基础类型，自己定义的一个 class，根据比较器比较】---> <font color="yellow">稳定性</font>
  - quick【int、double、char】

#### 比较器

```java
package heap;

import java.util.Arrays;
import java.util.Comparator;

public class Code_09_Comparator {

    public static class Student {
        public String name;
        public int id;
        public int age;

        public Student(String name, int id, int age) {
            this.name = name;
            this.id = id;
            this.age = age;
        }
    }
	
    //	实现 Comparator 接口，并重写 compare 方法
    public static class IdAscendingComparator implements Comparator<Student> {

        @Override
        public int compare(Student o1, Student o2) {
            return o1.id - o2.id;
        }

    }

    public static class IdDescendingComparator implements Comparator<Student> {

        @Override
        public int compare(Student o1, Student o2) {
            return o2.id - o1.id;
        }

    }

    public static class AgeAscendingComparator implements Comparator<Student> {

        @Override
        public int compare(Student o1, Student o2) {
            return o1.age - o2.age;
        }

    }

    public static class AgeDescendingComparator implements Comparator<Student> {

        @Override
        public int compare(Student o1, Student o2) {
            return o2.age - o1.age;
        }

    }

    public static void printStudents(Student[] students) {
        for (Student student : students) {
            System.out.println("Name : " + student.name + ", Id : " + student.id + ", Age : " + student.age);
        }
        System.out.println("===========================");
    }

    public static void main(String[] args) {
        Student student1 = new Student("A", 1, 23);
        Student student2 = new Student("B", 2, 21);
        Student student3 = new Student("C", 3, 22);

        Student[] students = new Student[] { student3, student2, student1 };
        printStudents(students);

        Arrays.sort(students, new IdAscendingComparator());	//	利用比较器进行排序
        printStudents(students);

        Arrays.sort(students, new IdDescendingComparator());
        printStudents(students);

        Arrays.sort(students, new AgeAscendingComparator());
        printStudents(students);

        Arrays.sort(students, new AgeDescendingComparator());
        printStudents(students);
    }
}
```

### 6.11 桶排序（bucket）【之前所以的排序都是基于比较】

我要准备桶把东西放进来，再依次倒出！！！

#### 计数排序（Counting）

统计 <font color="yellow">**词频**</font>

```java
// only for 0~200 value
public static void bucketSort(int[] arr) {
    if (arr == null || arr.length < 2) {
        return;
    }
    int max = Integer.MIN_VALUE;
    for (int i = 0; i < arr.length; i++) {
        max = Math.max(max, arr[i]);
    }
    //	我准备一个数组，长度为 200 + 1，
    int[] bucket = new int[max + 1];
    for (int i = 0; i < arr.length; i++) {
        bucket[arr[i]]++;	//	记录某一个数出现多少次（原始数组的值为 桶的下标）---》统计词频
    }
    int i = 0;
    for (int j = 0; j < bucket.length; j++) {
        while (bucket[j]-- > 0) {
            arr[i++] = j;	//	通过词频，再把 arr 拷贝回去
        }
    }
}
```

#### 基数排序（Radix）--- 稳定

![A4](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302161523785.png)

基本思想：

1. 将所有待比较的数统一为同样的长度，数位较短的前面补0
2. 从最低位开始，依次进行依次排序
3. 这样从最低位排序，一直到最高位排序完成后，就得到一个 有序序列

```java
public class Radix {
    static int count = 0;   //  次数取决于数组中位数最大鹅那个！！！
    static int[][] bucket;  //  桶（二维数组）
    //  记录每个桶【10个桶】中，实际存放了多少个数据
    //  可以这么理解：
    //  each_bucket_numIndex[0] 记录的就是 bucket[0] 桶放入数据个数
    static int[] each_bucket_numIndex = new int[10]; // 每个桶内所放数据的指针

    public static void main(String[] args) {
        int[] arr = {53, 3, 542, 748, 14, 214, 78787};
        for (int i = 0; i < arr.length; i++) {
            int length = (arr[i] + "").length();    //  整数 ---> 字符串
            if (count < length){
                count = length;
            }
        }
        BucketRadixSort(arr);
    }

    public static void BucketRadixSort(int[] arr){
        bucket = new int[10][arr.length];   //  桶
        for (int i = 0, n = 1; i < count; i++, n *= 10) { //  需要遍历的轮数
            for (int j = 0; j < arr.length; j++) {
                int bucket_number = arr[j] / n % 10;
                //  根据算出的 bucket_number 放入桶中，所放桶中指针 + 1
                bucket[bucket_number][each_bucket_numIndex[bucket_number]++] = arr[j];
            }
            popBucket(arr); //  弹出时，要清空 each_bucket_numIndex数组，即：重置每个桶内数据下标
            printArr(arr);
            System.out.println();
        }
    }

    public static void popBucket(int[] arr){ //  遍历每一个桶，并将桶中数据，返回到 arr
        int index = 0;  //  arr下标
        for (int i = 0; i < 10; i++) {
            if (each_bucket_numIndex[i] > 0){ //  第 i 号桶中有数据
                for (int j = 0; j < each_bucket_numIndex[i]; j++) {
                    arr[index++] = bucket[i][j];
                }
            }
        }
        // 全部出桶后，将各个桶中的指针全部置为 0
        for (int i = 0; i < 10; i++) {
            each_bucket_numIndex[i] = 0;
        }
    }

    public static void printArr(int[] arr){
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

#### 堆排序注意事项

由于基数排序是以 <font color="yellow">空间换时间</font>

假如要排 8000 0000 个数组

- 那么就需要 11【本身也算一个】 个 8000 0000 个大小的数组
- 11 * 8000 000 * 4（1个int 4 个字节）/ 1024 / 1024 = 3.3G
- 容易造成 OutOfMemoryError

#### 负数情况

1. 求绝对值
2. 取出的时候要进行一个反转

| 排序算法 | 平均时间复茶度 | 泵好情况       | 最坏情况       | 空问复杂度 | 排序方式  | 稳定性 |
| -------- | -------------- | -------------- | -------------- | ---------- | --------- | ------ |
| 冒泡排序 | $O(n^{2})$     | $O(n)$         | $O(n^{2})$     | $O(1)$     | ln-place  | 稳定   |
| 选择排序 | $O(n^{2})$     | $O(n^{2})$     | $O(n^{2})$     | $O(1)$     | ln-place  | 不稳定 |
| 插入排序 | $O(n^{2})$     | $O(n)$         | $O(n^{2})$     | $O(1)$     | In-place  | 稳定   |
| 希尔排序 | $O(nlogn)$     | $O(nlog^{2}n)$ | $O(nlog^{2}n)$ | $O(1)$     | ln-place  | 不稳定 |
| 归并排序 | $O(nlogn)$     | $O(nlogn)$     | $O(nlogn)$     | $O(n)$     | Out-place | 稳定   |
| 快速排序 | $O(nlogn)$     | $O(nlogn)$     | $O(n^{2})$     | $O(logn)$  | In-place  | 不稳定 |
| 维排序   | $O(nlogn)$     | $O(nlogn)$     | $O(nlogn)$     | $O(1)$     | ln-place  | 不稳定 |
| 计数排序 | $O(n + k)$     | $O(n + k)$     | $O(n + k)$     | $O(k)$     | Out-place | 稳定   |
| 桶排序   | $O(n + k)$     | $O(n + k)$     | $O(n^{2})$     | $O(n+k)$   | Out-place | 稳定   |
| 基数排序 | $O(n * k)$     | $O(n * k)$     | $O(n * k)$     | $O(n+k)$   | Out-place | 稳定   |

In-place：不占用额外内存

Out-place：占用额外内存

n：数据规模

k：桶的个数



## 7. 查找算法

### 1. 线性查找

```java
public class seqSearch {
    public static void main(String[] args) {
        int[] arr = {1, 9, 11, -1, 34, 89}; //  没有顺序的数组
        int index = seqSearch(arr, 11);
        if (index == -1){
            System.out.println("没有找到");
        }else {
            System.out.println("找到了，下标为：" + index);
        }
    }

    public static int seqSearch(int[] arr, int value){
        for (int i = 0; i < arr.length; i++) {
            //  逐一比对
            if (arr[i] == value){
                return i;
            }
        }
        return -1;
    }
}
```

### 2. 二分查找（前提：有序）

思路：

1. 确定中点：mid = left + (right - left) / 2
2. 比较：findVal 和 arr[mid]
   - findVal < arr[mid]：递归向左，查找
   - findVal > arr[mid]：递归向右，查找
3. findVal == arr[mid]，返回

什么时候结束递归：

- 找到就结束递归
- 递归完整个数组，仍然没有找到 findVal，也需要结束递归

```java
package search;

public class binarySearch {

    static int findVal = 100;
    public static void main(String[] args) {
        int[] arr = {1, 8, 10, 89, 1000, 1234};
        int i = binarySearch_(arr, 0, 5);
        if (i == -1){
            System.out.println("没有找到！！！");
        }else {
            System.out.println("找到了，index=" + i);
        }

    }

    public static int binarySearch_(int[] arr, int l, int r){
        //  递归了整个数组，但是没有找到！！！
        if (l > r){
            return -1;
        }

        int mid = l + (r - l) / 2;
        if (findVal < arr[mid]){    //  搜索是为了查找具体的数，所以递归时不要 mid了
            return binarySearch_(arr, l, mid - 1);
        }else if (findVal > arr[mid]){
            return binarySearch_(arr, mid + 1, r);
        }else {
            return mid;
        }
    }
}
```

改进版本：找出数组中的所有相同的值

- 找到 mid 后，不着急返回
- 向左扫描
- 向右扫描
- 放入一个 arrayList中

```java
package search;

public class binarySearch {

    static int findVal = 1000;
    static ArrayList<Integer> list = new ArrayList<>();
    public static void main(String[] args) {
        int[] arr = {1, 8, 10, 89, 1000, 1000, 1000, 1000,  1234};
        ArrayList<Integer> list = binarySearch_(arr, 0, arr.length - 1);
        if (list == null){
            System.out.println("没有找到");
        } else {
            System.out.println("找到了，list=" + list);
        }
    }

    public static ArrayList<Integer> binarySearch_(int[] arr, int l, int r){
        //  递归了整个数组，但是没有找到！！！
        if (l > r){
            return null;
        }

        int mid = l + (r - l) / 2;
        if (findVal < arr[mid]){    //  搜索是为了查找具体的数，所以递归时不要 mid了
            return binarySearch_(arr, l, mid - 1);
        }else if (findVal > arr[mid]){
            return binarySearch_(arr, mid + 1, r);
        }else {
            int temp1 = mid - 1;	//	向左扫描
            while (temp1 >= 0){  
                if (arr[temp1] != findVal){
                    break;
                }
                list.add(temp1);
                temp1--;
            }
            list.add(mid);  //  mid
            int temp2 = mid + 1;
            while (temp2 < arr.length){ //  向右扫描
                if (arr[temp2] != findVal){
                    break;
                }
                list.add(temp2);
                temp2++;
            }
            return list;
        }
    }
}
```

### 3. 插值查找（分布均匀）

引出：

```java
//	现在我们想查找 1
int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
```

假如我们使用二分查找的话：

![image-20230402194123675](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304021941824.png)

我们需要对 <font color="yellow">mid 值进行改进</font>，能够快速定位我们要查找的值

![image-20230402195457122](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304021954272.png)

原理：

1. 类似二分

2. 不同的是：每次从自适应 mid 处开始查找

3. 修改 二分查找中求 mid 的公式：
   $$
   mid=l+\frac{1}{2}(r-l)
   $$
   修改为如下公式：
   $$
   mid=l+\frac{key-a[l]}{a[r]-a[l]}(r-l)
   $$

在插值算法中修改 求 mid 的代码即可：

```java
//	mid：自适应！！！
int mid = l + (findVal - arr[l]) / (arr[r] - arr[l]) * (r - l); 
//	所以第一次查找时候：mid = 0 + 0 = 0
//	就只需要一次就可以找到了！！！
```

注意事项：

- 对于数据量大，关键字分布比较均匀的查找表来说，采用差值查找，速度较快
- 关键字分布不均匀的情况下，该方法不一定比折半查找要好

### 4. 斐波那契（Fibonacci）：黄金分割法 0.618

```java
package search;

public class FibonacciSearch {

    public static void main(String[] args) {
        int[] arr = {1, 8, 10, 89, 1000, 1234};
        System.out.println(fibSearch(arr, 1));
    }

    public static int Fibonacci(int i) {
        if (i == 0 || i == 1) {
            return 1;
        }
        return Fibonacci(i - 2) + Fibonacci(i - 1);
    }

    //  编写斐波那契查找算+法
    public static int fibSearch(int[] arr, int key) {
        int l = 0;
        int r = arr.length - 1;
        int k = 0;  //  斐波那契分割数值的下标
        int mid = 0;
        //  获取到斐波那契分割数值下标
        while (arr.length > Fibonacci(k)) {
            k++;
        }
        //  因为 f(k) 的值可能大于 arr 的长度，因此我们需要使用 Arrays类，构造一个新的数组，并指向 arr[]
        //  不足的部分用 0 填充
        arr = Arrays.copyOf(arr, Fibonacci(k));
        for (int i = r + 1; i < arr.length; i++) {
            arr[i] = arr[r];
        }

        //  使用 while 来循环处理，找到我们的 key
        while (l <= r) {
            mid = l + (Fibonacci(k - 1) - 1);
            if (key < arr[mid]) {
                r = mid - 1;
                //  为什么是 k--
                //  1. 全部元素 = 前面元素 + 后面元素
                //  2. f[k] = f[k - 1] + f[k - 2]
                //  因为 前面有 f[k - 1] 个元素，所以可以继续拆分 f[k-1]=f[k-2] + f[k-3]
                //  即：在 f[k - 1] 的前面继续查找 k--
                //  即：下次循环 mid = f[k - 1 - 1] - 1
                k--;
            } else if (key > arr[mid]) {
                l = mid + 1;
                k -= 2;
            } else {
                //  需要确定，返回的是哪个下标
                if (mid <= r) {
                    return mid;
                } else {
                    return r;
                }
            }
        }
        return -1;
    }
}
```

$$
\frac{0.382}{0.618}=\frac{0.618}{1}=0.618
$$

一个线段分成 2 份

- 其中一部分与全长之比
- 等于另一部分与这部分之比

斐波那契数列：

```bash
# 斐波那契数列的 2个相邻的数的比例
# 无限接近于 0.618

# 34 / 55 = 0.618
{1、1、2、3、5、8、13、21、34、55}
```

斐波那契查找原理与前2种类似，仅仅改变了中间节点 mid 的位置，使得 mid 位于 <font color="yellow">黄金分割点附近</font>
$$
mid=low+F(k-1)-1
$$

公式推导：

由于斐波那契公式可知：$F(k)=F(k-1)+F(k-2)$

- 2侧 同时减去 1：$F(k)-1=F(k-1)+F(k-2)-1$
- 右侧继续处理：$F(k)-1=[F(k-1)-1]+[F(k-2)-1]+1$
- 可以看作是
  - 顺序表的长度为为：$F(k)-1$，划分成3块，mid坐标为：low + 【F(k-1) - 1】
    - 左侧：$F(k-1)-1$
    - mid：1
    - 右侧：$F(k-2)-1$

但是顺序表长度n 不一定恰好 = F(k) - 1，所以需要 <font color="yellow">将原来的顺序表长度n 增加至 F(k )- 1</font>

- 这里的 k 只要能使得 F(k) - 1 恰好 大于或者等于 n 即可

- 由以下代码得到，顺序表长度增加后，新增的位置【从 n + 1 到 F[k - 1]位置】，都赋为 n 位置的值即可

  ```java
  //	n：顺序表长度
  while(n > fib(k) - 1){	//	在能力范围内，找到最好的
      k++;
  }
  ```
  



## 8. 哈希表（散列表）

![image-20230403112602838](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304031126044.png)

哈希表实现方式：【提升查找速度】

- 数组 + 链表
- 数组 + 二叉树

```java
package hashtab;

public class HashTableDemo {
    public static void main(String[] args) {
        HashTab tab = new HashTab(7);
        tab.add(new Emp(1, "爱新觉罗LQ"));
        tab.add(new Emp(2, "JACK"));
        tab.add(new Emp(3, "Alice"));
        tab.add(new Emp(8, "Bob"));
        tab.list();
        System.out.println("===============");
        tab.findEmpById(20);
    }
}

class Emp{
    public int id;
    public String name;
    public Emp next;

    public Emp(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public String toString() {
        return "Emp{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", next=" + next +
                '}';
    }
}

class EmpLinkedList{
    private Emp head;   //  头指针，指向第一个 Emp

    public void add(Emp emp){
        //  如果是添加第一个雇员
        if (head == null){
            head = emp;
            return;
        }
        //  如果不是第一个雇员，则使用一个辅助的指针，帮助定位到最后
        Emp curEmp = head;
        while (curEmp.next != null){
            curEmp = curEmp.next;   //  head 最终指向最后一个
        }
        curEmp.next = emp;
    }

    public void list(){
        if (head == null){
            System.out.println("列表为空");
            return;
        }
        Emp curEmp = head;
        while (curEmp != null){
            System.out.println(curEmp);
            curEmp = curEmp.next;
        }
    }

    public Emp findEmpById(int id){
        if (head == null){
            System.out.println("链表为空");
            return null;
        }
        //  辅助指针
        Emp curEmp = head;
        int index = 0;
        while (curEmp != null){
            if (id == curEmp.id){
                System.out.println("链表的第" + (index + 1) + "位置处找到了");
                return curEmp;
            }
            curEmp = curEmp.next;
            index++;
        }
        return null;
    }

}

//  创建 HashTab 管理多条链表
class HashTab{
    private EmpLinkedList[] tab;  //  管理一个数组，数组元素类型是 EmpLinkedList
    private int size;

    public HashTab(int size){
        this.size = size;
        tab = new EmpLinkedList[size];
        //  如果不初始化的话，tab 每个元素 默认都是 null
        //  所以要初始化
        for (int i = 0; i < tab.length; i++) {
            tab[i] = new EmpLinkedList();
        }
    }

    //  散列函数
    public int hashFun(int id){
        return id % size;
    }

    public void add(Emp emp){
        //  根据员工 id，分析出该员工应当添加到哪条链表
        int EmpLinkedListNo = hashFun(emp.id);  //  链表位置
        tab[EmpLinkedListNo].add(emp);
    }

    public void list(){
        for (int i = 0; i < size; i++) {
            System.out.println("第" + (i + 1) + "行");
            tab[i].list();
        }
    }

    public void findEmpById(int id){
        //  使用散列函数确定到哪条链表去查找
        int ListNo = hashFun(id);
        Emp emp = tab[ListNo].findEmpById(id);
        if (emp!=null){
            System.out.println("在tab第" + (ListNo+1) + "行找到了" + emp);    //  下标从 0 开始
            return;
        }
        System.out.println("没有找到");

    }
}
```

## 9. 树

提高数据存储、读取的效率，比如：二叉排序树（Binary Sort Tree），同时也保证：插入、删除、修改的速度

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

### 9.1 前序【Pre】

#### 递归

```java
//	前序遍历
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        preorder(root, res);
        
        return res;
    }
	//	当前结点不为 null，就按照 root ==> left ==> right 顺序 add
    public void preorder(TreeNode root, List<Integer> res){
        if (root == null) {	//	base case
            return;
         }
        res.add(root.val);
        preorder(root.left, res);
        preorder(root.right, res);
    }
}
```

#### 迭代（栈的 size = 0）

迭代版本：（本质上是在模拟递归，因为在递归的过程中使用了系统栈，所以在迭代的解法中常用`Stack`来模拟系统栈。）

![image-20230404102154536](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304041021735.png)

![image-20230404102024399](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304041020813.png)

思路：

- root 入栈，弹出打印
- 压入该节点的右孩子
- 压入该节点的左孩子

然后循环往复

| 栈内                | 出栈                                            |
| :------------------ | ----------------------------------------------- |
| 1                   | 1（出栈后，先压入它的右儿子，再压入它的左儿子） |
| 3、2                | 2                                               |
| 3、null、4          | 4                                               |
| 3、null、7、null    | null                                            |
| 3、null、7          | 7                                               |
| 3、null、null、null | null、null、null                                |
| 3                   | 3                                               |
| 6、5                | 5                                               |
| 6、null、null       | 6                                               |
| null，8             | 8                                               |
| null、null、null    | null                                            |

```java
class Solution {
     public List<Integer> preorderTraversal(TreeNode root) {
        Deque<TreeNode> s = new LinkedList<>();
        List<Integer> res = new ArrayList<>();
        if (root == null) {	//	//	当 root 为 null 时，返回一个 空数组
            return res;
        }
        s.push(root);
        while(!s.isEmpty()){
            TreeNode cur = s.removeFirst();   //  取出栈顶元素
            if(cur != null){   //  栈顶元素不为 null，弹出栈顶，压入右孩子，压入左孩子
                res.add(cur.val);    
                s.addFirst(cur.right);
                s.addFirst(cur.left);
            }
        }
        return res;
    }
}
```

### 9.2 中序【In】

#### 递归

```java
//	中序遍历
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        inorder(root, res);
        
        return res;
    }
	//	当前结点不为 null，就按照 left ==> root ==> right 顺序 add
    public void inorder(TreeNode root, List<Integer> res){
        if (root == null) {	//	base case
            return;
         }
        inorder(root.left, res);
        res.add(root.val);
        inorder(root.right, res);
    }
}
```

#### 迭代（栈的 size = 0，root 指向的节点为 null）

1. 迭代过程中，会出现 栈为 s.size() = 0 的时候，但只要root 指向的节点不为null 就继续进行入栈操作
2. 同时，当前值为 null 时，说明没有向栈中加入的数了，但是只要栈中还有数，就继续弹出
3. <font color="yellow">**终止条件**</font>：既没有要加入的数，也没有要弹出的数了

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {

        Deque<TreeNode> s = new LinkedList<>();
        List<Integer> res = new ArrayList<>();
        if (root == null){
            return res;
        }

        while(s.size() > 0 || root != null){
            if (root != null) { //  只要左侧有节点，就一直入栈
                s.addFirst(root);
                root = root.left;
            }else{
                TreeNode cur = s.removeFirst(); //  出栈，并返回栈顶的值
                res.add(cur.val);
                root = cur.right;  //   尝试向右寻找，可能为 null
            }
        }
        return res;
    }
}
```

### 9.3 后序【Post】

#### 递归

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        construct_Tree(root, res);
        return res;
    }
    
    public void construct_Tree(TreeNode root, List res){
        if (root == null){
            return;
        }
        //  后序遍历，L R root
        construct_Tree(root.left, res);
        construct_Tree(root.right, res);
        res.add(root.val);
    }
}
```

#### 迭代

思路：

- 左侧无脑入栈
- 出栈的时候，观察栈顶有无右孩子
  - 没有直接出栈
  - 有右孩子，还要看该右孩子是否是刚弹出的栈顶元素【TreeNode r = null，用于暂存 <font color="yellow">刚弹出的栈顶</font>】：只有弹栈时，r 才会更新
    - 该右孩子是刚弹出的栈顶元素，继续弹出栈顶元素
    - 该右孩子不是刚弹出的栈顶元素，压入右孩子

![image-20230404112001551](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304041120825.png)

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> s = new LinkedList<>();  //  定义辅助栈
        TreeNode r = null; // 用于储存刚弹出的栈顶
        
        while(s.size() > 0 || root != null){   //  后序 左 右 根 
            if(root != null){       //  left
                s.push(root);
                root = root.left;
            }else{  //  走到这步说明：root == null
                //  取出栈顶元素 cur
                TreeNode cur = s.peek();
                if(cur.right != null && cur.right != r){  //  栈顶有右儿子，且右儿子不是刚被弹出的节点
                    root = cur.right;
                }else{
                    //  没右孩子直接 弹
                   	//	有右孩子，但是刚弹出的那个就是它的右孩子，也弹
                    r = s.pop();    //  弹栈，并将 r 指向弹出的节点
                    res.add(cur.val);
                }
            }
        }
        return res;
    }
}
```

二叉树的非递归不仅仅是为了解决遍历问题

例如：后序遍历栈中保存的始终是该节点的 父节点，这一性质可以用于解决寻找 <font color="red">**公共祖先** </font>的问题

1. 节点会出现在栈顶 2 次
   - 第一次左节点访问完
   - 第二次右节点访问完
2. 遍历的时候，需要根据  第几次路过  来决定是否访问根节点
3. 解决方法 Solution：
   - 增加一个 r 指针，指向上次访问的节点

### 9.4 查找指定节点

![image-20230404121742762](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304041217931.png)

```java
public class BinaryTreeDemo {
    static List<TreeNode> list = new ArrayList<>();

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1, "宋江");
        TreeNode node1 = new TreeNode(2, "吴用");
        TreeNode node2 = new TreeNode(3, "卢俊义");
        TreeNode node3 = new TreeNode(4, "林冲");
        root.setLeft(node1);
        root.setRight(node2);
        node2.setRight(node3);

        root.postOrder(root, list);
        for (TreeNode treeNode : list) {
            System.out.println(treeNode);
        }
        System.out.println("===========");
        root.findByNo(list, 1);
    }

}

class TreeNode{
    private int no;
    private String name;
    private TreeNode left;
    private TreeNode right;

    public TreeNode() {
    }

    public TreeNode(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public TreeNode getLeft() {
        return left;
    }

    public void setLeft(TreeNode left) {
        this.left = left;
    }

    public TreeNode getRight() {
        return right;
    }

    public void setRight(TreeNode right) {
        this.right = right;
    }

    @Override
    public String toString() {
        return "TreeNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }

    //  前序
    public void preOrder(TreeNode root, List<TreeNode> res){
        if (root == null){
            return;
        }
        res.add(root);
        preOrder(root.left, res);
        preOrder(root.right, res);
    }
    //  中序
    public void inOrder(TreeNode root, List<TreeNode> res){
        if (root == null){
            return;
        }
        inOrder(root.left, res);
        res.add(root);
        inOrder(root.right, res);
    }

    //  后序
    public void postOrder(TreeNode root, List<TreeNode> res){
        if (root == null){
            return;
        }
        postOrder(root.left, res);
        postOrder(root.right, res);
        res.add(root);
    }

    //  前序遍历查找
    public void findByNo(List<TreeNode> res, int no){
        int index = 0;
        for (TreeNode re : res) {
            index++;
            if (no == re.no){
                System.out.println("找到了" + re + " 查找次数为：" + index);
                return;
            }
        }
        System.out.println("没有找到编号为：" + no + "成员");
    }
}
```

### 9.5 二叉树删除节点

规则：

1. 删除的是叶子节点，则删除该节点
2. 如果删除的是非叶子节点，则删除该子树
3. 测试：删除 5号叶子节点 和 3 号子树

![image-20230413101304365](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304131013607.png)

思路：

首先先处理：如果只有一个 root 节点，则等价将二叉树置空。考虑树为 null 的情况

1. 因为我们二叉树是单向的，所以我们判断的是 <font color="yellow">**当前节点的子节点**</font> 是否为待删除节点，而不能去判断当前这个节点是否是待删除节点
2. 如果当前左子树不为空，并且左子节点就是待删除节点，<font color="red">this.left = null</font>，并且就返回【结束递归】
3. 如果当前右子树不为空，并且左子节点就是待删除节点，<font color="red">this.left = null</font>，并且就返回【结束递归】
4. 如果 2、3都未删除节点
   - 就需要向左子树递归删除
   - 如果左子树也没有删除，则向右子树递归删除

```java
public void deleteByNo(int no, TreeNode treeNode){
    if (treeNode.left != null && treeNode.left.no == no){
        treeNode.left = null;
        return;
    }
    if (treeNode.right != null && treeNode.right.no == no){
        treeNode.right = null;
        return;
    }
    //  如果左右子树都没有找到待删除节点，那么需要向左子树递归删除、右子树递归删除
    if (treeNode.left != null){
        deleteByNo(no, treeNode.left);
    }

    if (treeNode.right != null){
        deleteByNo(no, treeNode.right);
    }
}
```

删除后，子节点向上递归

- 如果只有一个，直接替换
- 如果有 2个的话，左节点替换

```java
public void deleteByNo(int no, TreeNode treeNode){

    if (treeNode.left != null && treeNode.left.no == no){   //  在左侧找到待删除节点

        if (treeNode.left.left == null && treeNode.left.right == null){ // 待删除节点就是叶子节点
            treeNode.left = null;
        }else{
            if (treeNode.left.left != null){    //  左侧不为 null
                if (treeNode.left.right != null){   //  左右都有【相当于一个元素插入了链表】
                    treeNode.left.left.right = treeNode.right.right;
                    treeNode.left = treeNode.left.left;
                }else {
                    treeNode.left = treeNode.left.left; //  只有左侧
                }

            }else { //  只有右
                treeNode.left = treeNode.left.right;
            }
        }
        return;
    }


    if (treeNode.right != null && treeNode.right.no == no){
        if (treeNode.right.left == null && treeNode.right.right == null){
            treeNode.right = null;
        }else {
            if (treeNode.right.left != null){
                if (treeNode.right.right != null){   //  左右都有【相当于一个元素插入了链表】
                    treeNode.right.left.right = treeNode.right.right;
                    treeNode.right = treeNode.right.left;
                }else {
                    treeNode.right = treeNode.right.left;   //  只有左侧
                }

            }else { //  只有右节点
                treeNode.right = treeNode.right.right;
            }
        }
        return;
    }
    //  如果左右子树都没有找到待删除节点，那么需要向左子树递归删除、右子树递归删除
    if (treeNode.left != null){
        deleteByNo(no, treeNode.left);
    }

    if (treeNode.right != null){
        deleteByNo(no, treeNode.right);
    }
}
```

![image-20230413133354282](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304131333438.png)

这里只会考虑倒数第二层节点的情况！！！【后序在 BST 中还会深入】

后序遍历：【2、4、5、1】

![image-20230413133738481](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304131337645.png)



### 9.6 顺序存储二叉树

顺序存储二叉树

- 数组的方式存放 【1、2、3、4、5、6、7】
- 要求i遍历数组时，仍然 前、中、后序遍历完成！！！

![image-20230405114141700](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304051141849.png)

顺序存储二叉树特点：（n：表示：二叉树中的第几个元素，从 0 开始编号）

- 通常只考虑完全二叉树
- 第 n 个 元素的左子节点为 ：2 * n + 1
- 第 n 个 元素的右子节点为：2 * n + 2

实际应用：堆排序

```java
package Tree;

public class ArrBinaryTreeDemo {

    static int[] arr = {1, 2, 3, 4, 5 ,6 ,7};

    public static void main(String[] args) {
        ArrBinaryTree arrBinaryTree = new ArrBinaryTree(arr);
        arrBinaryTree.preOrder(0);
        System.out.println();
        arrBinaryTree.inOrder(0);
        System.out.println();
        arrBinaryTree.postOrder(0);

    }
}

//  编写一个 ArrayBinaryTree 实现顺序存储二叉树遍历

class ArrBinaryTree{
    private int[] arr;

    public ArrBinaryTree(int[] arr) {
        this.arr = arr;
    }

    //  编写一个方法，完成顺序存储二叉树前序遍历
    public void preOrder(int index){
        if (arr == null || arr.length == 0){
            System.out.println("数组为空，不能按照二叉树遍历");
            return;
        }
        //  输出当前元素
        System.out.print(arr[index] + " ");
        if (2 * index + 1 < arr.length){
            preOrder(2 * index + 1);    //  向左递归
        }
        if (2 * index + 2 < arr.length){
            preOrder(2 * index + 2);   //  向右递归
        }
    }

    public void inOrder(int index){
        if (arr == null || arr.length == 0){
            System.out.println("数组为空，不能按照二叉树遍历");
            return;
        }
        //  输出当前元素
        if (2 * index + 1 < arr.length){
            inOrder(2 * index + 1);    //  向左递归
        }
        System.out.print(arr[index] + " ");
        if (2 * index + 2 < arr.length){
            inOrder(2 * index + 2);   //  向右递归
        }
    }

    public void postOrder(int index){
        if (arr == null || arr.length == 0){
            System.out.println("数组为空，不能按照二叉树遍历");
            return;
        }
        //  输出当前元素
        if (2 * index + 1 < arr.length){
            postOrder(2 * index + 1);    //  向左递归
        }
        if (2 * index + 2 < arr.length){
            postOrder(2 * index + 2);   //  向右递归
        }
        System.out.print(arr[index] + " ");
    }
}
```

### 9.7 线索化二叉树 【n + 1 个空闲指针】

先看一个问题：

```bash
[1, 3, 6, 8, 10, 14] # 构建成一个二叉树
```

![image-20230412152631370](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304121526631.png)

问题分析：

1. 当我们对上面的二叉树中序遍历，数列为：8、3、1、14、6
2. 发现叶子节点 6、8、10、14 这几个节点的左右指针，并没有完全得利用上
3. 如果我们想充分利用各个节点得左右指针，让 <font color="yellow">**各个节点可以指向** </font>自己的 <font color="red">**前后节点**</font> 该怎么办？
4. 实现方法：线索二叉树

线索化二叉树基本介绍：

1. n个节点的二叉树中含有 n + 1 个空指针域	
   - 每个节点都有 2 个指针：n * 2
   - 非空指针就是二叉树边的个数：n - 1
   - 所以空指针个数为：n * 2 - (n - 1) = n + 1
2. 利用这些空指针域，存放指向该节点在某种遍历次序下的前驱和后继节点的指针【这些附加的指针称为 <font color="yellow">线索</font>】
3. 这种加上了线索的二叉链表称为线索链表，相应的二叉树称为线索二叉树（Thread BinaryTree）
4. 根据线索性质的不同，线索二叉树可以分为：
   - 前序线索二叉树
   - 中序线索二叉树
   - 后序线索二叉树
5. 一个节点的前一个节点，称为前驱节点
6. 一个节点的后一个节点，称为后继节点

线索化二叉树应用实例：

```bash
[1, 3, 6, 8, 10, 14] # 构建成中序搜索二叉树
```

![image-20230412154927535](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304121549656.png)

说明：当线索化二叉树后，Node 节点的属性 left 和 right，有如下情况：

1. left 指向的是左子树，也可能是指向的前驱节点
2. right 指向的是右子树，也可能是指向的后继节点

处理后继节点的时机：在下一次递归时，处理后继节点

![image-20230413154653165](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304131546320.png)

现在编写中序线索化二叉树方法：

```java

class TreeNode{
    private int no;
    private String name;
    private TreeNode left;
    private TreeNode right;

    private int leftType;   //  0：左子树，1：前驱节点
    private int rightType;  //  0：右子树，1：后继节点

    //  为了实现线索化，需要创建指向 【当前节点的前驱节点】的指针
    private TreeNode pre;   //  pre 总是保留前一个节点

    public void threadedNodes(TreeNode treeNode){
        if (treeNode == null){
            return;
        }

        //  1. 先线索化左子树
        threadedNodes(treeNode.left);

        //  2. 线索化当前节点
        //  2.1 看当前节点的前驱节点
        if (treeNode.left == null){
            //  就让当前节点的左指针指向前驱节点
            treeNode.left = pre;
            //  修改当前节点左指针类型
            treeNode.leftType = 1;
        }
        //  2.2 处理后继节点
        if (pre != null && pre.right == null){
            pre.right = treeNode;
            pre.rightType = 1;
        }

        //  2.3 每处理一个节点后，让当前节点是下一个节点的前驱节点
        pre = treeNode;

        //  3. 再线索化右子树
        threadedNodes(treeNode.right);

    }

 
    @Override
    public String toString() {
        return "TreeNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }
}
```

![image-20230413171314940](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304131713117.png)

由上图可知，线索化二叉树为无线循环的，所以 toString（）方法中不能带有 TreeNode属性，否则会造成栈溢出

#### 中序

![image-20230413171932392](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304131719553.png)

中序情形下：最左侧无前驱，最右侧无后继

线索化二叉树遍历：

```java
//  遍历线索化二叉树方法 【中序：左、根、右】
public void threadedList(TreeNode treeNode){
    //  定义一个变量，存储当前遍历节点，从 root 开始
    TreeNode cur = treeNode;

    //  循环的找到 leftType = 1 的节点
    while (cur != null){
        while (cur.leftType == 0){
            cur = cur.left;
        }
        // 打印当前节点
        System.out.println(cur);
        //  如果当前节点的右指针为后继节点，就一直输出
        while (cur.rightType == 1){ // 【右上】
            cur = cur.right;
            System.out.println(cur);
        }
        //  替换遍历的节点
        cur = cur.right;	//	【右下】
    }
}
```

![image-20230413194013890](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304131940081.png)

leftType = 1 的条件是 treeNode.left == null

所以：6 号节点的 treeNode.leftType = 0

同时它的 treeNode.rightType也 = 0

#### 前序

```java
public void preThreadedNodes(TreeNode treeNode){
    if (treeNode == null){
        return;
    }
    //  1. 线索化当前节点
    //  1.1 看当前节点的前驱节点
    if (treeNode.left == null){
        //  就让当前节点的左指针指向前驱节点
        treeNode.left = pre;
        //  修改当前节点左指针类型
        treeNode.leftType = 1;
    }
    //  1.2 处理后继节点
    if (pre != null && pre.right == null){
        pre.right = treeNode;
        pre.rightType = 1;
    }

    //  1.3 每处理一个节点后，让当前节点是下一个节点的前驱节点
    pre = treeNode;

    //  2. 那么继续向左，找到符合可以线索化的节点
    //  因为先处理的自己，如果 left == null，就已经线索化了
    //  再向左的话，就不能直接进入了
    //  需要判定，如果不是线索化节点，再进入
    //  比如：当前节点 8，前驱 left 被设置为了 3
    //  这里节点 8 的 left 就为 1 了，就不能继续递归，否则又回到了节点 3 上
    //  导致死循环了
    if (treeNode.leftType == 0) {
        threadedNodes(treeNode.left);
    }

    //  3. 再线索化右子树
    if (treeNode.rightType == 0) {
        threadedNodes(treeNode.right);
    }
}
```

遍历：

```java
public void preOrderThreadeList(TreeNode treeNode) {
    TreeNode node = treeNode;
    // 最后一个节点无后继节点，就会退出了
    // 前序：自己、左（递归）、右（递归）
    while (node != null) {
        // 先打印自己
        System.out.println(node);
   
        while (node.leftType == 0) {	//	一路向左
            node = node.left;
            System.out.println(node);
        }
        while (node.rightType == 1) {
            node = node.right;
            System.out.println(node);
        }
        node = node.right;
    }
}
```

![image-20230414115456854](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304141154080.png)

由于后继节点是由 pre 指向的，而 节点 14 没有做 pre 的机会，所以 14 的后继指针指向 null

![image-20230414120536420](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304141205591.png)

#### 后序

```java
private HeroNode parent; //默认null,父节点的指针（为了后序线索化使用）
```

后序遍历思路：

1. 遍历的起点：根节点左子树最左边的节点。该节点不一定是第一个打印数据的节点【8】，该节点可能存在右子树，存在右子树要遍历右子树。

2. 后继节点：

   - 右后继节点：一直遍历该节点的后继节点

   - 没有后继节点就判断是不是根节点
     - 根节点没有右子树时就会出现这种情况
     - 如果不是根节点，那就找到节点的父亲

![image-20230414123835029](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304141238192.png)

```java
public void postOrderThreadedList(TreeNode treeNode){
    if (treeNode == null){
        return;
    }

    TreeNode node = treeNode;

    TreeNode prevNode = null;

    while (node != null){
        while (node.left != null && node.leftType == 0){        //循环到最左节点
            node = node.left;
        }

        while (node != null && node.rightType == 1){    //循环后继节点
            System.out.print(node);
            prevNode = node;    //  记录刚处理完的节点
            node = node.right;	//	【3】
        }

        if (node == treeNode){  //  是否到达根节点
            System.out.println(node);
            return;
        }

        while (node != null && node.right == prevNode){    //判断到根节点 【判断右节点是否为刚处理过的节点】
            System.out.print(node);
            prevNode = node;
            node = node.parent; //回到父节点
        }

        if (node !=null && node.rightType == 0){  // 开始遍历根节点的右子树 【根节点有右子树】
            node = node.right;
        }
    }
}
```

```bash
TreeNode{no=8, name='mary', leftType=1, rightType=1}TreeNode{no=10, name='king', leftType=1, rightType=1}TreeNode{no=3, name='jack', leftType=0, rightType=0}TreeNode{no=14, name='dim', leftType=1, rightType=1}TreeNode{no=6, name='smith', leftType=0, rightType=1}TreeNode{no=1, name='tom', leftType=0, rightType=0}
```

```java
public void postThreadedNodes(TreeNode treeNode){
    if (treeNode == null){
        return;
    }
    
    if(treeNode.leftType == 0) {	//	左
        postThreadedNodes(treeNode.left);
    }

    if (treeNode.rightType == 0) {	//	右
        postThreadedNodes(treeNode.right);
    }


    //  1. 线索化当前节点
    //  1.1 看当前节点的前驱节点
    if (treeNode.left == null){
        //  就让当前节点的左指针指向前驱节点
        treeNode.left = pre;
        //  修改当前节点左指针类型
        treeNode.leftType = 1;
    }
    //  1.2 处理后继节点
    if (pre != null && pre.right == null){
        pre.right = treeNode;
        pre.rightType = 1;
    }

    //  1.3 每处理一个节点后，让当前节点是下一个节点的前驱节点
    pre = treeNode;
}
```

https://blog.csdn.net/weixin_43362650/article/details/105786001

递归方式遍历

https://blog.csdn.net/Kevinnsm/article/details/120534223

## 10. 堆

是一种选择排序，它的最好、最坏、平均复杂度均为 $O(nlogn)$

以大顶堆为例：

1. 构建大顶堆：heapInsert（）构建大顶堆【从下至上】
2. 排序：heapify()【从上至下】
   - 将堆顶与最后一个交换
   - 从上至下
   - 比 2个儿子小，与儿子中最大值交换
   - 一直到没有儿子比自己大

```java
package heap;

public class bigHeap {
    public static void main(String[] args) {
        int[] arr = {5, 7, 0 , 6, 8};
        for (int i = 0; i < arr.length; i++) {
            heapInsert(arr, i);
        }
        int heapSize = arr.length;
        swap(arr, 0, --heapSize);	//	0 位置和最后一个位置交换，并且堆的大小 - 1
        while (heapSize > 0) {
            heapify(arr, 0, heapSize);	//	调整成 大根堆
            swap(arr, 0, --heapSize);
        }
        printArr(arr);
    }

    public static void heapInsert(int[] arr, int index) {
        while (arr[index] > arr[(index - 1) / 2]) {
            swap(arr, index, (index - 1) / 2);
            index = (index - 1) / 2;
        }
    }
    
    public static void heapify(int[] arr, int index, int heapSize) {
        int left = index * 2 + 1;	//	左孩子
        while (left < heapSize) {	//	左孩子不越界
            int largest = left + 1 < heapSize && arr[left + 1] > arr[left] ? left + 1 : left;	//	右孩子不越界	[largest：孩子中最大值下标]
            largest = arr[largest] > arr[index] ? largest : index;	//	我 和 我 2 个孩子中，最大值下标
            if (largest == index) {
                break;	//	我就是最大的，无需交换了
            }
            swap(arr, largest, index);
            index = largest;
            left = index * 2 + 1;
        }
    }

    public static void swap(int[] arr, int i, int j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }

    public static void printArr(int[] arr){
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

大顶堆最后实现的排序是：升序排列

## 11. 霍夫曼树

1. 给定 n 个权值作为 n 个 <font color="yellow">**叶子** </font>节点，构造一棵二叉树，若该树的带权路径长度（WPL）达到最小。称这样的二叉树为最优二叉树，也成为霍夫曼树
2. 霍夫曼树是带权路径长度最短的树，权值较大的节点离根较近

霍夫曼树构建：

1. 从小到大排序，将每一个数据视为一个节点，每个节点可以看成是一个简单的二叉树

2. 取出根节点权值最小的两棵二叉树

3. 组成一个新的二叉树，该新的二叉树的根节点的权值是前面2棵二叉树根节点权值的和

4. 再将这棵新的二叉树，以根节点的权值大小再次排序

   不断重复，直到数列中，所有的数据都被处理，就得到一棵 霍夫曼树

![jiq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304151924860.png)

![image-20230415204617050](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304152046499.png)

```java
package huffmantree;

public class HuffmanTree {

    static int[] arr = {13, 7, 8, 3, 29, 6, 1};
    static List<Node> list = new ArrayList<>();

    static List<Integer> list1 = new ArrayList<>(); //  前序遍历使用


    public static void main(String[] args) {
        Node huffmanTree = createHuffmanTree(arr);
        System.out.println(huffmanTree);

        preOrder(huffmanTree, list1);
        System.out.println(list1);


    }

    //  构建霍夫曼树方法
    public static Node createHuffmanTree(int[] arr){
        for (int i : arr) {
            Node node = new Node(i);
            list.add(node);
        }
        Collections.sort(list); //  1. 排序完成
        //  2. 开始第二步操作了！！！
        while (list.size() > 1){
            //  取出最小的2个节点，合成一个节点
            Node nodeLeft = list.get(0);
            Node nodeRight = list.get(1);
            int value = nodeLeft.getValue() + nodeRight.getValue();
            Node parent = new Node(value);
            parent.setLeft(nodeLeft);
            parent.setRight(nodeRight);

            list.remove(nodeLeft); //  将左、右孩子弹出
            list.remove(nodeRight);
            list.add(parent);   //  传入新节点
            Collections.sort(list);
        }
        return list.get(0);
    }

    public static void preOrder(Node root, List<Integer> res){
        if (root == null) {	//	base case
            return;
        }
        res.add(root.getValue());
        preOrder(root.getLeft(), res);
        preOrder(root.getRight(), res);
    }
}


//  为了让 Node对象持续排序，Collections 集合排序
//  让 Node 实现 Comparable 接口
class Node implements Comparable<Node> {
    private int value;
    private Node left;
    private Node right;

    public Node(int value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }

    @Override
    public int compareTo(Node o) {
        return this.value - o.value;
    }

    public int getValue() {
        return value;
    }

    public void setValue(int value) {
        this.value = value;
    }

    public Node getLeft() {
        return left;
    }

    public void setLeft(Node left) {
        this.left = left;
    }

    public Node getRight() {
        return right;
    }

    public void setRight(Node right) {
        this.right = right;
    }
}
```



## 12. 霍夫曼编码【补码 <=> 原码】40 ---> 17（压缩）

通信领域中信息的处理方式：

- 定长编码【359】
  - 转成 ASCII码 ===> 二进制
- 变长编码 【多义性】
  - 按照各个字符出现的个数进行编码
  - 原则是：出现次数越多的，则编码越小，比如：空格出现 9 次，则编码为 0 ，其它以此类推

变长编码举例子

```bash
# 40 个字符
i like like like java do you like a java
```

| 字符 | 个数 |
| ---- | ---- |
| i    | 5    |
| 空格 | 9    |
| l    | 4    |
| k    | 4    |
| e    | 4    |
| j    | 2    |
| a    | 5    |
| v    | 2    |
| d    | 1    |
| o    | 2    |
| y    | 1    |
| u    | 1    |

按照二进制进行从大到小重排：

| 字符 | 编码 |
| ---- | ---- |
| 空格 | 0    |
| a    | 1    |
| i    | 10   |
| e    | 11   |
| k    | 100  |
| l    | 101  |
| o    | 110  |
| v    | 111  |
| j    | 1000 |
| u    | 1001 |
| y    | 1010 |
| d    | 1011 |

所以编码变为：

```bash
i like like like java do you like a java
```

![image-20230416125024230](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304161250612.png)

字符的编码都不能是其它字符编码的前缀，符合此要求的编码叫做前缀编码，即不能匹配到重复的编码 

前缀编码可以解决多义性

霍夫曼编码是一种前缀编码，构造方式如下：

![jiq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304161331282.png)

代码实现：

1. Node【data（存放数据）、weight（权重：个数）、left、right】
2. 得到 "i like like like java do you like a java"  对应的字符数组 byte[]
3. 编写一个方法，将准备构建 霍夫曼树的 Node 放入到 List
4. 可以通过 List 创建对应的 Huffman 树

因为顺序的不同，生成的 Huffman树可能不同，因为有相同权重的节点

只要最后的 <font color="yellow">**霍夫曼编码的长度** </font>符合就可以

```java
package huffmantree;

public class HuffmanCode {

    static List<HuffmanNode> list = new ArrayList<>();

    static HashMap<String , Integer> map = new HashMap<>();    //  字符 <==> 个数

    static HashMap<String , String> codeMap = new HashMap<>();    //  字符 <==> Huffman码

    public static void main(String[] args) {
        String str = "i like like like java do you like a java";
        for (char c : str.toCharArray()) {  //  利用 map 统计次数
            String s = c + "";
            if (!map.containsKey(s)){
                map.put(s, 1);
            }else {
                map.put(s, map.get(s) + 1);
            }
        }
        for (String s : map.keySet()) { //  将 Key 和 value 包装成 一个 HuffmanNode 对象，放入到 list 中去
            list.add(new HuffmanNode(s, map.get(s)));
        }
        Collections.sort(list); //  从小到大排序 ！！！
        System.out.println(list);

        //  开始构建 Huffman 树
        while (list.size() > 1){
            HuffmanNode huffmanNode1 = list.get(0);
            HuffmanNode huffmanNode2 = list.get(1);
            int weight = huffmanNode1.getWeight() + huffmanNode2.getWeight();
            HuffmanNode huffmanNodeParent = new HuffmanNode(null, weight);
            huffmanNodeParent.setLeft(huffmanNode1);
            huffmanNodeParent.setRight(huffmanNode2);
            list.remove(huffmanNode1);
            list.remove(huffmanNode2);
            list.add(huffmanNodeParent);
            Collections.sort(list);
        }
        System.out.println(list);
        HuffmanNode root = list.get(0);
        System.out.println(root.getLeft().getWeight());
        System.out.println(root.getRight().getWeight());

        System.out.println("前序遍历如下：");
        preorder(list.get(0));

        System.out.println("得到 HuffMan 编码");
        getHuffmanCode(list.get(0), "");
        for (String s : codeMap.keySet()) {
            System.out.println(s + " " + codeMap.get(s));
        }

    }

    //  前序遍历
    //	当前结点不为 null，就按照 root ==> left ==> right 顺序 add
    public static void preorder(HuffmanNode root){
        if (root == null) {	//	base case
            return;
        }
        System.out.println(root);
        preorder(root.left);
        preorder(root.right);
    }

    //  生成霍夫曼树编码
    //  在生成 Huffman 编码过程中，需要去拼接路径，定义一个 StringBuilder，存储叶子节点路径

    /**
     * 将传入的所有的叶子节点得到 Huffman 编码，并放入到 codeMap 集合中
     * @param huffmanNode 传入节点
     * @param code 路径：左 0，右 1
     * @return
     */
    public static void getHuffmanCode(HuffmanNode huffmanNode, String code){
        StringBuilder sb = new StringBuilder();
        sb.append(code);
        if (huffmanNode != null){
            if (huffmanNode.getData() == null){ //  非叶子节点
                getHuffmanCode(huffmanNode.left, code + "0");
                getHuffmanCode(huffmanNode.right, code + "1");
            }else { //  叶子节点
                codeMap.put(huffmanNode.getData(), sb+"");
            }
        }
    }
}

class HuffmanNode implements Comparable<HuffmanNode>{
    private String  data;
    private int weight;
    HuffmanNode left;
    HuffmanNode right;
}
```

![image-20230416201050800](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304162010034.png)

将字符串通过生成的 Huffman 编码表，返回压缩后的字节数组

```java
public static void zip(char[] chars, HashMap<String, String> codeMap){
    StringBuilder sb = new StringBuilder();
    for (char aChar : chars) {
        sb.append(codeMap.get(aChar+""));
    }
    System.out.println(sb);
}
```

```bash
# 压缩成 Huffman 编码如下：

# 8 位对应 1个 Byte
# 最后放入到 byte[] huffmanCodeBytes 中
1010100010111111110010001011111111001000101111111100100101001101110001110000011011101000111100101000101111111100110001001010011011100	# 长度：133 ===> 133 / 8 = 17
```

计算机中存储的是补码，而控制台打印出来的源码

所以要：补码 ===> 源码

- 正数：三码合一
- 负数
  - 反码：符号位不变，其余取反
  - 补码：反码 + 1

以前 8 位 为例

```bash
10101000 # 符号位为：1 
# 反码
10100111
# 原码
11011000 # -88
```

以上部分不用手动实现 ===> 在程序中，只要将 int ===> byte[] 即可！！！

```java
bytes[index++] = (byte) Integer.parseInt(strBytes, 2); 
```

```java
public static byte[] zip(char[] chars, HashMap<String, String> codeMap){

    StringBuilder sb = new StringBuilder();
    for (char aChar : chars) {
        sb.append(codeMap.get(aChar+""));   //  133 位都存储到 sb 中去
    }
    int length = sb.length() % 8 == 0 ? sb.length() / 8 :sb.length() / 8 + 1;
    byte[] bytes = new byte[length];  //  最终的字节数组

    int index = 0;  //  记录是第几个变量
    for (int i = 0; i < sb.length(); i += 8) {
        //  够 8 位的话，就取出 8 位，否则就有几位就取出几位
        String strBytes = (i + 8) > sb.length() ? sb.substring(i) : sb.substring(i, i + 8);
        bytes[index++] = (byte) Integer.parseInt(strBytes, 2);
    }
    return bytes;
}
```

![image-20230417111233639](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304171112998.png)

数据解压！！！

1. 将 byte[] byte 转回成字符串
2. 对照 huffman 编码重新转回 字符串

```java
//  将一个 byte 转成 二进制字符串
public static String byteToBitString(byte b, boolean flag){
    int temp = b;
    //  如果是正数，还存在补高位的问题
    if (flag){
        temp |= 256;
    }


    String string = Integer.toBinaryString(temp);    //  返回的是二进制对应的补码【int：4个字节 ===> 32 位】

    if (flag){
        return string.substring(string.length() - 8);
    }

    return string;  //  不需要补位的情况！！！
}
```

```java
byte[] zip = zip(str.toCharArray(), codeMap);
StringBuilder decoder = new StringBuilder();

for (int i = 0; i < zip.length; i++) {
    //  判断是不是最后一个字节
    boolean flag = (i != zip.length - 1);   //  最后一位时，不补位
    decoder.append(byteToBitString(zip[i], flag));
}
System.out.println("解码如下：");
System.out.println(decoder);
```

还原成初始值：初心不忘

```java
StringBuilder result = new StringBuilder();
StringBuilder temp = new StringBuilder();
//  遍历 decoder ，并还原成初始的样子
//  制作 decodeMap
for (Map.Entry<String, String> stringStringEntry : codeMap.entrySet()) {
    decodeMap.put(stringStringEntry.getValue(), stringStringEntry.getKey());
}

for (int i = 0; i < decoder.length(); i++) {
    temp.append(decoder.substring(i, i + 1));
    if (decodeMap.containsKey(temp+"")){
        result.append(decodeMap.get(temp+""));
        temp = new StringBuilder();
    }
}

System.out.println(result);
```

代码整合一下：

```java
package huffmantree;

public class HuffmanCode {

    static List<HuffmanNode> list = new ArrayList<>();

    static HashMap<String , Integer> map = new HashMap<>();    //  字符 data <==> 个数【weight】用户构建 huffman树

    static HashMap<String , String> codeMap = new HashMap<>();    //  字符 <==> Huffman码

    static HashMap<String , String> decodeMap = new HashMap<>();    //   Huffman码 <==> 字符

    public static void main(String[] args) {
        String str = "i like like like java do you like a java";
        HuffmanNode root = getHuffmanNode(str);

        System.out.println("得到 HuffMan 编码");
        String huffmanCode = getHuffmanCode(str, root, "");
        for (String s : codeMap.keySet()) {
            System.out.println(s + " " + codeMap.get(s));
        }

        System.out.println("Huffman 编码如下：");
        System.out.println(huffmanCode);

        System.out.println("每 8 位1个，进行压缩");
        byte[] zip = zip(str.toCharArray(), codeMap);
        for (byte b : zip) {
            System.out.print(b + " ");
        }

        System.out.println();

        String decoder = unZip(zip);
        System.out.println("编码解压缩：");
        System.out.println(decoder);

        System.out.println("回归初心");
        String s = huffmanCodeToStr(decoder);
        System.out.println(s);
    }
    
     public static HuffmanNode getHuffmanNode(String str){
        for (char c : str.toCharArray()) {
            String s = c + "";
            if (!map.containsKey(s)){
                map.put(s, 1);
            }else {
                map.put(s, map.get(s) + 1);
            }
        }

        for (String s : map.keySet()) { //  将 Key 和 value 包装成 一个 HuffmanNode 对象，放入到 list 中去
            list.add(new HuffmanNode(s, map.get(s)));
        }
        Collections.sort(list); //  从小到大排序 ！！！
        System.out.println(list);

        //  开始构建 Huffman 树
        while (list.size() > 1){
            HuffmanNode huffmanNode1 = list.get(0);
            HuffmanNode huffmanNode2 = list.get(1);
            int weight = huffmanNode1.getWeight() + huffmanNode2.getWeight();
            HuffmanNode huffmanNodeParent = new HuffmanNode(null, weight);
            huffmanNodeParent.setLeft(huffmanNode1);
            huffmanNodeParent.setRight(huffmanNode2);
            list.remove(huffmanNode1);
            list.remove(huffmanNode2);
            list.add(huffmanNodeParent);
            Collections.sort(list);
        }
        System.out.println(list);
        HuffmanNode root = list.get(0);
        System.out.println(root.getLeft().getWeight());
        System.out.println(root.getRight().getWeight());

        System.out.println("前序遍历如下：");
        preorder(list.get(0));

        return list.get(0);
    }

    public static String huffmanCodeToStr(String decoder){
        StringBuilder result = new StringBuilder();
        StringBuilder temp = new StringBuilder();
        //  遍历 decoder ，并还原成初始的样子
        //  制作 decodeMap
        for (Map.Entry<String, String> stringStringEntry : codeMap.entrySet()) {
            decodeMap.put(stringStringEntry.getValue(), stringStringEntry.getKey());
        }

        for (int i = 0; i < decoder.length(); i++) {
            temp.append(decoder, i, i + 1);
            if (decodeMap.containsKey(temp+"")){
                result.append(decodeMap.get(temp+""));
                temp = new StringBuilder();
            }
        }
        return result+"";
    }



    //  生成霍夫曼树编码
    //  在生成 Huffman 编码过程中，需要去拼接路径，定义一个 StringBuilder，存储叶子节点路径

    /**
     * 将传入的所有的叶子节点得到 Huffman 编码，并放入到 codeMap 集合中
     * @param huffmanNode 传入节点
     * @param code 路径：左 0，右 1
     * @return
     */
    public static String getHuffmanCode(String str, HuffmanNode huffmanNode, String code){
        StringBuilder sb = new StringBuilder();
        sb.append(code);
        if (huffmanNode != null){
            if (huffmanNode.getData() == null){ //  非叶子节点
                getHuffmanCode(str, huffmanNode.left, code + "0");
                getHuffmanCode(str, huffmanNode.right, code + "1");
            }else { //  叶子节点
                codeMap.put(huffmanNode.getData(), sb+"");
            }
        }
        StringBuilder huffmanCode = new StringBuilder();
        char[] chars = str.toCharArray();
        for (char aChar : chars) {
            huffmanCode.append(codeMap.get(aChar+""));
        }
        return huffmanCode+"";
    }
    
    public static byte[] zip(char[] chars, HashMap<String, String> codeMap){

        StringBuilder sb = new StringBuilder();

        for (char aChar : chars) {
            sb.append(codeMap.get(aChar+""));    //  133 位都存储到 sb 中去
        }

        //  每8个一组，进行压缩
        int length = sb.length() % 8 == 0 ? sb.length() / 8 :sb.length() / 8 + 1;
        byte[] result = new byte[length];  //  最终的字节数组

        int index = 0;  //  记录是第几个变量
        for (int i = 0; i < sb.length(); i += 8) {
            //  够 8 位的话，就取出 8 位，否则就有几位就取出几位
            String strBytes = (i + 8) > sb.length() ? sb.substring(i) : sb.substring(i, i + 8);
            result[index++] = (byte) Integer.parseInt(strBytes, 2);
        }
        return result;
    }

    //  将一个 byte 转成 二进制字符串
    public static String byteToBitString(byte b, boolean flag){
        int temp = b;
        //  如果是正数，还存在补高位的问题
        if (flag){
            temp |= 256; //	1 0000 0000
        }

        String string = Integer.toBinaryString(temp);    //  返回的是二进制对应的补码【int：4个字节 ===> 32 位】

        if (flag){
            return string.substring(string.length() - 8);
        }

        return string;  //  不需要补位的情况！！！
    }

    public static String unZip(byte[] zip){
        StringBuilder decoder = new StringBuilder();

        for (int i = 0; i < zip.length; i++) {
            //  判断是不是最后一个字节
            boolean flag = (i != zip.length - 1);   //  最后一位时，不补位
            decoder.append(byteToBitString(zip[i], flag));
        }
        return decoder+"";
    }

  
}

class HuffmanNode implements Comparable<HuffmanNode>{
    private String  data;
    private int weight;
    HuffmanNode left;
    HuffmanNode right;
}
```



文件压缩：

给一个图片 ===> 进行无损压缩

```java
//  编写方法，将一个文件压缩
public static void zipFile(String srcFile, String descFile) {
    FileInputStream is = null;
    //  输出流
    OutputStream os = null;
    ObjectOutputStream oos = null;
    try {
        is = new FileInputStream(srcFile);
        //  创建一个和源文件大小一样的 byte[]
        byte[] bytes = new byte[is.available()];
        is.read(bytes);
        //  使用 Huffman 编码进行压缩
        StringBuilder sb = new StringBuilder();
        for (byte aByte : bytes) {
            sb.append(aByte+"");
        }
        String s = sb + "";
        String huffmanCode = getHuffmanCode(s, getHuffmanNode(s), "");
        byte[] zip = zip(huffmanCode.toCharArray(), codeMap);

        //  创建文件的输出流，存放压缩文件

        os = new FileOutputStream(descFile);
        oos = new ObjectOutputStream(os);
        //  这里以对象流的形式，写入 huffman 编码 ===> 为了后面恢复源文件使用
        oos.writeObject(bytes);
        //  注意一定要把 Huffman 编码写入 压缩文件
        oos.writeObject(codeMap);

    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            is.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

文件解压：

![image-20230418153601153](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304181536412.png)

如果最后是 01001 ===>  那么最后转出来的数就是 1001 了

```java
package huffmantree;

public class HuffmanCode {
    static StringBuilder sbLastLength = new StringBuilder();


    static List<HuffmanNode> list = new ArrayList<>();

    static HashMap<Byte , Integer> map = new HashMap<>();    //  字符 <==> 个数

    static HashMap<Byte , String> codeMap = new HashMap<>();    //  字符 <==> Huffman码

    static HashMap<String , Byte> decodeMap = new HashMap<>();    //   Huffman码 <==> 字符

    public static void main(String[] args) {
        System.out.println("图片压缩");
        zipFile("D:/T2.bmp", "D:/dst.zip");
        unZipFile("D:/dst.zip", "D:/src.bmp");
    }

    //  编写方法，解压文件
    public static void unZipFile(String zipFile, String dstFile){
        //  定义文件输入流
        InputStream is = null;
        //  定义一个对象输入流
        ObjectInputStream ois = null;
        //  定义文件输出流
        OutputStream os = null;

        try {
            is = new FileInputStream(zipFile);
            ois = new ObjectInputStream(is);
            //  读取 byte[] 数组
            byte[] bytes = (byte[])ois.readObject();

            byte[] bytes2 = unZip(bytes);   //  这个只是解压后的霍夫曼编码

            os = new FileOutputStream(dstFile);
            os.write(bytes2);

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                os.close();
                ois.close();
                is.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    //  编写方法，将一个文件压缩
    public static void zipFile(String srcFile, String descFile) {
        FileInputStream is = null;
        //  输出流
        OutputStream os = null;
        ObjectOutputStream oos = null;
        try {
            is = new FileInputStream(srcFile);
            //  创建一个和源文件大小一样的 byte[]
            byte[] bytes = new byte[is.available()];

            is.read(bytes);

            for (byte aByte : bytes) {
                System.out.print(aByte + " ");
            }

            System.out.println(" ");

            String huffmanCode = getHuffmanCode(bytes, getHuffmanNode(bytes), "");

            System.out.println("huffmanCode=" + huffmanCode);

            byte[] zip = zip(bytes, codeMap);
            System.out.println("长度为：" + zip.length);
            System.out.println("霍夫曼编码后的字符串");
            for (byte b : zip) {
                System.out.print(b + " ");
            }

            System.out.println();

            for (Byte aByte : codeMap.keySet()) {
                System.out.println(aByte + "==" + codeMap.get(aByte));
            }

            //  创建文件的输出流，存放压缩文件
            os = new FileOutputStream(descFile);
            oos = new ObjectOutputStream(os);
            //  这里以对象流的形式，写入 huffman 编码 ===> 为了后面恢复源文件使用

            oos.writeObject(zip);
            //  注意一定要把 Huffman 编码写入 压缩文件

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                is.close();
                oos.close();
                os.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    public static HuffmanNode getHuffmanNode(byte[] bytes){
        for (byte aByte : bytes) {
            if (!map.containsKey(aByte)){
                map.put(aByte, 1);
            }else {
                map.put(aByte, map.get(aByte) + 1);
            }
        }
        for (Byte aByte : map.keySet()) {
            list.add(new HuffmanNode(aByte, map.get(aByte)));
        }

        Collections.sort(list); //  从小到大排序 ！！！
        System.out.println(list);

        //  开始构建 Huffman 树
        while (list.size() > 1){
            HuffmanNode huffmanNode1 = list.get(0);
            HuffmanNode huffmanNode2 = list.get(1);
            int weight = huffmanNode1.getWeight() + huffmanNode2.getWeight();
            HuffmanNode huffmanNodeParent = new HuffmanNode(null, weight);
            huffmanNodeParent.setLeft(huffmanNode1);
            huffmanNodeParent.setRight(huffmanNode2);
            list.remove(huffmanNode1);
            list.remove(huffmanNode2);
            list.add(huffmanNodeParent);
            Collections.sort(list);
        }
        System.out.println(list);
        HuffmanNode root = list.get(0);

        return list.get(0);
    }

    //  前序遍历
    //	当前结点不为 null，就按照 root ==> left ==> right 顺序 add
    public static void preorder(HuffmanNode root){
        if (root == null) {	//	base case
            return;
        }
        System.out.println(root);
        preorder(root.left);
        preorder(root.right);
    }

    //  生成霍夫曼树编码
    //  在生成 Huffman 编码过程中，需要去拼接路径，定义一个 StringBuilder，存储叶子节点路径

    /**
     * 将传入的所有的叶子节点得到 Huffman 编码，并放入到 codeMap 集合中
     * @param huffmanNode 传入节点
     * @param code 路径：左 0，右 1
     * @return
     */
    public static String getHuffmanCode(byte[] bytes, HuffmanNode huffmanNode, String code){
        StringBuilder sb = new StringBuilder();
        sb.append(code);
        if (huffmanNode != null){
            if (huffmanNode.getData() == null){ //  非叶子节点
                getHuffmanCode(bytes, huffmanNode.left, code + "0");
                getHuffmanCode(bytes, huffmanNode.right, code + "1");
            }else { //  叶子节点
                codeMap.put(huffmanNode.getData(), sb+"");
            }
        }
        StringBuilder huffmanCode = new StringBuilder();
        for (Byte aByte : bytes) {
            huffmanCode.append(codeMap.get(aByte));

        }

        return huffmanCode+"";
    }

    //  将一个 byte 转成 二进制字符串
    public static String byteToBitString(byte b, boolean flag){
        int temp = b;

        if (flag | temp < 0){  //  非最后一个，默认都是8位的【负数也都是8位的】
            temp |= 256;
        }

        String str = Integer.toBinaryString(temp);
        if (flag){  //  除了最后一个 flag 都是 true
            return str.substring(str.length() - 8);
        }else { //  最后一个
            StringBuilder sb = new StringBuilder();
            sb.append(str);
            while (sb.length() < sbLastLength.length()){    //  补充至原编码个数
                sb.insert(0, "0");
            }
            System.out.println("sbLength=" + sbLastLength);
            return sb+"";
        }
    }

    public static byte[] unZip(byte[] zip){
        StringBuilder decoder = new StringBuilder();

        for (int i = 0; i < zip.length; i++) {
            //  判断是不是最后一个字节
            boolean flag = (i != zip.length - 1);   //  最后一位时，不补位【正常都是 true】
            decoder.append(byteToBitString(zip[i], flag));  //  对应的 huffman编码
        }


        System.out.println();
        System.out.println("解压缩后的 huffman 编码" + decoder);

        //  制作 decodeMap
        for (Map.Entry<Byte, String> byteStringEntry : codeMap.entrySet()) {
            decodeMap.put(byteStringEntry.getValue(), byteStringEntry.getKey());
        }

        List<Byte> list = new ArrayList<>();

        for (int i = 0; i < decoder.length();) {
            int r = 1;
            boolean flag = true;
            Byte b = null;

            while (flag){
                String key = decoder.substring(i, i + r);
                b = decodeMap.get(key);
                if (b != null){
                    flag = false;
                }else {
                    r++;
                }
            }
            list.add(b);
            i += r;
        }

        System.out.println(list);
        byte[] bytes = new byte[list.size()];
        //  创建集合，存放 byte
        for (int i = 0; i < list.size(); i++) {
            bytes[i] = list.get(i);
        }
        return bytes;
    }

    /**
     * @param bytes
     * @param codeMap
     * @return
     */
    public static byte[] zip(byte[] bytes, HashMap<Byte, String> codeMap){

        StringBuilder sb = new StringBuilder();

        for (byte aByte : bytes) {
            sb.append(codeMap.get(aByte));
        }

        //  每8个一组，进行压缩
        int length = sb.length() % 8 == 0 ? sb.length() / 8 :sb.length() / 8 + 1;
        System.out.println("sb的大小=" + sb.length());
        byte[] result = new byte[length];  //  最终的字节数组

        int index = 0;  //  记录是第几个变量
        System.out.println(sb.length());
        for (int i = 0; i < sb.length(); i += 8) {
            //  够 8 位的话，就取出 8 位，否则就有几位就取出几位
            String strBytes = (i + 8) > sb.length() ? sb.substring(i) : sb.substring(i, i + 8);
            if (i + 8 >= sb.length()){
                System.out.println(sb.substring(i));
                sbLastLength.append(sb.substring(i));  // 记录编码个数

            }
            result[index++] = (byte) Integer.parseInt(strBytes, 2);
        }
        return result;
    }
}

class HuffmanNode implements Comparable<HuffmanNode>{
    private Byte  data;
    private int weight;
    HuffmanNode left;
    HuffmanNode right;
}
```

读取数据：

- 数据
- 霍夫曼编码表 codeMap

![iiq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304211210724.png)

再来看下普通字符串压缩与解压缩

```java
String str = "i like like like java do you like a java";

HuffmanNode huffmanNode = getHuffmanNode(str.getBytes());
String huffmanCode = getHuffmanCode(str.getBytes(), huffmanNode, "");
byte[] zip = zip(str.getBytes(), codeMap);
System.out.println("压缩后的长度= " + zip.length);
for (byte b : zip) {
    System.out.println(b);
}
byte[] bytes = unZip(zip);
System.out.println("============");
String s = new String(bytes);
System.out.println(s);
```



## 13. BST（Binary Sort(Search) Tree）二叉排序树

<font color="yellow">**右子树的 min > 左子树 max**</font>

对于 BST 的任何一个非叶子节点

- 左子节点的值 < 当前节点节点值
- 当前节点节点值 < 右子节点的值

举例：

```bash
# 中序遍历二叉树组
[7、3、10、12、5、1、9]
```

### 创建和遍历

```java
package binarySortTree;

public class BinarySortTreeDemo {
    static int[] arr = {7, 3, 10, 12, 5, 1, 9};

    public static void main(String[] args) {
        Node root = new Node(7);
        for (int i = 1; i < arr.length; i++) {
            root.add(new Node(arr[i]));
        }
        root.inorder(root);
    }
}

class Node{
    private int value;
    private Node left;
    private Node right;

    public Node(int value) {
        this.value = value;
    }

    //  添加节点方式
    //  递归方式添加，注意满足 BST 规则
    public void add(Node node){
        if (node == null){
            return;
        }
        if (this.value > node.value){   //  左侧
            if (this.left == null){
                this.left = node;
            }else {
                this.left.add(node);    //  递归地向左子树添加
            }
        }else { //  右侧
            if (this.right == null){
                this.right = node;
            }else {
                this.right.add(node);
            }
        }
    }

    public void inorder(Node root){
        if (root == null) {	//	base case
            return;
        }
        inorder(root.left);
        System.out.print(root.value + " ");
        inorder(root.right);
    }
}
```

### 删除

1. 叶子节点
   - 找到目标节点 targetNode
   - 找到该节点父节点 parentNode
   - 判断 targetNode 是 parentNode 的 left 还是 right
   - 根据前面情况来对应删除
2. 只有一棵子树的节点
   - 找到目标节点 targetNode
   - 找到该节点父节点 parentNode
   - 判断 targetNode 的节点是 left 还是 right
   - parentNode. left | right = targetNode.left | right
3. 有 2棵子树的节点
   - 找到目标节点 targetNode
   - 找到该节点父节点 parentNode
   - 判断 targetNode 是 parentNode 的 left 还是 right
   - 从 targetNode 的右子树找到最小的节点，保存在 temp中【或者左子树中最大的节点】
     - 将temp 从原位置删除
     - targetNode.value = temp

![jiqq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304201345112.png)

增加 parent 属性：

```java
//  添加节点方式
//  递归方式添加，注意满足 BST 规则
public void add(Node node) {
    if (node == null) {
        return;
    }
    if (this.value > node.value) {   //  左侧
        if (this.left == null) {
            node.parentNode = this;
            this.left = node;
        } else {
            this.left.add(node);    //  递归地向左子树添加
        }
    } else { //  右侧
        if (this.right == null) {
            node.parentNode = this;
            this.right = node;
        } else {
            this.right.add(node);
        }
    }
}
```

查找 targetNode

```java
//  根据值，查找 targetNode 节点
public Node searchTargetNode(int value) {
    if (value == this.value){
        return this;
    }else if (value < this.value){  //  左子树中去找
        if (this.left == null){
            return null;
        }else {
            return this.left.searchTargetNode(value);
        }
    }else {
        if (this.right == null){
            return null;
        }else {
            return this.right.searchTargetNode(value);
        }
    }
}
```

查找 parentNode

```java
//  查找 parentNode
public Node searchParentNode(Node targetNode){
    return targetNode.parentNode;
}
```

删除节点

```java
package binarySortTree;

public class BinarySortTreeDemo {
    static int[] arr = {7, 3, 10, 12, 5, 1, 9, 2};

    public static void main(String[] args) {
        BinarySortTree binarySortTree = new BinarySortTree();

        for (int i = 0; i < arr.length; i++) {
            binarySortTree.add(new Node(arr[i]));
        }
        binarySortTree.inorder();
        System.out.println();
        binarySortTree.delNode(2);
        binarySortTree.inorder();

        binarySortTree.delNode(5);
        binarySortTree.inorder();

        binarySortTree.delNode(9);
        binarySortTree.inorder();

        binarySortTree.delNode(12);
        binarySortTree.inorder();

        binarySortTree.delNode(7);
        binarySortTree.inorder();

        binarySortTree.delNode(3);
        binarySortTree.delNode(10);
        binarySortTree.delNode(1);
        Node root = binarySortTree.getRoot();
        System.out.println(root);
        binarySortTree.inorder();
        
    }
}

class BinarySortTree{
    private Node root;

    public Node getRoot() {
        return root;
    }

    public void add(Node node){
        if (root == null){
            root = node;
            return;
        }
        root.add(node);
    }

    public void inorder(){
        if (root == null){
            System.out.println("二叉树为空，不能遍历");
            return;
        }
        root.inorder(root);
    }

    //  查找待删除的节点
    public Node search(int value){
        if (root == null){
            return null;
        }
        return root.searchTargetNode(value);
    }

    //  查找父节点
    public Node searchParent(int value){
        if (root == null){
            return null;
        }
        return root.searchParentNode(root.searchTargetNode(value));
    }

    //  删除节点
    public void delNode(int value){
        Node targetNode = search(value);
        if (targetNode == null){    //  没有找到
            System.out.println("未找到该节点=" + value);
            return;
        }

        Node parentNode = searchParent(value);

        if (parentNode == null){    //  没有父节点【待删除的是父节点 root】
            System.out.println("该节点是 root=" + targetNode.getValue());
        }

        //  情景 1
        if (targetNode.getLeft() == null && targetNode.getRight() == null){
            if (parentNode == null){    //  只有一个 root 情况
                root = null;
                return;
            }
            if (parentNode.getLeft() == targetNode){
                parentNode.setLeft(null);
            }else {
                parentNode.setRight(null);
            }
        }else if (targetNode.getLeft() != null && targetNode.getRight() != null){ //  情形 3
            //  需要找到待删除节点 targetNode 右子树的最小值 min
            Node rightMinNode = root.findRightMinNode(targetNode.getRight());
            delNode(rightMinNode.getValue());
            targetNode.setValue(rightMinNode.getValue());
        }else { //  情形 2
            if (parentNode == null){    //  root 只有一个子树
                if (targetNode.getLeft() != null){
                    targetNode.getLeft().setParentNode(null);   // 【你成为父节点】
                    root = targetNode.getLeft();
                }else {
                    targetNode.getRight().setParentNode(null);
                    root = targetNode.getRight();
                }
                return;
            }

            if (parentNode.getRight() == targetNode){    //  父节点的右孩子
                if (targetNode.getLeft() != null){
                    parentNode.setRight(targetNode.getLeft());
                }else {
                    parentNode.setRight(targetNode.getRight());
                }
            }else { //  父节点的左孩子
                if (targetNode.getLeft() != null){
                    parentNode.setLeft(targetNode.getLeft());
                }else {
                    parentNode.setLeft(targetNode.getRight());
                }
            }
        }
    }
}


class Node {
    private int value;
    private Node left;
    private Node right;
    private Node parentNode;

    public Node(int value) {
        this.value = value;
    }

    //  添加节点方式
    //  递归方式添加，注意满足 BST 规则
    public void add(Node node) {
        if (node == null) {
            return;
        }
        if (this.value > node.value) {   //  左侧
            if (this.left == null) {
                node.parentNode = this;
                this.left = node;
            } else {
                this.left.add(node);    //  递归地向左子树添加
            }
        } else { //  右侧
            if (this.right == null) {
                node.parentNode = this;
                this.right = node;
            } else {
                this.right.add(node);
            }
        }
    }

    public void inorder(Node root) {
        if (root == null) {    //	base case
            return;
        }
        inorder(root.left);
        System.out.print(root.value + " ");
        inorder(root.right);
    }

    //  根据值，查找 targetNode 节点
    public Node searchTargetNode(int value) {
        if (value == this.value){
            return this;
        }else if (value < this.value){  //  左子树中去找
            if (this.left == null){
                return null;
            }else {
                return this.left.searchTargetNode(value);
            }
        }else {
            if (this.right == null){
                return null;
            }else {
                return this.right.searchTargetNode(value);
            }
        }
    }

    //  查找 parentNode
    public Node searchParentNode(Node targetNode){
        return targetNode.parentNode;
    }
    //  删除节点

    public Node findRightMinNode(Node node){    //  BST 的 Min 在最左侧
        while (node.left != null){
            node = node.left;
        }
        return node;
    }
}
```

![iiq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304211054538.png)

## 14. 平衡二叉树【AVL】：平衡的 BST

平衡二叉树也叫平衡二叉搜索树（Self-balancing binary search tree），又称为 AVL树，可以保证查询效率高

具有如下特点：

- 是一个空树
- 或者它的左右2个子树的 <font color="yellow">高度差的绝对值不超过1</font>
  - 左右2个子树都是一棵平衡二叉树

平衡二叉树实现方法：

- 红黑树
- AVL
- 替罪羊树
- Treap
- 伸展树

### 左旋转

左旋住实现步骤：

1. 创建一个新的节点 newNode（val = 4）

2. 将新节点的左子树设置位当前节点的左子树

   ```java
   newNode.left = this.left
   ```

3. 将新节点的右子树设置为当前节点右子树的左子树

   ```bash
   newNode.right = this.right.left
   ```

4. 将当前节点的值替换为右子节点的值

   ```java
   this.val = this.right.val
   ```

5. 将当前节点的右子树设置为右子树的右子树

   ```java
   this.right = this.right.right
   ```

6. 将当前节点的左子树设为新节点

   ```java
   this.left = newNode
   ```

![nnq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304211708622.png)

### 高度

```java
//  返回当前节点树的高度
public int height(){
    return Math.max(this.left == null? 0: this.left.height(), this.right == null? 0: this.right.height()) + 1;  //  本身就算 1 层
}

public int leftHeight(){
    if(this.left == null){
        return 0;
    };
    return this.left.height();
}

public int rightHeight(){
    if (this.right == null){
        return 0;
    }
    return this.right.height();
}

public int BF(){	//	BF 因子
    return this.leftHeight() - this.rightHeight();
}
```



当添加玩一个节点后，如果： BF < -1，就要左旋转

```java
//  添加节点方式
//  递归方式添加，注意满足 BST 规则
public void add(Node node) {
    if (node == null) {
        return;
    }
    if (this.value > node.value) {   //  左侧
        if (this.left == null) {
            node.parentNode = this;
            this.left = node;
        } else {
            this.left.add(node);    //  递归地向左子树添加
        }
    } else { //  右侧
        if (this.right == null) {
            node.parentNode = this;
            this.right = node;
        } else {
            this.right.add(node);
        }
    }

    //  当添加玩一个节点后，如果： BF < -1，就要左旋转
    if (this.BF() < -1){
        this.leftRotate();
    }
}
```

### 右旋转

```bash
{10, 12, 8, 9, 7, 6}
```

![nnq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304212015311.png)

### 双旋转

![nnq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304212049168.png)

解决方式：

- 先对当前节点的左节点进行左旋转
- 再对当前节点进行右旋转

![nnq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304221146539.png)

```java
public void add(Node node) {
    if (node == null) {
        return;
    }
    if (this.value > node.value) {   //  左侧
        if (this.left == null) {
            node.parentNode = this;
            this.left = node;
        } else {
            this.left.add(node);    //  递归地向左子树添加
        }
    } else { //  右侧
        if (this.right == null) {
            node.parentNode = this;
            this.right = node;
        } else {
            this.right.add(node);
        }
    }

    //  当添加玩一个节点后，如果： BF < -1，就要左旋转
    if (this.BF() < -1){    //  左旋转
        if (this.right != null && this.right.BF() > 0){   //  右子树左沉
            this.right.rightRotate();
        }
        this.leftRotate();
    }else if (this.BF() > 1){   //  右旋转
        if (this.left!= null && this.left.BF() < 0){    //  左子树右侧沉
            this.left.leftRotate();
        }
        this.rightRotate();
    }
}
```

平衡因子异号：需要进行双旋转

### 完整代码

```java
package avl;

public class AVLTreeDemo {
    public static void main(String[] args) {
        int[] arr = {10, 11, 7, 6, 8, 9};
        AVLTree avlTree = new AVLTree();
        for (int i : arr) {
            avlTree.add(new Node(i));
        }
        avlTree.inorder();
        System.out.println( );
        System.out.println("树的高度如下：");
        System.out.println(avlTree.getRoot().height());
        System.out.println("左子树高度 " + avlTree.getRoot().leftHeight());
        System.out.println("右子树高度 " + avlTree.getRoot().rightHeight());
        Node root = avlTree.getRoot();
        System.out.println(root.BF());
        avlTree.inorder();
        System.out.println(avlTree.getRoot());

    }
}

class AVLTree{
    private Node root;

    public Node getRoot() {
        return root;
    }

    public void add(Node node){
        if (root == null){
            root = node;
            return;
        }
        root.add(node);
    }

    public void inorder(){
        if (root == null){
            System.out.println("二叉树为空，不能遍历");
            return;
        }
        root.inorder(root);
    }

    //  查找待删除的节点
    public Node search(int value){
        if (root == null){
            return null;
        }
        return root.searchTargetNode(value);
    }

    //  查找父节点
    public Node searchParent(int value){
        if (root == null){
            return null;
        }
        return root.searchParentNode(root.searchTargetNode(value));
    }

    //  删除节点
    public void delNode(int value){
        Node targetNode = search(value);
        if (targetNode == null){    //  没有找到
            System.out.println("未找到该节点=" + value);
            return;
        }

        Node parentNode = searchParent(value);

        if (parentNode == null){    //  没有父节点【待删除的是父节点 root】
            System.out.println("该节点是 root=" + targetNode.getValue());
        }

        //  情景 1
        if (targetNode.getLeft() == null && targetNode.getRight() == null){
            if (parentNode == null){    //  只有一个 root 情况
                root = null;
                return;
            }
            if (parentNode.getLeft() == targetNode){
                parentNode.setLeft(null);
            }else {
                parentNode.setRight(null);
            }
        }else if (targetNode.getLeft() != null && targetNode.getRight() != null){ //  情形 3
            //  需要找到待删除节点 targetNode 右子树的最小值 min
            Node rightMinNode = root.findRightMinNode(targetNode.getRight());
            delNode(rightMinNode.getValue());
            targetNode.setValue(rightMinNode.getValue());
        }else { //  情形 2
            if (parentNode == null){    //  root 只有一个子树
                if (targetNode.getLeft() != null){
                    targetNode.getLeft().setParentNode(null);   // 【你成为父节点】
                    root = targetNode.getLeft();
                }else {
                    targetNode.getRight().setParentNode(null);
                    root = targetNode.getRight();
                }
                return;
            }

            if (parentNode.getRight() == targetNode){    //  父节点的右孩子
                if (targetNode.getLeft() != null){
                    parentNode.setRight(targetNode.getLeft());
                }else {
                    parentNode.setRight(targetNode.getRight());
                }
            }else { //  父节点的左孩子
                if (targetNode.getLeft() != null){
                    parentNode.setLeft(targetNode.getLeft());
                }else {
                    parentNode.setLeft(targetNode.getRight());
                }
            }
        }
    }

}



//  复用 Node 节点

class Node {
    private int value;
    private Node left;
    private Node right;
    private Node parentNode;

    public Node(int value) {
        this.value = value;
    }

    //  添加节点方式
    //  递归方式添加，注意满足 BST 规则
    public void add(Node node) {
        if (node == null) {
            return;
        }
        if (this.value > node.value) {   //  左侧
            if (this.left == null) {
                node.parentNode = this;
                this.left = node;
            } else {
                this.left.add(node);    //  递归地向左子树添加
            }
        } else { //  右侧
            if (this.right == null) {
                node.parentNode = this;
                this.right = node;
            } else {
                this.right.add(node);
            }
        }

        //  当添加玩一个节点后，如果： BF < -1，就要左旋转
        if (this.BF() < -1){    //  左旋转
            if (this.right != null && this.right.BF() > 0){   //  右子树左沉
                this.right.rightRotate();
            }
            this.leftRotate();
        }else if (this.BF() > 1){   //  右旋转
            if (this.left!= null && this.left.BF() < 0){    //  左子树右侧沉
                this.left.leftRotate();
            }
            this.rightRotate();
        }
    }

    public void inorder(Node root) {
        if (root == null) {    //	base case
            return;
        }
        inorder(root.left);
        System.out.print(root.value + " ");
        inorder(root.right);
    }

    //  根据值，查找 targetNode 节点
    public Node searchTargetNode(int value) {
        if (value == this.value){
            return this;
        }else if (value < this.value){  //  左子树中去找
            if (this.left == null){
                return null;
            }else {
                return this.left.searchTargetNode(value);
            }
        }else {
            if (this.right == null){
                return null;
            }else {
                return this.right.searchTargetNode(value);
            }
        }
    }

    //  查找 parentNode
    public Node searchParentNode(Node targetNode){
        return targetNode.parentNode;
    }
    //  删除节点

    public Node findRightMinNode(Node node){    //  BST 的 Min 在最左侧
        while (node.left != null){
            node = node.left;
        }
        return node;
    }

 
    //  返回当前节点树的高度
    public int height(){
        return Math.max(this.left == null? 0: this.left.height(), this.right == null? 0: this.right.height()) + 1;  //  本身就算 1 层
    }

    public int leftHeight(){
        if(this.left == null){
            return 0;
        };
        return this.left.height();
    }

    public int rightHeight(){
       if (this.right == null){
           return 0;
       }
       return this.right.height();
    }

    public int BF(){
        return this.leftHeight() - this.rightHeight();
    }

    public void leftRotate(){
        Node newNode = new Node(this.value);    //  1. 新建节点
        newNode.left = this.left;   //  2. new 左
        newNode.right = this.right.left;    //  3. new 右

        this.value = this.right.value;  //  4.  修改 root 的 value
        this.right = this.right.right;  //  5. 修改 root right
        this.left = newNode;    //  6. 修改 root left
    }

    public void rightRotate(){
        Node newNode = new Node(this.value);
        newNode.right = this.right;
        newNode.left = this.left.right;
        this.value = this.left.value;
        this.left = this.left.left;
        this.right = newNode;
    }
}
```

## 15. 多叉树

当二叉树节点较多时：

1. 构建二叉树，需要进行多次 IO 操作，对速度有影响
2. 节点海量，也会造成二叉树高度很高，会降低操作速度

树的 <font color="yellow">**度**</font>：

- <font color="blue">**节点** </font>的度：节点拥有的子树数目称为节点的度，叶子节点的度为0。
- <font color="blue">**树** </font>的度：树内各节点的度的最大值。

如果将树的度设置为 1024，在 600 亿个元素中最多只需要 4 次 IO操作就可以读取到想要的元素，B树广泛用于文件存储系统以及数据库系统中

### 2-3 树

1. 2-3 树的所有叶子节点都在同一层【只要是 B树都满足这个条件】

2. 有2个子节点的节点叫做 <font color="yellow">**二节点**</font>，二节点要么没有子节点，要么有2个子节点

   ![image-20230422134124738](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304221341891.png)

3. 有3个子节点的节点叫做 <font color="yellow">**三节点**</font>，三节点要么没有子节点，要么有3个子节点

   ![image-20230422134203208](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304221342357.png)

4. 2-3 树是由 2 节点 和 3节点构成的树

B-Trees 绘图举例：

```java
{16, 24, 12, 32, 14, 26, 34, 10, 8, 28, 38, 20}
```

![20230422_132524](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304221329958.gif)

插入规则：

1. 2-3树的所有叶子节点都在同一层【只要是 B树都满足这个条件】
2. 有 2 个子节点的叫二节点，二节点要么没有子节点，要么有 2 个子节点
3. 有  3 个子节点的节点叫做三节点，三节点要么没有子节点，要么有 3 个子节点
4. 当插入一个数到某个节点时，不能满足上面3个要求，就需要拆
   - 先向上拆
   - 如果上层满，则拆本层，拆后仍然要满足上面 3 个条件
5. 对于三节点的子树的值大小仍然遵守（BST）规则

![image-20230422153025065](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304221530198.png)

### B 树【也是 BST：左小右大】

Balanced 平衡

B-tree

B树说明

- B树的 **<font color="yellow">阶</font>**：节点的最多子节点个数
  - 2-3 树的阶是 3
  - 2-3-4 树的阶是 4
- B树搜索：
  - 从 root 开始，对节点内的关键字（有序）序列进行二分查找，如果命中则结束
  - 否则进入查询关键字所属范围的儿子节点，重复，直到所对应的儿子指针为空，或已经是叶子节点
- 关键字集合分布在整棵树中，即叶子节点和非叶子节点都存放数据
- 搜索有可能在非叶子节点结束
- 其搜索性能等价于在关键字全集内做一次二分查找 ===> 线性对数阶

### B+ 树

1. 所有关键字都出现在 <font color="yellow">**叶子节点** </font>的链表中
   - 数据只能存放在叶子节点
   - 链表中的关键字（数据）恰好是有序的
2. 非叶子节点相当于是叶子节点的索引【稀疏索引】
3. 适合文件索引系统

### B* 树

在 B+ 树的非根和非叶子节点再增加指向兄弟的指针

B* 树说明：

- 定义了非叶子节点关键字个数至少为 $\frac{2}{3}*M[度]$，即块的最低利用率为：$\frac{2}{3}$，而B+ 树块的最低利用率为 $\frac{1}{2}$
- 可以看出 B* 树分配新节点的概率比 B+ 树低，空间利用率更高

## 16. 线段树 Segment tree【小区间更新大区间，是平衡二叉树】

线段树是一种二叉树，广义上也被归类为 <font color="yellow">二叉搜索树</font>，是一种工具。

对于区间的修改、维护和查询时间复杂度优化为 $log$ 级别

![image-20230428133111103](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304281331270.png)

### 1. 局限性

问题需要满足：区间加法

- 对于 $[L,R]$ 的区间，它的答案可以由 $[L,M]$ 和 $[M+1, R]$ 的答案合并求出
- 满足的问题：区间求和，区间 max, min 值等

不满足的问题：

- 区间的众数
- 区间最长连续问题
- 最长不下降问题等

### 2. 解决问题步骤

#### 2.1 建树（4 * n）

对于一个维护序列长度为n 的线段树，用堆结构存储的话，数组大小应开 $4*n$

证明：

- 当n为2的正整数次幂时，设：$n=2^{k}$，此时线段树正好是一棵叶子数为 n 的满二叉树
- 若n更大一些，但还没超过 $ n=2^{k+1}$ 时 ，此时线段树的内存大小要按照 n=2^（k+1）的情况开。

这启示我们：

min

满二叉树所有节点总数：$2^{h}-1$ （h：树的高度）

满二叉树叶子节点：$2^{h-1}$ 

所以我们可以根据叶子节点个数 ---> 全部节点个数

- $n = 2^{h-1}$
- $h=log2n$
- 所以：sum = $2^{log2n}$- 1 = 2n -1（n：叶子节点）

max：【本层节点不够用了，只能用下层了】

sum = 2(2n) - 1 = 4n - 1

所以得出结论如下：
$$
2n-1<size<4n-1
$$
所以为了保险起见：空间开为：4n

![image-20230428160704758](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304281607933.png)

![image-20230428160727197](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304281607385.png)

![image-20230428160741633](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304281607800.png)

空间大小为：4 * n

#### 2.2 修改

##### 单点修改

修改数列中下标为 i 的数据

- 从根节点向下 DFS
  - 如果当前节点的左儿子的区间 【L, R】包含了 i，也就是 $L<=i<=R$，就访问左儿子
  - 否则访问右儿子
- 直到 $L=R$，也就是搜到了只包含这个数据的节点，就可以修改它
- 不要忘了将包含此数据的大区间的值更新

![jjaq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304281619818.png)

##### 区间查询

###### 仅有单点修改的区间查询

仅有单点修改的区间查询，不需要处理 lazy 标记

非常简单，步骤为：

- 如果要查询的区间完全覆盖当前区间
  - 直接返回当前区间的值 【baseCase】
- 如果查询区间和左儿子有交集搜索左儿子
- 如果查询区间和右儿子有交集就搜索右儿子
- 最后合并处理2边查询的数据

注意：处理完根节点的左儿子，处理完子问题后，还要回到大问题，看右儿子是否有交集

![image-20230428164605719](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304281646944.png)

###### 区间修改后的区间查询【lazy 标记】

步骤为：

- 如果要查询的区间完全覆盖当前区间
  - 直接更新这个区间，打上 lazy 标记
- 如果没有被完全包含，且 <font color="yellow">当前区间有 lazy 标记</font>
  - 先下传 lazy 标记到子区间
  - 再清除当前区间的 lazy 标记
- 如果查询区间和左儿子有交集搜索左儿子
- 如果查询区间和右儿子有交集就搜索右儿子
- 最后将当前区间的值更新

![image-20230428165812231](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304281658480.png)

修改完之后，打上 lazy 标记，然后更新当前区间的值

![image-20230428165844368](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304281658646.png)

下面我们再来查询 [1, 3]

![image-20230428170428181](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304281704377.png)

将 lazy 标记下传给 左右孩子，并清除自身的 lazy 标记

![image-20230428170445220](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304281704536.png)

然后继续看自身的左右儿子

![image-20230428170530351](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304281705537.png)

![image-20230428170551369](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304281705556.png)



#### 2.3 线段树模板例题

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

public class segmentTree {
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
            tree[i] = new Tree();   //  避免 null 指针，每一个都进行初始化
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
        tree[2 * i + 1].lazy += tree[i].lazy;   //  1. 左右儿子继承父亲的 lazy 标记
        m = (tree[i].l + tree[i].r) / 2;
        tree[2 * i].res += tree[i].lazy * (m - tree[i].l + 1);  //  2. 左右儿子更新节点区间和
        tree[2 * i + 1].res += tree[i].lazy * (tree[i].r - m);    //  【r - (mid + 1)】+ 1
        tree[i].lazy = 0;   //  3. 消除父亲的 lazy 标记
    }   
    
}
```

## 17. 并查集【并、查】Disjoint-set data structure

由于支持查询和合并这两种操作，并查集在英文中也被称为

- 联合-查找数据结构（Union-find data structure）
- 合并-查找集合（Merge-find set）

一种树型的数据结构：用于处理一些 <font color="yellow">不相交集合 </font>的 <font color="yellow">合并与查询 </font>问题。

例如：n 个元素，分属不同的 n 个集合

2种操作：

1. 给出 2个元素的关系，合并2个集合（x 与 y 是亲戚，亲戚的亲戚是亲戚），于是【x 所在集合】 与 【y 所在集合】 合并
2. 查询 2个元素是否存在关系（是否在同一个集合）

### 1. 用数据结构实现并查集

1. 一个集合构建一棵树，任选一个元素作为该集合的根节点
2. 建立 pre 数组记录每个元素的父节点
   - pre[当前节点] = 父节点
   - 根节点的父节点 = 自己本身
3. 并查集的操作分为2类：“并”和“查”，给出元素关系后，如何进行合并呢？
4. 将一个集合的树变成另一个集合的子树（将一个集合的  root 的父节点改为另一个集合的root），pre[B的根节点] = A的根节点，就可以将 2棵树合并为一棵树

![image-20230503141833123](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305031418260.png)

![image-20230503141903318](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305031419457.png)

### 2. 如何查询2个元素是否在同一集合

1. 从该元素开始访问父节点（一般用递归）
2. 直到一步步访问到 root
3. 再对2个元素的 root 进行对比判断

![image-20230503142428554](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305031424681.png)

### 3. 时间复杂度

并查集的“并”与“查”时间复杂度取决于树的深度

避免如下情况：

![image-20230503142505362](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305031425480.png)

### 4. 路径压缩

- 在查询时，最终目的：找到这个元素所在的这棵树的 root，那么它和其它元素是如何联系的，对我们来说就没有意义了。
- 在每次查询时，对被查询元素到 root 沿途经过的节点顺便进行一步路径压缩
- 将经过节点的父节点都更改为根节点，pre[经过节点]  = root，让树的形状尽量接近：

![image-20230503142708648](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305031427775.png)

### 5. 带权并查集 【食物链】

输入：

| 输入格式                               | 含义                                       |
| -------------------------------------- | ------------------------------------------ |
| N，M                                   | N个元素，M个操作                           |
| 接下来M行，每行包含三个整数（K，X，Y） |                                            |
| K = 1                                  | 合并 X 与 Y所在的集合                      |
| K = 2                                  | 输出X 与 Y是否在同一集合内【是：Y，否：N】 |

输出：

| 输出格式                              |                                   |
| ------------------------------------- | --------------------------------- |
| 对于每一个 K = 2 的操作，都有一行输出 | 每行包含一个大写字母，为 Y 或者 N |

### 6. 动物王国【NOI2001】

![image-20230503143226397](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305031432507.png)

![image-20230503174454129](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305031744312.png)

动物王国有 3 种动物，A吃B，B吃C，C吃A

现在有 N 个动物，以 1 - N 编号，每个动物都是 A，B，C 中的一种，但是我们并不知道它到底是哪一种

有人用 2种 说法对这 N 个动物构成的食物链关系进行描述



| 说法        | 含义          |
| ----------- | ------------- |
| 1    X    Y | X 和 Y 是同类 |
| 2    X    Y | X 吃 Y        |

此人对 N 个动物，用上述 2 种说法，一句接一句地说出 K 句话，这 K 句话有的是真的，有的是假的。

当满足如下3条之一时，就是 <font color="yellow">**假话**</font>，否则就是真话

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

数据范围

1≤N≤500001≤N≤50000,
0≤K≤1000000≤K≤100000

```bash
# 输入
100 7
1 101 1
2 1 2
2 2 3 
2 3 3  
1 1 3 
2 3 1 
1 5 5
```

```bash
# 输出
3
```



### 7. 分析

#### 7.1 解法1：种类并查集（多倍并查集）

这 3 个分类是一种 <font color="yellow">相对的关系</font>

![image_0.8005583959947538](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305031608282.gif)

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

可以由一些关系 <font color="red">**推出未知的关系**</font>：

1. x 和 y 都捕食 z，那么 x、y肯定是同类
2. x捕食y，y捕食z，那么 z 一定是 x 的天敌

```java
package com.alq.unionFind;

public class UnionFind {
    static int[] pre;
    static int ans;
    //  {1, x, y}：同类
    //  {2, x, y}：x 吃 y
    static int[][] array = {{100, 7}, {1, 101, 1}, {2, 1, 2}, {2, 2, 3}, {2, 3, 3},
                            {1, 1, 3}, {2, 3, 1}, {1, 5, 5}};

    public static void main(String[] args) {
        int N = array[0][0];    //  动物数 【1 - N：进行编号】每个动物都是 A、B、C 中的一种
        int K = array[0][1];  //    K 句话【假话：1. 当前话与前面某些话冲突 2. 当前 X 或者 Y 大于 N 3. 当前表示 X 吃 X】


        pre = new int[1000];
        for (int i = 1; i <= N * 3; i++) {  //  初始化每个元素的父节点为本身
            pre[i] = i;
        }

        for (int i = 1; i <= K; i++) {
            int k = array[i][0];
            int x = array[i][1];
            int y = array[i][2];
            System.out.println("第 " + i + "句话");
            judge(N, k, x, y);
        }

        System.out.println(ans);
    }

    public static int find(int x){
        if (x != pre[x]){   //  x = pre[x] 范围递归边界，也就是查询到根节点
            pre[x] = find(pre[x]);  //  路径压缩，路过节点的父节点都指向根节点
        }

        return pre[x];
    }

    public static void union(int x, int y){
        int x0 = find(x);
        int y0 = find(y);
        pre[x0] = y0;
    }

    public static void judge(int n, int k, int x, int y){
        if (x > n || y > n){
            System.out.println("这句话错了");
            UnionFind.ans++;
            return;
        }

        if (k == 1){    //  x、y 同类
            if (find(x + n) == find(y) || find(x + 2*n) == find(y)){   //  x 与 y 捕食或被捕
                System.out.println("这句话错了");
                UnionFind.ans++;
                return;
            }
            //  如果不是上面情况，那就是真话
            union(x, y);    //  x 与 y 为同类
            union(x + n, y + n);    //  x 的猎物是 y 的猎物
            union(x + 2*n, y + 2*n);     //  x 的天敌是 y 的天敌
        }

        if (k == 2){    //  x 吃 y
            if (find(x) == find(y) || find(x + 2*n) == find(y)){
                System.out.println("这句话错了");
                UnionFind.ans++;
                return;
            }
            //  如果不是上述情况，那就是真话
            union(find(x + n), find(y));    //  x 的猎物是 y 的同类
            union(find(x + 2 * n), find(y + n));   //   x 的天敌是 y的猎物
            union(find(x), find(y + 2*n));    //    x 同类是 y 的天敌
        }
    }
}
```

#### 7.2 解法2：带权并查集

2个元素建立联系时

- 并不只把它们所在的集合合并
- 还要给它们之间赋一个权值，来表示它们之间的关系

<font color="yellow">难点</font>：路径压缩 / 集合合并时，关系值的改变。

怎么找到当前节点 x 与 根节点 的关系？

![image-20230503200305787](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305032003008.png)

数学思维：<font color="yellow">向量 </font>计算

![image-20230503161349937](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305031613085.png)

因为我们的关系只有：0、1、2 三个值，所以求出之后，还要对 3 进行 取模：mod 3 ===> 求出 3 对 1 的关系

这里看下 pre 数组

![image-20230504000418903](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305040004066.png)

![image-20230503161402810](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305031614961.png)



路径压缩解决后，就是集合合并了，根节点之间的关系如何赋值？

- 应用向量的知识
- 由于 re[y] - re[x] 可能为负数，所以要 + 3，最后还要取模 mod3

![image-20230503200812058](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305032008237.png)

这里 k：是 x 与 y 的关系：计算时为 0【真话时就合并】

判断真假话和之前不太一样了

![image-20230503201023965](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305032010178.png)

```java
package com.alq.unionFind;

public class UnionFind {
    static int[] pre;
    static int[] re;	//	权值
    static int ans;
    //  {1, x, y}：同类
    //  {2, x, y}：x 吃 y
    static int[][] array = {{100, 7}, {1, 101, 1}, {2, 1, 2}, {2, 2, 3}, {2, 3, 3},
                            {1, 1, 3}, {2, 3, 1}, {1, 5, 5}};

    public static void main(String[] args) {
        int N = array[0][0];    //  动物数 【1 - N：进行编号】每个动物都是 A、B、C 中的一种
        int K = array[0][1];  //    K 句话【假话：1. 当前话与前面某些话冲突 2. 当前 X 或者 Y 大于 N 3. 当前表示 X 吃 X】


        pre = new int[1000];
        re = new int[1000];
        for (int i = 1; i <= N; i++) {  //  初始化每个元素的父节点为本身
            pre[i] = i;
        }

        for (int i = 1; i <= K; i++) {
            int k = array[i][0];
            int x = array[i][1];
            int y = array[i][2];
            System.out.println("第 " + i + "句话");
            judge(N, k, x, y);
        }

        System.out.println(ans);

        for (int i = 0; i < 300; i++) {
            System.out.println(pre[i]);
        }
    }

    public static int find(int x){
        int prex;

        if (x != pre[x]){   //  x = pre[x] 范围递归边界，也就是查询到根节点
            prex = pre[x];  //  先将父节点的位置存在 prex 中，以便于父节点更新
            pre[x] = find(prex);  //  路径压缩，路过节点的父节点都指向根节点
            re[x] = (re[x] + re[prex]) % 3;
        }

        return pre[x];
    }
    

    public static void judge(int n, int k, int x, int y){
        if (x > n || y > n){
            System.out.println("这句话错了");
            UnionFind.ans++;
            return;
        }
        int a = find(x);
        int b = find(y);

        if (k == 1){    //  x、y 同类
            if ((a == b) && re[x] != re[y]){   //  x、y 在同一个集合，且与根节点的关系不同，那么是假话
                System.out.println("这句话错了");
                UnionFind.ans++;
                return;
            }
            //  不在同一集合，说明之前不存在关系，是真话，合并 x、y 所在集合
            if (a != b){
                pre[a] = b;
                re[a] = (re[y] - re[x] + 3) % 3;
            }
        }

        if (k == 2){    //  x 吃 y
            //  如果 x y 在同一集合，且不是捕食关系，那么是假话
            if (a == b && (re[x] - re[y] + 3) % 3 != 1){
                System.out.println("这句话错了");
                UnionFind.ans++;
                return;
            }
            //  不在同一集合，说明之前不存在关系，是真话，合并 x、y 所在集合
            if (a != b){
                pre[a] = b;
                re[a] = (re[y] - re[x] + 3) % 3;    //  更新权值
            }
        }
    }
}
```







## 18. 图

常用概念：

- 顶点：vertex
- 边：edge
- 路径
- 无向图
- 有向图
- 带权图

图的表示方式：

1. 二维数组（邻接矩阵）

   ![image-20230422162350007](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304221623138.png)

2. 链表（邻接表）

   数组 + 链表

   ![image-20230422162506207](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304221625343.png)

### 简易无向图实现

```java
package graph;

public class Graph {
    private ArrayList<String> vertexList = new ArrayList<>();   //  点
    private int[][] edges;    //  边
    private int numOfEdges; //  边的数目

    public static void main(String[] args) {

        Graph graph = new Graph(5);
        graph.insertVertx("A");
        graph.insertVertx("B");
        graph.insertVertx("C");
        graph.insertEdge("A", "B", 2);
        graph.printArr();
        System.out.println("顶点个数 " +  graph.getNumOfVertex());

        System.out.println("边的个数 " + graph.getNumOfEdges());
        System.out.println("A和B间的 weight= " +  graph.getWeight("A", "B"));

    }

    public Graph(int n){
        edges = new int[n][n];
    }

    //  插入节点
    public void insertVertx(String vertex){
        vertexList.add(vertex);
    }

    //  添加边
    public void insertEdge(String vertexA, String vertexB, int weight){
        int i = vertexList.indexOf(vertexA);
        int j = vertexList.indexOf(vertexB);
        if (edges[i][j] == 0) {
            edges[i][j] = weight;
            edges[j][i] = weight;
            numOfEdges++;
        }
    }

    public int getNumOfVertex(){
        return vertexList.size();
    }

    public int getNumOfEdges(){
        return numOfEdges;
    }

    //  得到权值
    public int getWeight(String vertexA, String vertexB){
        int i = vertexList.indexOf(vertexA);
        int j = vertexList.indexOf(vertexB);
        return edges[i][j];
    }

    public void printArr(){
        for (int i = 0; i < edges.length; i++) {
            for (int j = 0; j < edges.length; j++) {
                System.out.print(edges[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```

### 深度优先遍历 DFS【Depth-frist search】

w：列数

i：行

![nnq](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202304231557169.png)

```bash
0 1 1 0 0 
1 0 1 1 1 
1 1 0 0 0 
0 1 0 0 0 
0 1 0 0 0 
```

```java
private boolean[] isVisited;

//  得到第一个邻接节点的下标
public int getFirstNeighbor(String vertex){
    int cur = vertexList.indexOf(vertex); //  当前顶点下标
    for (int j = 0; j < vertexList.size(); j++) {
        if (edges[cur][j] > 0){	//	【自己和自己的 weight = 0】
            return j;
        }
    }
    return -1;
}

// 根据第一个节点的下标来获取下一个邻接节点
public int getNextNeighbor(int v1, int v2){	//	v1：行数 v2：列数
    for (int j = v2 + 1; j < vertexList.size() ; j++) {
        if (edges[v1][j] > 0){
            return j;
        }
    }
    return -1;
}

public void dfs(boolean[] isVisited, int i){
    String s = vertexList.get(i);
    System.out.print(s + " ");
    isVisited[i] = true;
    //  查找第一个邻接节点
    int w = getFirstNeighbor(s);
    while (w != -1){    //  w 存在
        if (!isVisited[w]){ //  还没访问过
            dfs(isVisited, w);
        }
        //  已经访问过
        w =  getNextNeighbor(i, w);
    }
}

//  对 dfs进行重载，遍历我们所有的节点，并进行 dfs ===> 解决非连通图
public void dfs(){
    //  遍历我们所有的顶点，进行DFS
    for (int i = 0; i < getNumOfVertex(); i++) {
        if (!isVisited[i]){
            dfs(isVisited, i);
        }
    }
}
```



### 广度优先遍历 BFS【Breadth -frist search】使用队列实现 ==> 无权图的最短路径

类似于分层搜索

- 需要一个队列保存访问过的节点的顺序
- 以便于按照这个顺序来访问这些站点的邻接节点

```java
public void bfs(boolean[] isVisited, int i){
    int u;  //  队列头节点对应的下标
    int w;  //  邻接节点 w
    Deque<String> queue = new LinkedList<>(); //  队列，记录节点访问顺序
    String vertex = vertexList.get(i);

    //  访问节点，输出节点信息
    System.out.print(vertex + " ");
    //  标记为已访问
    isVisited[i] = true;
    queue.addLast(vertex);
    while (!queue.isEmpty()){
        //  取出队列的头节点下标
        u = vertexList.indexOf(queue.removeFirst());
        w = getFirstNeighbor(vertex);
        while (w != -1){
            if (!isVisited[w]){ //  没被访问过
                System.out.print(vertexList.get(w) + " ");
                // 标记为已经访问
                isVisited[w] = true;
                // 入队
                queue.addLast(vertexList.get(w));
            }else {
                // 以 u 为前驱节点，找 w 后面的下一个邻节点
                w = getNextNeighbor(u, w); //  体现出广度优先
            }
        }
    }
}

//  对所有节点进行 BFS
public void bfs(){
    for (int i = 0; i < getNumOfVertex(); i++) {
        if (!isVisited[i]){
            bfs(isVisited, i);
        }
    }
}
```

BFS和DFS区别

```java
package graph;

public class Graph {
    private ArrayList<String> vertexList = new ArrayList<>();   //  点
    private int[][] edges;    //  边
    private int numOfEdges; //  边的数目

    private boolean[] isVisited;

    public static void main(String[] args) {

        Graph graph = new Graph(8);
        //  添加边
        //  A - B
        //  A - C
        //  B - C
        //  B - D
        //  B - E
        graph.insertVertx("A");
        graph.insertVertx("B");
        graph.insertVertx("C");
        graph.insertVertx("D");
        graph.insertVertx("E");
        graph.insertVertx("F");
        graph.insertVertx("G");
        graph.insertVertx("H");


        graph.insertEdge("A", "B", 1);
        graph.insertEdge("A", "C", 1);
        graph.insertEdge("B", "D", 1);
        graph.insertEdge("B", "E", 1);
        graph.insertEdge("C", "F", 1);
        graph.insertEdge("C", "G", 1);
        graph.insertEdge("D", "H", 1);
        graph.insertEdge("E", "H", 1);

        graph.printArr();
        System.out.println("顶点个数 " +  graph.getNumOfVertex());

        System.out.println("边的个数 " + graph.getNumOfEdges());
        System.out.println("A和B间的 weight= " +  graph.getWeight("A", "B"));

        System.out.println("==============");

        System.out.println("DFS遍历如下：");
        graph.dfs();

        System.out.println();
        System.out.println("广度优先遍历如下：");
        graph.bfs();

    }

    public Graph(int n){
        edges = new int[n][n];
        isVisited = new boolean[n];
    }

    //  插入节点
    public void insertVertx(String vertex){
        vertexList.add(vertex);
    }

    //  添加边
    public void insertEdge(String vertexA, String vertexB, int weight){
        int i = vertexList.indexOf(vertexA);
        int j = vertexList.indexOf(vertexB);
        if (edges[i][j] == 0) {
            edges[i][j] = weight;
            edges[j][i] = weight;
            numOfEdges++;
        }
    }

    public int getNumOfVertex(){
        return vertexList.size();
    }

    public int getNumOfEdges(){
        return numOfEdges;
    }

    //  得到权值
    public int getWeight(String vertexA, String vertexB){
        int i = vertexList.indexOf(vertexA);
        int j = vertexList.indexOf(vertexB);
        return edges[i][j];
    }

    public void printArr(){
        for (int i = 0; i < edges.length; i++) {
            for (int j = 0; j < edges.length; j++) {
                System.out.print(edges[i][j] + " ");
            }
            System.out.println();
        }
    }

    //  得到第一个邻接节点的下标
    public int getFirstNeighbor(String vertex){
        int cur = vertexList.indexOf(vertex); //  当前顶点下标
        for (int j = 0; j < vertexList.size(); j++) {
            if (edges[cur][j] > 0){
                return j;
            }
        }
        return -1;
    }

    // 根据第一个节点的下标来获取下一个邻接节点
    public int getNextNeighbor(int v1, int v2){
        for (int j = v2 + 1; j < vertexList.size() ; j++) {
            if (edges[v1][j] > 0){
                return j;
            }
        }
        return -1;
    }


    public void dfs(boolean[] isVisited, int i){
        String s = vertexList.get(i);

        System.out.print(s + " ");
        isVisited[i] = true;
        //  查找第一个邻接节点
        int w = getFirstNeighbor(s);
        while (w != -1){    //  w 存在
            if (!isVisited[w]){ //  还没访问过
                dfs(isVisited, w);
            }else {
                //  已经访问过
                w = getNextNeighbor(i, w);
            }
        }
    }

    //  对 dfs进行重载，遍历我们所有的节点，并进行 dfs ===> 解决非连通图
    public void dfs(){
        //  遍历我们所有的顶点，进行DFS【回溯】
        for (int i = 0; i < getNumOfVertex(); i++) {
            if (!isVisited[i]){
                dfs(isVisited, i);
            }
        }
    }

    public void bfs(boolean[] isVisited, int i){
        int u;  //  队列头节点对应的下标
        int w;  //  邻接节点 w
        Deque<String> queue = new LinkedList<>(); //  队列，记录节点访问顺序
        String vertex = vertexList.get(i);

        //  访问节点，输出节点信息
        System.out.print(vertex + " ");
        //  标记为已访问
        isVisited[i] = true;
        queue.addLast(vertex);
        while (!queue.isEmpty()){
            //  取出队列的头节点下标
             u = vertexList.indexOf(queue.removeFirst());
             w = getFirstNeighbor(vertex);
             while (w != -1){
                 if (!isVisited[w]){ //  没被访问过
                     System.out.print(vertexList.get(w) + " ");
                     // 标记为已经访问
                     isVisited[w] = true;
                     // 入队
                     queue.addLast(vertexList.get(w));
                 }else {
                     // 以 u 为前驱节点，找 w 后面的下一个邻节点
                     w = getNextNeighbor(u, w); //  体现出广度优先
                 }
             }
        }
    }

    //  对所有节点进行 BFS
    public void bfs(){
        //  清空 isVisited
        Arrays.fill(isVisited, false);


        for (int i = 0; i < getNumOfVertex(); i++) {
            if (!isVisited[i]){
                bfs(isVisited, i);
            }
        }
    }
}
```

### 最小生成树【贪心策略不同】（minimum spanning tree，MST）

无向图中：任意 2 个顶点都有路径相通，则称为连通图

![image-20230504003947836](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305040039019.png)

一个连通图不存在回路，也就是不存在环，称为树

![image-20230504004425080](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305040044284.png)

一个连通图的连通子图，它含有连通图中所有顶点，有且仅有足以构成一棵树的 n - 1 条边【如果生成树再加一条边，必定构成环】

![image-20230504004633685](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305040046929.png)

连通图的所有生成树中，边权之和代价最小的生成树

例题：铺设光缆！！！

#### PRIM【稠密图】

类似求单源最短路径的 Dijkstra算法

1. 从已知某个顶点开始向外扩散，寻找最小的邻边，挨个将顶点加入集合
2. 已加入集合的点不能再加入（否则会成环）
3. 直到所有顶点加入集合，构成 MST

![jjii](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305040939256.png)

代码实现：

![image-20230504111923934](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305041119114.png)

```java
package com.alq.mst;

public class Prim {
    static int weightTotal;

    public static void main(String[] args) {
        char[] data = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        int vertex = data.length;
        int[][] weight = {{10000, 5, 7, 10000, 10000, 10000, 2},
                {5, 10000, 10000, 9, 10000, 10000, 3},
                {7, 10000, 10000, 10000, 8, 10000, 10000},
                {10000, 9, 10000, 10000, 10000, 4, 10000},
                {10000, 10000, 8, 10000, 10000, 5, 4},
                {10000, 10000, 10000, 4, 5, 10000, 6},
                {2, 3, 10000, 10000, 4, 6, 10000}
        };
        //  创建 MGraph 对象
        MGraph mGraph = new MGraph(vertex);
        //  创建一个 MiniTree
        MiniTree miniTree = new MiniTree();
        miniTree.createGraph(mGraph, vertex, data, weight);
        miniTree.showGraph(mGraph);


        miniTree.prim(mGraph, 0);

        System.out.println("Prim 权值和为：" + weightTotal);


    }
}

//  创建 MST
class MiniTree{
    //  创建图的邻接矩阵
    public void createGraph(MGraph mGraph, int vertex, char[] data, int[][] weight){
        for (int i = 0; i < vertex; i++) {
            mGraph.data[i] = data[i];
            for (int j = 0; j < vertex; j++) {
                mGraph.weight[i][j] = weight[i][j];
            }
        }
    }

    //  显示图的方法
    public void showGraph(MGraph mGraph){
        for (int i = 0; i < mGraph.vertex; i++) {
            for (int j = 0; j < mGraph.vertex; j++) {
                System.out.print(mGraph.weight[i][j] + "\t");
            }
            System.out.println();
        }
    }

    //  编写 Prim 算法，得到 MST

    /**
     * @param mGraph：图
     * @param v：从图的哪个顶点开始生成
     */
    public void prim(MGraph mGraph, int v){
        int[] visited = new int[mGraph.vertex];
        //  将当前这个点标记为已访问
        visited[v] = 1;
        //  记录 2 个顶点的下标
        int h1 = -1;
        int h2 = -1;
        //  将 miniWeigth 初始成一个大数，后面在遍历过程中，会被替换
        int mimiWeight = 10000;

        for (int k = 1; k < mGraph.vertex; k++) {   //  prim 算法结束后，有 n - 1 条边
            for (int i = 0; i < mGraph.vertex; i++) {   //  被访问过的节点
                for (int j = 0; j < mGraph.vertex; j++) {    //  还没有被访问的节点
                    if (visited[i] == 1 && visited[j] == 0 && mGraph.weight[i][j] < mimiWeight){
                        mimiWeight = mGraph.weight[i][j];
                        h1 = i;
                        h2 = j;
                    }
                }
            }
            Prim.weightTotal += mimiWeight;

            //  找到一条边是最小
            System.out.println("边" + mGraph.data[h1] + "==>" + mGraph.data[h2] + " 权值:" + mimiWeight);

            visited[h2] = 1;    //  将找到的节点标记为：已经访问
            //  重置：miniWeight 为最大值
            mimiWeight = 10000;
        }

    }
}

class MGraph{
    int vertex;  //  表示图节点个数
    char[] data;  //  存放节点数据
    int[][] weight; //  存放边（邻接矩阵）


    public MGraph(int vertex) {
        this.vertex = vertex;
        data = new char[vertex];
        weight = new int[vertex][vertex];
    }
}
```



#### KRUSKAL【稀疏图】---> 对所有的边进行贪心，只需一次排序

1. 从整个连通图最小的一条边开始贪心，边能最小就要最小的【只要不成环】
2. 不断合并最后构成 MST
3. 是否生成环，以及合并是利用 <font color="yellow">**并查集** </font>来辅助操作的

注意：初始化的时候会各自为集合

![jjii](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305040951130.png)

分析：

1. 对图的所有边按照权值大小进行排序
   - 排序
2. 将边添加到 MST中时，怎么判断是否形成了回路
   - 记录顶点在 MST 中的终点
   - 顶点的终点是 在 MST 中与它连通的最大顶点
   - 然后每次需要将一条边添加到 MST 时，判断该边的 2个顶点的终点是否重合，重合的话则会构成回路

![image-20230504114700088](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305041147280.png)

终点说明：

将所有顶点按照从小到大的顺序排好之后，某个顶点的终点就是与连通的最大顶点

```java
package com.alq.mst;

public class KRUSKAL {

    private int edgeNum;
    private char[] vertex;
    private int[][] matrix;

    private static final int INF = Integer.MAX_VALUE;

    public static void main(String[] args) {
        char[] vertex = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        int matrix[][] = {
                {0, 12, INF, INF, INF, 16, 14},
                {12, 0, 10, INF, INF, 7, INF},
                {INF, 10, 0, 3, 5, 6, INF},
                {INF, INF, 3, 0, 4, INF, INF},
                {INF, INF, 5, 4, 0, 2, 8},
                {16, 7, 6, INF, 2, 0, 9},
                {14, INF, INF, INF, 8, 9, 0}
        };

        KRUSKAL kruskal = new KRUSKAL(vertex, matrix);
        kruskal.print();
        EData[] edges = kruskal.getEdges();
        kruskal.sortEdges(edges);
        for (EData edge : edges) {
            System.out.println(edge);
        }
        kruskal.kruskal();

    }

    public void kruskal(){
        int index = 0;  //  最后结果数组的索引
        //  用于保存 "MST" 中每个顶点在 MST 中的终点
        int[] ends = new int[vertex.length];
        //  创建结果数组，保存最后的 MST
        EData[] results = new EData[vertex.length - 1];
        EData[] edges = getEdges();
        //  按照边的权值大小进行排序
        sortEdges(edges);
        //  遍历 edges 数组，将边添加到 MST中时，怎么样判断是否形成了回路
        //  1. 如果没有构成回路，就加入到 results 中
        //  2. 否则不加入
        for (int i = 0; i < edgeNum; i++) {
            //  获取第 i 条边的起点
            int p1 = getPosition(edges[i].start);   //  p1 = 4
            //  获取第 i 条边的终点
            int p2 = getPosition(edges[i].end); //  p2 = 5

            //  分别获取在 已有 MST 中的终点【逐步形成】
            int p1End = getEnd(ends, p1);   //  4 【ends 初始值都是0，所以返回当前下标 4】
            int p2End = getEnd(ends, p2);   //  5
            if (p1End != p2End){    //  没有构成回路
                ends[p1End] = p2End;    //  设置 m 在 已有 ”MST“ 中的终点
                results[index++] = edges[i];  //  有一条边加入到 results
            }
        }
        //  统计并打印 MST
        System.out.println("生成的 MST 如下；");
        for (EData result : results) {
            System.out.println(result);
        }

    }


    public KRUSKAL(char[] vertex, int[][] matrix) {
        int vlen = vertex.length;
        //  初始化顶点，复制拷贝方式
        this.vertex = new char[vlen];
        for (int i = 0; i < vlen; i++) {
            this.vertex[i] = vertex[i];
        }
        //  初始化边
        this.matrix = new int[vlen][vlen];
        for (int i = 0; i < vlen; i++) {
            for (int j = 0; j < vlen; j++) {
                this.matrix[i][j] = matrix[i][j];
            }
        }
        //  统计边
        for (int i = 0; i < vlen; i++) {
            for (int j = i + 1; j < vlen; j++) {    //  这里也仅统计上三角
                if (this.matrix[i][j] != INF){
                    edgeNum++;
                }
            }
        }
    }

    public void print(){
        for (int i = 0; i < vertex.length; i++) {
            for (int j = 0; j < vertex.length; j++) {
                System.out.print(matrix[i][j] + "\t\t");
            }
            System.out.println();
        }
    }

    //  对边进行排序处理，冒泡排序
    private void sortEdges(EData[] edges){
        for (int i = edges.length - 1; i >= 1 ; i--) {
            for (int j = 0; j < i; j++) {
                if (edges[j].weight > edges[j + 1].weight){ //  大数沉底
                    swap(edges, j, j + 1);
                }
            }
        }
    }

    public void swap(EData[] edges, int i, int j){
        EData data = edges[i];
        edges[i] = edges[j];
        edges[j] = data;
    }

    //  返回 ch 对应的下标
    private int getPosition(char ch){
        for (int i = 0; i < vertex.length; i++) {
            if (vertex[i] == ch){
                return i;
            }
        }
        return -1;
    }

    //  获取图中边，放到 EData[] 数组中，后面我们需要遍历该数组
    //  通过 matrix 邻接矩阵得到
    //  EData[] 形式：{{'A', 'B', 12}. {'B', 'F', 7}, ...}
    private EData[] getEdges(){
        int index = 0;
        EData[] edges = new EData[edgeNum];
        for (int i = 0; i < vertex.length; i++) {
            for (int j = i + 1; j < vertex.length; j++) {   //  只有上三角的
                if (matrix[i][j] != INF){
                    edges[index++] = new EData(vertex[i], vertex[j], matrix[i][j]);
                }
            }
        }
        return edges;
    }

    //  获取下标为 i 的顶点的终点，用于后面判断2个顶点的终点是否相同
    //  ends[]：记录各个顶点对应的终点
    private int getEnd(int[] ends, int i){
        while (ends[i] != 0){
            i = ends[i];
        }
        return i;
    }
}

class EData{
    char start;
    char end;
    int weight;

    public EData(char start, char end, int weight) {
        this.start = start;
        this.end = end;
        this.weight = weight;
    }

    @Override
    public String toString() {
        return "EData{" +
                "start=" + start +
                ", end=" + end +
                ", weight=" + weight +
                '}';
    }
}
```

![image-20230504184101616](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305041841915.png)
