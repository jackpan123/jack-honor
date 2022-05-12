### 3.1

| 操作数          | 值    |
| --------------- | ----- |
| %rax            | 0x100 |
| 0x104           | 0xAB  |
| $0x108          | 0x108 |
| (%rax)          | 0xFF  |
| 4(%rax)         | 0xAB  |
| 9(%rax, %rdx)   | 0x11  |
| 260(%rcx, %rdx) |       |
| 0xFC(, %rcx, 4) |       |
| (%rax, %rdx, 4) | 0xAB  |



疑问：超出地址后会怎么样？

### 3.2

movl

movw

movb

movb

mobq

movw



### 3.3

movb $0xF, (%ebx) 不能使用%ebx作为地址寄存器，这个寄存器由被调用者持有

movl %rax, (%rsp) %rax存储位数超过32位 指令和寄存器大小不匹配

movw (%rax), 4(%rsp) 不能在两个内存地址中做移位操作

movb %al, %sl 寄存器不存在

movq %rax, $0x123 mov指令的目标只能是内存地址或者是寄存器，不能是立即数

movl %eax, %rdx 目的数的寄存器是不正确的，应该是%edx

movb %si，8(%rbp) %si是存放双字节的数据，单字节不够存放，指令和寄存器大小不匹配

### 3.4

| src_t         | dest_t        | 指令                                      |
| ------------- | ------------- | ----------------------------------------- |
| long          | long          | movq (%rdi), %rax <br>movq %rax, (%rsi)   |
| char          | int           | movsbl (%rdi), %eax <br>movl %eax, (%rsi) |
| char          | unsigned      | movsbl (%rdi), %eax<br>movl %eax, (%rsi)  |
| unsigned char | long          | movzbl (%rdi), %eax<br/>movq %eax, (%rsi) |
| int           | char          | movl (%rdi), %eax<br>movb %al, (%sil)     |
| unsigned      | unsigned char | movl (%edi), %eax<br/>movb %al, (%rsi)    |
| char          | short         | movbw (%rsi), %ax<br/>movsbw %ax, (%rsi)  |



### 3.5

```c
void decode1(long *xp, long *yp, long *zp) {
    long x = *xp;
    long y = *yp;
    long z = *zp;
    *yp = x;
    *zp = y;
    *xp = z;
}
```

### 3.6

| 表达式                       | 结果   |
| ---------------------------- | ------ |
| leaq 6 (%ax), %rdx           | x+6    |
| leaq  (%rax, %rcx), %rdx     | x+y    |
| leaq  (%rax, %rcx, 4), %rdx  | x+4y   |
| leaq 7 (%rax, %rax, 8), %rdx | 9x+7   |
| leaq  0xA(, %rcx, 4), %rdx   | 4y+10  |
| leaq 9 (%rax, %rcx, 2), %rdx | x+2y+9 |

### 3.7

```c
long scale2(long x, long y, long z) {
    long t = 5 * x + 2 * y + 8 * z;
    return t;
}
```

### 3.8

| 指令                       | 目的  | 值    |
| -------------------------- | ----- | ----- |
| addq %rcx, (%rax)          | 0x100 | 0x100 |
| subq %rdx, 8(%rax)         | 0x108 | 0xA8  |
| imulq $16, (%rax, %rdx, 8) | 0x118 | 0x110 |
| incq 16(%rax)              | 0x110 | 0x14  |
| decq %rcx                  | %rcx  | 0x0   |
| subq %rdx, %rax            | %rax  | 0xFD  |

### 3.9

```
shift_left4_rightn:
  movq %rdi, %rax
  salq $4, %rax
  movl $esi, %ecx
  sarq %cl, %rax
```

### 3.10

```c
long arith2(long x, long y, long z) {
    long t1 = x | y;
    long t2 = x >> 3;
    long t3 = ~t2;
    long t4 = z - t3;
    return t4;
}
```

### 3.11

1.将寄存器设置为0，运用对任意x，x^x=0的这一特性，它对应于C语句的x=0

2.使用指令 movq $0, %rdx

3.xorl %edx, %edx, movl $0,%edx

### 3.12

```assembly
uremdiv:
	movq %rdx, %r8
	movq %rdi, %rax
	movl $0, %edx
	divq %rsi
	movq %rax, (%r8)
	movq %rdx, (%rcx)
	ret
```

### 3.13

A int   <

B short >=

C unsigned char <=

D long >=

### 3.14

A long >=

B short =

C unsigned char >

D int !=

### 3.15

A 0xe

B 0xf3

C 400543 400545

D 4005f7



### 3.16

A

```c

if (p) {
  goto done;
}
if (a >= *p) {
  goto done;
}
*p = a;
done:
  return;
```

B 因为根据if的短路原则，如果一个条件的判断足够了，就会跳过不进行第二次判断。



### 3.17

A

```c
long result;
if (x >= y)
  goto x_ge_y;

lt_cnt++;
result = y - x;
goto done;
x_ge_y:
  ge_cnt++;
  result = x -y;
done:
  return result;
```

B 代码逻辑表达需要清晰，并且简洁。



### 3.18

```c
long test(long x, long y, long z) {
  long val = x + y + z;
  if (x < -3) {
    if (z < y) {
      val = x * z;
    } else {
      val = y * z;
    }
  } else if (x > 4) {
    val = x * z;
  }
}
```



### 3.19

A 30个时钟周期

B 16 - 46个时钟周期



### 3.20

A  x /  8

B 

```
arith:
  leaq    7(%rdi), %rax  x + 7
  testq   %rdi, %rdi x not equals negative
  cmovns  %rdi, %rax 非负数进行移动
  sarq    $3, %rax  向右边移动3位
  ret
```

### 3.21

```c
long test(long x, long y) {
  long val = 8 * x;
  if (y) {
    if (x > y) {
      val = x - y;
    } else {
      val = y - x;
    }
  } else if ( y <= -2) {
    val = x + y
  }
  return val;
}
```



### 3.22

A n的最大值就是int的最大值

B n的最大值就是long的最大值



### 3.23

A

%rdi %rax 存放x

%rcx存放 y

%rdx 存放n

B

因为没有实质性的数据改变，所以这个两行代码可以直接去除了

C

2 将x值移动到寄存器%rax中

3 将x值移动到寄存器%rcx中 

4 y = x * x

5 n = x + x = 2 * x

7 x = x + y

8 n--

9,10 n > 0



### 3.24

```c
long loop_while(long a, long b) {
  long result = 1;
  while (a < b) {
    result = (x + y) * result;
    a = a++;
  }
  
  return result;
}
```

### 3.25

```c
long loop_while2(long a, long b) {
  long result = b;
  while (b > 0) {
    result = a * b;
    b = b - a;
  }
}
```

### 3.26

```c
long fun_a(unsigned long x) {
  long val = 0;
  while (x != 0) {
    val = x;
    x = x / 2;
  }
  return val
}
```

A jump to the middle

C 计算除以2的余数

### 3.27



### 3.28

### 3.29

### 3.30

