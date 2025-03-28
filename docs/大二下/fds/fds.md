# FDS(数据结构基础)
## 1.算法基础  

---

## 2.表，栈，队列

---

## 3.树

---

## 4.优先队列（堆）

### 4.1 模型 
至少完成以下两种的数据结构：Insert ，DeleteMin（此时为最小堆，最小元素位于顶层，优先出列）

![4.1.1](.\4.1.1.png "4.1.1")

### 4.2 简单实现
- 二叉堆
  - 结构性质  
  是一颗被完全填满的二叉树，底层元素从左到右填入，称为完全二叉树（complete binary tree）

  ![4.2.1](.\4.1.2.png "4.2.1")
  易证高为h的完全二叉树有2^h^到2^h+1^-1个节点，故h=log2(N)向下取整，是O（log N）  

  它可以用数组表示，而不需要指针，对任一i，其左子节点为2i，右子节点为2i+1，其父节点为i/2向下取整。该数组的0位置为空（不存放有效值），1位置为根节点，2位置为根节点的左子节点，依次类推。
  ![4.2.2](.\4.2.2.png "4.2.2")
  - 堆序性质（最小堆）
  对于任一节点X，其父节点小于（或等于）其中的关键字，根节点除外。
  ```c
  struct PriorityQueue{
    int capacity;
    int size;
    ElementType *Eelements;
  };//堆的抽象数据类型声明

  PriorityQueue *CreatePriorityQueue(int MaxElements){
    PriorityQueue *Q;
    if(MaxElements<MinQOSize) Error("Priority Queue is too small");
    Q = (PriorityQueue*)malloc(sizeof(struct PriorityQueue));
    if(Q==NULL) Error("Out of space!!!");
    Q->Elements = (ElementType*)malloc((MaxElements+1)*sizeof(ElementType));
    if(Q->Elements==NULL) Error("Out of space!!!");
    Q->Capacity = MaxElements;//最大容量
    Q->Size = 0;
    Q->Elements[0] = MinData;//0位置为空
    return Q;
  }//创建一个优先队列
  ```
### 4.3 堆的基本操作
- **Insert**
  - 堆的插入操作，将新元素插入到堆的末尾，然后通过上浮操作（percolateUp）调整堆的堆序性质。
  ```c
  H->Elements[0] is a sentinel(标记，足够小，类似于链表中头节点)
  void Insert(ElementType X,PriorityQueue *Q){
    int i;
    if(Q->Size == Q->Capacity) Error("Priority Queue is full");
    for(i=Q->Size+1;Q->Elements[i/2]>X;i/=2){
        Q->ELements[i]=Q->Elements[i/2];
    }
    Q->Elements[i] = X;
  }//T(N)=O(logN)
  ```  
- **DeleteMin**
  - 堆的删除操作，删除堆顶元素，然后通过下沉操作（percolateDown）调整堆的堆序性质。将两个Child中较小的一个与其交换，将空位向下移。（需要考虑只有一个孩子节点的情况）
  ```c
  ElementType DeleteMin(PriorityQueue *Q){
    int i,child;
    ElementType MinElement,LastElement;
    if(IsEmpty(Q)){
        Error("Priority Queue is empty");
        return Q->Elements[0];
    }
    MinELement = Q->Elements[1];
    LastElement = Q->Elements[Q->Size--];//[Q->Size],Size--,把最后以为移至第一位
    for(i=1;i*2<=Q->Size;i=child){
        child = i*2;
        if(child!=Q->Size && Q->Elements[child+1]<Q->Elements[child]) child++;
        if(LastElement>Q->Elements[child]) Q->Elements[i] = Q->Elements[child];
        else break;
    }
    Q->Elements[i] = LastElement;
    return MinElement;
  }///T(N)=O(logN)，被放到根处的元素几乎都下移到底层
  ```
- **其他堆操作**
  - **DecreaseKey**
  减小堆元素，上移调整堆的堆序性质。
  - **IncreaseKey**
  增大堆元素，下移调整堆的堆序性质。
  - **Delete**
  先通过执行**DecreaseKey**操作，将目标点移至堆顶，再执行**DeleteMin**操作。
  - **BuildHeap**
  堆的建立操作，将数组中的元素依次插入堆中（从上到下，从左到右），不管正确性，然后通过倒序下沉操作处理所有“根”调整堆的堆序性质。
