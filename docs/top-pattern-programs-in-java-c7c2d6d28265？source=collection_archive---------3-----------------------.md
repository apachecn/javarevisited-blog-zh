# Java 中的顶级模式程序

> 原文：<https://medium.com/javarevisited/top-pattern-programs-in-java-c7c2d6d28265?source=collection_archive---------3----------------------->

![](img/04d238a146b67c82e799b8f0ebccb4ba.png)

Java 模式程序对于有抱负的 Java 专业人员理解和实践是至关重要的，因为基于模式的 Java 程序有助于分析编程能力。在这篇文章中，我们试图提供用 Java 编写的模式程序，并给出详细的解释。这个博客将作为 Java 打印模式的综合指南。

# Java 星形模式

1.  **方形图案**

**输入:**模式 _ 大小= 6

**输出:**

```
 *****

                                *****

                                *****

                                *****

                                *****

                                *****
```

**代码:**

```
ze; i++) {
 //inner loop
 for (int j = 0; j < pattern_size; j++) {
 //print star
 System.out.print(“*”);
 }

//print new line
 System.out.println(“\n”);
 }
 }
}
```

**2。空心方形图案**

**输入:**模式大小= 6

**输出:**

```
 * * * * * *

                       *         *

                       *         *

                       *         *

                       *         *

                       * * * * * *
```

**代码:**

```
import java.io.*;
import java.util.*;

class Solution {
 public static void main (String[] args) {
        //pattern_size of the square
                  int pattern_size = 6;
                 //outer loop
                 for (int i = 0; i < pattern_size; i++) {
                //inner loop
                for (int j = 0; j < pattern_size; j++) {
                      //print only star in first and last row
                     if (i == 0 || i == pattern_size - 1) {
                           System.out.print("*");
                     }
                     else {
                           //print star only at first and last position row
                          if (j == 0 || j == pattern_size - 1) {
                                System.out.print("*");
                          }
                          else {

                                //print empty space
                                System.out.print(" ");
                          }
               }
          }
             //print new line
             System.out.println("\n");
        }
   }
} 
```

3.**Java 中的左三角模式程序**

**输入:**模式大小= 6

**输出:**

```
 *

                            **

                            ***

                            ****

                            *****

                            ******
```

**代码:**

```
import java.io.*;
import java.util.*;

class Solution {
 public static void main (String[] args) {
        //pattern_size of the triangle
                  int pattern_size = 5;
                  //loop to print the pattern
                  for (int i = 0; i < pattern_size; i++) {
                        //print column
                        for (int j = 0; j <= i; j++) {
                              //print star
                              System.out.print("*");
                        }
                  //print new line
                  System.out.println("\n");
                 }
 }
   } 
```

4.**Java 中的直角三角形模式程序**

**输入:**模式 _ 大小= 6

**输出:**

```
 *

                             **

                            ***

                           ****

                          *****

                         ******
```

**代码:**

**导入 Java . io . *；
导入 Java . util . *；**

```
import java.io.*;
import java.util.*;

class Solution {
 public static void main (String[] args) {
      // pattern_size of the triangle
                int pattern_size = 6;
                // loop to print the pattern
               for (int i = 0; i < pattern_size; i++) {
               // print spaces
                   for (int j = 1; j < pattern_size - i; j++) {
                        //print empty space
                        System.out.print(" ");
                   }
                  // print stars
                  for (int z = 0; z <= i; z++) {
                      //print start
                       System.out.print("*");
                  }
                  //print new line
                  System.out.println("\n");
              }
     }
}
```

5.**左下三角形**

**输入:**模式大小= 5

**输出:**

```
 ******

                            *****

                            ****

                            ***

                            **

                            *
```

**代码:**

**导入 Java . io . *；
导入 Java . util . *；**

```
import java.io.*;
import java.util.*;

class Solution {
 public static void main (String[] args) {
     // pattern_size of the triangle
               int pattern_size = 5;
               for (int i = 0; i < pattern_size; i++) {
               // print stars
                   for (int j = 0; j < pattern_size - i; j++) {
                       //print star
                       System.out.print("*");
                   }
                   //print new line
                   System.out.println("\n");
              }
       }
} 
```

6.**右下三角形**

**输入:**模式 _ 大小= 5

**输出:**

```
 * * * * *

                              * * * *

                               * * *

                                * *

                                 *
```

**代码:**

**导入 Java . io . *；
导入 Java . util . *；**

```
 import java.io.*;
import java.util.*;

class Solution {
 public static void main (String[] args) {
  // pattern_size of the triangle
  int pattern_size = 5;
  for (int i = 0; i < pattern_size; i++) {
    // print spaces
    for (int j = 0; j < i; j++) {
      //print empty space
      System.out.print(" ");
    }
    // print stars
    for (int j = pattern_size; j > i; j--) {
//print star
      System.out.print("*");
    }
    //print new line
    System.out.println("\n");
  }
    }
}
```

7.**Java 中的空心三角形星形图案**

**输入:**模式 _ 大小= 5

**输出:**

```
 *

                              **

                              * *

                              *  *

                              *   *

                              ******
```

**代码:**

**导入 Java . io . *；
导入 Java . util . *；**

```
import java.io.*;
import java.util.*;

class Solution {
 public static void main (String[] args) {
  // pattern_size of the triangle
  int pattern_size = 5;
  for (int i = 1; i <= pattern_size; i++) {
    for (int j = 0; j < i; j++) {
      // not last row
      if (i != pattern_size) {
        if (j == 0 || j == i - 1) {
          System.out.print("*");
        } else {
          //print empty space
          System.out.print(" ");
        }
      }
      // last row
      else {
//print star
        System.out.print("*");
      }
    }
    //print new line
    System.out.println("\n");
  }
    }
}
```

8.**Java 中的金字塔星形模式**

**输入:** pattern_size = 5

**输出:**

```
 *

                           ***

                           *****

                           *******

                           *********
```

**代码:**

```
import java.io.*;
import java.util.*;
class Solution {
 public static void main (String[] args) {
 int pattern_size = 5;
 for (int i = 0; i < pattern_size; i++) {
 // print spaces
 for (int j = 0; j < pattern_size — i — 1; j++) {
 //print empty space
 System.out.print(“ “);
 }

// print stars
 for (int z = 0; z < 2 * i + 1; z++) {
//print star
 System.out.print(“*”);
 }

//print new line
 System.out.println(“\n”);
 }
   }
}
```

# Java 数字模式

1.  **左三角数字模式**

**输入:** pattern_size = 6

**输出:**

```
 1

                            12

                            123

                            1234

                            12345

                            123456
```

**代码:**

```
import java.io.*;
import java.util.*;

class Solution {
 public static void main (String[] args) {

  int pattern_size = 6;
  // loop to print the pattern
  for (int i = 0; i < pattern_size; i++) {
    // print column
    for (int j = 0; j <= i; j++) {
      System.out.print(j+1);
    }
    //print new line
    System.out.println("\n");
  }
    }
}
```

2 **。直角三角形数字模式**

**输入:** pattern_size = 6

**输出:**

```
 1

                                12

                               123

                              1234

                             12345

                            123456 
```

**代码:**

```
import java.io.*;
import java.util.*;
class Solution {
 public static void main (String[] args) {
 int pattern_size = 6;
 // loop to print the pattern
 for (int i = 0; i < pattern_size; i++) {
 // print spaces
 for (int j = 1; j < pattern_size — i; j++) {

//print empty space
 System.out.print(“ “);
 }

// print number
 for (int z = 0; z <= i; z++) {
 System.out.print(z+1);
 }

//print new line
 System.out.println(“\n”);
 }
  }
}
```

3.**数字金字塔图案**

**输入:**模式 _ 大小= 5

**输出:**

```
 1

                               123

                              12345

                             1234567

                            123456789
```

**代码:**

```
import java.io.*;
import java.util.*;
class Solution {
 public static void main (String[] args) {
int pattern_size = 5;
 for (int i = 0; i < pattern_size; i++) {
 // print spaces
 for (int j = 0; j < pattern_size — i — 1; j++) {

//print empty space
 System.out.print(“ “);
 }

// print number
 for (int z = 0; z < 2 * i + 1; z++) {
 System.out.print(z+1);
 }

 //print new line
System.out.println(“\n”);
 }
   }
}
```

4.**数字金字塔反转模式**

**输入:**模式大小= 5

**输出:**

```
 123456789

                                 1234567

                                  12345

                                   123

                                    1
```

**代码:**

```
import java.io.*;
import java.util.*;
class Solution {
 public static void main (String[] args) {
 int pattern_size = 5;
 for (int i = 0; i < pattern_size; i++) {
 // print spaces
 for (int j = 0; j < i; j++) {

 //print empty space
 System.out.print(“ “);
 }

// print number
 for (int z = 0; z < 2 * (pattern_size — i) — 1; z++) {
 System.out.print(z+1);
 }

//print new line
 System.out.println(“\n”);
 }
   }
}
```

5 **。空心数字金字塔图案**

**输入:**模式 _ 大小= 5

**输出:**

```
 1

                           1   2

                          1     2

                         1       2

                         123456789
```

**代码:**

```
n”);
 }
   }
}
```

6 **。数字菱形图案**

**输入:** pattern_size = 5

**输出:**

```
1
123
12345import java.io.*;
import java.util.*;
class Solution {
 public static void main (String[] args) {

int pattern_size = 5;
 for (int i = 0; i < pattern_size; i++) {
 // print spaces
 for (int j = 0; j < pattern_size — i — 1; j++) {
 //print empty space
 System.out.print(“ “);
 }

// print number
 int num = 1;
 for (int z = 0; z < 2 * i + 1; z++) {
 if (i == 0 || i == pattern_size — 1) {
 System.out.print(num++);
 } else {
 if (z == 0 || z == 2 * i) {
 System.out.print(num++);
 } else {

//print empty space
 System.out.print(“ “);
      }
    }
  }
 //print new line
 System.out.println(“\
```

```
 1234567

                           123456789

                            1234567

                             12345

                              123

                               1
```

**代码:**

```
import java.io.*;
import java.util.*;
class Solution {
 public static void main (String[] args) {
int pattern_size = 5;
 int num = 1;
 // upside pyramid
 for (int i = 1; i <= pattern_size; i++) {
 // printing spaces
 for (int j = pattern_size; j > i; j — ) {
 //print empty space
 System.out.print(“ “);
 }
 // printing number
 for (int z = 0; z < i * 2–1; z++) {
 System.out.print(num++);
 }
 // set the number to 1
 num = 1;
 //print new line
 System.out.println(“\n”);
 }
 // downside pyramid
 for (int i = 1; i <= pattern_size — 1; i++) {
 // printing spaces
 for (int j = 0; j < i; j++) {
 //print empty space
 System.out.print(“ “);
 }
 // printing number
 for (int z = (pattern_size — i) * 2–1; z > 0; z — ) {
 System.out.print(num++);
 }
 // set num to 1
 num = 1;

//print new line
 System.out.println(“\n”);
 }
   }
}
```

7.**空心数字菱形图案**

**输入:** pattern_size = 5

**输出:**

```
 1

                           1     2

                        1           2

                     1                 2

                  1                       2

                    1                   2

                       1              2

                          1        2

                               1
```

**代码:**

```
import java.io.*;
import java.util.*;

class Solution {
 public static void main (String[] args) {
     int pattern_size = 5;
     int num = 1;
  // upside pyramid
  for (int i = 1; i <= pattern_size; i++) {
    // printing spaces
    for (int j = pattern_size; j > i; j--) {
          //print empty space
      System.out.print(" ");
```

```
 }
    // printing number
    for (int z = 0; z < i * 2 - 1; z++) {
      if (z == 0 || z == 2 * i - 2) {
       System.out.print(num++);
      } else {
          //print empty space
        System.out.print(" ");
      }
    }
    // set the number to 1
    num = 1;
    //print new line
    System.out.println("\n");
  }
  // downside triangle
  for (int i = 1; i < pattern_size; i++) {
    // printing spaces
    for (int j = 0; j < i; j++) {
          //print empty space
      System.out.print(" ");
    }
    // printing number
    for (int z = (pattern_size - i) * 2 - 1; z >= 1; z--) {
      if (z == 1 || z == (pattern_size - i) * 2 - 1) {
        System.out.print(num++);
      } else {
          //print empty space
        System.out.print(" ");
      }
    }
    // set the number to 1
    num = 1;
```

```
//print new line
 System.out.println(“\n”);
 }
   }
}
```

# Java 字符模式

1.  **字母金字塔图案**

**输入:**模式 _ 大小= 6

**输出:**

```
 A

                              ABC

                             ABCDE

                            ABCDEFG

                           ABCDEFGHI

                          ABCDEFGHIJ
```

**代码:**

```
import java.io.*;
import java.util.*;
class Solution {
 public static void main (String[] args) {
 int pattern_size = 6;
```

```
 int alpha = 65;
 for (int i = 0; i < pattern_size; i++) {
 // print spaces
 for (int j = 0; j < pattern_size — i — 1; j++) {
 //print empty space
 System.out.print(“ “);
 }
 // print alphabets
 for (int z = 0; z < 2 * i + 1; z++) {
 System.out.print((char)(alpha+z));
 }

//print new line
 System.out.println(“\n”);
 }
   }
}
```

2.**倒字母金字塔图案**

**输入:**模式 _ 大小= 6

**输出:**

```
 ABCDEFGHIJ

                           ABCDEFGHI

                            ABCDEFG

                             ABCDE

                              ABC

                               A 
```

**代码:**

```
import java.io.*;
import java.util.*;
class Solution {
 public static void main (String[] args) {
 // pattern_size of the square
 int pattern_size = 6;
int alpha = 65;
 for (int i = 0; i < pattern_size; i++) {
 // print spaces
 for (int j = 0; j < i; j++) {
 //print empty space
 System.out.print(“ “);
 }
 // print alphabets
 for (int z = 0; z < 2 * (pattern_size — i) — 1; z++) {
 System.out.print((char)(alpha+z));
 }

//print new line
 System.out.println(“\n”);
 }
   }
}
```

3.**空心字母金字塔图案**

**输入:**模式 _ 大小= 5

**输出:**

```
 A

                             B     C

                             D     E

                             F     G

                             HIJKLMNOP
```

**代码:**

```
import java.io.*;
import java.util.*;
class Solution {
 public static void main (String[] args) {
 int pattern_size = 5;
int alpha = 65;
int num = 0;
 for (int i = 0; i < pattern_size; i++) {
 // print spaces
 for (int j = 0; j < pattern_size — i — 1; j++) {
 //print empty space
 System.out.print(“ “);
 }
 // print alphabets
 for (int z = 0; z < 2 * i + 1; z++) {
 if (i == 0 || i == pattern_size — 1) {
 System.out.print((char)(alpha+num++));
 } else {
 if (z == 0 || z == 2 * i) {
 System.out.print((char)(alpha+num++));
 } else {
 //print empty space
 System.out.print(“ “);
 }
 }
 }

//print new line
 System.out.println(“\n”);
 }
   }
}
```

4.**字母菱形图案**

**输入:**模式 _ 大小= 5

**输出:**

```
 A

                           ABC

                          ABCDE

                         ABCDEFG

                        ABCDEFGHI

                         ABCDEFG

                          ABCDE

                           ABC

                            A
```

**代码:**

```
import java.io.*;
import java.util.*;
class Solution {
 public static void main (String[] args) {
 int pattern_size = 5;
 int alpha = 65;
 int num = 0;
 // upside pyramid
 for (int i = 1; i <= pattern_size; i++) {
 // printing spaces
 for (int j = pattern_size; j > i; j — ) {
 //print empty space
 System.out.print(“ “);
 }
 // printing alphabets
 for (int z = 0; z < i * 2–1; z++) {
 System.out.print((char)(alpha+num++));
 }
 // set the number to 0
 num = 0;
 //print new line
 System.out.println(“\n”);
 }
 // downside pyramid
 for (int i = 1; i <= pattern_size — 1; i++) {
 // printing spaces
 for (int j = 0; j < i; j++) {
 //print empty space
 System.out.print(“ “);
 }
 // printing alphabets
 for (int z = (pattern_size — i) * 2–1; z > 0; z — ) {
 System.out.print((char)(alpha+num++));
 }
 // set num to 0
 num = 0;

//print new line
 System.out.println(“\n”);
 }
   }
}
```

5.**空心字母菱形图案**

**输入:**模式 _ 大小= 5

**输出:**

```
 A

                              A        B

                           A              B

                         A                   B

                       A                        B

                         A                   B

                            A              B

                                A       B

                                    A
```

**代码:**

```
import java.io.*;
import java.util.*;
class Solution {
 public static void main (String[] args) {
 int pattern_size = 5;
 int alpha = 65;
 int num = 0;
 // upside pyramid
 for (int i = 1; i <= pattern_size; i++) {
 // printing spaces
 for (int j = pattern_size; j > i; j — ) {
 //print empty space
 System.out.print(“ “);
 }
 // printing alphabets
 for (int z = 0; z < i * 2–1; z++) {
 if (z == 0 || z == 2 * i — 2) {
 System.out.print((char)(alpha+num++));
 } else {
 //print empty space
 System.out.print(“ “);
 }
 }
 // set the number to 0
 num = 0;
 //print new line
 System.out.println(“\n”);
 }
 // downside triangle
 for (int i = 1; i < pattern_size; i++) {
 // printing spaces
 for (int j = 0; j < i; j++) {
 //print empty space
 System.out.print(“ “);
 }

// printing alphabets
 for (int z = (pattern_size — i) * 2–1; z >= 1; z — ) {
 if (z == 1 || z == (pattern_size — i) * 2–1) {
 System.out.print((char)(alpha+num++));
 } else {

//print empty space
 System.out.print(“ “);
 }
 }

// set the number to 0
 num = 0;
 //print new line
 System.out.println(“\n”);
 }
   }
}
```

# 结论

本文涵盖了 20 种不同的 [**Java**](https://www.scaler.com/topics/java/) 模式程序，这些程序使用了 [**Java 访谈**](https://www.interviewbit.com/java-interview-questions/) 中要求的星号、数字和字母。