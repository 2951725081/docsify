# Sequential Circiuts
## Part1 Storage Elements and Sequential Circuit Analysis
### Introduction to Sequential Circuits
A sequential circuit contains:
- Storage elements: Latches锁存器 or Flip-Flops触发器
- Combinational Logic:
    - Inputs：
        - signals from the outside
        - signals from storage elements called State or Present State（现态$Q_n$）.
    - Outputs:
        - signals to the outside
        - signals to storage elements called Next State(次态$Q_{n+1}$)

我们将某一时刻存储存储元件(storage element) 中的二进制信息称为该时刻时序电路的**状态(state)**

**Combinational Logic**:
- Next state function: $Q_{n+1}=f(Q_n,X)$,其中$X$为输入变量。次态方程
- 根据输出信号是否与输入信号有关，可将时序电路分为：
    - Output function: **（Mealy型）**$Y=g(Q_n,X)$，是输入和现态的函数
    - Output function: **（Moore型）**$Y=h(Q_n)$，是状态函数，输入不会直接改变输出，而是通过改变状态来间接改变输出。

![4.35](4.35.png)

时序逻辑电路是数字逻辑电路的重要组成部分,时序逻辑电路又称时序电路,主要由**存储元件**和**组合逻辑电路**两部分组成。它和我们熟悉的其他电路不同,其在任何一个时刻的输出状态由当时的输入信号和电路原来的状态共同决定,而它的状态主要是由存储电路来记忆和表示的。同时时序逻辑电路在结构以及功能上的特殊性,相较其他种类的数字逻辑电路而言,往往具有难度大、电路复杂并且应用范围广的特点。
![4.1](4.1.png)

### Type of Sequential Circuit
- Depends on the times at which:
    - storage elements observe their inputs, and
    - storage elements change their state
- 根据输入信号的时间和内部状态改变的时间，可将时序电路分为：
    - **Synchronous(同步时序)**
        - 状态更新一定发生在时钟周期的整周期上
        - 根据对离散时间信号的了解确定行为
        - 存储元件观察输入，**只能根据定时信号（来自时钟的时钟脉冲）改变状态**
        - 把时钟脉冲作为存储元件输入信号的同步时序电路称为钟控时序电路（clocked sequential circuit）
        - 主要表现是所有触发器共用一个时钟信号源
        - **优点是所有触发器同时刷新，信号延迟低，缺点结构复杂**
        ![4.2](4.2.png)  有触发器flip-flop
    - **Asynchronous（异步时序）**
        - 状态更新可以在任意时间发生
        - 如果时钟也被看做一个输入，那么所有电路都是 Asynchronous
        - Asynchronous 可以让我们在有需要的时候更新电路，降低电路的功耗
        - 所有触发器不是共用一个时钟信号源
        - **优点结构简单，缺点触发器刷新不同步，信号延迟会累积而状态异常**
        ![4.3](4.3.png)

### Discrete Event Simulation 离散事件模拟
可以通过离散事件模拟来理解时序电路的时间行为。

- 毛刺（glitch）:或称瞬态错误

### Latch 锁存器
只要输入信号不发生变化，存储元件将能够无限期保存二进制数据。最基本的存储元件是锁存器。通常情况下，触发器是由锁存器构成的。
- Many components to store historical state
    - Capacitors, Inductors, etc.
    - Latches, Triggers
- Satisfy the following three conditions can be referred to as **latches**
    - There are two stable states, i.e., "0", "1";2个稳态
    - Long term maintaining a given stable state;能长期保持给定的稳定状态
    - Under certain conditions, it can change state, such as setting “1” or resetting “0”.
- The simplest latches are RS latch and D latch

#### RS latch
1. Basic (NAND) $\overline{S}-\overline{R}\  Latch$
![4.4](4.4.png)
- 是0触发的，即$S'=0$时将$Q$置于1
- **最后一步，两个引脚同时从 0 -> 1**, 那么两个与非门的输出都期望变成 0, 但只要有一个门的输出变为 0 另一个门就被锁住变成 1, 因此两个门不可能同时变化。但我们无法确定是哪个门会变成 1.
所以均为0是禁止的。会振荡，输出是不稳定的(unstable)
- 正常情况下，Q与Q’是相反的，不可能相同，所以这种是Q与Q’均为1为非正常状态。
- $S=0,R=0$是禁止的，因为两个输入信号不能同时为1

- The principle of a latch is quite similar to a seesaw:
    - The two ends of the seesaw represent the outputs Q and Q.
    - The two people, Mr. R and Mr. S, represent the inputs R and S.
    - Only one person is allowed to sit at a time — they can't sit together.
    - When Mr. S sits on the seesaw, the output Q becomes high.
    - Even if Mr. S gets off the seesaw, the seesaw keeps its state.
    - When Mr. R sits on the seesaw, Q becomes low. When Mr. R gets off, the seesaw still holds that state.
- 锁存器的原理与跷跷板十分相似：
    - 跷跷板的两端分别代表输出端 Q 和 Q。
    - R 先生和 S 先生这两个人分别代表输入端 R 和 S。
    - 每次只能坐一个人，他们不能坐在一起。
    - 当 S 先生坐在跷跷板上时，输出 Q 变为高电平。
    - 即使 S 先生从跷跷板上下来，跷跷板也会保持其状态。
    - 当 R 先生坐在跷跷板上时，Q 变为低电平。当 R 先生离开时，跷跷板仍保持该状态。

![4.5](4.5.png)

2. Basic (NOR) $S-R$ Latch
![4.6](4.6.png)

S=1，R=1时是禁止的（forbidden）或未定义的（undefined），此时$Q=0,   Q'=0$同时成立。一般不使用这种状态。

SR锁存器，任何时刻都能变化。

3. Clocked $S-R$ Latch :Synchronous Circuit
![4.7](4.7.png)
$C$作为一个 ENABLE 的功能。当$C=0$时，$Q$不会发生改变。当$C=1$时，上面相当于$\overline{S}$,下面相当于$\overline{R}$, 变成一个钟控的 S-R 锁存器。

|$C$|$S$|$R$|$Q(t+1)$|
|-|-|-|-|
|0|x|x|no change|
|1|0|0|no change|
|1|0|1|0:Claer Q|
|1|1|0|1:Set Q|
|1|1|1|Indeterminate|

$Q(t+1)$based on current state$Q(t)$and current inputs $(S,R,C)$

#### D latch
![4.8](4.8.png)

即当$C=1$时，$Q=D$

![4.9](4.9.png)
这个Latch 是 透明的(transparent)，就是说**输入的变换立即就能传递到输出端口**，当几个透明的 latch 级联时，**输入端的信号也能立即传递到输出端**。输出能根据输入的变化而变化。当给 latch 添加额外的逻辑电路（比如使能信号 enable 无效时），就会使它变为 不透明的(non-transparent)。

### Flip-Flop触发器
锁存器不适合使用在电路中(存在时序问题**the latch timing problem**)：不能做到一个周期内状态值更新一次。稳定性差（是transparent的，当时钟脉冲有效时，C=1,输出能根据输入的变化而变化）。而触发器可以仅在clk上升或下降时更新一次。

![4.10](4.10.png)

在锁存器电路中，可能存在通过组合逻辑的路径：
- 从一个存储元件到另一个存储元件
- 从一个存储元件返回到同一存储元件
- 锁存器输出端和锁存器输入端之间的组合逻辑可以是简单的互连。
- 对于时钟 D 锁存器，只要时钟输入值为 1，输出 Q 就取决于输入 D(transparent的)。

![4.11](4.11.png)

![4.12](4.12.png)
**闩锁时序问题**的解决方案是在存储元件内打破从 Q 到 Q 的封闭路径
- 常用的路径断开解决方案是用以下器件取代 S-R 锁存器：
- 触发触发式触发器（主从触发式触发器）
- 边缘触发触发器
- 控制输入端的值发生变化时，触发器中锁存器的状态就会发生切换。这种变化称为触发。

#### S-R Master-Slave Flip-Flop 脉冲触发SR主从触发器
![4.13](4.13.png)
前面称为 master(主锁存器), 后面称为 slave(从锁存器)
当$C=0$时，主锁存器不变。
$C$从 0 变为 1 时，主锁存器被使能，Q 改变，但从锁存器不变。
在clk有效电平期间，主触发器课多次翻转，需要**考虑整个有效电平期间输入信号变化**，再确定最后状态。
当$C$从1变为0时，主锁存器固定，Q 不变（为最后状态），从锁存器改变，其值即为此时主锁存器的输出值。
- 周期变长一倍**一次性采样问题**(1s catching problem)：当 S,R 均为 0 时如果有小扰动，出现了S=1或R=1的毛刺（glitch），就会改变主锁存器的值，不会像期望一样处于保持状态，并会传递给从锁存器，无法复原，所以要求主从触发器避免 S，R 的扰动（要求其输入信号在半个时钟周期内不改变，C=0的时间内）

Such a circuit is called a **pulse-triggered or level-triggered flip-flop**脉冲触发的触发器.

#### Edge-Triggered D Flip-Flop 边沿触发的D触发器
An **edge-triggered flip-flop** ignores the pulse while it is at a constant level and triggers only during a transition of the clock signal.忽略在时钟信号保持阶段的输入信号变化，只考虑时钟信号转换时的输入信号。
A **master-slave D flip-flop** which also exhibits **edge-triggered** behavior can be used.
边缘触发触发器在脉冲处于恒定电平时忽略脉冲，仅在**时钟信号转换时触发**。
主从式 D 触发器也具有边缘触发特性。
**Negative edge-triggered flip-flop** 下降沿触发器

![4.14](4.14.png)

The delay of the S-R master-slave flip-flop can be avoided since the 1s-catching behavior is not present with D replacing S and R inputs. (D 锁存器不会出现 S，R 同时为 0 的情况)
1个反相器
**Positive-Edge Triggered D Flip-Flop** is Formed by adding inverter to clock input. (上升沿触发器)
![4.15](4.15.png)
Q changes to the value on D applied at the positive clock edge within timing constraints to be specified.两个反相器

#### J-K Flip-Flop 脉冲触发JK触发器
- Behavior
    - 类似脉冲 SR 主从触发器, $J$相当于$S$,$K$相当于$R$
    - $J=0,K=0$时，输出信号保持不变
    - $J=0,K=1$时，输出信号变为0
    - $J=1,K=0$时，输出信号变为1
    - 但$J=K=1$时，触发器相当于是一个求反的功能，$Q$输出反转
    - 也有**一次性采样的问题**
- Implementation
![4.16](4.16.png)
![4.32](4.32.png)

- 在整个clk高电平期间，主触发器只可能反转一次。
- 由于有$Q'$反馈,找出Q‘最后状态，才能确定最后状态
![4.33](4.33.png)
![4.34](4.34.png)
- Symbol
（**三角表明是正脉冲触发**，若为**圆圈和三角**则是负脉冲触发）

#### T Flip-Flop
- Behavior
    - 类似 JK 触发器, 相当于$J=K=T$. 当$T=0$, 状态不变; 当$T=1$, 状态求反
    - 存在一次性采样的问题
    - 无法预置状态，因此需要一个 Reset 信号
- Implementation
![4.17](4.17.png)

#### 标准图形符号
所有触发器和锁存器都用一个矩形块来表示，输入信号在左侧，输出信号在右侧。一个输出端用来标明触发器当前的正常状态，另一个带有小圆圈的输出端代表触发器当前的状态的非。在SR锁存器和SR触发器中，输入信号S,R标注在矩形内部，在$\overline{S}\overline{R}$锁存器的符号中，每个输入端都加了小圆圈（表示低电平有效），表示输入信号为0时置位和复位。D锁存器或D触发器中，输入信号D和C标注在矩形内部。
![4.18](4.18.png)
- 对于**脉冲触发式触发器**，在其输出端前面有一个直角符号，称之为延时输出指示器（Postponed output indicators），表示输出信号在**脉冲的结尾发生改变**，但是**输出结果与有效电平期间的输入信号变化有关**。
- **边沿触发器**clk输入带三角形，**默认为正边沿**，前面再加一个圆圈，表示负边沿触发器，只与电平变化边沿输入信号有关。

#### 直接输入
![4.19](4.19.png)
![4.20](4.20.png)

直接输入用来进行初始化，可以在时钟正常运行之前将书法其设置成一个初始状态。

### Analysis
![4.24](4.24.png)
总模型
同步时序电路里 D 触发器的时钟输入端，统一接在一个系统时钟输入 CLK 信号上。（规定所有触发器何时进行状态改变，是额外提供的引脚，不属于整个系统的信号输入）

- **Current State** at time (t) is stored in an array of flip-flops.
- **Next State** at time (t+1) is a Boolean function of State and Inputs.
- **Outputs** at time (t) are a Boolean function of State (t) and (sometimes) Inputs (t).
- 特征表(characteristic table)：定义了下一状态与当前状态和输入的关系。
- 特征方程(characteristic equation)：用布尔函数来表示下一状态与当前状态和输入的关系。
常用于设计（从状态中得到输入）：
- 激励表(excitation table)：列出所有可能的当前状态和下一状态，从而确定输入

![4.25](4.25.png)
驱动方程是各触发器输入信号的逻辑函数式

分析过程：
1. Derive the input equations, next state equations and output equations推导输入方程、下一状态方程和输出方程
2. Derive the state table (truth table with state):
    - Inputs: inputs of circuit, present state of the circuit
    - Outputs: outputs of circuit, next state of all flip-fops
    推导状态表（带状态的真值表）：
    - 输入：电路的输入、电路的当前状态
    - 输出：电路的输出、所有触发器的下一个状态
3. List the next state of the sequential circuit
4. Obtain a state diagram
5. Analyze the performance of the circuit
6. Verify the correctness of the circuit, check the self-recovery capability and draw the timing parameters

#### 1. State table 状态表
- State table – a multiple variable table with the following four sections:
    - Input – the input combinations allowed.
    - Present State – the values of the state variables for each allowed state.
    - Next-state – the value of the state at time (t+1) based on the present state and the input.
    - Output – the value of the output as a function of the present state and (sometimes) the input.
- From the viewpoint of a truth table:
    - the inputs are Input, Present State
    - the outputs are Output, Next State

![4.26](4.26.png)


#### 2. State diagrams
- The sequential circuit function can be represented in graphical form as a state diagram with the following components:
    - A circle with the state name in it for each state每个状态都有一个带有状态名称的圆圈
    - A directed arc from the Present State to the Next State for each state transition每个状态转换的 “当前状态 ”到 “下一状态 ”的有向弧
    - A label on each directed arc with the Input values which causes the state transition, and A label:在每条有向弧上标注**导致状态转换的输入值和标签：$X/Y$**
    **输出和输入是否有关，无关标在圈里面，有关标在外面**
        - On each circle with the output value produced, or
        - On each directed arc with the output value produced.

- Moore type output depends only on state(输出画在圈里面) 只跟状态有关
- Mealy type output depends on state and input(输出画在有向弧上) 跟状态和输入有关
![4.27](4.27.png)
![4.29](4.29.png)

在状态转换图中以圆圈表示电路的各个状态， 以箭头表示状态转换的方向。同时， 还在箭头旁注明了状态转换前的输人变量取值和输出值。通常将输人变量取值写在斜线以上， 将输出值写在斜线以下。所示中电路没有输入逻辑变量， 所以斜线上方没有注字。
![4.28](4.28.png)

**Equivalent State Definitions**
两个状态，无论输入是什么，都会使这两个状态输出相同，次态也相同，那么这两个状态是等效状态。




#### 3. Flip-Flop Timing Parameters
![4.21](4.21.png)
- **建立时间**（$T_{su}$：set up time $t_s$）：是指在触发器的时钟信号上升沿到来以前，数据稳定不变的时间，如果建立时间不够，数据将不能在这个时钟上升沿被稳定的打入触发器，Tsu就是指这个最小的稳定时间。时钟事件发生前，数据输入应保持稳定的最短时间。这样才能将数据成功存储到触发器中。
  - 对于主从触发器，为了避免一次性采样问题，其 setup time 应为**半个时钟周期**。即对于高电平激活的电路，其在时钟信号维持高电平期间不能改变输出。
  - 对于边缘触发器，其所需要的 setup time 比主从触发器小很多，但也并不是0
- **保持时间**（Th：hold time $t_h$）：是指在触发器的时钟信号上升沿到来以后，数据稳定不变的时间，如果保持时间不够，数据同样不能被稳定的打入触发器，Th就是指这个最小的保持时间。时钟事件发生后，数据输入应保持稳定的最短时间，以便时钟能可靠地对数据进行采样。
  - If the data changes within the "setup-hold-window", the output is unpredictable or metastable.输入数据必须在该段时间内保持稳定，不然会进入不稳定状态。
  - $t_h$ - hold time - Often equal to zero 近似等于0,即时钟脉冲之后堆输入信号是否保持没有要求

![4.22](4.22.png)
![4.23](4.23.png)
- $t_s$ - setup time
    - Master-slave - Equal to the width of the triggering pulse脉冲的宽度
    - Edge-triggered - Equal to a time interval that is generally much less than the width of the the triggering pulse小于脉冲宽度
- $t_w$- 时钟脉冲宽度(clock pulse width)：一个时钟周期中，时钟信号为高电平（$t_{wh}$）或低电平($t_{wl}$)的时间。取决于触发器是在时钟信号的上升沿触发还是在时钟信号的下降沿触发。
- $t_{px}$ - propagation delay传输延迟
    - Same parameters as for gates except
    - Measured from clock edge that triggers the output change to the output change
    - 输出信号从高到低所需要的传播延迟定义为$t_{PHL}$ ;输出信号从低到高所需要的传播延迟定义为 $t_{PLH}$;对触发器传播延迟的定义可取两者的最大值或平均值

#### 4. Timing Analysis of Sequential Circuits
The ultimate goal of timing analysis is to determine the **maximum clock frequency of the circuit**.
- Timing Constraints Components
    - $t_p$ - clock period - The interval between occurrences of a specific clock edge in a periodic clock
    - $t_{pd,FF}$ - **flip-flop propagation delay** - The amount of time from clock edge to when the flip-flop output becomes stable
    - $t_{pd,COMB}$ - **combinational logic delay** – The total delay of combinational logic along the path from flip-flop output to flip-flop input
    - $t_s$ – **flip-flop setup time** - The amount of time data input should be held steady before the clock event.
    - $t_{slack}$ - extra time in the clock period
    中间三个时间为路径上的时间延迟

![4.30](4.30.png)

![4.31](4.31.png)
考虑$t_{stack}=0$,得到$$t_p\geq Max(t_{pd,FF}+t_{pd,COMB}+t_s)$$这是所有路径中，信号从触发器传播到触发器的最长时间。
Flip-Flop的传播延迟时间$t_{pd}$(propagation delay)与保持时间$t_h$计时起点完全相同，但$t_{pd}$的时延一定大于$t_h$，所以在计算时钟边沿到时钟边沿的传播延迟时，只要考虑传播延迟时间$t_{pd}$，而不需要再次累加$t_h$。

### 状态赋值 State Assignment
用n个二进制给m种状态各自分配一个不同的编码，需要使用n个触发器。剩余$2^m-n$个状态未使用，视为不定项。
|赋值方法|特性|
| :------: |:-------:|
|按计数顺序（counting order）分配||
|按格雷码（Gray code）分配）|在使用卡诺图化简方便|
|单热点(one-hot)赋值|每个状态对应一个触发器，跳变最少使用效率最高|