---

date: 2022-02-25T17:45:00+08:00
---

[BJTU-2021届新生寒训 STL专题 - Virtual Judge (vjudge.net)](https://vjudge.net/contest/481856)

题源 Codeforces

## A - Three Indices

CF：[Problem - A - Codeforces](http://codeforces.com/contest/1380/problem/A)

洛谷：[CF1380A Three Indices - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/CF1380A)

### 题目

给定一个 1~n 的排列 $p_1 ... p_n$，试找到三个下标 $i, j, k$ 使得：

- $i < j < k$
- $p_i < p_j$ 且 $p_j > p_k$

### 思路

假设存在 $i, j, k$，如果 $p_j$ 与 $p_i$ 不相邻，考虑 $p_{j-1}$：

- 若 $p_{j-1} < p_j$ 那么 $p_{j-1}$ 其实可以代替 $p_i$，存在了 $j-1, j, k$ 这三个下标满足题意（1）
- 若 $p_{j-1} > p_j$ 那么其实就存在了 $i, j-1, j$ 这三个下标满足题意（2）

  如果令这三个下标为新的 $i, j, k$ 再考虑新的 $p_{j-1}$，则要么满足（1）要么一路迭代这样的操作直到 $i = j-1$ 同样也会达到一个 $j-1, j, k$ 满足题意。

对于 $p_k$ 同理，那么可以得到题意等价于存在一个下标 $j$ 使得 $p_{j-1} < p_j < p_{j+1}$ 。

---

扫一遍 $O(N)$。

```c++
#include <cstdio>
#include <iostream>

using namespace std;

int main() {
    int t; cin >> t;

    // freopen("A.out", "w", stdout);
    int a, b, c, n;
    bool flag;
    while (t--) {
        flag = false;
        a = b = c = 0;
        cin >> n;
        for (int i = 1; i <= n; i++) {
            a = b; b = c; cin >> c;
            if (flag) continue;
            if (a && a<b && b>c) {
                cout << "YES" << endl;
                cout << i - 2 << ' ' << i - 1 << ' ' << i << endl;
                flag = true;
            }
        }
        if (!flag) cout << "NO" << endl;

    }

    return 0;
}

```

## B - Infinity Gauntlet

CF：[Problem - A - Codeforces](http://codeforces.com/contest/987/problem/A)

洛谷：[CF987A Infinity Gauntlet - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/CF987A)

### 题目

输入有的宝石的颜色，输出对应缺少的宝石名。

### 思路

map + set。

```c++
#include <iostream>
#include <set>
#include <map>

using namespace std;

map<string, string> m;
set<string> s;
int main() {
    // freopen("B.out", "w", stdout);
    m["purple"] = "Power";
    m["green"] = "Time";
    m["blue"] = "Space";
    m["orange"] = "Soul";
    m["red"] = "Reality";
    m["yellow"] = "Mind";
  
    for (map<string, string>::iterator it = m.begin(); it != m.end(); it++) {
        s.insert(it->second);
    }

    int n; cin >> n;

    string str;
    for (int i = 1; i <= n; i++) {
        cin >> str;
        s.erase(m[str]);
    }

    cout << s.size() << endl;
    for (set<string>::iterator it = s.begin(); it != s.end(); it++) {
        cout << *it << endl;
    }

    return 0;
}

```

## C - Another Sorting Problem

CF：[Problem - A - Codeforces](http://codeforces.com/contest/1575/problem/A)

洛谷：[CF1575A Another Sorting Problem - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/CF1575A)

### 题目

输入字符串，排序（奇数位降序，偶数位升序）。

### 思路

STL + 重载运算符。

```c++
#include <algorithm>
#include <iostream>

using namespace std;

const int MAXN = 1E6 + 7;

int n, m;

struct node {
    string str;
    int idx;
    friend bool operator<(const node &a, const node &b) {
        for (int i = 0; i < m; i++) {
            if (a.str[i] == b.str[i]) continue;
            return (i+1)%2 ? a.str[i] < b.str[i] : a.str[i] > b.str[i];
        }
        return 0;
    }
} a[MAXN];

int main() {
    // freopen("C.out", "w", stdout);
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> a[i].str;
        a[i].idx = i;
    }

    sort(a+1, a+n+1);

    for (int i = 1; i <= n; i++) {
        // cout << a[i].str << ' ';
        cout << a[i].idx << ' ';
    }
    cout << endl;

    return 0;
}

```

## D - ConneR and the A.R.C. Markland-N

CF：[Problem - A - Codeforces](http://codeforces.com/contest/1293/problem/A)

洛谷：[CF1293A ConneR and the A.R.C. Markland-N - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/CF1293A)

### 题目

求整数1~n中去掉k个数后距离s最近的数与s的距离。

### 思路

set标记后扫一遍。

```c++
#include <set>
#include <cstdlib>
#include <cstdio>

using namespace std;

const int MAXN = 2E9 + 7;
const int MAXK = 1E3 + 7;

int n, s, k;

set<int> closed;
int main() {
    // freopen("D.out", "w", stdout);
    int t; scanf("%d", &t);

    while (t--) {
        closed.clear();
        scanf("%d%d%d", &n, &s, &k);
        for (int i = 0, floor; i < k; i++) {
            scanf("%d", &floor);
            closed.insert(floor);
        }
    
        if (!closed.count(s)) {
            printf("%d\n", 0);
            continue;
        }
    
        for (int i = 1; i <= k; i++) {
            if (s-i >= max(1, s-k)) {
                if (!closed.count(s-i)) {
                    printf("%d\n", i);
                    break;
                }
            }
            if (s+i <= min(n, s+k)) {
                if (!closed.count(s+i)) {
                    printf("%d\n", i);
                    break;
                }
            }
        }
    }

    return 0;
}

```

## E - Delete Two Elements

CF：[Problem - C - Codeforces](http://codeforces.com/contest/1598/problem/C)

洛谷：[CF1598C Delete Two Elements - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/CF1598C)

### 题目

给定n个整数，求删除两个数平均值不变的选法数。

### 思路

删除两个书平均值不变，即这两个数和为平均值二倍。

map存整数个数。

$Avg \times 2$ 若非整数，那么选法数必然为0，因为整数加整数不可能为小数

然后枚举，计数。

```c++
#include <cstdio>
#include <iostream>
#include <map>

using namespace std;

const int MAXN = 2E5 + 7;

map<int, int> m;
int a[MAXN];
int main() {
    // freopen("E.out", "w", stdout);
    int t; cin >> t;

    int n;
    long long avg2, cnt;
    while (t--) {
        m.clear(); avg2 = cnt = 0;

        cin >> n;
        for (int i = 0; i < n; i++) {
            cin >> a[i];
            avg2 += a[i];
            m[a[i]]++;
        }
        avg2 <<= 1;

        if (avg2 % n) {
            cout << 0 << endl;
            continue;
        }
        avg2 /= n;

        for (int i = 0; i < n; i++) {
            m[a[i]]--;
            cnt += m[avg2 - a[i]];
        }

        cout << cnt << endl;
    }
    return 0;
}

```