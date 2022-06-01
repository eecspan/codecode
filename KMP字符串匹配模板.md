## 题目描述：

[洛谷KMP字符串匹配模板题](https://www.luogu.com.cn/problem/P3375)



## 思路：

​		思路可以看[知乎讲解](https://www.zhihu.com/question/21923021/answer/1032665486)。

​		应该是还有优化版的，但是下面的代码并不是。

​		代码如下：

```c++
#include<bits/stdc++.h>

using namespace std;


void buildNext(string& substr, vector<int>& next)
{
    next[0] = 0;    // next[0]必然为0
    int now = 0, i = 1;
    while (i < substr.size())
    {
        // 已经知道了next[i-1]  要求next[i]
        // 如果substr[now]就是求next[i-1]时，第一个不在匹配前缀里的字符
        // 例如，如果next[i-1] = 5，说明[0:i-1]这一个子串中，前面5个字符和后面5个字符相同，此时now就是5
        if (substr[i] == substr[now])
        {
            now += 1;
            next[i] = now;
            i += 1;
        }
        // 如果substr[now]和substr[i]不相同，那么next[i]会在next[i-1]的基础上缩短
        else if (now > 0)
        {
            // 如果next[i-1] = 5，说明[0:i-1]这一个子串中，前面5个字符和后面5个字符相同，此时now就是5
            // now-1 = 4，此时next[4]就是[0:i-1]这一个子串前面五个字符，即[0:4]这个子串的前缀长度，
            // 假设是2，那么下一个比对的就是substr[2]与substr[i]
            now = next[now-1];
        }
        else 
        {
            // 如果now = 0，说明没有任何前缀相同
            next[i] = 0;
            i += 1;
        }
    }
}


int main()
{
    string instr, substr;
    cin>>instr>>substr;
    vector<int> next(substr.size(), 0);  // next数组
    buildNext(substr, next);
    // 建立next数组之后，开始搜索

    int tar = 0, pos = 0;  // tar是大字符串中的下标，pos是匹配字符串中的下标
    while (tar < instr.size())
    {
        if (instr[tar] == substr[pos])
        {
            tar += 1;
            pos += 1;
        }
        else if (pos > 0)  
        {
            // 如果instr[tar]和substr[pos]不一样
            // pos要根据next数组的内容向前移动
            pos = next[pos-1];
        }
        else
        {
            // 如果pos已经变成了0   同时instr[tar]与substr[0]不匹配
            tar += 1;
        }

        if (pos == substr.size())  // 如果已经匹配成功了
        {
            cout<<tar-pos+1<<endl;
            pos = next[pos-1];     // pos根据next数组的内容向前移动
        }
    }

    for (int i = 0; i < next.size(); ++i)
        cout<<next[i]<<" ";

    return 0;
}
```
