---
layout: post
title: 三顾茅庐【优化算法】
categories: [Java，算法，蓝桥杯]
description: 在蓝桥杯官网练题库的时候遇到了这样一道题，觉得很有趣，分享给大家！
keywords: Java, 蓝桥杯 , 算法 ,时间复杂度
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

今天在蓝桥杯官网练题库的时候遇到了这样一道题，觉得很有趣，分享给大家！

# 三顾茅庐【优化算法】


---

### 问题描述

**卧龙凤雏，得一者可得天下！**

在东汉末年，英雄豪杰聚集，刘备帐下的智囊徐庶家母遭曹操绑架，只得投奔曹家。在告别刘备之际，他将卧龙诸葛亮的所在告知刘备，引得刘备三顾茅庐，欲邀卧龙出山相助。

卧龙知晓刘备之雄心壮志，为考其统御之才，决定以一道难题来考验。

> "我门前有一株神奇柳树，初高 _x_，每次操作可将其高度变为 ∣ 当前高度 −y∣，问经 k 次操作后，树高为何？"

这个难题或许能考验刘备的智慧，望你能帮助他解答这一难题。

### 输入格式

第一行输入一个整数 `t(1≤t≤105)`表示卧龙的询问次数。

接下来 `t`行，每行三个整数 `x,y,k(0≤x,y,k≤109)` 表示一次询问。

### 输出格式

输出 `t` 行，每行一个整数表示答案。

### 输入样例

```text
3
10 1 3
10 1000 99999
10 0 0
```

### 输出样例

```text
7
990
10
```

---

最开始我觉得这是一道简单的算法题，直接暴力美学，硬算就能出结果，我的思路是通过迭代计算，反复执行指定次数（`k`）的 `x = Math.abs(x - y)` 操作，并输出最终的结果，于是有了下面的代码：

```java
import java.util.Scanner;
// 1:无需package
// 2: 类名必须Main, 不可修改
import java.util.Scanner;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int cishu = sc.nextInt();
        for (int i = 0; i < cishu; i++) {
            int x = sc.nextInt();
            int y = sc.nextInt();
            int k = sc.nextInt();
            for (int j = 1; j <= k; j++) {
                x = Math.abs(x - y);
            }
            System.out.println(x);
        }
    }
}
```

虽然测试结果正确，但提交时会发现`运行超时`，因为蓝桥杯比赛是要求限制代码的运行内存和运行时间的。

| 语言       | 最大运行时间 | 最大运行内存 |
| ---------- | ------------ | ------------ |
| C++        | 1s           | 512M         |
| C          | 1s           | 512M         |
| Java       | 2s           | 512M         |
| Python3    | 3s           | 512M         |
| PyPy3      | 3s           | 512M         |
| Go         | 3s           | 512M         |
| JavaScript | 3s           | 512M         |

运行超时的问题可能是因为在输入的大数据情况下，`k` 的值过大，导致循环执行时间过长。

经过思考，我们可以分情况处理问题，以减少多余的计算操作。以下是详细的分情况算法逻辑和实现：

---

### **思路分析**

1. **没有出现负数的情况**：
   - 当 `x − y × k ≥ 0`，直接计算 `x = x − y × k` 。
   - 这相当于一次直接运算，复杂度极低。
2. **出现负数的情况**：
   - 找到使 `x − y × i < 0` 的最大 `i`，等价于`i = min(k, x / y)`。
   - 减去 `i` 次后，更新剩余的 `k` 值为 `k' = k - i`。
   - 对剩余的 `k'`：i
     - 如果 `k'` 是偶数，结果保持为当前 `x`。
     - 如果 `k'` 是奇数，结果为 `|y - x|`。

---

### **代码实现**

根据上述思路，编写优化代码：

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt(); // 测试次数
        StringBuilder result = new StringBuilder();

        for (int i = 0; i < t; i++) {
            long x = sc.nextLong();
            long y = sc.nextLong();
            long k = sc.nextLong();

            if (y == 0) {
                // 特殊情况：y == 0，直接输出 x
                result.append(x).append("\n");
                continue;
            }

            if (x - y * k >= 0) {
                // 情况 1：没有出现负数
                result.append(x - y * k).append("\n");
            } else {
                // 情况 2：会出现负数
                long maxi = x / y; // 最大可减次数
                long tUsed = Math.min(k, maxi); // 实际进行的次数
                x -= tUsed * y; // 更新 x
                k -= tUsed; // 更新剩余次数

                if (k % 2 == 0) {
                    // 如果剩余 k 为偶数，x 不变
                    result.append(x).append("\n");
                } else {
                    // 如果剩余 k 为奇数，结果为 |y - x|
                    result.append(Math.abs(x - y)).append("\n");
                }

            }

        }
        System.out.print(result);
    }
}
```

---

### **逻辑说明**

1. 快速判断是否会出现负数：
   - `x − y × k ≥ 0` 时直接计算，避免多余操作。
2. 逐步减小 `x` 的操作：
   - 利用 `t = x / y`，快速确定可以减去多少次。
3. 剩余 `k` 的奇偶性判断：
   - `k \% 2 = 0` 时，不影响最终结果。
   - `k \% 2 = 1` 时，最终结果是 `|y - x|`。

---

### **复杂度分析**

1. **时间复杂度**：
   - 每组数据最多执行一次减法计算 O(1)。
   - 总复杂度为 O(t)，其中 t 是测试组数。
2. **空间复杂度**：
   - 输出结果使用 O(t) 的空间存储。

---

### **输入输出示例**

#### 输入：

```text
3
10 1 3
10 1000 99999
10 0 0
```

#### 输出：

```text
7
990
10
```

#### 输入解释：

1. 第一组：
   - `x = 10 , y = 1 , k = 3`
   - 没有出现负数，直接 `x = x − y × k = 10 − 3 = 7`
2. 第二组：
   - `x = 10 , y = 1000 , k = 99999`
   - 减 `x = x − y × t` ，`t=0`，剩余 `k \% 2 = 1`，结果为 `y - x| = 990`。
3. 第三组：
   - `y = 0`，结果直接为 `x = 10`。

---

测试通过，该算法充分利用输入数据的规律，通过快速减法和奇偶性判断，最大限度地减少计算次数，适用于大规模测试数据场景。

## Q&A

### **一、`StringBuilder` 是什么？**

`StringBuilder` 是 Java 中的一个 **类**，用来构建和操作 **可变字符串**。它位于 `java.lang` 包中。

普通的字符串（`String`）在 Java 中是 **不可变的**，一旦创建，内容就不能被修改。`StringBuilder` 提供了一种高效的方法，可以动态修改字符串内容，而不会像 `String` 那样频繁创建新的对象。

---

### **特点**

1. **可变性**：
   - `StringBuilder` 的内容可以修改，而不需要创建新的对象。
   - 比如在 `StringBuilder` 中追加、删除或插入字符，会直接修改当前对象，而不会生成新对象。
2. **高效性**：
   - 当需要频繁对字符串进行修改（比如追加、删除、插入）时，`StringBuilder` 比 `String` 更高效，尤其是在循环中频繁操作字符串时。
3. **线程不安全**：
   - `StringBuilder` 是非线程安全的，适用于单线程环境。如果需要线程安全的操作，可以使用 **`StringBuffer`**。

---

### **常用方法**

| 方法                                    | 描述                             | 示例                         |
| --------------------------------------- | -------------------------------- | ---------------------------- |
| `append(String s)`                      | 追加字符串到末尾                 | `sb.append("Hello")`         |
| `insert(int offset, String s)`          | 在指定位置插入字符串             | `sb.insert(5, " World")`     |
| `delete(int start, int end)`            | 删除指定范围内的字符             | `sb.delete(5, 11)`           |
| `replace(int start, int end, String s)` | 替换指定范围内的字符为新的字符串 | `sb.replace(5, 11, "Java")`  |
| `reverse()`                             | 将字符串反转                     | `sb.reverse()`               |
| `toString()`                            | 将 `StringBuilder` 转为 `String` | `String str = sb.toString()` |
| `length()`                              | 获取当前字符串的长度             | `sb.length()`                |

---

### **使用示例**

#### **基本操作**

```java
public class Main {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("Hello");
        sb.append(" World"); // 追加
        sb.insert(6, "Java "); // 插入
        sb.delete(0, 6); // 删除
        sb.reverse(); // 反转
        System.out.println(sb); // 输出结果
    }
}
```

#### **运行结果**

```
dlroW avaJ
```

---

#### **对比 `String` 和 `StringBuilder`**

**使用 `String`：**

```java
String str = "Hello";
str += " World"; // 创建了一个新的字符串对象
```

- 每次修改字符串，都会创建新的 `String` 对象，这种做法在大量操作时效率较低。

**使用 `StringBuilder`：**

```java
StringBuilder sb = new StringBuilder("Hello");
sb.append(" World"); // 修改了原来的对象，无需创建新对象
```

- 修改发生在同一个对象上，更节省内存和时间。

---

### **适用场景**

1. **需要频繁修改字符串内容**：
   - 如追加字符、删除字符、替换字符等。
   - 示例：拼接日志、生成动态 HTML。
2. **性能要求较高**：
   - 当需要在循环中多次操作字符串时，`StringBuilder` 比 `String` 更高效。

---

### **总结**

- **`StringBuilder` 是一个可变字符串类**，可以高效地修改字符串内容。
- 它适用于单线程环境，在需要频繁操作字符串时是一个理想的选择。
- 如果需要线程安全，使用 **`StringBuffer`**。
