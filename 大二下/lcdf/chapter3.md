# Chapter3 Combinational Logic Design
## Part1 Implementation Technology and Logic Design
有两种类型的逻辑电路：
- combinational logic circuit(组合电路)
  - 有m个输入，n个输出，期中包含2^m^种输入组合，以及n个不同的输出函数
  - 它的输出只依赖于m个输入的组合（不包含回路）
- sequential logic circuit(时序电路)
  - 它的输出不但取决于输入，而且也依赖于之前的输入（有记忆功能）
### 组合电路
表示逻辑的方法：
1. 真值表(Truth Table)；
2. 布尔函数(Boolean Function)；
3. 卡诺图(Karnaugh Maps)；
4. 时序图(Timing Diagram)；
5. 逻辑电路图(Logic Circuit)；

其中，方法1，3，4在功能确定的情况下，其表示是唯一的。  

而我们的设计，就是在满足功能的前提下，尽可能优化和找到最好的设计。
而主要的设计过程如下：
1. 确定系统的行为；
2. 阐述输入和输出之间的逻辑关系，并用真值表或逻辑表达式表达出来；
3. 优化逻辑表达以减少成本（比如使用卡诺图）；
4. 将优化后的逻辑设计工艺映射到硬件实现上；
5. 验证正确性（在仿真环境中）；

### 分层设计
分层设计即将复杂问题模块化分解为若干层次，然后逐个抽象解决。
其设计方法分为自顶向下(Top-Down)和自底向上(Bottom-Up)。
前者从需求开始，自顶向下分解功能设计；后者根据现有的元件去组合成目标功能。
### 集成电路（integrated circuits）
集成电路(integrated circuits)又叫芯片(chip)，分为如下若干等级：
- SSI(small-scale integrated) 内含不到 10 个 gates；
- MSI(medium-scale integrated) 内含 10 ~ 100 个 gates；
- LSI(large-scale integrated) 内含 成百上千 个 gates；
- VLSI(very large-scale integrated) 内含 成千上亿 个 gates；
### 技术参数（technical parameters）

|name|description|
|---|---|
|fan-in|一个门的可用输入数量|
|fan-out|一个门输出驱动的标准负载数量|
|logic levels|高低电平输入输出电压范围|
|noise margin|噪声容错能力|
|cost for one gate|一个门成本|
|propagation delay|信号改变后从输入到输出所需变化时间|
|power dissipation|一个门消耗的能量|

### Fan-in and Fan-out
扇入描述了一个门能够接受的最多输入量，如一个四输入与非门的扇入就是 4；而扇出描述的则是一个门的输出（栅极输出）在不降低工作性能的情况下能够负载多少门，例如一个非门的输出能够同时负载 4 个非门并且都能正常工作，则其扇出为 4，其也能通过标准负载来定义。

### propagation delay
- Propagation delay is the time for a change on an input of a gate to propagate to the output.
- High-to-low $(t_{PHL})$ and low-to-high $(t_{PLH})$ output signal changes may have different propagation delays.门输入输出
- High-to-low $(t_{HL})$ and low-to-high $(t_{LH})$ transitions are defined with respect to the output, not the input. 输出端变化

![3.13](3.13.png)
![3.14](3.14.png)
$t_{pd}=(t_{PHL}+t_{PLH})/2$ 平均值
如果有n个非门串联,计算$t_{PHL}$需要$t_{nPHL}+t_{(n-1)PLH}+...$,从靠近结尾处倒推，如果只求$t_{pd}$只需要将每级的$t_{pd}$相加即可。

### delay models
- Transport delay
a change in the output in response to a change on the inputs occurs after a fixed specified delay
输出整体往后移

![3.15](3.15.png)

- Inertial delay  
similar to transport delay, except that if the input changes such that the output is to change twice in a time interval less than the **rejection time**, the output changes do not occur. Models typical electronic circuit behavior, namely, rejects narrow “pulses” on the outputs 除了输出往后移，在惯性延迟下，很窄的脉冲（小于 rejection time）会被消除掉。

![3.16](3.16.png)

### Circuit Delay
> **Example**
![3.17](3.17.png)
>- 最开始 S 由 0->1 后 0.9s Y 从 0->1
>- S 从 1->0 后下方的与门 0.4s 后会从 1->0, 但上方的与门 0.6s 后才会从 0->1. 但 0.9s 后 Y 才会 1->0, 此后再过 0.2s(共 1.1s) 后 Y 从 0-> 1.
这里 Y 出现了一个小尖峰，我们称之为电路产生的**冒险**。
S 的两条路径我们发现延迟不同，这种我们称之为电路中的**竞争**。

### Fan-out and Delay
The fan-out loading a gate’s output affects the gate’s propagation delay。
SL(Standard Load): 以非门为标准。带一个非门需要...的负载
> **Example**
One realistic equation for $t_{pd}$ for a NAND gate with 4 inputs is: $t_{pd}=0.07+0.021\times SL$
> - SL is the number of standard loads the gate is driving.
> - i. e., its fan-out in standard loads For $SL$ = 4.5, $t_{pd}$ = 0.165 ns
由工艺程度+负载情况决定

## Part2 Combinational Functional Blocks
### 基本逻辑函数
- 常数函数 $$F=0$$or$$F=1$$

- 传输函数 $$F=X$$直接输出输入值  

- 逆变函数 $$F=\overline{X}$$输出输入相反值

- 使能函数 $$F=X(EN)$$or$$F=X+\overline{EN}$$通过控制使能控制输入是否可变（注意区别与三态门，高阻值Hi-Zor定值）
![3.1](3.1.png)
### 基本功能块
- 译码器 decoder
- 编码器 encoder
- (三端)多路复用器（multiplexer）MUX
- (三端)信号分配器（demultiplexer）DEMUX
### 译码器 （$n-2^n$ line BCD decoder）
稠密->稀疏/稠密

译码器可以由与门或与非门来负责输出。若使用与门，当所有的输入均为高电平时，输出才为高电平，这样的输出称为“高电平有效”的输出；若使用与非门，则当所有的输入均为高电平时，输出才为低电平，这样的输出称为“低电平有效”的输出。

更复杂的译码器是n线－2^n^线类型的二进制译码器。这类译码器是一种组合逻辑电路，能从已编码的n个输入，将二进制信息转换为2^n^个独特的输出中最大个数的输出。我们说2^n^个输出的最大个数，是因为当n位已编码信息中有未使用的位组合时，译码器可能会有少于2^n^个输出。
>3-8 decoder
![3-8 decoder](2.jpg "3-8 decoder")
![3-8 decoder](3.png "3-8 decoder")
one hot code 稀疏
![3-8 decoder](4.png "3-8 decoder")
### 编码器
与译码器作用相反
稠密/稀疏->稠密
> 如上定义的编码器有一个限制，即任何时候输入只能有一个是活动的，即输入是one-hot的

优先编码器
>优先编码器能够实现优先级函数，它不要求输入是 one-hot 的，而是总是关注有效输入中优先级最高的那一个。即比如当优先级最高的那一位是 1 时，其它所有优先级不如它的位置的值都是我们不关心的内容了。
![priority encoder](5.png "priority encoder")

可用卡诺图化简输出结果：
![3.2](3.2.png)

### 选择器
Selecting of data or information is a critical function in digital systems and computers
- Circuits that perform selecting have:
  •A set of information inputs from which the selection is made
  •A set of control lines for making the selection
  •A single output

Logic circuits that perform selecting are called **multiplexers**.
A typical multiplexer has $n$ control inputs $(S_{n-1}, … ,S_0)$ called selection inputs, $2^n$ information inputs $(I_{2^n-1} , … I_0)$, and one output Y.
如果输入$m<2^n$也可以设计为 $n$ select lines 的 multiplexers.
![3.3](3.3.png)
> **2 to 1 Multiplexer**
![3.4](3.4.png)

In general, $2^n$-to-1-line multiplexers:
- n-to-$2^n$-line decoder
- $2^n$*2 AND-OR
>**Example**
![3.5](3.5.png)
![3.6](3.6.png)
多位的数据选择。这里有四组信号，每组信号都是四个输入的一位，但选择逻辑对于四组信号是一样的，因此最后选出来的都是同一组信号。即最后输出的四位信号都来自同一根总线，

我们也可以不用与或结构，使用三态门实现 mux.
>**Example**
![3.7](3.7.png)
(利用三态门可以将输出并在一起，同时最多只有一个三态门有有效输出。我们这里译码器只会有一个输出为 1, 保证了电路安全；这样还可以降低成本)
我们还可以将译码器也使用三态门：
![3.8](3.8.png)

对于一个 n 变量的逻辑函数，我们可以把它抽象为 n 个输入对应一个输出。我们可以用 Mux 对应真值表中的$2^n$行的结果，用 n 输入作为选择线来查表。
> Gray to Binary Code
![3.9](3.9.png)
相当于利用 ABC 查表，如果 mux 选择出一位（根据真值表得到）
注意引脚顺序！

我们可以做进一步改进，n 变量用 m-wide $2^{n-1}$- to - 1 - line mux

对于 $F(A,B,C,D)$,当 A B 确定时，最后可能输出只可能为 1,0,$C$,$\overline{C}$
利用这点我们可以改造真值表，
![3.10](3.10.png)
![3.11](3.11.png)
![3.12](3.12.png)

## Part3 Arithmetic Functions
### 1. Iterative Combinational Circuits

- Arithmetic functions
  •Operate on binary vectors
  •Use the same subfunction in each bit position
- Can design functional block for subfunction and repeat to obtain functional block for overall function
- $Cell$ - subfunction block
- $Iterative\ array$ - an array of interconnected cells
- An iterative array can be in a single dimension (1D) or multiple dimensions

### 2. Functional Blocks: Addition
Addition Development:

- **Half-Adder (HA)**, a 2-input bit-wise addition functional block.(no carry input)
- **Full-Adder (FA)**, a 3-input bit-wise addition functional block.
- **Ripple Carry Adder**, an iterative array to perform binary addition.
- **Carry-Look-Ahead Adder (CLA)**, a hierarchical structure to improve performance.

#### **Half-Adder**
![3.18](3.18.png)

![3.19](3.19.png)

#### **Full-Adder**
![3.20](3.20.png)

![3.21](3.21.png)

$S=X\overline{Y}\overline{Z}+\overline{X}Y\overline{Z}+\overline{X}\overline{Y}Z+XYZ=X\oplus Y\oplus Z$
$C=XY+YZ+XZ=XY+(X\oplus Y)Z$
- The term $X·Y$ is carry generate.(XY=1时一定会有进位)
- The term $X\oplus Y$ is carry propagate.(
$X\oplus Y=1$ 时, X,Y有一个是 0, 一定会把进位传下去，即 $C=Z$)
>注意 C 的改写，这里改为异或不改变结果，同时因为已经有 xor 了，可以节约一个门。

![3.22](3.22.png)

#### Binary Adders
实现二进制多位加法

![3.23](3.23.png)

存在一个问题：随着加法器位数的增加，延迟会越来越大。

**Revisit the Full Adder**
对于状态 i, 我们称$G_i$为 generate, $P_i$为 propagate.
- $G_i$,$P_i$, and $S_i$ are local to each cell of the adder
- $C_iS$is also local each cell

![3.24](3.24.png)

这样$C_{i+1}$可以从 cells 中去掉，同时我们可以推导得到一组跨越多个单元的进位方程：
![3.25](3.25.png)
于是，我们可以得到下面的Carry-Look-Ahead Adder:
![3.26](3.26.png)
这样的超前进位全加器，避免了因为位过多而造成延迟过大。高位的结果直接由低位的结果得到。
This could be extended to more than four bits; in practice, due to limited gate fan-in, such extension is not feasible.
![3.27](3.27.png)
所以可得到16-bits adder
![3.28](3.28.png)

### 3. Unsigned Subtraction **无符号位**
- Subtract the subtrahend(减数) N from the minuend(被减数) M  $M-N$
- If no end borrow occurs(没借位), then$M\geq N$, and the result is a non-negative number and correct.
- If an end borrow occurs（借位）, then $N>M$ and the difference $M-N+2^n$ is subtracted from $2^n$, and a minus sign is appended to the result.（用$2^n$减去（借位计算）结果，答案为$N-M$，再在前面加上负号，实际上就是求结果的补码）

![3.29](3.29.png)
![3.30](3.30.png)

#### Complements
- Two complements:
  - Radix Complement
    - **2's complement in binary** **补码** 
    - r's complement for radix r
    - Defined as $r^n-N$
    - **反码加一** $r^n-1-N+1$
  - Diminished Radix Complement of N(反码)
    - （r-1）'s complement for radix r
    - **1's complement for radix 2** **反码**
    - Defined as $(r^n-1)-N$
    - $r^n-1$是bits[n-1:0]全为1的二进制数，用它减去N即可得到N按位取反的结果，即反码

Subtraction is done by adding **the complement of the subtrahend减数**.
**采用补码的（无符号）二进制减法**
- For n-digit, unsigned numbers M and N, find $M - N$ in base 2:
  - Add the 2's complement of the subtrahend N to the minuend M ($–N$ and $2^n−N$ are congruent modulo $2^n$): 加上减数的补码
    $M - N \equiv M + (2^n - N) = M - N + 2^n(mod\ 2^n)$
  - If $M \geq N$, the sum produces end carry $r^n$ which is discarded; from above, $M - N$ remains.(算出来的数有多一位1,进了一位1，就是$r^n$，舍掉，保留余下的数)
  - If $M < N$, the sum does not produce an end carry and, from above, is equal to $2^n - ( N - M )$, the 2's complement of $( N - M )$.没进位1，算出来的数相当于结果的补码，所以在求补码，再在前面加上负号
  - To obtain the correct result $- (N – M)$
    - take the 2's complement of the sum $(2^n-(2^n-(N-M))=N-M)$
    - place a $-$ to its left $(-(N-M))$.
  
**Example**
![3.31](3.31.png)
进位是 1 表明结果为正（未发生溢出），不需对结果修正
![3.32](3.32.png)
进位是 0 表明结果为负（发生溢出），需对结果修正（再进行取反加一，取补码）

**无符号整数减法：**
1. 被减数不变，减数全部位按位取反，末位加一（取补码），减法变加法
2. 从最低位开始，按位相加，并往更高位进位（根据是否有更高的进位判断答案是否是正）

### 4. Signed Integers
- **原码signed-magnitude 符号数值表示法**
  - 是最简单的机器数表示法，用最高位表示符号位，其他位存放该数的二进制的绝对值（1为负数，0为正数）。
- **反码(1's complement)**：
  - 正数的反码和原码一致；负数的反码就是它的原码除**符号位**外，按位取反。 符号位不变
- **补码(2's complement)**：
  - 正数的原码、反码、补码都一致；负数的补码等于反码+1。**计算机中一般用补码表示数值。**

符号位在内存中存放的最左边一位，如果该位位0，则说明该数为正；若为1，则说明该数为负。

#### 1. signed-Magnitude arithmetic(原码算数)
**符号-数值表示法计算，左边的符号位与n-1位数值部分分别处理。数值部分计算与无符号加减法一致。**
- 检查三个符号位的奇偶性（两个操作数的符号位和加减法的符号位，我们一般认为加法是 0, 减法是 1）用于判断**溢出**
可能溢出的情况：正加正(000), 正减负(011), 负减正(110), 负加负(101)

**Parity of the Three Signs**：$Op\oplus Opd1\oplus Opd2$

- If the parity of the three signs is 0:(overflow may happen) 
  - Add the magnitudes. 数值位相加
  - Check for overflow (a carry out of the MSB) 看是否溢出
  - The sign of the result is the same as the sign of the first operand. 符号位和第一个符号相同
- If the parity of the three signs is 1:
  - Subtract the second magnitude from the first.
  - If a borrow occurs:
    - take the two’s complement of result and 
    - make the result sign the complement of the sign of the first operand.结果的符号位是第一个的相反数
  - Overflow will never occur. 不会溢出

#### 2. signed-Complement arthimetic 
**符号-补码表示法的算术运算**
- Addition:
  - Add the numbers including the sign bits, discarding a carry out of the sign bits (2's Complement), or using an end-around carry (1's Complement).
  - If the sign bits were the same for both numbers and the sign of the result is different, an overflow has occurred.
  - The sign of the result is computed in step 1.
- Subtraction:
  - Form the complement of the number you are subtracting and follow the rules for addition.


计算时都用**补码**进行操作（已取补码，用符号-补码表示法）
- **加法**：从最低位开始，按位相加（符号位参与运算），并往更高位进位，符号位产生的进位全部丢弃，若结果为负数，算出来的自动是正确的补码表示。
- **减法**：
  1. 被减数不变，减数按位取反加一（包括符号位）
  2. 从最低位开始，按位相加，并往更高位进位（包括符号位），丢弃符号位处产生的进位。结果自动为补码表示法。

**溢出（overflow）**：
- 当2个无符号数相加，如果最高位有进位，说明溢出
- 当2个无符号数相减，溢出不会发生。
- 当两个有符号数相加时，符号位作为数的一部分处理，如果符号位有进位，不能说明溢出。
  - 对于有符号数加法，如果一正一负，不会溢出
  - 如果2个正数或两个负数，那么溢出可能发生。