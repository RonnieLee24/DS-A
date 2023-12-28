# newCode

![图片](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301101441119.png)

## 1. 字符串最后一个单词长度

注意：【next() 和 nextLine() 区别】

- next(); 读取一个字符串，该字符在一个 <font color="yellow">**空白符** </font>之前结束
- nextLine(); 读取一行文本（即以按下 <font color="yellow">**enter键** </font>为结束标志）--- 识别 空格

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String s = in.nextLine();
        String[] s1 = s.split(" ");
        System.out.println(s1[s1.length - 1].length());
    }
}
```

## 2. 计算某字符出现的次数

f由于要求不区分大小写

- 那么我们一开始就都转化为 小写，来规避这个问题

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        int count = 0;
        Scanner in = new Scanner(System.in);
        String s = in.nextLine().toLowerCase();
        String b = in.nextLine().toLowerCase();
        char[] chars = s.toCharArray();
        for (int i = 0; i < chars.length; i++) {
            if (b.equals(String.valueOf(chars[i]))){
                count++;
            }
        }
        System.out.println(count);
    }
}
```

## 3. 删除重复数字，并排序

- 去重
- 排序
- TreeSet

```java
import java.util.HashSet;
import java.util.Scanner;
import java.util.Arrays;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
      HashSet<Integer> set = new HashSet<>();
        Scanner in = new Scanner(System.in);
        String s = in.nextLine();
        int i = Integer.parseInt(s);	//	个数
        for (int j = 0; j < i; j++) {
            set.add(Integer.parseInt(in.nextLine()));
        }

        Object[] objects = set.toArray();
        Arrays.sort(objects);
        for (Object object : objects) {
            System.out.println(object);
        }
    }
}
```

## 4. 字符串分隔

```java
import java.util.Scanner;
import java.util.Arrays;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
         while (in.hasNext()) { // 注意 while 处理多个 case
            String s = in.next();
            char[] chars = s.toCharArray();
            int length = chars.length;
            if (length < 8) {
                char[] chars1 = Arrays.copyOf(chars, 8);
                for (int i = 0; i < 8; i++) {
                    if (chars1[i] == '\0') {
                        chars1[i] = '0';
                    }
                }
                System.out.print(chars1);
            } else {
                int count = length / 8;    //  有多少个 8
                int remain = length % 8;
                for (int i = 1; i <= count; i++) {
                    for (int j = (i - 1) * 8; j < i * 8; j++) {
                        System.out.print(chars[j]);
                    }
                    System.out.println();
                }
                for (int i = length - remain; i < length; i++) {
                    System.out.print(chars[i]);
                }
                if (remain != 0) {
                    for (int i = 0; i < 8 - remain; i++) {
                        System.out.print("0");
                    }
                }
            }
        }
    }
}
```

## 5. 进制转换（Hex ---> Dec）

```java
public class No1 {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        while (in.hasNext()) { // 注意 while 处理多个 case
            int sum = 0;
            String s = in.next();
            String s1 = s.substring(2).toLowerCase();
            char[] chars = s1.toCharArray();
            int length = chars.length;
            for (int i = 0; i < length; i++) {
                if (chars[i] < 'a'){	//	字母
                    int j = (chars[i] - '0');
                    sum += j * Math.pow(16, length - i - 1);
                }else {	//	数字
                    int j = (chars[i] - 'a') + 10;
                    sum += j * Math.pow(16, length - i - 1);
                }
            }
            System.out.println(sum);
        }
    }
}
```

## 6. 质数因子

- 本身是质数的情况（而且没有子质数）
- 2000000014 时，boolean 数组 boolean[] isPrime = new boolean[n]; 越界

```java
//	其他的就不说了，主要是关键代码那里，不超时。原理是平方数大于要判断的数，那肯定只剩自身了（不知道正不正确）。
import java.util.*;

public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int s = sc.nextInt();
        int x = s;
        int y = 2;
        StringBuilder sb = new StringBuilder("");
        while(x>1){
            if(x%y==0){
                x=x/y;
                sb.append(y+" ");
            }else{
                // 不超时的关键代码
                if(y>x/y){
                    y = x;
                }
                else {
                    y++;
                }
            }
        }
        System.out.println(sb.toString());
    }
}
```



```java
public class quzheng {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        while (in.hasNext()) { // 注意 while 处理多个 case
            String next = in.next();
            String[] split = next.split("\\.");	//	转义符号
            int i = Integer.parseInt(split[0]);
            int j = Integer.parseInt(split[1].charAt(0)+"");
            if (j >= 5){
                System.out.println(i + 1);
                continue;
            }
            System.out.println(i);
        }
    }
}
```



## 8. 合并表记录

```java
import java.util.*;
import java.util.Scanner;
// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
         Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        HashMap<String, Integer> map = new HashMap<>();
        String next1 = in.nextLine();
        int i1 = Integer.parseInt(next1);
        while (in.hasNextLine()) { // 注意 while 处理多个 case
            String next = in.nextLine();
            String[] s = next.split(" ");
            String s1 = s[0];
            int i = Integer.parseInt(s[1]);
            if (!map.containsKey(s1)){
                map.put(s1, i);
            }else {
                map.put(s1, map.get(s1) + i);
            }
            i1--;
            if (i1 == 0){
                break;
            }
        }
        //  HashMap 是无序的，要进行排序
        ArrayList<Integer> list = new ArrayList<>();
        for (String s : map.keySet()) {
            list.add(Integer.parseInt(s));
        }
        Collections.sort(list);
        for (Integer s : list) {
            System.out.println(s + " " + map.get(s+""));
        }
    }
}
```

注意：TreeMap 虽然会自动排序，但是如果key 为String的话，排序会有问题【会按照首位数字排序】

![image-20230115104318875](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301151043979.png)

修改后，代码如下：

```java
import java.util.*;
import java.util.Scanner;
// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        TreeMap<Integer, Integer> map = new TreeMap<>();
        String next1 = in.nextLine();
        int i1 = Integer.parseInt(next1);
        while (in.hasNextLine()) { // 注意 while 处理多个 case
            String next = in.nextLine();
            String[] s = next.split(" ");
            int j = Integer.parseInt(s[0]);
            String s1 = s[0];
            int i = Integer.parseInt(s[1]);
            if (!map.containsKey(j)){
                map.put(j, i);
            }else {
                map.put(j, map.get(j) + i);
            }
            i1--;
            if (i1 == 0){
                break;
            }
        }
        for (Integer integer : map.keySet()) {
            System.out.println(integer + " " + map.get(integer));
        }
    }
}
```

## 9. 提取不重复的整数

由于这个提，只是需要输出就行，所以可以利用

```java
set.add() //	这个方法返回的是 boolean 类型来进行判定，是否可以加入
```

```java
public class quzheng {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        String s = in.next();
        Deque<String> set = new LinkedList<>();
        for (int i = s.length() - 1; i >= 0; i--) {
            if (!set.contains(s.charAt(i) + "")) {
                set.addLast(s.charAt(i) + "");
            }
        }
        while (!set.isEmpty()){
            System.out.print(set.removeFirst() + " ");
        }
    }
}
```

```java
public class Main{
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        HashSet<Integer> hs = new HashSet<>();
        String str = sc.next();
        for (int i = str.length() - 1; i >= 0; i--) {
            int n = Integer.parseInt(String.valueOf(str.charAt(i)));
            if(hs.add(n)){
                System.out.print(n);
            }
        }
    }
}
```

## 10. 字符个数统计

字符 ---> ACSII码

```java
import java.util.Scanner;
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
     int count = 0;
        HashSet<Character> set = new HashSet<>();
        // 注意 hasNext 和 hasNextLine 的区别
        while (in.hasNext()) { // 注意 while 处理多个 case
            String s = in.next();
            char[] chars = s.toCharArray();
            for (int i = 0; i < chars.length; i++) {
                char aChar = chars[i];
                if ((int)(aChar)>=0 && (int)(aChar)<= 126&&set.add(aChar)){
                    count++;
                }
            }
        }
        System.out.println(count);
    }
}
```

## 11. 数字颠倒

```java
import java.util.Scanner;
import java.util.*;


// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        Deque<Character> list = new LinkedList<>();
        int count = 0;  //  记录 0 的个数
        // 注意 hasNext 和 hasNextLine 的区别
        char[] chars = in.next().toCharArray();
        for (char aChar : chars) {
            list.addFirst(aChar);
        }
        
        while (list.peekFirst() == 0){
            list.removeFirst();
            count++;
        }
        if (count == chars.length){
            System.out.println(0);
            return;
        }
        
        while (!list.isEmpty()){
            System.out.print(list.removeFirst());
        }
    }
}
```

不考虑多个 0 的时候：

```java
//	直接 reverse 即可
public class quzheng {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String str = in.nextLine();
        StringBuilder sb = new StringBuilder(str);
        sb.reverse();
        System.out.println(sb);
    }
}
```

## 12. 字符串反转

```java
//	直接 reverse 即可
public class quzheng {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String str = in.nextLine();
        StringBuilder sb = new StringBuilder(str);
        sb.reverse();
        System.out.println(sb);
    }
}
```

## 13. 句子逆序

```java
Scanner in = new Scanner(System.in);
// 注意 hasNext 和 hasNextLine 的区别
String[] s = in.nextLine().split(" ");
for (int i = s.length - 1; i >= 0; i--) {
    System.out.print(s[i] + " ");
}
```

## 14. 字符串排序

没说不重复所以用 ArrayList来储存

```java
import java.util.Scanner;
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
       Scanner in = new Scanner(System.in);
        int i = in.nextInt();
        ArrayList<String> list = new ArrayList<>();
        // 注意 hasNext 和 hasNextLine 的区别
        while (in.hasNextLine()&& i > 0) { // 注意 while 处理多个 case
            String s = in.next();
            list.add(s);
            i--;
        }
        Collections.sort(list);
        for (String s : list) {
            System.out.println(s);
        }
    }
}
```

也可以直接用数组

```java
import java.util.*;

public class Main {
    public static void main(String[] args) 
    {
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        String[] array = new String[n];
        for (int i = 0; i < n; i++) {
            array[i] = in.next();
        }
        Arrays.sort(array);
        for (String str : array) {
            System.out.println(str);
        }
    }
}
```

## 15. int 型整数在内存中存储时 1 的个数

统计二级制下 1 的个数

```java
Integer.bitCount()
```

```java
import java.net.Socket;
import java.util.*;

public class quzheng {
    public static void main(String[] args) {
        int a = 0xfffffff;  //  int 4 个字节：4 * 8 = 32 位
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        int i = in.nextInt();
        int i1 = Integer.bitCount(i & a);   //  计算二级制下1的个数
        System.out.println(i1);
    }
}

```

```java
import java.util.*;
public class Main{
    public static void main(String[] args){
       Scanner sc = new Scanner(System.in);
        //输入一个32位int型数字
        Integer input = sc.nextInt();
        //整数 转 二进制字符串
        String bstr =  Integer.toBinaryString(input);
        char[]  bArr = bstr.toCharArray();
        int count = 0;
        for(int i=0;i<bstr.length();i++){
            if(bArr[i]== '1'){
                count ++;
            }
        }
        System.out.println(count);
    }
}
```

## 21. 简单密码

- 小写字母：变成 9 键对应的数字
- 大写字母：变成小写字母之后，往后移一位【Z 往后移动是 a】
- 数字和其它的符号都不做变换

```java
import java.util.Scanner;
import java.util.HashMap;
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String s1 = in.next();
        StringBuilder sb = new StringBuilder();
        HashMap<String, String> map = new HashMap<>();
        map.put("1", "1");
        map.put("abc", "2");
        map.put("def", "3");
        map.put("ghi", "4");
        map.put("jkl", "5");
        map.put("mno", "6");
        map.put("pqrs", "7");
        map.put("tuv", "8");
        map.put("wxyz", "9");
        map.put("0", "0");
        for (int i = 0; i < s1.length(); i++) {
            String substring = s1.substring(i, i + 1);
            if (substring.matches("[a-z]")){    //  小写字母
                for (String s : map.keySet()) {
                    if (s.contains(substring)){
                        sb.append(map.get(s));
                    }
                }
            }else if(substring.matches("[A-Z]")){ //  大写字母
                String ss = substring.toLowerCase();
                String s2 = (ss.equals("z"))? "a" : (char)(ss.charAt(0) + 1) + "";
                sb.append(s2);
            }else { //  数字 和 其它字符
                sb.append(substring);
            }
        }
        System.out.println(sb);
    }
}
```

## 23. 删除字符串中出现次数最少的字符

```java
//	Map中会存储一一对应的key和value。
//	如果 在Map中存在key，则返回key所对应的的value。
//	如果 在Map中不存在key，则返回默认值
Map.getOrDefault(key，默认值)；
```

```java
import java.util.Collections;
import java.util.HashMap;
import java.util.Scanner;
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNext()) {
            String s = scanner.nextLine();
            char[] chars = s.toCharArray();
            //统计每个字母的数量
            HashMap<Character, Integer> map = new HashMap<>();
            for (char aChar : chars) {
                map.put(aChar, (map.getOrDefault(aChar, 0) + 1));
            }
            //找到数量最少的字符数量
            Collection<Integer> values = map.values();
            Integer min = Collections.min(values);

            //用空字符串替换该字母
            for (Character character : map.keySet()) {
                if (map.get(character) == min){
                    s = s.replaceAll(String.valueOf(character), "");
                }
            }
            System.out.println(s);
        }
    }
}
```

- 实现删除字符串中出现次数最少的字符，若出现次数最少的字符有多个，则把出现次数最少的字符都删除。
- 输出删除这些单词后的字符串，字符串中其它字符保持原来的顺序。

```java
import java.util.Scanner;
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String s = in.next();
        HashSet<String> set = new HashSet<>();
        HashMap<String, Integer> map = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            String substring = s.substring(i, i + 1);
            set.add(substring);
        }
        for (String s1 : set) {
            int count = 0;
            for (int i = 0; i < s.length(); i++) {
                String substring = s.substring(i, i + 1);
                if (s1.equals(substring)){
                    count++;
                }
            }
            map.put(s1, count);	//	唯一出现次数
        }
        int min = Collections.min(map.values());
        for (String s1 : map.keySet()) {
            if (map.get(s1) == min){
                s = s.replaceAll(s1, "");	//	逐个删除！！！
            }
        }
        System.out.println(s);
    }
}
```

## 26. 字符串排序

1. 英文字母从 A 到 Z 排列，不区分大小写。
2. 同一个英文字母的大小写同时存在时，按照输入顺序排列
3. 非英文字母的其它字符保持原来的位置。

```java
import java.util.Scanner;
import java.util.*;
// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        String s = in.nextLine();
        int length = s.length();
        StringBuilder sb = new StringBuilder();
        TreeSet<Integer> set1 = new TreeSet<>();
        TreeMap<Integer, String> map = new TreeMap<>();
        for (int i = 0; i < s.length(); i++) {
            String substring = s.substring(i, i + 1);
            if (!substring.matches("[a-zA-Z]")){
                set1.add(i); //  空格和其它字符都不动，只需改变 字母的舒徐即可【这些可以通过 map 取出】
            }else {
                sb.append(substring);	//	sb 储存所有的字母
            }
            map.put(i, substring);  //  空格和其它字符都不动，只需改变 字母的舒徐即可
        }
        String s1 = sb.toString();
        char[] chars = s1.toCharArray();
        String[] strings = new String[chars.length];
        for (int i = 0; i < chars.length; i++) {
            strings[i] = chars[i]+"";
        }
        Arrays.sort(strings, String.CASE_INSENSITIVE_ORDER);
        int count = 0;	//	用于遍历 Strings（字母）
        for (int i = 0; i < length; i++) {
            if (!set1.contains(i)){	//	如果需要改变
                map.put(i, strings[count++]);
            }
        }
        for (Integer integer : map.keySet()) {
            System.out.print(map.get(integer));
        }
    }
}
```

```java
import java.util.*;

public class Main {

    public static String sort(String str) {
        // 先将英文字母收集起来
        List<Character> letters = new ArrayList<>();
        for (char ch : str.toCharArray()) {
            if (Character.isLetter(ch)) {
                letters.add(ch);
            }
        }
        // 将英文字母先排序好
        letters.sort(new Comparator<Character>() {
            public int compare(Character o1, Character o2) {
                return Character.toLowerCase(o1) - Character.toLowerCase(o2);
            }
        });
        // 若是非英文字母则直接添加
        StringBuilder result = new StringBuilder();
        for (int i = 0, j = 0; i < str.length(); i++) {
            if (Character.isLetter(str.charAt(i))) {
                result.append(letters.get(j++));
            }
            else {
                result.append(str.charAt(i));
            }
        }
        return result.toString();
    }

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        while (in.hasNextLine()) {
            String str = in.nextLine();
            String res = sort(str);
            System.out.println(res);
        }
    } 
}
```

## 27. 查找兄弟单词

1. 定义一个单词的“兄弟单词”为：交换该单词字母顺序（注：可以交换任意次），而不添加、删除、修改原有的字母就能生成的单词。
2. 兄弟单词要求和原来的单词不同。例如： ab 和 ba 是兄弟单词。 ab 和 ab 则不是兄弟单词。
3. 现在给定你 n 个单词，另外再给你一个单词 x ，让你寻找 x 的兄弟单词里，按字典序排列后的第 k 个单词是什么？
4. 注意：字典中可能有重复单词。

| 输入                | 输出                    |
| ------------------- | ----------------------- |
| 3 abc bca cab abc 1 | 2 【兄弟单词个数】      |
| 3【单词个数】       | bca 【第 k 个兄弟单词】 |
| abc                 |                         |
| bca                 |                         |
| cab                 |                         |
| abc 【单词 x】      |                         |
| 1 【整数 k】        |                         |

```java
import java.util.Scanner;
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        int count = 0;
        String s = in.nextLine();
        ArrayList<String> list = new ArrayList<>();
        String[] s1 = s.split(" ");
        String ss = s1[s1.length - 2];  //  要查找的 字符串 【倒数第二个】
        int index = Integer.parseInt(s1[s1.length - 1]);    //  第 k 个 【最后一个】
        for (int i = 1; i < s1.length - 2; i++) {
            //  个数相同
            if (isBrother(s1[i], ss)){
                count++;
                list.add(s1[i]);
            }
        }
        Collections.sort(list);
        System.out.println(count);
        if (count >= index) {	//	第 k 个 不存在时，不输出！！！
            System.out.println(list.get(index - 1));
        }
    }

    public static boolean isBrother(String str1, String des_str){
        if (str1.length() != des_str.length()){ //  长度不等
            return false;
        }
        if (str1.equals(des_str)){  //  完全一样
            return false;
        }

        //  2个字串排序后应该完全一致
        char[] chars1 = str1.toCharArray();
        char[] chars2 = des_str.toCharArray();
        Arrays.sort(chars1);
        Arrays.sort(chars2);
        return Arrays.equals(chars1, chars2);  //  相等返回 false，否则就是兄弟
    }
}
```

## 29. 字符串加解密

对输入的字符串进行加解密，并输出。

1. 加密方法为：
   - 当内容是英文字母时
     - 用该英文字母的 <font color="yellow">**后一个字母** </font>替换
     - 同时字母 <font color="yellow">**变换大小写**</font>,如字母a时则替换为B；字母Z时则替换为a；
   - 当内容是数字时则把该数字加1，如0替换1，1替换2，9替换0；
   - 其他字符不做变化。
2. 解密方法为加密的逆过程
   - 字母：
     - 变换大小写
     - 用前一个替换
   - 数字
     - -1
     - 9 ---> 0

数据范围：输入的两个字符串长度满足 1 \le n \le 1000 \1≤*n*≤1000 ，保证输入的字符串都是只由大小写字母或者数字组成

```java
import java.util.Scanner;
public class fasfe {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        String s1 = in.nextLine();
        String s2 = in.nextLine();
        encrypt(s1);
        System.out.println();
        decrypt(s2);
    }

    public static void encrypt(String str) {
        char[] chars = str.toCharArray();
        for (int i = 0; i < chars.length; i++) {
            String s = chars[i] + "";
            if (s.matches("[a-zA-Z]")) {   //  字母
                if (s.matches("[a-z]")) {    //  小写字母
                    if (chars[i] == 'z'){
                        chars[i] = 'A';
                    }else {
                        chars[i] = ((char) (s.toUpperCase().charAt(0) + 1));
                    }
                } else {    //  大写字母
                    if (chars[i] == 'Z'){
                        chars[i] = 'a';
                    }else {
                        chars[i] = ((char) (s.toLowerCase().charAt(0) + 1));
                    }
                }
            } else {
                int j = Integer.parseInt(s);
                if (j == 9) {
                    chars[i] = '0';
                } else {
                    chars[i] = (char) (j + 1 + '0');
                }
            }
        }
        for (char aChar : chars) {
            System.out.print(aChar);
        }
    }

    public static void decrypt(String str) {
        char[] chars1 = str.toCharArray();
        for (int i = 0; i < chars1.length; i++) {
            String s = chars1[i] + "";
            if (s.matches("[a-zA-Z]")) {   //  字母
                if (s.matches("[a-z]")) {    //  小写字母
                    if (chars1[i] == 'a'){
                        chars1[i] = 'Z';
                    }else {
                        chars1[i] = ((char) (s.toUpperCase().charAt(0) - 1));
                    }
                } else {
                    if (chars1[i] == 'A'){
                        chars1[i] = 'z';
                    }else {
                        chars1[i] = ((char) (s.toLowerCase().charAt(0) - 1));
                    }                
                }
            } else {
                int j = Integer.parseInt(s);
                if (j == 0) {
                    chars1[i] = '9';
                } else {
                    chars1[i] = (char) (j - 1);
                }
            }
        }
        for (char aChar : chars1) {
            System.out.print(aChar);
        }
    }
}
```

## 31. 单词倒排

利用正则表达式，根据一个或多个空格来划分

```java
//	s:space 空格!!!
String[] split = s1.split("[\\s]+");
```

```java
public class qqq {
    public static void main(String[] args) {
        //  $bo*y gi!r#l
        Scanner in = new Scanner(System.in);
        StringBuilder sb = new StringBuilder();
        // 注意 hasNext 和 hasNextLine 的区别
        String s = in.nextLine();
        for (int i = 0; i < s.length(); i++) {
            String substring = s.substring(i, i + 1);
            if (!substring.matches("[a-zA-Z]")){    //  特殊符号
                sb.append(" ");
            }else { //  字母
                sb.append(substring);
            }
        }
        String s1 = sb.toString();
        String[] split = s1.split("[\\s]+");
        for (int i = split.length - 1; i >= 0; i--) {
            System.out.print(split[i] + " ");
        }
    }
}
```

## 33. 整数与 IP 地址间的转换

Java数字

数据大小，类型范围

- int 占 4个字节，共有 32 位置：4 * 8 = 32【4294967296】
- Integer 表示数字的范围：
- JAVA中没有无符号的数，换言之，java中的数都是有符号的
- 在计算机运算的时候，都是以 <font color=orange>**补码**</font> 的方式来运算的
- 当我们看运算结果时，要看它的原码

| 最大值                             | 最小值                              |
| ---------------------------------- | ----------------------------------- |
| **Integer.MAX_VALUE** = 2147483647 | **Integer.MIN_VALUE** = -2147483648 |
| INT_MAX 0x7fffffff                 | INT_MIN 0x80000000                  |



| 整型数据类型       | 数据范围                                 |
| ------------------ | ---------------------------------------- |
| byte（1字节)       | -128 ~ 127（-2^7 ~ 2^7-1)                |
| short（2字节）     | -32768 ~ 32767（-2^15 ~ 2^15-1           |
| 有符号int（4字节） | -2147483648 ~ 2147483647（-2^31 ~ 2^31） |
| 无符号int（4字节） | 0 ~ 2^32 -1 （4294967295）               |
| long（8字节）      | -2^63 ~ 2^63-1                           |

```java
//	二进制 ---> 转为整数
//	在符合 int 范围时，可以直接转换
int i = Integer.parseInt(sb.toString(), 2);
```

当输入用例

```python
# 考虑边界值
39.66.68.72 ---> 255.255.255.255 【FF FF FF FF】
3868643487 【2147483647】
```

自己的方法：

```java
import java.lang.reflect.Array;
import java.util.*;
public class qqq {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String s1 = in.nextLine();  //  ip
        String s3 = in.nextLine();  //  十进制数
        long i2 = Long.parseLong(s3);	//	转成 long 类型 
        StringBuilder sb = new StringBuilder();
        StringBuilder sb1 = new StringBuilder();
        String[] split = s1.split("\\.");
        for (int i = 0; i < split.length; i++) {
            int i1 = Integer.parseInt(split[i]);
            String s2 = Integer.toBinaryString(i1);
            while (s2.length() < 8){
                s2 = "0" + s2;
            }
            split[i] = s2;
        }
        for (String s : split) {
            sb.append(s);
        }
        long i = Long.parseLong(sb.toString(), 2);
        System.out.println(i);
        String s = Long.toBinaryString(i2);
        while (s.length() < 32){
            s= "0" + s;
        }
        for (int j = 0; j <= s.length() - 8; j += 8) {
            String substring = s.substring(j, j + 8);
            long i1 = Long.parseLong(substring, 2);
            sb1.append(i1).append(".");
        }
        sb1.deleteCharAt(sb1.length() - 1); //  删除最后的点！！！
        System.out.println(sb1);
    }
}
```

256进制

```java
import java.util.*;

public class Main {

    private final int N = 4;
    public Main() {
    }

    public String convert(String str) {
        // ipv4 -> int
        if (str.contains(".")) {	//	ip 地址【255.255.255.255】---> XXXX
            String[] fields = str.split("\\.");
            long result = 0;
            //	result = 0位置 + 1位置 * 256 + 2 位置 * 256 * 256 + 3 位置 * 256 * 256 * 256
		   //   result = 0 位置 + 256（1位置 + 256 * 2 位置 + 256 * 256 * 3 位置）
            for (int i = 0; i < N; i++) {	//	i：0 1 2 3
                result = result * 256 + Integer.parseInt(fields[i]);
            }
            return "" + result;
        }
        // int -> ipv4
        else {	//	整数
            long ipv4 = Long.parseLong(str);
            String result = "";
            for (int i = 0; i < N; i++) {
                result = ipv4 % 256 + "." + result;
                ipv4 /= 256;	//	砍掉末位
            }
            return result.substring(0, result.length() - 1);
        }
    }

    public static void main(String[] args) {
        Main solution = new Main();
        Scanner in = new Scanner(System.in);
        while (in.hasNext()) {
            String str = in.next();
            String res = solution.convert(str);
            System.out.println(res);
        }
    } 
}
```

思路：

result = 0位置 * 256 * 256 * 256 + 1位置 * 256 * 256 + 2位置 * 256 + 3位置
          = (0位置 * 256 * 256 + 1位置 * 256 + 2位置) * 256 + 3位置  
         =  {[(<font color="yellow">0</font>位置 * 256 + <font color="yellow">1</font>位置) * 256] + <font color="yellow">2</font>位置} * 256 + <font color="yellow">3</font>位置

## 34. 图片整理

```java
import java.util.Scanner;
// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String s = in.nextLine();
        String[] s1 = s.split(" ");
        System.out.println(s1[s1.length - 1].length());
    }
}
```

## 35. 蛇形矩阵

```java
import java.util.Arrays;
import java.util.Scanner;
public class sdfe {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int[][] arr = new int[n][n];
        for (int i = 0; i < arr.length; i++) {
            arr[i][0] = f(i);   //  给第1列赋值！！！
        }
        //  根据第一列给其它地方赋值
        //  arr[i--][j++] ---> {i >= 0 && j < arr.length}
        for (int i = arr.length - 1; i >= 0; i--) { //  4、3、2、1、0
            for (int j = 0; j < arr.length; j++) {  //  从第2列开始赋值
                int row = i;
                int col = j;
                while (row >= 1 && col < arr.length - 1){
                    arr[row - 1][col + 1] = arr[row][col] + 1;
                    row--;
                    col++;
                }
            }
        }
        printArr(arr);
    }
    public static void printArr(int[][] arr){   //  输出上半三角形
        for (int i = 0; i < arr.length; i++) { //   0、1、2、3、4【行数】
            for (int j = 0; j < arr.length - i; j++) {   //  5、4、3、2、1【个数】
                System.out.print(arr[i][j]+ " ");
            }
            System.out.println();
        }
    }
    public static int f(int n){
        //  baseCase
        if (n == 0){
            return 1;
        }
        return n + f(n - 1);
    }
}
```

## 36. 字符串加密

需求描述：

1. 首先，选择一个单词作为密匙，如 TRAILBLAZERS
2. 如果单词中包含有重复的字母，只保留第1个，将所得结果作为新字母表开头，并将新建立的字母表中未出现的字母按照正常字母表顺序加入新字母表。



## 37. 统计每个月的兔子

从第3个月开始：

每次都会有一只小的变成大的：

|      | 1    | 2    | 3                             | 4         | 5          | 6         | 7         | 8         |      |      |
| ---- | ---- | ---- | ----------------------------- | --------- | ---------- | --------- | --------- | --------- | ---- | ---- |
| 总数 | 1    | 1    | 2                             | 3         | 5          | 8         | 13        | 21        | ……   | ……   |
| 大   | 0    | 0    | <font color="yellow">1</font> | 1         | 2          | 3         | 5         | 8         | ……   | ……   |
| 小   | 1    | 1    | 1(1)                          | 1(1) 1(2) | 2(1)  1(2) | 3(1) 2(2) | 5(1) 3(2) | 8(1) 5(2) | ……   | ……   |

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
       Scanner in = new Scanner(System.in);
        int i = in.nextInt();
        System.out.println(rabbit(i));
    }

    public static int rabbit(int n){
        if (n == 1 || n == 2){
            return 1;
        }
        return rabbit(n - 1) + rabbit(n - 2); 
    }
}
```

## 38. 求小球落地5次后所经历的路程和第5次反弹的高度

## 39. 判断 2 个 ip 是否属同一个子网

```java
254.255.0.0	//	不符合规则！！！
85.122.52.249
10.57.28.117
//  掩码 mask 格式问题：111111111100000000 【1后面都是0】
1.255.255.0
187.39.235.7
219.79.189.231
```

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
       String mask = "";
        String ip1 = "";
        String ip2 = "";
        Scanner in = new Scanner(System.in);
        int count = 0;
        // 注意 hasNext 和 hasNextLine 的区别
        while (in.hasNextLine()){
            count++;    
            if (count == 1){
                mask = in.nextLine();
            }else if (count == 2){
                ip1 = in.nextLine();
            }else if (count == 3){
                ip2 = in.nextLine();
                System.out.println(netWork_Num(mask, ip1, ip2));
                count = 0;
            }
        }
    }

    public static int netWork_Num(String mask, String ip1, String ip2){
        String[] mask_1 = mask.split("\\.");
        String[] ip1_1 = ip1.split("\\.");
        String[] ip2_1 = ip2.split("\\.");

        for (int i = 0; i < mask_1.length; i++) {   //  越界问题
            int j = Integer.parseInt(mask_1[i]);
            int k = Integer.parseInt(ip1_1[i]);
            int l = Integer.parseInt(ip2_1[i]);
            if (j < 0 || j > 255 || k < 0 || k > 255 || l < 0 || l > 255) {
                return 1;
            }
        }

        StringBuilder sb = new StringBuilder();
        //  掩码 mask 格式问题：111111111100000000 【1后面都是0】
        for (String s : mask_1) {
            int i = Integer.parseInt(s);
            String s1 = Integer.toBinaryString(i);
             while (s1.length() < 8){
                s1 = "0" + s1;
            }
            sb.append(s1);
        }
        String s = sb.toString();
        while (s.startsWith("1")){
            s = s.substring(1);
        }
        if (s.contains("1")){
            return 1;
        }


        //  根据 mask 的位数来比较是否相等
        int count = 0;
        for (int i = mask_1.length - 1; i >= 0; i--) {
            if (!mask_1[i].equals("0")) {   //  如果市 0 的话
                break;
            }
            count++;
        }
        for (int i = 0; i < count; i++) {   //  比较前几个是否相同即可！！！
            if (!ip1_1[i].equals(ip2_1[i])) {
                return 2;
            }
        }
        return 0;   //  同一个子网
    }
}
```

## 41. 称砝码

|          | 输入 |      | 输出                |
| -------- | ---- | ---- | ------------------- |
| 砝码种数 | 2    |      |                     |
| 砝码重量 | 1    | 2    |                     |
| 砝码个数 | 2    | 1    | 5 【0、1、2、3、4】 |









## 40. 统计字符

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
       Scanner in = new Scanner(System.in);
        String str = in.nextLine();
        
        int eng = 0;
        int space = 0;
        int num = 0;
        int other = 0;
        for (int i = 0; i < str.length(); i++) {
            String substring = str.substring(i, i + 1);
            if (substring.matches("[a-zA-Z]")){
                eng++;
            }else if (substring.matches("\\s")){
                space++;
            }else if (substring.matches("[0-9]")){
                num++;
            }else {
                other++;
            }
        }
        System.out.println(eng);
        System.out.println(space);
        System.out.println(num);
        System.out.println(other);
    }
}
```

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = br.readLine();
        int[] count = new int[4];
        for(int i = 0;i < line.length();i++){
            char ch = line.charAt(i);
            if(Character.isLetter(ch)){
                count[0]++;
            }else if(ch == ' '){
                count[1]++;
            }else if(Character.isDigit(ch)){
                count[2]++;
            }else{
                count[3]++;
            }
        }
        for(int i = 0;i < 4;i++){
            System.out.println(count[i]);
        }
    }
}
```

## 46. 截取字符串

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String str = in.next();
        int i = in.nextInt();
        System.out.println(str.substring(0, i));
    }
}
```

## 48. **从单向链表中删除指定值的节点**

```java
import java.util.Scanner;
import java.util.*;
// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        ArrayList<String> list = new ArrayList<>();
        String s = in.nextLine();
        String[] s1 = s.split(" ");
        //	待删除节点
        String delete = s1[s1.length - 1];
        list.add(s1[1]);    //  list 插入 head
        for (int i = 2; i < s1.length - 1; i+=2) {  //   从 2 开始，每 2 个一组
            int index = list.indexOf(s1[i + 1]);    //  找到位置
            list.add(index + 1, s1[i]); //  在该位置后面插入！！！
        }

        if (list.remove(delete)){
            for (String s2 : list) {
                System.out.print(s2 + " ");
            }
        }
    }
}
```

## 51. **输出单向链表中倒数第k个结点**

```java
import java.util.Scanner;
import java.util.LinkedList;


// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
       Scanner in = new Scanner(System.in);
        while (in.hasNextLine()) {
            String i = in.nextLine();
            int i1 = Integer.parseInt(i);
            String str = in.nextLine();
            String num = in.nextLine();
            int i2 = Integer.parseInt(num);
            LinkedList<String> list = new LinkedList<>();
            String[] s = str.split(" ");
            for (String s1 : s) {
                list.add(s1);
            }

            if (i1 - i2 >= 0) {
                System.out.println(list.get(i1 - i2));
            }
        }
    }
}
```

## 53. 杨辉三角的变形 【普通方法超时了！！！】

​	![image-20230119161435734](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301191614855.png)

找规律：

![image-20230119163054372](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301191630442.png)

递推方法：**<font color="yellow">超时</font>** ！！！

```java
import java.util.Scanner;

public class triangle {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int[][] arr = new int[n][n];
        arr[0][n - 1] = 1;  //  初始值

        //  列边界：n - 1
        for (int i = 1; i <= n - 1 ; i++) {  //  从第二行开始逐行赋值
            for (int j = n - i - 1; j <= n - 1; j++) {
                if (j != n - 1) {   // 不是最后1列
                    if (i == n - 1 && j == 0) {
                        arr[i][0] = 1;
                    }else { //  防止越界
                        arr[i][j] = arr[i - 1][j - 1] + arr[i - 1][j] + arr[i - 1][j + 1];
                    }
                } else {
                     arr[i][j] = 2 * arr[i-1][j-1] + arr[i-1][j];
                }
            }
        }
        
        boolean flag = false;
        for (int i = 0; i < n; i++) {
            if (arr[n-1][i] % 2 == 0){
                flag = true;
                System.out.println(i + 1);
                break;
            }
        }
        if (!flag){
            System.out.println("-1");
        }
    }

    public static void arrPrint(int arr[][]){
        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < arr[0].length; j++) {
                System.out.print(arr[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```

![image-20230119180237353](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202301191802416.png)

## 54. 表达式求值

中缀 ---> 后缀

按照后缀规则来进行计算



## 56. 完全数计算

完全数（Perfect number）

它所有的真因子（即除了自身以外的约数）的和（即因子函数），恰好等于它本身。

例如：28，它有约数1、2、4、7、14、28，除去它本身28外，其余5个数相加，1+2+4+7+14=28。

## 58. 输入n个整数，输出其中最小的k个

```java
import java.util.Scanner;
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        ArrayList<Integer> list = new ArrayList<>();
        String s1 = in.nextLine();
        String s2 = in.nextLine();
        String[] s = s1.split(" ");
        int i = Integer.parseInt(s[1]);

        String[] split = s2.split(" ");
        for (String s3 : split) {
            list.add(Integer.parseInt(s3));
        }

        Collections.sort(list);
        for (int j = 0; j < i; j++) {
            System.out.print(list.get(j) + " ");
        }
    }
}
```

## 59. 找出字符串中第一个只出现一次的字符

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String result = "";
        String str = in.nextLine();
        boolean flag = false;
        while (str.length() > 0){
            String substring = str.substring(0, 1);	//	每次都取出首字母
            if (!str.substring(1).contains(substring)){	//	如果后面没有的话，输出
                result = substring;
                flag = true;
                break;
            }
            str = str.replaceAll(substring, "");	//	全部替换为 ""
        }
        if (!flag){
            System.out.println("-1");
        }else {
            System.out.println(result);
        }
    }
}
```

## 60. **查找组成一个偶数最接近的两个素数**（⭐）

​	任意一个 <font color="yellow">**偶数**</font>（大于2）都可以 <font color="red">**由2个素数** </font>组成，组成偶数的2个素数有很多种情况，本题目要求输出组成指定偶数的两个素数 <font color="yellow">**差值最小** </font>的素数对。

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
   public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNext()){
            int num = scanner.nextInt();
            solution(num);
        }
    }
 
    private static void solution(int num) {
        //如num=10, 遍历:5,6,7,8
        // 从最接近的两个中位数开始处理判断
        for(int i = num / 2; i < num - 1; i++) {	//	因为另外一个数必须大于2，所以这里 i < num - 1
            if(isPrime(i) && isPrime(num - i)) {
                System.out.println((num - i) + "\n" + i);
                return;
            }
        }
    }
    // 判断是否素数
    private static boolean isPrime(int num) {
        for(int i = 2; i <= Math.sqrt(num); i++) {
            if(num % i == 0) {
                return false;
            }
        }
        return true;
    }
}
```

## 81. 字符串字符匹配

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        String a = in.nextLine();
        String b = in.nextLine();
        for (char c : a.toCharArray()) {
            if (b.indexOf(c) == -1){
                System.out.println("false");
                return;
            }
        }
        System.out.println("true");
    }
}
```

## 84. 统计大写字母个数

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        int sum = 0;
        String s = in.nextLine();
        for (char c : s.toCharArray()) {
            if (Character.isUpperCase(c)){
                sum++;
            }
        }
        System.out.println(sum);
    }
}
```

## 86. 求最大连续 bit 数

```java
public class fe {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        int len = 0;
        int maxLen = 0;
        int i = in.nextInt();
        while (i != 0){
            if ((i & 1) == 1){  //  看结尾是否是 1
                len++;
            }else {
                len = 0;
            }
            maxLen = Math.max(maxLen, len); //  更新

            i >>= 1;    //  右移一位
        }
        System.out.println(maxLen);
    }
}
```

## 91. 走方格的方案数

```java
public class fe {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        String s = in.nextLine();
        String[] s1 = s.split(" ");

        int n = Integer.parseInt(s1[0]) + 1;    //  因为是沿着线走，所以 + 1
        System.out.println(n);
        int m = Integer.parseInt(s1[1]) + 1;
        System.out.println(m);

        int[][] dp = new int[n + 1][m + 1];
        dp[0][1] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]; //  当前 = 上 + 左
            }
        }
        System.out.println(dp[n][m]);
    }
}
```



## 106. 字符逆序

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        String s = in.nextLine();
        char[] chars = s.toCharArray();
        swap(chars, 0, chars.length - 1);
        System.out.println(new String(chars));
    }

    public static void swap(char[] chars, int l, int r){
        while (l < r) {
            char temp = chars[l];
            chars[l] = chars[r];
            chars[r] = temp;
            l++;
            r--;
        }
    }
}
```

