# C #中的格式说明符

> 原文：<https://www.freecodecamp.org/news/format-specifiers-in-c/>

格式说明符定义了要在标准输出上打印的数据类型。无论是用`printf()`打印格式化输出还是用`scanf()`接受输入，都需要使用格式说明符。

您可以在 ANSI C 中使用的一些%说明符如下:

| 分类符 | 用于 |
| --- | --- |
| %c | 单个字符 |
| %s | 一根绳子 |
| %嗨 | 简短(签名) |
| %hu | 短整型(无符号) |
| %Lf | 长双份 |
| %n | 不打印任何内容 |
| %d | 十进制整数(假定基数为 10) |
| %i | 十进制整数(自动检测基数) |
| %o | 八进制(以 8 为基数)整数 |
| %x | 十六进制(以 16 为基数)整数 |
| %p | 地址(或指针) |
| %f | 浮点的浮点数 |
| %u | int 无符号十进制 |
| %e | 科学记数法中的浮点数 |
| %E | 科学记数法中的浮点数 |
| %% | %符号 |

## 示例:

### `%c`单字符格式说明符:

```
#include <stdio.h> 

int main() { 
  char first_ch = 'f'; 
  printf("%c\n", first_ch); 
  return 0; 
} 
```

**输出:**

```
f
```

### `%s`字符串格式说明符:

```
#include <stdio.h> 

int main() { 
  char str[] = "freeCodeCamp"; 
  printf("%s\n", str); 
  return 0; 
} 
```

**输出:**

```
freeCodeCamp
```

### 使用`%c`格式说明符的字符输入:

```
#include <stdio.h> 

int main() { 
  char user_ch; 
  scanf("%c", &user_ch); // user inputs Y
  printf("%c\n", user_ch); 
  return 0; 
} 
```

**输出:**

```
Y
```

### 带`%s`格式说明符的字符串输入:

```
#include <stdio.h> 

int main() { 
  char user_str[20]; 
  scanf("%s", user_str); // user inputs fCC
  printf("%s\n", user_str); 
  return 0; 
} 
```

**输出:**

```
fCC
```

### `%d`和`%i`十进制整数格式说明符:

```
#include <stdio.h> 

int main() { 
  int found = 2015, curr = 2020; 
  printf("%d\n", found); 
  printf("%i\n", curr); 
  return 0; 
} 
```

**输出:**

```
2015
2020
```

### `%f`和`%e`浮点数格式说明符:

```
#include <stdio.h>

int main() { 
  float num = 19.99; 
  printf("%f\n", num); 
  printf("%e\n", num); 
  return 0; 
}
```

**输出:**

```
19.990000
1.999000e+01
```

### `%o`八进制整数格式说明符:

```
#include <stdio.h> 

int main() { 
  int num = 31; 
  printf("%o\n", num); 
  return 0; 
}
```

**输出:**

```
37
```

### `%x`十六进制整数格式说明符:

```
#include <stdio.h> 

int main() { 
  int c = 28; 
  printf("%x\n", c); 
  return 0; 
} 
```

**输出:**

```
1c
```