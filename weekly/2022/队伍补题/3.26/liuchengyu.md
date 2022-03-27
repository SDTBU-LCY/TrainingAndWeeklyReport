# 补题报告

## 牛牛吃米粒(https://ac.nowcoder.com/acm/contest/11179/A)

## 思路

可以发现这题就是去年编程赛H题的出处，相比编程赛的H题数据被加强了。很容易想到思路是看s这个数的二进制位上是1的位所对应的格子是不是超出了格子的数目或者对应格子上的米被拿走了，只要有一个位不满足条件，答案就是NO，如果都满足答案就是YES。注意这题s的范围，1<=s<2^64，s用long long都存不下，得用unsigned long long，我刚开始用的long long，结果不是答案错误居然是运行错误？可能是对s进行位运算的缘故吧。

## 代码

```cpp
#include <iostream>
using namespace std;
#define N 105
int dit[N];
int main()
{
    int n,m;
    unsigned long long k;
    cin >> n >> m >> k;
    for(int i=0;i<n;i++) dit[i]=1;
    while(m--)
    {
        int pos;
        cin >> pos;
        dit[pos-1]=0;
    }
    int flag=1;
    for(int i=0;i<64;i++)
    {
        if(((k>>i)&1)&&!dit[i])
        {
            flag=0;
            break;
        }
    }
    if(flag) cout << "YES" << endl;
    else cout << "NO" << endl;
    return 0;
}
```

## 牛牛嚯可乐(https://ac.nowcoder.com/acm/contest/11179/B)

## 思路

这题问你最小交换次数，但字符串中的字符并不是唯一的，如果是唯一的话可以考虑用寻找循环节的方法求解。既然不是唯一的就要另寻他法，会发现这题字符串的长度只有8，最近一直在刷往年蓝桥杯题目的我很直接地想到了深搜。考虑按位搜索，下标从0开始，假设当前位为pos，那么前面的0~pos-1位都已排好序。深搜终止的条件是当pos等于8时，代表所有位均已排好序，如果该种排序所需的交换次数小于答案，就更新答案，然后返回。否则，分两种情况考虑，如果当前位上的字符与排好序的字符串所对应位上的字符相同，则不需要交换，继续深搜下一个位置。否则，从下一位开始一直到最后一个字符，依次判断那一位上的字符是否和排好序的字符串当前位上的字符相同，如果相同，则进行一次交换，交换次数加1，继续深搜下一个位置。这样深搜完毕后，得到的答案就是最优解，利用了回溯的思想。

## 代码

```cpp
#include <iostream>
using namespace std;
int ans=0x7fffffff;
string s,res="cocacola";
void dfs(int pos,int step)
{
    if(pos==8)
    {
        ans=min(ans,step);
        return ;
    }
    if(s[pos]==res[pos])
        dfs(pos+1,step);
    else
    {
        for(int i=pos+1;i<8;i++)
        {
            if(s[i]==res[pos])
            {
                swap(s[i],s[pos]);
                dfs(pos+1,step+1);
                swap(s[i],s[pos]);
            }
        }
    }
}
int main()
{
    cin >> s;
    dfs(0,0);
    cout << ans << endl;
    return 0;
}
```

## 牛牛吃豆人(https://ac.nowcoder.com/acm/contest/11179/C)

## 思路

这题很像那道传纸条的dp题，都是找两条起点、终点都相同但是中间没有重合的路径，但是这题的数据范围太大了，开数组空间会爆，而且这题根本就不涉及到最优解，这题问能否实现目的，所以这题用的就不是dp的思路。再观察题目会发现，这题的图只有3列，所以我很自然地想到了考虑每个人从哪一行向右走一步。经过进一步的观察可以发现，可以用贪心的思想考虑这题能否实现目的，令第一个人从起点开始沿着第一列一直向下走，直到不能走为止，记录下这个横坐标，令第二个人从终点开始沿着第三列一直向上走，直到不能走为止，同样也记录下这个横坐标。然后比较这两个横坐标的大小，如果第一个人的横坐标大于第二个人的横坐标，则代表路径没有交叉，当前一步可行，否者代表无论如何路径都会有交叉，输出"NO"。在第一步可行的情况下，再看第二列从第一个人记录的横坐标开始一直到最后是否都能走以及第二列从头开始一直到第二个人记录的横坐标是否都能走，如果都能走，输出"YES"，否则输出"NO"，另外可以用前缀和来简化判断。

## 代码

```cpp
#include <iostream>
using namespace std;
#define N 1000005
int sum[N];
int main()
{
    int n,m;
    cin >> n >> m;
    for(int i=1;i<=n;i++) sum[i]=1;
    int Min=n+1,Max=0;
    while(m--)
    {
        int y,x;
        cin >> y >> x;
        if(y==1) Min=min(Min,x);
        if(y==3) Max=max(Max,x);
        if(y==2) sum[x]=0;
    }
    for(int i=1;i<=n;i++) sum[i]+=sum[i-1];
    Min--,Max++;
    if(Min-Max>=1&&sum[Max]==Max&&sum[n]-sum[Min-1]==n-Min+1) cout << "YES" << endl;
    else cout << "NO" << endl;
    return 0;
}
```
