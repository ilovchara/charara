---
title: six
date: 2023-04-28 09:09:57
tags: 算法
---

## 动态规划

```c++
1.背包问题
2.线性dp
3.各种dp
```

## 背包问题

### 01背包问题

![image-20230319202209832](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230319202209832.png)

![image-20230319214127252](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230319214127252.png)

```c++
 //动态规划重点的是状态转移方程
//01背包，代表着物品只有两种可能性 - 放和不放
 f[i][j] 中的前i件物品放入容量为j的背包中。我们很容易可以看出，第一个维度我们存储的是物品的不同种类，第二个维度存储的是我们的背包容量。
 更新容量我们就使用：max(f[i-1][j-w[i]]+c[i],f[i-1][j])
//两种情况，我们分别判断一下
     第一种：我们放入我们的i物品，那么f[i][j] = f[i-1][j-w[i]]+c[i],就是这样子，我们从两个维度解释一下： 第一个维度f[i-1]:很简单就是我们已经选择了i物品了，那么从全部物品中，就少了我们的i，因此就是i-1. 第二个维度就是f[j-w[i]],就是我们既然选择了i，那么就把我们的背包中直接塞入我们的物品，这个物品就会占据位置嗯。
     第二种：不放我们的i物品，就是f[i][j] = f[i-1][j],不放物品我们的j这容量肯定就不会变化，同时，我们不放这个物品的话，就只能从别的物品选择，那就是只剩下i-1种物品了。
```

![image-20230319202536339](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230319202536339.png)

```c++
//模版思想研究
//用二维数组来理解
 for(int i = 1;i<=n;i++)
        for(int j = 1;j<=m;j++)
            if(j<w[i]) //先决条件：这个位置放不了i这个物品了
                f[i][j] = f[i-1][j];
   else
                f[i][j] = max(f[i-1][j],f[i-1][j-w[i]]+c[i]); 
 cout<<f[n][m];
//用滚动数组来理解
 for(int i = 1;i<=n;i++)
       for(int j = 1;j<=n;j++)
           if(j<w[i])
               f[j] = f[j]; //容量没变化 - 就是没放入我们的i
     else
               f[j] = max(f[j],f[j-w[i]]+c[i]); //c[i]存储的是我们装入这个物品带来的价值收入
```

![image-20230319202555236](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230319202555236.png)

```c++
//用j逆序排序
 for(int i = 1;i<=n;i++)
        for(int j = m;j>=1;j--)
            if(j<w[i])
                f[j] = f[j];
   else f[j] = max(f[j],f[j-w[i]]+c[i]);
```

![image-20230319202608455](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230319202608455.png)

```c++
//模版
#include <iostream>
#include <cstring>
using namespace std;

const int MAXN = 1005;

int N, V; // N表示物品个数，V表示背包容量
int w[MAXN], v[MAXN]; // w[i]表示第i个物品的重量，v[i]表示第i个物品的价值
int dp[MAXN]; // dp[i]表示容量为i的背包所能装下的最大价值

int main() {
    cin >> N >> V;
    for (int i = 1; i <= N; i++) {
        cin >> w[i] >> v[i];
    }

    memset(dp, 0, sizeof(dp)); // 初始化dp数组
    for (int i = 1; i <= N; i++) {
        for (int j = V; j >= w[i]; j--) { // 从后往前遍历，避免重复选择物品
            dp[j] = max(dp[j], dp[j - w[i]] + v[i]); // 状态转移方程
        }
    }

    cout << dp[V] << endl; // 输出结果

    return 0;
}
```

![image-20230320144822881](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320144822881.png)

![image-20230320144830870](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320144830870.png)

![image-20230320144922085](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320144922085.png)

### 完全背包问题

![image-20230320175854308](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320175854308.png)

![image-20230320175908237](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320175908237.png)

```c++
//完全背包问题  - 就是01背包的变形，物品可以无限次放入
 f[i][j]依然和01背包问题相同的，i表示的是我们物品的种类，j表示的是我们背包的容量。
    放入物品的时候，我们需要选择放入这个物品我们的价值是否会增加
//过程
     1.背包容量放不下我们当前这个物品 条件就是j<w[i] 所以说我们的容量还是不变
        f[i][j] = f[i-1][j]; //这也就解释了我们可以使用一维数组来维护我们的背包 - 这里的i-1表示的是前一个物品，就是我们不会改变当前的背包状态
 2.背包可以放入我们当前这个物品 条件也就是j>=w[i]
        我们有两种选择 - 放入和不放入  
        //放入的话就是
        f[i][j] = f[i][j-w[i]]+c[i] //c[i]是我们的价值(这里的i的意思就是我们物品是无穷的，每一个i表示的意思就是取出当前i这个物品的其中一个)
        //不放入就是
        f[i][j] = f[i-1][j] //j就不会变化
     3.我们最终的转移方程就是 上面两个合并的结果
        f[i][j] = f[i-1][j] (j<w[i])
        f[i][j] = max(f[i-1][j],f[i][j-w[i]]+c[i])
        //重点就是理解，变量其实是我们选不选择这个物品！！！！ 
```

![image-20230320175946146](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320175946146.png)

![image-20230320175955400](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320175955400.png)

![image-20230320180004395](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320180004395.png)

![image-20230320180015757](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320180015757.png)

![image-20230320180027068](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320180027068.png)

```c++
//遍历是怎么遍历的
1.我们每次遍历的是当前背包塞满了物品i最终的结果
 看上面的图都是这样子的。 
//代码
    for(int i = 1;i<=n;i++){
        for(int j = 1;j<=m;j++)
            if(j<w[i])
                f[i][j] = f[i-1][j]//原状
            else
                f[i][j] = max(f[i-1][j],f[i][j-w[i]]+c[i]);//新状态
    }
 cout<<f[n][m];
2.为什么一维数组适合
    //使用动态规划算法解决完全背包问题时，可以采用一维数组进行实现。这是因为完全背包问题具有“无后效性”和“最优子结构”这两个特点。
    //无后效性指的是，当我们决定选择一个物品放入背包时，我们不需要再考虑该物品是否被选择过或未来是否还会选择该物品。也就是说，每个阶段的最优状态可以通过之前阶段的某个状态推导得到，而不受后续状态的影响。
    //最优子结构指的是，每个阶段的最优状态可以由前一个阶段的最优状态推导得到，并且每个阶段的最优状态之间没有相互制约的关系，即它们是相互独立的。

3.一维数组实现 // 不太理解
    for(int i = 1;i<=n;i++)
        for(int j = 1;j<=m;j++)
            if(j<w[i]) f[j] = f[j];
     else f[j] = max(f[j],f[j-w[i]]+c[i]);

```

![image-20230320180039797](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320180039797.png)

![image-20230320180103129](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320180103129.png)

```c++
#include <iostream>
#include <vector>
using namespace std;

// 完全背包问题的动态规划解法
int knapsack(int W, vector<int>& wt, vector<int>& val) {
    int n = wt.size();
    vector<int> dp(W + 1, 0);

    for (int i = 1; i <= n; i++) {
        for (int j = wt[i-1]; j <= W; j++) {
            dp[j] = max(dp[j], dp[j - wt[i-1]] + val[i-1]);
        }
    }

    return dp[W];
}

// 测试
int main() {
    vector<int> wt = {2, 3, 4, 5};
    vector<int> val = {3, 4, 5, 6};
    int W = 8;
    cout << "最大价值为：" << knapsack(W, wt, val) << endl;
    return 0;
}
```

![image-20230320182532855](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320182532855.png)

### 多重背包问题

![image-20230320145257361](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320145257361.png)

![image-20230320145415756](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320145415756.png)

```c++
//在01背包的基础上，让每一件物品都有他们的数量
 1.//v[i],w[i] 体积和价值
 for(int i = 1;i<=n;i++) //物品种类
        for(int j = m; j>=v[i];j--) //背包容量
            f[j] = max(f[j],f[j-v[i]]+w[i]);
    2.//v[i],w[i],s[i]; 体积 价值 数量
        for(int i = 1;i<=n;i++)
            for(int j = m;j>=v[i];j--)
                for(int k = 0;k<=s[i]&&k*v[i]<=j;k++) 
                    //这里的k表示的是选择物品的数量 - s[i]表示的是当前位置的i物品的数量
                    f[j] = max(f[j],f[j-k*v[i]]+k*w[i]);
```

```c++
//朴素做法
const int N = 1e5+10;
int v[N],w[N],s[N]; //体积 价值 数量
int f[N][N]; //这个是我们的状态转移数组

int main()
{
    cin>>n>>m;
    
    for( int i = 1;i<=n;i++) cin>>v[i]>>w[i]>>s[i]; //这里就是输入对应的价值，由i下标规定数据
    for(int i = 1;j<=n;j++)
        for(int j = 0;j<=m;j++)
            for(int k = 0;k<=s[i]&&k*v[i]<=j;k++)
                f[i][j] = max(f[i][j],f[i-1][j-v[i]*k]+w[i]*k); //这里i-1是把这个i物品选择了（选够了就剔除） - 只要找到一个符合的k就行（我们不管怎么实现的就行）
    cout<<f[n][m]<<endl;
    return 0;
    
}
```

![image-20230320145513754](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320145513754.png)

![image-20230320145617888](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320145617888.png)

```c++
//二进制的转化就是 - 我们不再是一个一个数了，我们现在是两个两个数的
int num = 1;//拆分计数
for(int i = 1;i<=n;i++){
    //v,w,s; 体积，价值，数量
    cin>>v>>w>>s;
    for(int j = 1;j<=s;j<<=1){ //<<=是左移位赋值运算符，它将左操作数向左移动右操作数指定的位数，并将结果赋值给左操作数
        vv[num] = j*v; //存体积
        ww[num++] = j*w; //存价值
        s-=j;
    }
    if(s){
        vv[num] = s*v;
        ww[num++] = s*w;
    }
}

//01背包问题 - 基础
 for(i = 1;i<num;i++)
        for(j = m;j>=vv[i];j--)
            f[j] = max(f[j],f[j-vv[i]]+ww[i]);
cout<<f[m];

```

```c++
#include <iostream>
#include <cstring>
using namespace std;

const int MAXN = 1005; // 物品的最大数量
const int MAXV = 100005; // 背包的最大容量
int n, V; // 物品的数量和背包的容量
int w[MAXN], v[MAXN], s[MAXN]; // 分别表示物品的重量、价值和数量
int dp[MAXV]; // dp[i]表示容量为i的情况下，可以获得的最大价值

// 二进制优化
void binary_optimization(int x, int y) {
    for (int j = V; j >= x; j--) {
        for (int k = 0; k <= s[y] && k * x <= j; k++) {
            dp[j] = max(dp[j], dp[j - k * x] + k * y);
        }
    }
}

int main() {
    cin >> n >> V;
    for (int i = 1; i <= n; i++) {
        cin >> w[i] >> v[i] >> s[i];
    }

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= s[i]; j *= 2) {
            binary_optimization(w[i] * j, v[i] * j); // 调用二进制优化函数
            s[i] -= j;
        }
        if (s[i] > 0) {
            binary_optimization(w[i] * s[i], v[i] * s[i]);
        }
    }

    cout << dp[V] << endl; // 输出答案
    return 0;
}
```

```c++
//二进制优化
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 12010, M = 2010;

int n, m;
int v[N], w[N];
int f[M];

int main()
{
    cin >> n >> m;

    int cnt = 0;
    for (int i = 1; i <= n; i ++ )
    {
        int a, b, s;
        cin >> a >> b >> s;
        int k = 1;
        while (k <= s)
        {
            cnt ++ ;
            //这里是二进制优化的核心 - 就是将s拆成2进制
            //核心是将物品拆分为2的幂次方个物品
            //每次都拿s拆开二进制的数量
            v[cnt] = a * k;
            w[cnt] = b * k;
            s -= k;
            k *= 2;
        }
        if (s > 0)
        {
            cnt ++ ;
            v[cnt] = a * s;
            w[cnt] = b * s;
        }
    }

    n = cnt;

    for (int i = 1; i <= n; i ++ )
        for (int j = m; j >= v[i]; j -- )
            f[j] = max(f[j], f[j - v[i]] + w[i]);

    cout << f[m] << endl;

    return 0;
}

```

![image-20230321174757054](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230321174757054.png)

### 分组背包问题

![image-20230321181451351](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230321181451351.png)

![image-20230321181510913](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230321181510913.png)

```c++
//朴素做法
for(int i = 1;i<=n;i++)//物品 - 组？ 我还是不理解
    for(int j = 1;j<=V;j++)//体积
        for(int k = 0;k<=s[i];k++) { //决策
            if(j>=v[i][k])
                f[i][j] = max(f[i][j],f[i-1][j-v[i][k]]+w[i][k]);
                //不选取i  选取i
        }
cout<<f[n][V];
//为什么w[i][j] - 这里 i是组号 j是组内编号（组这个概念是啥）
//V这个是背包体积吗 - 啥意思
```

![image-20230321181526186](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230321181526186.png)

![image-20230321181540822](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230321181540822.png)

```c++
//分组朴素做法
for(int i = 1;i<=n;i++)
 for(int j = 1;j<=V;j++)
  for(int k = 0;k<=s[i];k++)
  {
   if(j>=v[i][k])
    f[i][j] = max(f[i][j],f[i-1][j-v[i][k]]+w[i][k]);
  }
cout<<f[n][V];
```

![image-20230321181555557](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230321181555557.png)

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 12010, M = 2010;

int n, m;
int v[N], w[N];
int f[M];

int main()
{
    cin >> n >> m;

    //1.这段程序是将一个数量为n的物品分成若干组，每组物品有一个共同的体积上限，要求选出的物品不能超过各自的体积上限，且所有选出的物品的总体积不能超过背包容量。每组物品中的物品可以有不同的体积和价值。在这段程序中，将每个物品的数量s分成二进制，然后将每个物品拆分成若干个体积和价值相同的物品，这样就构造了一个二进制数组。最后将物品的数量n更新为拆分后的物品数量cnt。
    int cnt = 0;
    for (int i = 1; i <= n; i ++ )
    {
        int a, b, s;
        cin >> a >> b >> s;
        int k = 1;
        //这里是将s数量分二进制
        while (k <= s)
        {
            cnt ++ ;
            v[cnt] = a * k;
            w[cnt] = b * k;
            s -= k;
            k *= 2;
        }
        //这里就构造二进制数组
        if (s > 0)
        {
            cnt ++ ;
            v[cnt] = a * s;
            w[cnt] = b * s;
        }
    }

    n = cnt;

    //2.这里就是选择最大
    for (int i = 1; i <= n; i ++ )
        for (int j = m; j >= v[i]; j -- )
            f[j] = max(f[j], f[j - v[i]] + w[i]);

    cout << f[m] << endl;

    return 0;
}
```

### 混合背包问题

![image-20230320145732542](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320145732542.png)

![image-20230320145752872](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320145752872.png)

```c++

```

![image-20230320145806496](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320145806496.png)

``` c++
//三个背包综合考虑
```

## 线性dp

### 大盗阿福

![image-20230320151317664](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320151317664.png)

![image-20230320151339739](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320151339739.png)

```c++
//线性dp
 在这个题目中，f数组存储的是我们偷取的物品价值是吧，然后我们要选择偷取价值最大化，每一个下标都代表着一个店铺是这个样子？
//写
 scanf("%d",&t);
 while(t--)
    {
        scanf("%d",&n);
        for(int i = 1;i<=n;i++)
            scanf("%d",&w[i]);
        
        f[0] = 0;
        f[1] = w[1];
        
        //用下标代表选择 - 是这个样子的
        //用数组存储价值
        for(int i = 2;i<=n;i++){
            f[i] = max(f[i-1],f[i-2]+w[i]);
        }
        printf("%d\n",f[n]);
    }
 
```

![image-20230320151353844](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320151353844.png)

```c++
//用二维数组构造
 scanf("%d",&t);
 while(t--)
    {
        scanf("%d",&n);
        for(int i = 1;i<=n;i++)
            scanf("%d",&w[i]);
        //用第二维的数据表示选择 0不选 1选择
        f[1][0] = 0;f[1][1] = w[1];
        for(int i = 2;i<=n;i++){
            //不选择i
            f[1][0] = max(f[i-1][0],f[i-1][1]);
            //选择了i这个物品
            f[i][1] = f[i-1][0]+w[i];
        }
        printf("%d",max(f[n][0],f[n][1]));
        
    }
```

![image-20230320151402877](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320151402877.png)

### 数字三角形

```c++
//数字三角形
#include<iostream>
using namespace std;

const int N = 510,INF = 0x3f3f3f3f;
int g[N][N],f[N][N];

int main()
{
    int n;
    cin>>n;
    for(int i = 1;i<=n;i++)
        for(int j = 1;j<=i;j++)
        cin>>g[i][j];
    
    for(int i = 1;i<=n;i++)
        for(int j = 0;j<=i+1;j++)
        f[i][j] = -INF;//j从0开始是为了确定子树的选择
    
    f[1][1] = g[1][1];
    for(int i = 2;i<=n;i++)
        for(int j = 1;j<=i;j++)
        f[i][j] = g[i][j] + max(f[i-1][j-1],f[i-1][j]);
    
    int res = -INF;
    for(int i = 1;i<=n;i++) res = max(res,f[n][i]);//第n行 选择i的结果
    cout<<res<<endl;

}
```

### 最长上升子序列 or 二分优化

```c++
//最长上升子序列
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int n;
int a[N], f[N];

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);

    for (int i = 1; i <= n; i ++ )
    {
        f[i] = 1; // 只有a[i]一个数
        for (int j = 1; j < i; j ++ )
        //前小于后 严格单调
            if (a[j] < a[i])
                f[i] = max(f[i], f[j] + 1);//连续走 or 跳着走
    }

    int res = 0;
    for (int i = 1; i <= n; i ++ ) res = max(res, f[i]);

    printf("%d\n", res);

    return 0;
}
```

```c++
//二分优化
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int n;
int a[N];
int q[N];

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &a[i]);

    int len = 0;
    for (int i = 0; i < n; i ++ )
    {
        int l = 0, r = len;
        while (l < r)
        {
            int mid = l + r + 1 >> 1;
            if (q[mid] < a[i]) l = mid;
            else r = mid - 1;
        }
        //二分找目标值 更新len
        len = max(len, l + 1);
        q[r + 1] = a[i];
    }

    printf("%d\n", len);

    return 0;
}
```

### 求公共子序列

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
char a[N], b[N];
int f[N][N];//一维指的是a前n个数据 - 二位指的是b前n个数据？

int main()
{
    scanf("%d%d", &n, &m);
    scanf("%s%s", a + 1, b + 1);

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            //f[i-1][j] 表示删除a的第i个字符后与b的前j个字符的最长公共子序列长度；
     //f[i][j-1] 表示删除b的第j个字符后与a的前i个字符的最长公共子序列长度；
            f[i][j] = max(f[i - 1][j], f[i][j - 1]);
            if (a[i] == b[j]) f[i][j] = max(f[i][j], f[i - 1][j - 1] + 1);//符合就选择两者变化的最大值
        }

    printf("%d\n", f[n][m]);

    return 0;
}
```

![image-20230326134216185](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230326134216185.png)

### 最短编辑距离

![image-20230326135739598](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230326135739598.png)

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
char a[N], b[N];
int f[N][N];

int main()
{
    scanf("%d%s", &n, a + 1);
    scanf("%d%s", &m, b + 1);

    //初始化竖和
    for (int i = 0; i <= m; i ++ ) f[0][i] = i;
    for (int i = 0; i <= n; i ++ ) f[i][0] = i;

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            f[i][j] = min(f[i - 1][j] + 1, f[i][j - 1] + 1);
            if (a[i] == b[j]) f[i][j] = min(f[i][j], f[i - 1][j - 1]);
            else f[i][j] = min(f[i][j], f[i - 1][j - 1] + 1);
        }

    printf("%d\n", f[n][m]);

    return 0;
}
```

## 区间dp

![image-20230320151438854](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320151438854.png)

![image-20230320151457211](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320151457211.png)

![image-20230320151516636](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320151516636.png)

```c++
/
```

![image-20230320151549135](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320151549135.png)

## 数位统计dp

![image-20230320153227900](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320153227900.png)

![image-20230320153241076](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320153241076.png)

![image-20230320153256785](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320153256785.png)

![image-20230320153310081](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320153310081.png)

## 状态dp

```c++
//状态压缩
 通过将每一种状态映射成一个整数 - 二进制数据。这样每一个数据都是一种状态，然后我们对状态处理出我们想要的结果即可
//具体
     一般一个int整形是4个字节，也就是32位bit，我们通过这32位bit上0和1的组合可以表示多大21亿个不同的数。如果我们把这32位bit看成是一个集合，那么每一个数都应该对应集合的一种状态，并且每个数的状态都是不同的。(组合个数大)
//细节
     整数的二进制表示可以代表一个二元集合的状态，既然是状态就可以转移。在此基础上，我们可以得出另一个非常重要的结论——我们可以用整数的加减表示状态之间的转移。   
     总结一下，我们用二进制的0和1表示一个二元集合的状态。可以简单认为某个物品存在或者不存在的状态。由于二进制的0和1可以转化成一个int整数，也就是说我们用整数代表了一个集合的状态。这样一来，我们可以用整数的加减计算来代表集合状态的变化。   
```

### 蒙德里安的幻想

![image-20230327164339347](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230327164339347.png)

![image-20230327164353684](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230327164353684.png)

![image-20230327164407998](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230327164407998.png)

```c++
//用二进制表示不同的状态
  //预处理：判断合并列的状态i是否合法（这里的i应该用二进制表示是吧）
  //如果合并列的某行是1表示横放，是0表示竖放(用二进制表示块摆放状态)
  //如果合并列不存在连续的奇数个0，就是合法状态
  for(int i = 0; i < 1<<n;i++){
            st[i] = true;
            int cnt = 0;//记录合并列的连续0个数
            for(int j = 0;j<=n;j++)
                if(i>>j & 1)//判断二进制i中1的个数
                 if(cnt&1)//如果连续的0的个数是奇数
                    {
                        st[i] = false; //记录i不合法
                        break;
                    }else cnt++; //记录0的个数
            
            if(cnt&1) st[i] = false;//处理高位0的个数            
        }
```

![image-20230327164424745](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230327164424745.png)

![image-20230327164500795](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230327164500795.png)

![image-20230327164516348](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230327164516348.png)

```c++
//模版

```

```c++
//代码
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 12, M = 1 << N;

int n, m;
long long f[N][M];
bool st[M];

int main()
{
    while (cin >> n >> m, n || m)
    {
        for (int i = 0; i < 1 << n; i ++ )
        {
            int cnt = 0;
            st[i] = true;
            for (int j = 0; j < n; j ++ )
                if (i >> j & 1)
                {
                    if (cnt & 1) st[i] = false;
                    cnt = 0;
                }
                else cnt ++ ;
            if (cnt & 1) st[i] = false;
        }

        memset(f, 0, sizeof f);
        f[0][0] = 1;
        for (int i = 1; i <= m; i ++ )
            for (int j = 0; j < 1 << n; j ++ )
                for (int k = 0; k < 1 << n; k ++ )
                    if ((j & k) == 0 && st[j | k])
                        f[i][j] += f[i - 1][k];

        cout << f[m][0] << endl;
    }
    return 0;
}
```

## 树型dp

### 树的中心

![image-20230320153539710](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320153539710.png)

![image-20230320153602922](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320153602922.png)

![image-20230320153621674](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320153621674.png)

![image-20230320153639330](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320153639330.png)

![image-20230320153645886](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320153645886.png)

## 记忆化搜索

### 树塔

![image-20230320153726163](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320153726163.png)

![image-20230320153741418](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320153741418.png)

![image-20230320153805063](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230320153805063.png)
