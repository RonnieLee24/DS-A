# ACM

## 1. Java ACM 处理

### Scanner对象创建，以及next、nextLine方法

Java语言最常用的输入获取方式就是使用java.util.Scanner类，通常情况下，我们会使用Scanner类来获取控制台输入，因此在创建Scanner对象时，会传入控制台输入流System.in

```java
import java.util.Scanner;
 
public class Main {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
  }
}
```

Scanner对象有两种方式获取控制台输入的字符串信息：next和nextLine，两种方式的区别是：

- nextLine是按行截取，即将换行符作为nextLine输入获取的截止符

  ![image-20230530150036636](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305301500701.png)

  ```java
  import java.util.*;
  
  import java.util.Scanner;
  
  // 注意类名必须为 Main, 不要有任何 package xxx 信息
  public class Main {
      public static void main(String[] args) {
          Scanner in = new Scanner(System.in);
          // 注意 hasNext 和 hasNextLine 的区别
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

- next是按空格截取，即将空格作为next输入获取的截止符

  ![image-20230530150436226](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202305301504265.png)

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

## 2. hasNext() 和 hasNextLine

判断输入的字符是否符合规则要求

- HasNext会以空格为结束标志，空格后的数字会抛弃掉
- HashNextLine会以Enter为结束标志