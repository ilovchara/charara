---
title: Four
date: 2023-04-28 09:05:38
tags: 算法
---

## 第四章 - 图论的拓展

```c++
1.基础图论模型
2.图论算法
```

​  图是用点和边组成的一种数据类型，点与点之间用边来联系在一起。（图通常用于解决，最短路径问题、最小生成树问题、最大流问题）有关于图的问题，大概就是最短路，最小生成树，贪心价值最大，还有图的两种遍历。有关于图的数学知识我们尚不讨论，但是关于图的遍历我们需要了解。目前，常规的遍历方法有两种，1.是竖向的dfs递归遍历 2.是横向的bfs递归遍历。我们现在先要求掌握两种遍历，然后才开始下一阶段的学习。

### 二叉树

​  要了解上述两个遍历模型，就要了解二叉树。二叉树是图的一种特殊造型，二叉树有两种主要形式：满二叉树和完全二叉树.满二叉树的深度和结点是有关系的，一个深度为k的满二叉树，它的节点数是2^k - 1（每一层是两个，减去我们的头结点就ok了）

#### 1.完全二叉树

​  完全⼆叉树的定义如下：在完全⼆叉树中，除了最底层节点可能没填满外，其余每层节点数 都达到最⼤值，并且最下⾯⼀层的节点都集中在该层最左边的若⼲位置。若最底层为第 h层，则该层包含 1~ 2^h -1 个节点。（就是叶子结点左边可以不满，其他必须满是这个意思是吗）。

​        ![image-20230216131827500](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230216131827500.png)

```c++
//二叉搜索树（建立在完全二叉树的基础之上吗）
有如下的规则（简单来说，左边的都是小于根节点，右边的都是大于根节点）
 -若它的左⼦树不空，则左⼦树上所有结点的值均⼩于它的根结点的值； 
 -若它的右⼦树不空，则右⼦树上所有结点的值均⼤于它的根结点的值；
 -它的左、右⼦树也分别为⼆叉排序树
```

![image-20230216133015856](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230216133015856.png)

#### 2.平衡二叉搜索树

平衡⼆叉搜索树：又被称为AVL（Adelson-Velsky and Landis）树，且具有以下性质：它是 ⼀棵空树或它的左右两个⼦树的⾼度差的绝对值不超过1，并且左右两个⼦树都是⼀棵平衡⼆叉树。（高度差 注意是高度差！！！！！）

![image-20230216133247245](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230216133247245.png)

#### 3.用数组实现二叉树

![image-20230216134441813](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230216134441813.png)

```c++
⽤数组来存储⼆叉树如何遍历的呢？ 如果⽗节点的数组下表是i，那么它的左孩⼦就是i * 2 + 1，右孩⼦就是 i * 2 + 2。 
但是⽤链式表⽰的⼆叉树，更有利于我们理解，所以⼀般我们都是⽤链式存储⼆叉树。
所以⼤家要了解，⽤数组依然可以表⽰⼆叉树
```

#### 4.二叉树的链式实现

```c++
struct TreeNode{
  int val;
  TreeNode *left;//定义left指针 指向左子树
  TreeNode *right;//right指针 指向右子树
  TreeNode(int x):val(x),left(NULL),right(NULL){}//构造函数  
};
```

#### 5.二叉树的遍历步骤

```c++
//实现递归的三要素
1. 确定递归函数的参数和返回值： 确定哪些参数是递归的过程中需要处理的，那么就在递归函数⾥加上这个参数， 并且还要明确每次递归的返回值是什么进⽽确定递归函数的返回类型。
2. 确定终⽌条件： 写完了递归算法, 运⾏的时候，经常会遇到栈溢出的错误，就是没写终⽌条件或者 终⽌条件写的不对，操作系统也是⽤⼀个栈的结构来保存每⼀层递归的信息，如果 递归没有终⽌，操作系统的内存栈必然就会溢出。
3. 确定单层递归的逻辑：
确定每⼀层递归需要处理的信息。在这⾥也就会重复调⽤⾃⼰来实现递归的过程
//前序遍历（就是前中后）
1. 确定递归函数的参数和返回值：因为要打印出前序遍历节点的数值，'所以参数⾥需 要传⼊vector在放节点的数值，除了这⼀点就不需要在处理什么数据了也不需要有返回值，所以递归函数返回类型就是void，代码如下：
void traversal(TreeNode* cur, vector<int>& vec) //（一个存放的是结构体的指针，一个存放的是存储数据的容器）
2.确定终⽌条件：在递归的过程中，如何算是递归结束了呢，当然是当前遍历的节点 是空了，那么本层递归就要要结束了，所以如果当前遍历的这个节点是空，就直接return，代码如下：
if (cur == NULL) return;
3.确定单层递归的逻辑：前序遍历是中左右的循序，所以在单层递归的逻辑，是要先 取中节点的数值，代码如下：
vec.push_back(cur->val); // 中
traversal(cur->left, vec); // 左
traversal(cur->right, vec); // 右
```

```c++
//前序遍历
class Solution { 
  public: void traversal(TreeNode* cur, vector<int>& vec) {
   if (cur == NULL) return; 
   vec.push_back(cur->val);
  // 中
   traversal(cur->left, vec); 
   // 左 
   traversal(cur->right, vec); 
   // 右
}
vector<int> preorderTraversal(TreeNode* root) { 
   vector<int> result; traversal(root, result); return result;
 }
};

//中序遍历
void traversal(TreeNode* cur, vector<int>& vec) { 
  if (cur == NULL) return; 
  traversal(cur->left, vec); 
  // 左 
  vec.push_back(cur->val);
  // 中 
  traversal(cur->right, vec);
  // 右
}

//后续遍历
void traversal(TreeNode* cur, vector<int>& vec) { 
  if (cur == NULL) return;
  traversal(cur->left, vec); 
  // 左 
  traversal(cur->right, vec); 
  // 右
  vec.push_back(cur->val);
  // 中
}
```

![image-20230216135315873](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230216135315873.png)

![image-20230216135326265](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230216135326265.png)

### 深度优先遍历

#### 0.前置知识 图的存储

![image-20230305080127311](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230305080127311.png)

##### 领接矩阵

```c++
//1.领接矩阵
 领接矩阵声明，开一个二维数组； 1.二维数组w[u][v],存储u - v的边权（就是这个连线多长）2.只能在稠密图上使用（稠密图 就是边和点不是一个数量级的  点小于边）
        //应该是 一个数组存储点 一个数组存储边； 用数组下标表示点和边的关系

int w[N][N]; //声明边权数组 - 例如w[a][b] = c; 说明的就是a连上了b 线的长度 = c
int vis[N];//点集

void dfs(int u){
    vis[u] = true;
    for(int v = 1;v<=n;v++){
        printf("%d,%d,%d\n",u,v,w[u][v]);
        if(vis[u]) continue;
        dfs(v);
    }
}

int main()
{
    cin>>n>>m;
    for(int i = 1;i<=m;i++){
        cin>>a>>b;
        w[a][b] = c; //赋予权值（a点 和 b点）（算是连线）
    }
    dfs(1);
    return 0;
}
```

##### 边集数组

![image-20230305080154969](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230305080154969.png)

```c++
//边集数组
//结构体下标联系三者
struct edge(){
    int u,v,w; //两点 和 权
}e[M]; //边集合
int vis[N]; //点

void bfs(int u){
    vis[u] = true;
    for(int i = 1;i<=m;i++){
        int v = e[i].v,w = e[i].w; //用结构体中对应的值
        printf("%d,%d,%d",u,v,w); //
        if(vis[v]) continue; //这个点到过了
        dfs(e[i].v);//下一个点
    }
}

int main()
{
    cin>>n>>m;
    for(int i = 1;i<=m;i++)
    {
        cin>>a>>b>>c;
        e[i] = {a,b,c};
        //e[i] = {b,a,c}; //无向图就要加上
    }
    dfs(1);
    return 0;
}
```

##### 领接表（重点）

![image-20230302110544398](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302110544398.png)

```c++
//领接表（将图用dfs序输出）
出边数组e[u][i] 存储u点的所有出边{终点 v ， 边权 w}。
    
struct edga{int v,w;}; //声明结构体（代表每个点有的属性）
vector<edga> e[N];//存储的是每一个起点（有n个）

void dfs(int u,int fa)  //1.从哪一个点开始深搜  2.当前节点的父节点
{
    for(auto ed:e[u]) //遍历当前层的元素
    {
        int v = ed.v,w = ed.w;
        if(v==fa) continue;//判重（父节点记录父节点是否走过）（u和fa是交替使用的）
        printf("%d，%d,%d\n",u,v,w);
        dfs(v,u);// （u,fa,v,u,v,u.......） //fa算是根节点的父节点（根节点没有父节点所以就归0就ok了）
    }   
}

int main()
{
    cin>>n>>m;
    for(int i = 1;i<=m;i++){
        cin>>a>>b>>c;
        //无向图 两边都要连接
        e[a].push_back({b,c});
        e[b].push_back({c,b});
    }
    //1.从哪一个点开始深搜  2.当前节点的父节点
    dfs(1,0);
    return 0;   
    
}
//父节点是相互的，我们用领接表存储的时候，只要连接就是父和子的叠加态（这也解释了为什么可以回溯）

```

##### 链式领接表

![image-20230302111110291](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302111110291.png)

```c++
//链式领接表
两个变量：1.边集数组 2.表头数组
1.边集数组存储的是第几条边的属性：e[j]存储第j条边的{起点u，终点v，边权w}
2.表头数组存储的是u点的所有出边的编号：h[u][j](u表示的是当前的点)（j表示的是当前点连接的出边）
    
struct edge{int u,v,w};
vector<edge> e; //边集合（边的属性）
vector<int> h[N]; //点的所有出边

void add(int a,int b,int c)
{
    e.push_back({a,b,c}); // 对应边的属性压入
    h[a].push_back(e.szie()-1); //当前边权数组的大小-1（这里的应该是桶数组）    
}
//u当前节点 fa-父节点
void dfs(int u,int fa)
{
    for(int i = 0;i<h[u].size();i++){
        int j = h[u][j];//表示的是当前的点？
        int v = e[j].v,w = e[j].w;
        if(v == fa) continue;
        printf("%d,%d,%d\n",u,v,w);
        dfs(v,u);
    }
}

int main()
{
    cin>>n>>m;
    for(int i = 1;i<=m;i++){
        cin>>a>>b>>c;
        //邻接表存储数据
        add(a,b,c);
        add(b,a,c);
    }
    dfs(1,0);// 1是当前节点 0是当前节点的父节点（由于根没有父节点，初始化为0）
    return 0;
    
    
}

```

##### 链式前向星（重点）

![image-20230302112003101](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302112003101.png)

```c++
//链式前向星 - 用边集数组和领接表结合成的 - （还是不理解）
数组1：边集数组 - e[i]存储第i条出边的{终点v,边权w，下一条边ne}
数组2：表头数组 - h[u]存储的是u点的第一条出边的编号（理解为第一条边就可以了）
边的编号：由idx分配

//输入 变量 1.点的个数 2.边的个数
struct edge{int v,w,ne;};//ne是下一条边
edge e[M]; // 存储的是所有的边
int idx,h[N]; //点的第一条出边（每个点都有）
//表头数组相当于给我们一个起点，让我们访问到别的边

//获取第一个出边
void add(int a,int b,int c){
    e[idx] = {b,c,h[a]}; // e结构体存储的是这个边的信息
    //这样理解吗 b就是a的出边 - 每次进来的a都会标记 而且idx会变化 不用担心重复
    h[a] = idx++;
}

//每次添加一条边，都会把这条边的ne指针指向上一次添加的边的编号，这样就形成了一个链表。例如，如果先添加了a到b的边，再添加了a到c的边，那么h[a]就会指向a到c的边的编号，而a到c的边的ne指针就会指向a到b的边的编号，这样就可以通过h[a]和e[i].ne访问到a点的所有出边。
//这个函数的最后一个参数是e[idx].ne，表示这条边的下一条边的编号。赋值的是h[a]，表示a点的第一条出边的编号。这样就把这条边和a点的原来的第一条出边连接起来了，形成了一个链表。表头数组只存储了每个点的第一条出边的编号，而不是所有的边。

void dfs(int u,int fa)
{
    //横向遍历是为了回溯
    for(int i = h[u];~i;i=e[i].ne){ //~i表示i不等于-1
        int v = e[i].v,w = e[i].w;
        if(v == fa) continue;//判重 回溯
        printf("%d,%d,%d\n",u,v,w);
        dfs(v,u);/深入
    }    
}
int main()
{
    cin>>n>>m;
    memset(h,-1,sizeof h);//初始化表头
    for(int i = 1;i<=m;i++){
        cin>>a>>b>>c;
        //无向图构图
        add(a,b,c);
        add(b,a,c);
    }
    dfs(1,0);
    return 0;     
}

//说人话就是，输入一组数据 a b c。 可以将a点作为出边点 那么b就是出边。重复构建图就是这样
```

```c++
//ai给的例子
好的，我可以给你举一个例子。假设有一个无向图，有三个点，分别是1，2，3，有三条边，分别是1到2，2到3，1到3。那么输入数据的过程如下：

输入第一条边1到2，调用add(1,2)和add(2,1)来添加这条边。
此时e[0] = {2,0,-1}，表示从1到2的边的信息，其中v=2表示终点是2，w=0表示权值是0（这里没有用到），ne=-1表示没有下一条边。
同时h[1] = 0，表示1点的第一条出边的编号是0。
e[1] = {1,0,-1}，表示从2到1的边的信息。
h[2] = 1，表示2点的第一条出边的编号是1。
输入第二条边2到3，调用add(2,3)和add(3,2)来添加这条边。
此时e[2] = {3,0,1}，表示从2到3的边的信息，其中ne=1表示下一条边的编号是1（即从2到1的边）。
同时h[2] = 2，表示2点的第一条出边的编号是2（即从2到3的边）。
e[3] = {2,0,-1}，表示从3到2的边的信息。
h[3] = 3，表示3点的第一条出边的编号是3。
输入第三条边1到3，调用add(1,3)和add(3,1)来添加这条边。
此时e[4] = {3,0,0}，表示从1到3的边的信息，其中ne=0表示下一条边的编号是0（即从1到2的边）。
同时h[1] = 4，表示1点的第一条出边的编号是4（即从1到3的边）。
e[5] = {1,0,3}，表示从3到1的边的信息，其中ne=3表示下一条边的编号是3（即从3到2的边）。
同时h[3] = 5，表示3点的第一条出边的编号是5（即从3到1的边）。
这样就完成了输入数据和构建链式前向星存储结构。你明白了吗？
```

![image-20230302112236202](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302112236202.png)

#### DFS

dfs算法是一种深度优先搜索算法，它的特点是沿着树的深度遍历树的节点，尽可能深的搜索树的分支。它可以用来解决许多不同类型的问题，如寻找最短路径、搜索树、图等。dfs又被称之为递归，算是一种用暴力方法求解全部点的连通性。这种方法使用于树（树是图的一种，它具有没有环的性质），下面我们用一种简单的二叉树的图进行讨论。

![image-20230216151302883](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230216151302883.png)

##### 1.dfs过程的实现

```c++
1.深搜的过程
   从根进入，向下走 - 走到底。 无路可走，回溯向上走，绕树根兜圈子，最后从根退出。
2.深搜的实现
   用系统栈实现的。递归调用的过程，系统自动通过栈去维护函数的状态空间。系统栈记录函数的返回地址，局部变量，参数传递。 向下走 压栈； 向上走，弹栈。 // 可以看做 用栈维护不同的序列 - 弹出栈作为新的栈
3.代码（模板）
vector<int> e[N]; // 表示一颗树（或者是图）
void dfs(int u,int fa){
  for(auto v:e[N]){ // v遍历我们的e数组（v变量维护我们的栈）
    if(v==fa) continue; // 就相当于 在数组中构造树（有标记的数据我们不走?） - 这里才是回溯的重点 - 问ai
    dfs(v,u);
  }
}
int main(){
  cin>>n>>m;
  for(int  i = 1;i<=m;i++)
  cin>>a>>b, e[a].push_back(b),e[b] = push_back(a); //这个是啥意思
  dfs(1,0);
  return 0;
}
4.深搜的计算（暂时不想看）
```

##### 2.dfs序列

```c++
1.dfs模板（感觉是遍历 这个树hhh）
int g[N][N]; //这个就是 表示树（宽度 和 深度）
void dfs(int u,int fa)//fa表示树的根结点（醍醐灌顶）
{
   int sz = g[u].size(); // 整个树的节点个数
   for(int i = 0;i<sz;i++) // 遍历每一个节点
    {
       if(g[u][i]!=fa) // u（代表当前层的一个数据）的i(表示下一层的数据)没用过
        {
           dfs(g[u][i],u);
        }
    }
}
//加一个辅助数组
void dfs(int u,int fa){
  dfs_[++len] = u; //遍历的就是当前的父节点 记录一下 （每次走过的就当是根 - 父节点的就可以了）
  int sz = g[u].size();
  for(int i = 0;i<sz;i++)
  {
     if(g[u][i]!=fa){
        dfs(g[u][i],u);//根节点替换
      }
  }
}
//实例代码
#include<iostream>
#include<algorithm>
using namespace std;
//一维数组 可以用作二维数组吗
vector<int> g[100010];
int dfs_[200020],len;

void dfs(int u,int fa)
{
    dfs_[++len]=u;  
    int sz=g[u].size();
    for(int i=0;i<sz;i++)
    {
        if(g[u][i]!=fa)
        {
            dfs(g[u][i],u);
        }
    }
}

int main()
{
    int n;
    scanf("%d",&n);
    for(int i=1;i<=n;i++)
    {
      //两个点 不同方向都有一条边
        int from,to;
        scanf("%d%d",&from,&to);
        g[from].push_back(to);
        g[to].push_back(from);
    }
    dfs(1,0);
    for(int i=1;i<=len;i++)
    {
        printf("%d ",dfs_[i]);
    }
    printf("\n");
}

```

```c++
//dfs序列作用
1.子树加权
   在dfs序列中，一个结点的子树序列是连续的。 - 看下面的树： abdegcfh 我们关注 b结点 发现b - deg 这一段是连续的，就可以利用差分的操作就行加权。然后我们可以发现B字树B-D-E-G，C子树C-F-H都在一段连续的区间中。那么这有什么好处呢？比如说现在有一道题：给你一颗树，给出m个x和w，意为将x子树中的所有点加上一个权值w，最后询问所有点的权值 - 既然dfs序中x和他的所有子节点都在连续的区间上，那么我们就可以将它简化成差分的问题。比如说给b节点加2，就可以简化为给b的差分数组+2，c的差分数组-2 。（又涉及到了差分数组了）
   - 怎么找第一个不在B子树中的点 引入时间戳
2.时间戳
   作用：记录dfs第一次开始访问这个点的时间以及最后结束访问的时间。（用数组记录时间）
void dfs(int u,int fa)
{
   int x = len+1;//数组拓展
    s[++len] = ++time; 
   dfs_[len] = u;//存储dfs序列
   int sz = g[u].size();
   for(int i = 0;i<sz;i++)
    {
       if(g[u][i] == fa){
          dfs(g[u][i],u);
        }
    }
   e[x] = time;//存储对应 根节点到子节点的时间区间
}
//如果一个点的起始时间和终结时间被另一个点包括，这个点肯定是另一个点的子节点。（算导里称之为括号化定理）
```

![img](https://images2017.cnblogs.com/blog/1188068/201710/1188068-20171027104122883-1380446385.png)

![image-20230301212454581](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230301212454581.png)

```c++
//代码 
#include<vector>
#include<cstdio>
#include<cstring>
#include<iostream>
#include<algorithm>
using namespace std;

vector<int> g[100010];
int dfs_[200020],len,time,s[200020],e[200020],pos[200020];

void dfs(int u,int fa)
{
    int x=len+1;
    s[++len]=++time;//当前点 时间起点
    dfs_[len]=u;
    pos[u]=len;
    int sz=g[u].size();
    for(int i=0;i<sz;i++)
    {
        if(g[u][i]!=fa)
        {
            dfs(g[u][i],u);
        }
    }
    e[x]=time;//时间终点
}

int main()
{
    int n,m;
    scanf("%d %d",&n,&m);
  //输入我们的树
    for(int i=1;i<=n;i++) 
   //这段代码读入树中的边并将它们存储在邻接表 g 中。对于每条边，它都读入两个整数 from 和 to，表示边的两个端点。然后，它将 to 添加到 g[from] 的末尾，并将 from 添加到 g[to] 的末尾。这样，对于每个节点，其邻接表中都存储了与其相连的所有节点。
    {
      //用邻接表表示树
        int from,to;
        scanf("%d%d",&from,&to);
        g[from].push_back(to);
        g[to].push_back(from);
    }
    dfs(1,0);
    for(int i=1;i<=m;i++)
    {
        int x,y;
        scanf("%d%d",&x,&y);
        x=pos[x];
        y=pos[y];
        if(s[x]<=s[y]&&e[y]<=e[x])
        {
            printf("YES\n");
        }
        else
        {
            printf("NO\n");
        }
    }
}
//邻接表是一种常用的表示图的数据结构，它在存储稀疏图（即边数远小于节点数平方的图）时非常高效。邻接表通过为每个节点维护一个列表来存储与其相连的所有边，因此它只需要存储 2m 个整数（其中 m 是边数），空间复杂度为 O(m)。此外，使用邻接表可以快速访问与给定节点相连的所有边，时间复杂度为 O(度数)。当然，邻接表也有一些缺点。例如，它不适用于存储稠密图（即边数接近节点数平方的图），因为这样会浪费大量空间。此外，使用邻接表判断两个节点之间是否存在边需要遍历其中一个节点的邻接表，时间复杂度为 O(度数)，不如使用邻接矩阵来得快。
```

#### BFS

##### 0.前置知识：队列

```c++
//队列是一种先进先出的线性表，它只允许在一端插入元素，另一端删除元素1。队列可以用数组或者链表来实现23。队列的常见操作有入队（enqueue），出队（dequeue），获取队首元素（peek），判断队列是否为空（isEmpty）等

//在bfs中： 队列的作用是存储待访问的顶点，保证按照图的宽度优先的顺序进行遍历。队列可以帮助我们记录当前层的顶点和下一层的顶点，从而找到最短路径或者最优解。队列也可以避免重复访问已经遍历过的顶点，提高搜索效率。

//1.用数组模拟队列
#include<iostream>
using namespace std;

const int N = 100010;
int q[N];
int hh,tt; //定义队头和队尾的下标 初始化 hh = 0,tt = -1;

//向队尾插入一个元素 x
void push(int x){
    q[++tt] = x; //理解tt是向右边延伸的
}

//从队头弹出一个数（） - 队列先进先出
void pop(){
    hh++;
}

//查询队头元素
int front(){
    return q[hh];
}

//判断队列是否为空
bool empty(){
    return hh > tt;
}

int main()
{
    int m; //输入操作次数
    cin >> m;
    while (m--) {
        string op; //输入操作类型
        cin >> op;
        int x; //输入操作参数
        if (op == "push") { //如果是入队操作
            cin >> x;
            push(x);
        } else if (op == "pop") { //如果是出队操作
            pop();
        } else if (op == "empty") { //如果是判断空操作
            cout << (empty() ? "YES" : "NO") << endl;
        } else { //如果是查询对头操作
            cout << front() << endl;
        }
    }
    return 0;
    
}

//2.用stl实现队列
#include <iostream>
#include <queue> //引入STL中的queue头文件
using namespace std;

queue<int> q; //定义一个int类型的队列q

//向队尾插入一个元素x
void push(int x) {
    q.push(x); //调用STL中的push函数
}

//从对头弹出一个数
void pop() {
    q.pop(); //调用STL中的pop函数
}

//查询对头元素
int front() {
    return q.front(); //调用STL中的front函数
}

//判断队列是否为空
bool empty() {
    return q.empty(); //调用STL中的empty函数
}

int main() {
    int m; //输入操作次数
    cin >> m;
    while (m--) {
        string op; //输入操作类型
        cin >> op;
        int x; //输入操作参数
        if (op == "push") { //如果是入队操作
            cin >> x;
            push(x);
        } else if (op == "pop") { //如果是出队操作
            pop();
        } else if (op == "empty") { //如果是判断空操作
            cout << (empty() ? "YES" : "NO") << endl;
        } else { //如果是查询对头操作
            cout << front() << endl;
        }
    }
    return 0;
}

```

##### 1.bfs算法模版

![image-20230216163555284](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230216163555284.png)

![image-20230310225256235](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230310225256235.png)

![image-20230310225331114](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230310225331114.png)

```c++
//代码模版
vector<int> e[N];//存储的领接表 - 输入的时候的数据
int vis[N];//标记该点是否遍历过
queue<int> q;

void bfs()
{
    vis[1] = 1; q.push(1); //起点标记 和 起点入队
    while(q.size()){
        int x = q.front(); q.pop();
        // 这里可以打印 bfs 序列
        //cout<<x<<endl;
        
        //这一段是遍历e[x]中的所有相邻点y，如果y已经被访问过，就跳过，否则就标记为已访问，并把y入队，表示下一次要访问y的相邻点。
        for(int y: e[x]){
            if(vis[y]) continue;
            vis[y] = 1;
            //入队 - 起点连接的下一层出队
            //cout<<y<<endl;
            q.push(y);
        }
    }
    
}
```

##### 2. bfs序列（树与图的遍历）

![image-20230310225353381](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230310225353381.png)

```c++
//bfs搜索树
vector<int> e[N];
int vis[N];
queue<int> q;

void bfs(){
    vis[1] = 1; q.push(1);
    while(q.size()){
        int x = q.front(); q.pop();
        //出队的元素 （这里输出就是bfs树）
        //printf("%d\n",x);
        for(int y:e[x]){
            if(vis[y]) continue;
            vis[y] = 1;
            printf("%d 入队 "，y); //伪代码
            q.push(y); //搜索同层序的 入队吗
        }
    }
}

```

![image-20230310225411526](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230310225411526.png)

### 拓扑排序

![image-20230302193314874](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302193314874.png)

![image-20230302195721755](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302195721755.png)

![image-20230302195739338](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302195739338.png)

![image-20230302195757415](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302195757415.png)

```c++
//简单来说就是 按照 前面点最少的顺序排序（也就是入度从小到大）
1.Kahn算法
   起点是从入度为0的点开始的（这样才能入度从小到大），使用队列来维护我们的点；1.初始化：将所有入度为0的点压入我们的队列（顺序不重要）；2.每次从q中取出一个点x放入数组tp（存储最终序列）；3.将x的所有出边删除：这个时候的x是队头，边被删除那么对应的连接点的入度就变成0，就可以压入队列；4.重复23步骤。5.如果tp中的数据和我们的点的数量一样，那么就有拓扑序列，如果不一样，那就没有。（有向 无环图才有拓扑序列）
//***
vector<int> e[N],tp;//tp是我们最终输出的拓扑序列
int din[N]//存储着点的入度（画图是不是要用领接表）

bool toposort()
{
    queue<int> q; //让入度0的进入队伍
 for(int i = 1;i<=n;i++)
        if(din[i] == 0) q.push(i); // din数组记录的是入度
    while(q.size()){
        int x = q.front();
        q.pop(); //让队头出栈 并且删除对应的边
        tp.push_back(x);
        for(auto y:e[N]){
            if(--din[y]==0) q.push(y);//删除对应的边 并且让对应的数据入队
        }
    }    
    return tp.size() = n;
}

int main()
{
 cin>>n>>m;
    for(int i = 0;i<m;i++){
        cin>>a>>b;
        e[a].push_back(b);
        din[b]++;
    }
    if(!toposort()) puts("-1") ; //如果数量不满足 则说明这个不是拓扑序列
    else for(auto x:tp) printf("%d",x); //将拓扑序列输出
    return 0;
}

```

```c++
//dfs求拓扑序列(变色法)
//不懂翻转序列
1.染色法（yxc用的）
    每个点的颜色都会变化，从0 - -1 - 1，经历三次变色；1.初始状态，所有点染色为0；2.枚举每一个点，进入x点（是我们的指针），把x染色为-1，枚举x的儿子y，如果y的颜色为0，那么说明没碰过该点，进入y继续走（这里应该是检测有无环 - 会不会回到x）；3.如果枚举完x（当前数据 - 也算是队头）的儿子，将x压入tp数组；4.如果发现，有-1的出现（那么就是有环出现了），返回false，退出。

vector<int> e[N],tp;//e[N] 应该是树，，或者是领接表画的图
int c[N]; //染色数组

bool dfs(int x)
{
    c[x] = -1;
    for(int y:e[x]){
        if(c[y]<0) return 0; //有环
        else if(!c[y])
            if(!dfs(y)) return 0;
    }
    c[x] = 1;
    tp.push_back(x); //当前这个数据遍历完成 压入我们的tp数组
    return 1;
}

bool toposort(){
    memset(c,0,sizeof(c)); // 初始化 - 刚开始全部点的颜色为0
    for(int x = 1;x<=n;x++)
        if(!c[x])
            if(!dfs(x)) return 0;
 reverse(tp.begin(),tp.end());//翻转序列 为啥？
    return 1;
}
```

### Dijkstra - 最短路算法

![image-20230302202401928](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302202401928.png)

![image-20230302204100958](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302204100958.png)

```c++
//简单来说：就是每次选择最短路线进行前进（画最小生成树）
三个数组： 1.e[u] 存储节点u的所有出边的终点和边权 2.d[u] 存储u到源点的最小距离（源点就是当前连边的点）（d[u]需要遍历）3.vis[u] 标记是否出圈
    1.初始的时候，所有点都在圈中，vis = 0,d[s] = 0,d[其他点] = 正无穷
    2.从圈中选择一个距离最小的点，u，打标记出圈（贪心）
    3.对u的所有出边执行松弛操作 - 尝试更新邻点v的最小距离
    4.重复2,3操作，直到圈内为空
//代码
struct edge{int v,w;};//点 和 权
vector<edge> e[N];//边
int d[N],vis[N];//d数组是存储当前点的最小距离 - vis标记当前集合中的点（有没有调用这个点）

void dijkstra(int s)
{
    for(int i = 0;i<=n;i++) d[i] = inf;//初始化 全部点的距离都为无穷大
    d[s] = 0;
    //枚举每个点 内部枚举全部的点比较他们的距离大小（选最小的边权） - 就是每个点都要比较它自身和其它点的距离关系  （图遍历就是两层 for的）
    for(int i = 1;i<n;i++){
        int u = 0;
        for(int j = 1;j<=n;j++)//枚举全部点（包括自身）
            if(!vis[j]&&d[j]<d[u]) u = j; //如果这个点没被测过 并且 当前边权小
        
        vis[u] = 1;//标记u点（下次就选不到了）
        //遍历全部的点 v点的距离更新为最短的点 ed是迭代器：用处是迭代全部的点
        for(auto ed:e[u]){
            int v = ed.v,w = ed.w;
            if(d[v]>d[u]+w){//无穷大 > 其他（这样来筛数据）
                d[v] = d[u]+w;
            }
        }
    }
}

int main()
{
    cin>>n>>m>>s;
    for(int i = 0;i<m;i++){
        cin>>a>>b>>c;
        //领接表插入图
        e[a].push_back({b,c});//点 连接点 边权
    }    
    dijkstra(s);
}
```

![image-20230302204110774](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302204110774.png)

![image-20230302205328621](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302205328621.png)

```c++
//堆优化 - 用优先队列维护别更新点的集合。
struct edge{int v,w;};
vector<edge> e[N];
int d[N],vis[N];
//优先队列
priority_queue<pair<int,int>> q;//大根堆（把距离取负值） - 距离最小的元素最大 - 一定在堆顶（懂了 距离从小到大优化）

void dijkstra(int s)
{
    //1.全部点的距离都是无穷大
    for(int i = 0;i<=n;i++) d[i] = inf;
    d[s] = 0; q.push({0,s});//自己和自己距离为0
    //2.枚举进入队列中的数据
    while(q.size()){
        auto t = q.top(); q.pop();
        //u是点吗
        int u = t.second;
        if(vis[u]) continue;//判重
        vis[u] = true; //之前忘记标记出队了
        //当前点 遍历全部点 出来的边权最小的数据
        for(auto ed:e[u]){
            int v = ed.v,w = ed.w;
            if(d[v]>d[u]+w){
                d[v] = d[u]+w;
                q.push({-d[v],v}); //大根堆
            }
        }
    }
}
```

![image-20230302205410861](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302205410861.png)

```c++
//就是两个模板 优化的地方就是枚举的时候用队列维护
struct edge{int v,w;};
vector<edge> e[N];
int d[N],vis[N]; //边 和 点
//s是起点
void dijkstra(int s)
{
    for(int i = 0;i<=n;i++) d[i] = 0x3f3f3f; //无穷大的值
    d[s] = 0; //对于自己和自己的距离当然是0了
    
    for(int i = 0;i<=n;i++){//枚举次数
        int u = 0;
        //优化 就是把这里优化了 取消了枚举全部点
        for(int j = 1;j<=n;j++) //枚举点
            if(!vis[j]&&d[j]<d[u]) u = j;
        vis[u] = 1;
        for(auto ed:e[u]){ //ed 我们可以看做 就是e[u]（数组长度u）内部的数据（一个点）
            int v = ed.v,w = ed.w;
            if(d[v]>d[u]+w) {
                d[v] = d[u]+w;
                q.push_back({-d[v],v});//插入到 大根堆上
            }
        }            
    }    
}
```

![image-20230302205417632](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302205417632.png)

### Bellman-Ford  - 处理负权边的最短路算法

```c++
//llman-Ford 算法是一种用于求解带权图中单源最短路径的算法，可以处理负权边，但不能处理负权环。
 它的时间复杂度为 $O(VE)$，其中 $V$ 是顶点数，$E$ 是边数。Bellman-Ford 算法的基本思想是对所有的边进行 $V-1$ 轮松弛操作，以求出所有可能的最短路径。如果在第 $V$ 轮松弛操作中仍然存在松弛的边，则说明图中存在负权环。
    
//Bellman-Ford算法是一种用于计算带权有向图中单源最短路径的算法。
    它由Richard Bellman和Lester Ford分别在1958年和1956年发表，而实际上Edward F. Moore也在1957年发布了相同的算法，因此，此算法也常被称为Bellman-Ford-Moore算法1。它比Dijkstra的算法慢，但更通用，因为它能够处理边权值为负数的图2。

//单源最短路
    单源最短路问题是图论中的一个基本问题，它指的是给定一张有权图，如何求某两点之间的最短路径1。解决这个问题的算法有很多，比如Dijkstra算法和Bellman-Ford算法等。
```

![image-20230304212846831](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230304212846831.png)

![image-20230304210834724](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230304210834724.png)

![image-20230304212028882](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230304212028882.png)

```c++
//目的是算出图中的最短路（单源最短路：是指在一个图中，给你一个起点（起点固定），然后终点不是固定的，求起点到任意终点的最短路径）

struct edge{int v,w;}; //这里是 点 和 权
vector<edge> e[N];
int d[N];

//内部变量是起点
bool bellmanford(int s)
{
    //初始化边集
    memset(d,inf,sizeof d);
    d[s] = 0;
    //标记
    bool flag;
    for(int i = 1;i<=n;i++){//n轮更新
        flag = false; //标记（看有无更新）
        for(int u = 1;u<=n;u++){ //每个点枚举出边
            if(d[u] == inf) continue;
            //枚举u的领点 就行松弛操作
            for(auto ed:e[u]){ //u的出边
                int v = ed.v,w = ed.w;
                if(d[v]>d[u]+w){
                    d[v] = d[u] + w;
                    flag = true; //更新完成 就变true
                }
            }
        }
        if(!flag) break; //没有更新就退出
    }
    return flag; //第n轮 = true 那么说明就有环    
}
```

### spfa 算法 - 用bf就好了

```c++
SPFA 算法是Bellman-Ford算法的队列优化算法的别称，通常用于求含负权边的单源最短路径，以及判负权环。SPFA 最坏情况下复杂度和朴素Bellman-Ford相同，为o(VE).
//其实还不如直接用Bellman-ford
```

![image-20230304212218685](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230304212218685.png)

![image-20230304212529352](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230304212529352.png)

![image-20230304212738052](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230304212738052.png)

```c++
//堆优化 bellman 算法 - spfa算法
struct edge{int v,w;};
vector<edge> e[N];//点集合
int d[N],cnt[N],vis[N]; //边 边数 开关
queue<int> q; //队列

bool spfa(int s){
    memset(d,inf,sizeof d);
    //开始起点 1.起点到起点的距离 = 0  2.vis是开关 判断这个数据是否使用过 3.d是边集
    d[s] = 0; vis[s] = 1; q.push(s); 
    while(q.size()){
        //这里是将前面初始化的点操作的地方，每次都只操作一个点
        int u = q.front(); q.pop(); vis[u] = 0;
     for(auto ed:e[u]){
            int v = ed.v , w = ed.w;
            //比较枚举点和当前点 的长度 更新最短值
            if(d[v]>d[u]+w){
                d[v] = d[u]+w;
                cnt[v] = cnt[u]+1; //记录边数
                if(cnt[v]>=n) return true;
                if(!vis[v]) q.push(v),vis[v] = 1;
            }
        }
    }
    return false;
}
```

### Floyd算法 - 点到点的最短路

![image-20230307160057133](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230307160057133.png)

![image-20230307160205801](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230307160205801.png)

![image-20230307160839691](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230307160839691.png)

```c++
// 求图中两点的最短路（用的是动态规划） - （我感觉更像塔桥）
// 简单来说 就是两点中，构造一个中间点 k（可能有很多也不一定是中间），不断松弛两点之间的距离（刚开始距离全部点的距离都初始化为无穷）

//原始代码（可以优化） - k层一定是在外面的（这就是被称之为插入法的原因）
void floyd(){
    for(int k;k<=n;k++) //以k为桥(k枚举的是所有点)
        for(int i=1 ;i<=n;i++)
            for(int j = 1;j<=n;j++)
                d[i][j] = mid(d[i][j],d[i][k]+d[k][j]); //二维数组理解为 i - j 和 k - j就好 （k的作用是中间桥连接点 - d[i][j]的作用是存储i - j的距离）
}
```

![image-20230307161152125](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230307161152125.png)

```c++
//路径记录原理不了解
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 210;
int n,m,k;
int d[N][N];
void floyd(){
    for(int k = 1;k <= n;k++)
        for(int i = 1;i <= n;i++)
            for(int j = 1;j <= n;j++)
                d[i][j] = min(d[i][j],d[i][k]+d[k][j]);
}
int main(){
    cin >> n >> m >> k;
    fill(d[0],d[0]+N*N,0x3f3f3f3f);
    for(int i = 1;i <= n;i++) d[i][i] = 0;
    while(m--){
        int a,b,c; cin >> a >> b >> c;
        d[a][b] = min(d[a][b],c);
    }
    floyd();
    while(k--){
        int a,b; cin >> a >> b;
        if(d[a][b] > 0x3f3f3f3f/2) cout << "impossible" << endl;
        else cout << d[a][b] << endl;
    }
}
```

![image-20230307161434260](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230307161434260.png)

### 最小生成树 - prim算法

![image-20230308192754299](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230308192754299.png)

```c++
//没有优化版本 - 基于贪心算法
struct edge{int v,w;};
vector<edge> e[N];
int d[N],vis[N];//d是边的长度 

bool prim(int s){
    for(int i = 0;i<=n;i++) d[i] = inf;
    d[s] = 0;
    for(int i = 1;i<=n;i++){
        int u = 0;
        for(int j = 1;j<=n;j++)
            if(!vis[j]&&d[j]<d[u]) u = j; //排除选过的点 - 这里是选领点的（懂了）
        vis[u] = 1;
        ans+=d[u];//边权和（最小生成树的边权和）
        if(d[u]!=inf) cnt++; //判断是否联通
        //遍历到u这个点（u之前的也是一起的，算是连续的）
        for(auto ed:e[u]){
            int v = ed.v,w = ed.w;
            if(d[v]>w){
                d[v] = w;   
            }
        }
    }
    return cnt == n; //返回true就是有最小生成树的 返回false
}


```

![image-20230308110618789](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230308110618789.png)

```c++
//简单来说： 起初所有的点都是在队列中，每次取出一个点，将这个点的路径进行选择，选择路径最小的。 更新，重复知道队列中没有点即可。
int n,m,s,a,b,c,ans,cnt;
struct edge{int v,w;}; //构造一个结构体 内部有点和边的属性
vector<edge> e[N];//领点（也算是所有点？）
int d[N],vis[N];
priority_queue<pair<int,int>> q; //这里创一个优先队列（就是堆）
//s是起点
bool prim(int s){
    for(int i = 0;i<=n;i++) d[i] = inf; //先初始化全部的边 - 每个边都是无穷大
    //起点
    d[s] = 0;q.push({0,s});
    while(q.size()){
        //取出q队列的点？ - 后面有进入点？
        int u = q.top().second; q.pop();
        if(vis[u]) continue; 
        vis[u] = 1;
        ans+=d[u]; cnt++;
        //这里应该就是插入 领点的步骤
        for(auto ed:e[u]){
            int v = ed.v,w = ed.w;
            if(d[v]>w){
                d[v] = w; //对应点v边权最小的边
                q.push({-d[v],v});//大根堆
            }
        }
    }
    return cnt == n; //这里判断的是啥？
}


```

![image-20230308110724388](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230308110724388.png)

### 最小生成树 - 克鲁斯卡尔算法 - 并查集

![image-20230308125205044](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230308125205044.png)

![image-20230308125225033](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230308125225033.png)

```c++
克鲁斯卡尔算法（Kruskal）是一种使用贪婪方法的最小生成树算法。该算法初始将图视为森林，图中的每一个顶点视为一棵单独的树。一棵树只与它的邻接顶点中权值最小且不违反最小生成树属性（不构成环）的树之间建立连边。
```

```c++
#include<bits/stdc++.h>
using namespace std;
const int N=100010,M=200010;
int n,m;
//结构体存储边的信息
struct edge{
    int a,b,w;
}e[M];
//比较函数 （用来作为排序算法的参数）
bool cmp(edge x,edge y){
    return x.w<y.w;
}
//并查集
int p[N];
int find(int x){
    if(p[x]!=x) p[x]=find(p[x]);
    return p[x];
}
int main(){
    scanf("%d%d",&n,&m);
    for(int i=1;i<=m;i++) scanf("%d%d%d",&e[i].a,&e[i].b,&e[i].w);
    //排序边权（我们是用贪心来筛选的）
    sort(e+1,e+m+1,cmp);
    //初始化并查集
    for(int i=1;i<=n;i++) p[i]=i;
    int res=0,cnt=0;
    for(int i=1;i<=m;i++){
        //每一个节点初始都是祖宗节点（用边来合并我们的连通块 最终达到组成树的目的）
        int a=find(e[i].a),b=find(e[i].b),w=e[i].w;
        //查一下是否是连通块
        if(a!=b){
            //纳入后宫
            p[a]=b;
            res+=w;
            cnt++;
        }
        if(cnt==n-1) break;  //成树
    }
    printf("%d\n",res);//输出边权
}
```

### 总结

![image-20230308100226384](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230308100226384.png)

## 补充

### 染色法判断二分图*

![image-20230314143817025](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230314143817025.png)

```c++
//染色法判断二分图
首先随机选择一个未染色的顶点，将其染成红色或蓝色（或其他任意两种不同颜色）。
然后将与该顶点相邻的所有顶点染成与其不同的颜色。
重复上述过程，直到所有顶点都被染色或者发现某个顶点和它相邻的顶点已经被染成了相同的颜色。
如果所有顶点都被染色，那么这个图就是二分图；如果发现有冲突，那么这个图就不是二分图。

//二分图
二分图是图论中的一种特殊模型，它的定义是1234：如果一个无向图的顶点集可以分成两个互不相交的子集，使得每条边的两个端点分别属于这两个子集，那么这个无向图就是二分图。例如，下图就是一个二分图：
    A   B   C
    | / | / |
    D   E   F
就是映射是吧，两个不同的集合中有连边，相同集合的不连边
```

```c++
//判断此图是否是二分图
#include<bits/stdc++.h>
using namespace std;

const int N = 1e6+10,M = 2e6+10;

int h[N],e[N],ne[M],idx; //e是点集 ne是边集
int color[N]; //存储每一个点的状态 一共有三种 起始0 染色 1 - 2

void add(int a,int b){
    e[idx] = b, ne[idx] = h[a],h[a] = idx++; //把h[a]理解为前面的桶形数组就好了
}
//每次访问一个顶点u，就会先递归地访问它的所有邻接点，直到没有未访问的邻接点为止，
//然后再回溯到上一层。这样可以保证每个连通分量内的顶点都被染色。
bool dfs(int u,int c)
{
    color[u] = c; //c是什么意思 - 是当前点的染色状态 （只有三种 0 1 2 用3减去就前后不一样了）
    
    for(int i = h[u]; ~i;i = ne[i]){
        int j = e[i]; //邻点
        if(!color[j]){
            if(!dfs(j,3-c)) return false;
        //这个是递归地调用dfs函数，给顶点j和它的邻接点染色，并判断是否有冲突。
        //如果返回false，就说明发现了不符合二分图的情况，就返回false。
        //冲突是指同一个子集内的顶点颜色相同，或者不同子集内的顶点颜色不同。
        //这些情况都不满足二分图的定义，所以要返回false。
            }
        else if(color[j] == c) return false;
        //如果顶点j已经被染色，并且与u的颜色相同，就说明同一个子集内有边相连，不符合二分图的定义，就返回false。
    }
    return true;
}

int main()
{
    int n,m;
    cin>>n>>m;
    memset(h, -1, sizeof h);
    
    while(m--){
        int a,b;
        cin>>a>>b;
        add(a,b); add(b,a); //无向图
    }
    bool flag = true;
    for(int i = 1;i<=n;i++)
        if(!color[i])
        {
           if(!dfs(i,1))
           {
               flag = false;
               break;
           }
        }
    if(flag) puts("Yes");
    else puts("No");
    
    return 0;
    
    
}
```

![image-20230314144148781](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230314144148781.png)

![image-20230314144232392](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230314144232392.png)

```c++
#include <iostream>
#include <vector>
using namespace std;

const int N = 510; // 顶点数的最大值
int n1, n2; // 二分图左右两边的顶点数
vector<int> g[N]; // 邻接表存储图
int match[N]; // match[i]表示右边第i个点当前匹配的左边的点
bool st[N]; // st[i]表示右边第i个点是否已经被遍历过

// 在二分图中寻找增广路
bool find(int x) {
    for (int i = 0; i < g[x].size(); i++) {
        int j = g[x][i];
        if (!st[j]) {
            st[j] = true;
            if (match[j] == 0 || find(match[j])) {
                match[j] = x;
                return true;
            }
        }
    }
    return false;
}

// 求二分图最大匹配数
int main() {
    cin >> n1 >> n2;
    int m; // 边数
    cin >> m;
    while (m--) {
        int a, b;
        cin >> a >> b;
        g[a].push_back(b);
    }

    int res = 0; // 最大匹配数
    for (int i = 1; i <= n1; i++) {
        memset(st, false, sizeof st);
        if (find(i)) res++;
    }

    cout << res << endl;

    return 0;
}
```

### 二分图的最大匹配

#### 匈牙利算法*

![image-20230314205307317](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230314205307317.png)

```c++
1.匈牙利算法 - 二分图的最大匹配
 二分图，简单来说就是不同集合有联系；同一集合不可以有联系； - 边数最多的一组匹配被称之为最大匹配
 在二分图的前提下： 1.交替路 2.增广路  - 交替路就是匹配和不匹配边交替出现 - 增光路就是匹配和不匹配交换身份，看有没有多路径，多了就是增广路

const int N = 1e5+10,M = 2e5+10;
int n,m,k,a,b,ans,idx;
struct edge{int v,ne;}e[M]; //点（对应位置的妹子） ne 另一集合的点是吗
int h[N],idx;
int vis[N],match[M]; //标记 和 匹配

//链式前向星
void add(int a, int b){
    e[++idx] = {b,h[a]};
    h[a] = idx++; //横置数组向右移动
}
//男女匹配问题 (boy and girl）
bool bfs(int u){
    //每一个都要匹配看看
    for(int i = h[u]; i;i = e[i].ne){
        int v = e[i].v; //妹子
        if(vis[v]) continue;
        vis[v] = 1; //标记
        if(!match[v]||dfs(match[v])){ //没有匹配 || 能不能换（dfs的功能就是判断能不能换）
            match[v] = u;//成对
            return 1;
        }
    }
    return 0;
}

int main()
{
    cin>>n>>m>>k;
    for(int i = 0;i<k;i++) cin>>a>>b,add(a,b); //建图
    for(int i = 1;i<=n;i++){
        memset(vis,0,sizeof vis);
        if(dfs(i)) ans++;
    }
    cout<<ans;
    return 0; 
    
}

```

#### 染色法判断二分图

![image-20230315104701595](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230315104701595.png)

![image-20230315123714291](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230315123714291.png)

```c++
//1.二分图的定义
 两个不同的集合，互相联通 - 相同集合不可能联通
//2.染色法
    可以用dfs和bfs来实现染色法，重点是怎么实现前后颜色不同。 我们运用两个标记来判断对应节点的状态： 1 表示这个节点是红 2表示是黑 0表示没选这个（加上个判重）
   
struct edge{int v,ne;}e[M];
int h[N];
int color[N]; //颜色有三种 0 1 2

void add(int a,int b){
    e[++idx] = {b,h[a]}; //用链式前向星存储的
    h[a] = idx++;
}
//u点的颜色c 
bool bfs(int u,int c)
{
    color[u] = c;
    //枚举u的领边
    for(int i = h[u];i;e[i].ne)
    {
        int v = e[i].v;
        if(!color[v]){ //还没有被访问
            if(dfs(v,3-c)) return 1; //改变不同层的颜色
        }
        else if(color[v] == c) return 1;
    }   
    
}

int main()
{
    cin>>n>>m;
    for(int i = 0;i<m;i++){
        int a,b;
        cin>>a>>b;
        add(a,b);
        add(b,a);
    }
    bool flag = 0;
    for(int i = 1;i<=n;i++)
        if(!color[i])
            if(!dfs(i,1)){
                flag = 1;//有奇环
                break;
            }
    if(flag) puts("No");
    else puts("Yes");
    return 0;    
}
```

![image-20230314142804998](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230314142804998.png)

![image-20230314143439093](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230314143439093.png)

![image-20230314143503237](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230314143503237.png)

### [最近公共祖先](https://www.bilibili.com/video/BV1A94y12737/?spm_id_from=333.999.0.0&vd_source=731595967596af37618c926a191e7811)

#### 朴素方法

![image-20230319121246852](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230319121246852.png)

```c++
#include <iostream>
#include <vector>
using namespace std;

const int MAXN = 100005;
int n, m;
vector<int> tree[MAXN]; // 邻接表存储树
int depth[MAXN], parent[MAXN]; // 深度和父亲节点

// 深度优先搜索遍历计算深度和父亲节点
void dfs(int u, int p) {
    depth[u] = depth[p] + 1;
    parent[u] = p;
    for (int v : tree[u]) {
        if (v != p) dfs(v, u);
    }
}

// 计算两个节点的最近公共祖先
int lca(int u, int v) {
    while (depth[u] > depth[v]) u = parent[u];
    while (depth[v] > depth[u]) v = parent[v];
    while (u != v) {
        u = parent[u];
        v = parent[v];
    }
    return u;
}

int main() {
    cin >> n >> m; // 读入节点数和查询数量
    for (int i = 1; i < n; i++) {
        int u, v;
        cin >> u >> v; // 读入边
        tree[u].push_back(v);
        tree[v].push_back(u);
    }
    
    dfs(1, 0); // 计算深度和父亲节点
    
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v; // 读入查询
        cout << lca(u, v) << endl; // 输出结果
    }
    
    return 0;
}
这个模板中，n 是树的节点数，m 是查询数量。对于每组查询，它读入两个节点编号 u 和 v，然后调用函数 lca(u,v) 来获取它们的最近公共祖先。
```

#### Tarjan算法

![image-20230314144355261](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230314144355261.png)

```c++
//Tarjan算法是由Robert Tarjan发明的一种图算法。它可以用于解决许多不同类型的问题，包括寻找强连通分量、双连通分量、割点和割边等。其中一种应用是在树中查找节点对的最近公共祖先（LCA）。Tarjan算法通过深度优先搜索和并查集数据结构来高效地解决这个问题。

//tarjan算法 ： 利用并查集
vector<int> e[N];
vector<pair<int,int>> query[N];
int fa[N],vis[N],ans[M];
//并查集
int find(int u)
{
    if(u == fa[u]) return u;
    return fa[u] = find(fa[u]);    
}
void tarjan(int u){
    fa[u] = u; // 初始化父亲为自己
    vis[u] = true; // 标记
    for(auto v:e[u])
    {
        if(!vis[v]){
            tarjan[v];
            fa[v] = u;
        }
    }
    for(auto q:query[u]){
        int v = q.first,i = q.second;
        if(vis[v]) ans[i] = find(v);
    }
}
```

#### 树链剖分(不理解)

![image-20230315210121556](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230315210121556.png)

![image-20230315210610462](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230315210610462.png)

```c++
vector<int> e[N];
int fa[N],dep[N],son[N],sz[N];
int top[N];

void dfs1(int u,int father){
    fa[u] = father, dep[u] = dep[father]+1,sz[u] = 1;
    for(int v:e[u]){
        if(v==father) continue;
        dfs1(v,u);
        sz[u] += sz[v];
        if(sz[son[u]]<sz[v]) son[u] = v;
    }
}

void dfs2(int u,int t)
{
    top[u] = t;
    if(!son[u]) return;
    dfs2(son[u],t);
    for(int v:e[u]){
        if(v == fa[u] || v == son[u]) continue;
        dfs2(v,v);
    }
}

int lca(int u,int v){
    while(top[u]!=top[v]){
        if(dep[top[u]]<dep[top[v]]) swap(u,v);
        u = fa[top[u]];
    }
    return dep[u]<dep[v]?u:v;
}
```

#### 倍增算法（不理解）

![image-20230315210919875](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230315210919875.png)

![image-20230315210938761](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230315210938761.png)

![image-20230315210958755](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230315210958755.png)

```c++

```

#### 总

![image-20230315210622550](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230315210622550.png)

## 补充2

### 线段树*

![image-20230401100823894](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230401100823894.png)

![image-20230401100840271](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230401100840271.png)

![image-20230401100850209](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230401100850209.png)

![image-20230401100859800](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230401100859800.png)

![image-20230401100913730](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230401100913730.png)

### 树状数组*
