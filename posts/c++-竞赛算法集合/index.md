# C++ 竞赛算法集合


用于算法竞赛（如ACM、ICPC）的不同算法的集合。

<!--more-->

## 动态规划

### 凸包技巧

https://codeforces.com/contest/319/problem/C

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>
using namespace std;

typedef long long ll;

const int MAXN = 105000;

int n;
ll height[MAXN], tax[MAXN];
ll dp[MAXN];
vector<ll> mvals, bvals;
int cur = 0;

// Suppose the last 3 lines added are : (l1, l2, l3)
// Line l2 becomes irrelevant, if l1/l3 x-intersection is to the left of l1/l2 x-intersection
bool bad(ll m1, ll b1, ll m2, ll b2, ll m3, ll b3) {
    // 转为 double 避免溢出
    return 1.0 * (b1 - b3) * (m2 - m1) < 1.0 * (b1 - b2) * (m3 - m1);
}

void add(ll m, ll b) {
    while ( (int) mvals.size() >= 2 && bad(mvals[mvals.size() - 2], bvals[bvals.size() - 2], mvals[mvals.size() - 1], bvals[bvals.size() - 1], m, b)) {
        mvals.pop_back();
        bvals.pop_back();
    }
    mvals.push_back(m);
    bvals.push_back(b);
}

void setCur(ll x) {
    if (cur > (int) mvals.size() - 1)
        cur = (int) mvals.size() - 1;
    // Best-line pointer goes to the right only when queries are non-decreasing (x argument grows)
    while (cur < (int) mvals.size() - 1 && 1.0 * mvals[cur + 1] * x + bvals[cur + 1] <= 1.0 * mvals[cur] * x + bvals[cur])
        cur++;
}

int main() {
    //freopen("input.txt", "r", stdin);
    //freopen("output.txt", "w", stdout);

    scanf("%d", &n);
    for (int i = 1; i <= n; i++)
        scanf("%I64d", &height[i]);
    for (int i = 1; i <= n; i++)
        scanf("%I64d", &tax[i]);

    // Formula is dp[i] = min(tax[j] * height[i] + dp[j] | j = 1 .. i - 1)
    // Here tax[j] is considered as m value, and dp[j] as b value in line equation y = m * x + b
    for (int i = 1; i <= n; i++) {
        if (i == 1) {
            dp[i] = 0;
        } else {
            setCur(height[i]);
            dp[i] = mvals[cur] * height[i] + bvals[cur];
        }
        add(tax[i], dp[i]);
    }

    cout << dp[n];

    return 0;
}
```

### 最长递增序列

https://informatics.msk.ru/mod/statements/view3.php?id=766&chapterid=1794

$O(N \log N)$

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>

using namespace std;

const int MAXN = 105000;
const int INF = 1000 * 1000 * 1000;

int n;
int k, b, m;
int a[MAXN];
int d[MAXN];
int ind[MAXN], pr[MAXN];
vector <int> ansv;
int ans = 1;

int main() {
    //assert(freopen("input.txt","r",stdin));
    //assert(freopen("output.txt","w",stdout));

    scanf("%d", &n);
    scanf("%d %d %d %d", &a[1], &k, &b, &m);

    for (int i = 2; i <= n; i++)
        a[i] = (k * a[i - 1] + b) % m;

    d[0] = -INF;
    for (int i = 1; i <= n; i++)
        d[i] = INF;

    for (int i = 1; i <= n; i++) {
        int pos = upper_bound(d + 1, d + n + 1, a[i]) - d;
        if (d[pos - 1] < a[i] && a[i] < d[pos]) {
            d[pos] = a[i];
            ind[pos] = i;
            pr[i] = ind[pos - 1];
            if (pos > ans) {
                ans = pos;
            }
        }
    }

    if (ans == 1) {
        printf("%d", a[1]);
    }
    else {
        int cur = ind[ans];
        while (cur != 0) {
            ansv.push_back(a[cur]);
            cur = pr[cur];
        }

        for (int i = (int) ansv.size() - 1; i >= 0; i--)
            printf("%d ", ansv[i]);
    }

    return 0;
}
```

## 数据结构

### 笛卡尔树

Balanced Binary Search Tree

https://informatics.msk.ru/mod/statements/view3.php?chapterid=2782

$O(\log N)$

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <utility>
#include <iomanip>

using namespace std;

const int mod = 1000 * 1000 * 1000;

struct node {
    int x, y;
    node *l, *r;
    node(int new_x, int new_y) {
        x = new_x; y = new_y;
        l = NULL; r = NULL;
    }
};

typedef node * pnode;

void merge(pnode &t, pnode l, pnode r) {
    if (l == NULL)
        t = r;
    else if (r == NULL)
        t = l;
    else if (l->y > r->y) {
        merge(l->r, l->r, r);
        t = l;
    }
    else {
        merge(r->l, l, r->l);
        t = r;
    }
}

void split(pnode t, int x, pnode &l, pnode &r) {
    if (t == NULL)
        l = r = NULL;
    else if (t->x > x) {
        split(t->l, x, l, t->l);
        r = t;
    }
    else {
        split(t->r, x, t->r, r);
        l = t;
    }
}

void add(pnode &t, pnode a) {
    if (t == NULL)
        t = a;
    else if (a->y > t->y) {
        split(t, a->x, a->l, a->r);
        t = a;
    }
    else {
        if (t->x < a->x)
            add(t->r, a);
        else
            add(t->l, a);
    }
}

int next(pnode t, int x) {
    int ans = -1;
    while (t != NULL) {
        if (t->x < x)
            t = t->r;
        else {
            if (ans == -1 || ans > t->x)
                ans = t->x;
            t = t->l;
        }
    }
    return ans;
}

int n, ans, x;
char qt, prev_qt;
pnode root = NULL, num;

int main() {
    //freopen("input.txt","r",stdin);
    //freopen("output.txt","w",stdout);

    scanf("%d\n", &n);
    for (int i = 1; i <= n; i++) {
        scanf("%c %d\n", &qt, &x);
        if (qt == '+') {
            if (prev_qt == '?')
                x = (x + ans) % mod;
            num = new node(x, rand());
            add(root, num);
        }
        else {
            ans = next(root, x);
            printf("%d\n", ans);
        }
        prev_qt = qt;
    }

    return 0;
}
```

### 带有隐式键的笛卡尔树

https://informatics.msk.ru/mod/statements/view3.php?chapterid=111240

$O(\log N)$

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <utility>
#include <cstring>
#include <iomanip>

using namespace std;

const int INF = 2 * 1000 * 1000 * 1000;

struct node {
    int y, val;
    int sz, mn;
    bool rev;
    node *l, *r;
    node (int new_val, int new_y) {
        y = new_y; val = new_val;
        sz = 1; mn = val;
        rev = false;
        l = NULL; r = NULL;
    }
};

typedef node * pnode;

int getsize(pnode t) {
    if (t == NULL)
        return 0;
    return t->sz;
}

int getmin(pnode t) {
    if (t == NULL)
        return INF;
    return t->mn;
}

void update(pnode t) {
    if (t == NULL)
        return;
    t->sz = getsize(t->l) + 1 + getsize(t->r);
    t->mn = min(t->val, min(getmin(t->r), getmin(t->l)));
}

void push(pnode t) {
    if (t && t->rev) {
        swap(t->l, t->r);
        if (t->l)
            t->l->rev ^= true;
        if (t->r)
            t->r->rev ^= true;
        t->rev = false;
    }
}

void merge(pnode &t, pnode l, pnode r) {
    push(l); push(r);
    if (l == NULL)
        t = r;
    else if (r == NULL)
        t = l;
    else if (l->y > r->y) {
        merge(l->r, l->r, r);
        t = l;
    }
    else {
        merge(r->l, l, r->l);
        t = r;
    }
    update(t);
}

void split(pnode t, pnode &l, pnode &r, int x, int add = 0) {
    push(t);
    if (t == NULL) {
        l = r = NULL;
        return;
    }
    int key = getsize(t->l) + 1 + add;
    if (x <= key) {
        split(t->l, l, t->l, x, add);
        r = t;
    }
    else {
        split(t->r, t->r, r, x, add + getsize(t->l) + 1);
        l = t;
    }
    update(t);
}

void reverse(pnode t, int l, int r) {
    pnode a, b;
    split(t, t, a, l, 0);
    split(a, a, b, r - l + 2, 0);
    a->rev ^= true;
    merge(t, t, a);
    merge(t, t, b);
}

int getmin(pnode t, int l, int r) {
    int ans;
    pnode a, b;
    split(t, t, a, l, 0);
    split(a, a, b, r - l + 2, 0);
    ans = getmin(a);
    merge(t, t, a);
    merge(t, t, b);
    return ans;
}

int n, m;
int qt, l, r;
pnode root = NULL, add;

int main() {
    //freopen("input.txt","r",stdin);
    //freopen("output.txt","w",stdout);

    scanf("%d %d\n", &n, &m);
    for (int i = 1; i <= n; i++) {
        int x;
        scanf("%d", &x);
        add = new node(x, rand());
        merge(root, root, add);
    }

    for (int i = 1; i <= m; i++) {
        scanf("%d %d %d",&qt, &l, &r);
        if (qt == 1) {
            reverse(root, l, r);
        }
        else {
            printf("%d\n", getmin(root, l, r));
        }
    }

    return 0;
}
```

### 树状数组

$O(\log N)$

https://informatics.msk.ru/mod/statements/view.php?chapterid=3317

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>

using namespace std;

const int MAXN = 105000;

int n, m;
int a[MAXN];
long long f[MAXN];
char q;
int l, r;

void update(int pos, int delta) {
    for (; pos <= n; pos = (pos | (pos + 1)))
        f[pos] += delta;
}

long long sum(int pos) {
    long long res = 0;
    for (; pos > 0; pos = (pos & (pos + 1)) - 1)
        res += f[pos];
    return res;
}

long long sum(int l, int r) {
    return sum(r) - sum(l - 1);
}

int main() {
    //assert(freopen("input.txt","r",stdin));
    //assert(freopen("output.txt","w",stdout));

    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &a[i]);
        update(i, a[i]);
    }

    scanf("%d\n", &m);
    for (int i = 1; i <= m; i++) {
        scanf("%c %d %d\n", &q, &l, &r);
        if (q == 's') {
            cout << sum(l, r) << " ";
        }
        else {
            int delta = r - a[l];
            a[l] = r;
            update(l, delta);
        }
    }

    return 0;
}
```

### 二维树状数组

$O((\log N)^2)$

https://informatics.msk.ru/mod/statements/view.php?chapterid=3013

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>

using namespace std;

const int MAXN = 1050;

int n, m;
int qn;
char q[10];
int f[MAXN][MAXN];

void update(int x, int y, int delta) {
    for (int i = x; i <= n; i = i | (i + 1))
        for (int j = y; j <= m; j = j | (j + 1))
            f[i][j] += delta;
}

int getSum(int x, int y) {
    int res = 0;
    for (int i = x; i > 0; i = (i & (i + 1)) - 1)
        for (int j = y; j > 0; j = (j & (j + 1)) - 1)
            res += f[i][j];
    return res;
}

int getSum(int xFrom, int xTo, int yFrom, int yTo) {
    return getSum(xTo, yTo) - getSum(xTo, yFrom - 1) - getSum(xFrom - 1, yTo) + getSum(xFrom - 1, yFrom - 1);
}

int main() {
    //assert(freopen("input.txt","r",stdin));
    //assert(freopen("output.txt","w",stdout));

    scanf("%d %d\n", &n, &qn);
    m = n;

    for (int i = 1; i <= qn; i++) {
        scanf("%s", &q);
        if (q[0] == 'A') {
            int x, y;
            scanf("%d %d\n", &x, &y);
            update(x, y, 1);
        }
        else {
            int xFrom, xTo, yFrom, yTo;
            scanf("%d %d %d %d\n", &xFrom, &yFrom, &xTo, &yTo);
            if (xFrom > xTo)
                swap(xFrom, xTo);
            if (yFrom > yTo)
                swap(yFrom, yTo);
            printf("%d\n", getSum(xFrom, xTo, yFrom, yTo));
        }
    }

    return 0;
}
```

### 隐式的线段树

- 时间复杂度：$O(\log N)$
- 空间复杂度：$O(N \log N)$

https://informatics.msk.ru/mod/statements/view.php?chapterid=3327

```cpp
#include <iostream>
#include <cstdio>
#include <cstdlib>

using namespace std;

typedef long long ll;

struct Node {
    ll sum;
    Node *l, *r;

    Node() : sum(0), l(NULL), r(NULL) { }
};

void add(Node *v, int l, int r, int q_l, int q_r, ll val) {
    if (l > r || q_r < l || q_l > r)
        return;

    if (q_l <= l && r <= q_r) {
        v -> sum += val;
        return;
    }

    int mid = (l + r) >> 1;

    if (v -> l == NULL)
        v -> l = new Node();

    if (v -> r == NULL)
        v -> r = new Node();

    add(v -> l, l, mid, q_l, q_r, val);
    add(v -> r, mid + 1, r, q_l, q_r, val);
}

ll get(Node *v, int l, int r, int pos) {
    if (!v || l > r || pos < l || pos > r)
        return 0;

    if (l == r)
        return v -> sum;

    int mid = (l + r) >> 1;

    return v -> sum + get(v -> l, l, mid, pos) + get(v -> r, mid + 1, r, pos);
}

int n, m, t, x, y, val;
char c;

int main() {
    //freopen("input.txt", "r", stdin);
    //freopen("output.txt", "w", stdout);

    Node *root = new Node();

    scanf("%d", &n);

    for (int i = 0; i < n; i++) {
        scanf("%d", &x);
        add(root, 0, n - 1, i, i, x);
    }

    scanf("%d", &m);

    for (int i = 0; i < m; i++) {
        scanf("\n%c", &c);

        if (c == 'a') {
            scanf("%d%d%d", &x, &y, &val);

            add(root, 0, n - 1, --x, --y, val);
        } else {
            scanf("%d", &x);

            printf("%I64d ", get(root, 0, n - 1, --x));
        }
    }

    return 0;
}
```

### 最小队列

- 时间复杂度：$O(1)$

https://informatics.msk.ru//mod/statements/view.php?chapterid=756

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>

using namespace std;

const int MAXN = 205000;

int n, m;
deque < pair <int, int> > d;
int a[MAXN];

void enqueue(int x) {
    int num = 1;
    while (!d.empty() && d.back().first > x) {
        num += d.back().second;
        d.pop_back();
    }
    d.push_back(make_pair(x, num));
}

void dequeue() {
    if (d.front().second == 1) {
        d.pop_front();
    }
    else {
        d.front().second--;
    }
}

int getMin() {
    return d.front().first;
}

int main() {
    //assert(freopen("input.txt","r",stdin));
    //assert(freopen("output.txt","w",stdout));

    scanf("%d %d", &n, &m);

    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i]);

    for (int i = 1; i <= m; i++) {
        enqueue(a[i]);
    }

    printf("%d\n", getMin());

    for (int i = m + 1; i <= n; i++) {
        dequeue();
        enqueue(a[i]);
        printf("%d\n", getMin());
    }

    return 0;
}
```

### 线段树（加法-最小间隔-最大间隔）

https://codeforces.com/contest/1263/problem/E

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>

using namespace std;

const int MAXN = 1000 * 1000 + 100;
const int INF = (int) 1e9;

struct node {
    int mx, mn;
    int add;
};

int n;
int val[MAXN];
string s;
int pos;
node tree[4 * MAXN];

void add(int v, int L, int R, int l, int r, int val) {
    if (l > r)
        return;
    if (L == l && R == r) {
        tree[v].add += val;
    } else {
        int mid = L + (R - L) / 2;
        add(2 * v + 1, L, mid, l, min(mid, r), val);
        add(2 * v + 2, mid + 1, R, max(mid + 1, l), r, val);
        tree[v].mx = max(tree[2 * v + 1].mx + tree[2 * v + 1].add, tree[2 * v + 2].mx + tree[2 * v + 2].add);
        tree[v].mn = min(tree[2 * v + 1].mn + tree[2 * v + 1].add, tree[2 * v + 2].mn + tree[2 * v + 2].add);
    }
}

int getMin(int v, int L, int R, int l, int r) {
    if (l > r) {
        return INF;
    }
    if (L == l && R == r) {
        return tree[v].mn + tree[v].add;
    }
    int mid = L + (R - L) / 2;
    return tree[v].add + min(getMin(2 * v + 1, L, mid, l, min(mid, r)), getMin(2 * v + 2, mid + 1, R, max(l, mid + 1), r));
}

int getMax(int v, int L, int R, int l, int r) {
    if (l > r) {
        return -INF;
    }
    if (L == l && R == r) {
        return tree[v].mx + tree[v].add;
    }
    int mid = L + (R - L) / 2;
    return tree[v].add + max(getMax(2 * v + 1, L, mid, l, min(mid, r)), getMax(2 * v + 2, mid + 1, R, max(l, mid + 1), r));
}

int main() {
    //assert(freopen("input.txt","r",stdin));
    //assert(freopen("output.txt","w",stdout));

    scanf("%d\n", &n);
    getline(cin, s);
    pos = 0;

    for (int i = 0; i < (int) s.length(); i++) {
        if (s[i] == 'L') {
            if (pos > 0) pos--;
        } else if (s[i] == 'R') {
            pos++;
        } else {
            int newVal = 0;
            if (s[i] == '(') {
                newVal = 1;
            } else if (s[i] == ')') {
                newVal = -1;
            }
            int delta = newVal - val[pos];
            val[pos] = newVal;
            add(0, 0, n - 1, pos, n - 1, delta);
        }
        int mn = getMin(0, 0, n - 1, 0, n - 1);
        int lastVal = getMin(0, 0, n - 1, n - 1, n - 1);
        if (mn < 0 || lastVal != 0) {
            printf("%d ", -1);
        } else {
            printf("%d ", getMax(0, 0, n - 1, 0, n - 1));
        }
    }

    cout << endl;

    return 0;
}
```

### 线段树（分配-求和）

https://codeforces.com/gym/100093

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>

using namespace std;

const int MAXN = 105000;
const int zero = -1;

struct node {
    long long sum;
    int val;
    int size;
};

int n, m;
char qt;
int x, l, r;
int a[MAXN];
vector <node> tree;

void build (int v, int L, int R, int a[]) {
    if (L == R) {
        tree[v].sum = tree[v].val = a[L];
        tree[v].size = 1;
    }
    else {
        int mid = L + (R - L) / 2;
        build(2 * v, L, mid, a);
        build(2 * v + 1, mid + 1, R, a);
        tree[v].sum = tree[2 * v].sum + tree[2 * v + 1].sum;
        tree[v].val = zero;
        tree[v].size = tree[2 * v + 1].size + tree[2 * v].size;
    }
}

void push(int v) {
    if (tree[v].val == zero)
        return;
    if (tree[v].size != 1) {
        tree[2 * v].val = tree[v].val;
        tree[2 * v + 1].val = tree[v].val;
    }
    tree[v].sum = 1ll * tree[v].size * tree[v].val;
    tree[v].val = zero;
}

void assign(int v, int L, int R, int l, int r, int val) {
    if (l > r)
        return;
    push(v);
    if (L == l && R == r) {
        tree[v].val = val;
        tree[v].sum = 1ll * val * tree[v].size;
    } else {
        int mid = L + (R - L) / 2;
        assign(2 * v, L, mid, l, min(mid, r), val);
        assign(2 * v + 1, mid + 1, R, max(mid + 1, l), r, val);
        push(2 * v);
        push(2 * v + 1);
        tree[v].sum = tree[2 * v].sum + tree[2 * v + 1].sum;
    }
    push(v);
}

long long getsum(int v, int L, int R, int l, int r) {
    if (l > r)
        return 0;
    push(v);
    if (l == L && r == R)
        return tree[v].sum;
    int mid = L + (R - L) / 2;
    long long res = getsum(2 * v, L, mid, l, min(mid, r)) + getsum(2 * v + 1, mid + 1, R, max(l, mid + 1), r);
    return res;
}

int main() {
    assert(freopen("sum.in","r",stdin));
    assert(freopen("sum.out","w",stdout));

    scanf("%d %d\n", &n, &m);

    tree.resize(4 * n);
    build(1, 1, n, a);

    for (int i = 1; i <= m; i++) {
        scanf("%c", &qt);
        if (qt == 'A') {
            scanf("%d %d %d\n", &l, &r, &x);
            assign(1, 1, n, l, r, x);
        }
        else {
            scanf("%d %d\n", &l, &r);
            cout << getsum(1, 1, n, l, r) << endl;
        }
    }

    return 0;
}
```

### 线段树（最小值-值更新）

- 预先计算：$O(N \log N)$
- 查询：$O(1)$

https://informatics.msk.ru/mod/statements/view.php?chapterid=3309

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>

using namespace std;

const int MAXN = 105000;
const int INF = (int) 1e9;

int n, num, qn;
int a[MAXN];
int tree[4 * MAXN];
int l, r;

int getMax(int l, int r) {
    l = num + l - 1; r = num + r - 1;
    int res = -INF;

    while (l <= r) {
        if (l & 1) {
            res = max(res, tree[l]);
            l++;
        }
        if (r % 2 == 0) {
            res = max(res, tree[r]);
            r--;
        }
        l /= 2; r /= 2;
    }

    return res;
}

void update(int pos, int val) {
    pos = num + pos - 1;
    tree[pos] = val;
    pos /= 2;
    while (pos >= 1) {
        tree[pos] = max(tree[pos * 2], tree[pos * 2 + 1]);
        pos /= 2;
    }
}

int main() {
    //assert(freopen("input.txt","r",stdin));
    //assert(freopen("output.txt","w",stdout));

    scanf("%d", &n);
    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i]);

    num = 1;
    while (num < n)
        num *= 2;

    for (int i = num; i < 2 * num; i++) {
        if (i - num + 1 <= n)
            tree[i] = a[i - num + 1];
        else
            tree[i] = -INF;
    }

    for (int i = num - 1; i >= 1; i--) {
        tree[i] = max(tree[i * 2], tree[i * 2 + 1]);
    }

    scanf("%d", &qn);
    for (int i = 1; i <= qn; i++) {
        scanf("%d %d", &l, &r);
        printf("%d ", getMax(l, r));
    }

    return 0;
}
```

### 稀疏表

- 预先计算：$O(N)$
- 查询：$O(\log N)$

https://informatics.msk.ru/mod/statements/view.php?chapterid=3309

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>

using namespace std;

const int MAXN = 2 * 105000;
const int MAXLOG = 20;

int n, m;
int a[MAXN];
int table[MAXLOG][MAXN];
int numlog[MAXN];

void buildTable() {
    numlog[1] = 0;
    for (int i = 2; i <= n; i++)
        numlog[i] = numlog[i / 2] + 1;

    for (int i = 0; i <= numlog[n]; i++) {
        int curlen = 1 << i;
        for (int j = 1; j <= n; j++) {
            if (i == 0) {
                table[i][j] = a[j];
                continue;
            }
            table[i][j] = max(table[i - 1][j], table[i - 1][j + curlen / 2]);
        }
    }
}

int getMax(int l, int r) {
    int curlog = numlog[r - l + 1];
    return max(table[curlog][l], table[curlog][r - (1 << curlog) + 1]);
}

int main() {
    //assert(freopen("input.txt","r",stdin));
    //assert(freopen("output.txt","w",stdout));

    scanf("%d", &n);

    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i]);

    buildTable();

    scanf("%d", &m);

    for (int i = 1; i <= m; i++) {
        int l, r;
        scanf("%d %d", &l, &r);
        printf("%d ", getMax(l, r));
    }

    return 0;
}
```

## 几何

### 最近点对

- 时间复杂度：$O(N \log N)$

https://www.spoj.com/problems/CLOPPAIR/

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>

using namespace std;

#define sqr(x) ((x) * (x))

const double inf = 1e100;
const int MAXN = 105000;

struct point {
    double x, y;
    int ind;
};

bool cmp(point a, point b) {
    return (a.x < b.x || (a.x == b.x && a.y < b.y));
}

double dist(point a, point b) {
    return sqrt(sqr(a.x - b.x) + sqr(a.y - b.y));
}

int n;
int a[MAXN];
point p[MAXN], tmp[MAXN];
double ans = inf;
int p1, p2;

void updateAnswer(point a, point b) {
    double d = dist(a, b);
    if (d < ans) {
        ans = d;
        p1 = a.ind; p2 = b.ind;
    }
}

void closestPair(int l, int r) {
    if (l >= r)
        return;

    if (r - l == 1) {
        if (p[l].y > p[r].y)
            swap(p[l], p[r]);
        updateAnswer(p[l], p[r]);
        return;
    }

    int m = (l + r) / 2;
    double mx = p[m].x;

    closestPair(l, m);
    closestPair(m + 1, r);

    int lp = l, rp = m + 1, sz = 1;
    while (lp <= m || rp <= r) {
        if (lp > m || ((rp <= r && p[rp].y < p[lp].y))) {
            tmp[sz] = p[rp];
            rp++;
        }
        else {
            tmp[sz] = p[lp];
            lp++;
        }
        sz++;
    }

    for (int i = l; i <= r; i++)
        p[i] = tmp[i - l + 1];

    sz = 0;
    for (int i = l; i <= r; i++)
        if (abs(p[i].x - mx) < ans) {
            sz++;
            tmp[sz] = p[i];
        }

    for (int i = 1; i <= sz; i++) {
        for (int j = i - 1; j >= 1; j--) {
            if (tmp[i].y - tmp[j].y >= ans)
                break;
            updateAnswer(tmp[i], tmp[j]);
        }
    }
}

int main() {
    //assert(freopen("input.txt","r",stdin));
    //assert(freopen("output.txt","w",stdout));

    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        scanf("%lf %lf", &p[i].x, &p[i].y);
        p[i].ind = i - 1;
    }

    sort(p + 1, p + n + 1, cmp);

    closestPair(1, n);

    printf("%d %d %.6lf\n", min(p1, p2), max(p1, p2), ans);

    return 0;
}
```

```cpp
#include <iostream>
#include <cstdlib>
#include <cstdio>
#include <algorithm>
#include <cmath>

using namespace std;

#define sqr(a) ((a)*(a))
const double inf = 1e100;
const int MAXN = 55000;


struct point {
    double x, y;
    int ind;
    void read() {
        scanf("%lf%lf", &x, &y);
    }
    double dist_to(point& r) {
        return sqrt(sqr(x - r.x) + sqr(y - r.y));
    }
    bool operator<(const point&r) const {
        return x < r.x || (x == r.x && y < r.y);
    }
};

point aux[MAXN], P[MAXN], v[MAXN]; int vn;

int a, b, n;
double ans = inf;

// ans contains closest distance, a, b - indices of points.
void closest_pair(point p[], int n) {
    if (n <= 1) return ;
    if (n == 2) {
        if (p[0].y > p[1].y)
            swap(p[0], p[1]);
        double d = p[0].dist_to(p[1]);
        if (d < ans)
            ans = d, a = p[0].ind, b = p[1].ind;
        return;
    }
    int m = n / 2;
    double x = p[m].x;
    closest_pair(p, m); // left
    closest_pair(p + m, n - m); //right

    int il = 0, ir = m, i = 0;
    while (il < m && ir < n) { // merging two halves
        if (p[il].y < p[ir].y) aux[i ++] = p[il ++];
        else aux[i ++] = p[ir ++];
    }
    while (il < m)
        aux[i ++] = p[il ++];
    while (ir < n)
        aux[i ++] = p[ir ++];

    vn = 0;
    for (int j = 0 ; j < n ; j ++) { // copying back into p
        p[j] = aux[j];
        if (fabs(p[j].x - x) < ans) // looking at the strip of width 2*ans
            v[vn ++] = p[j];
    }

    for (int j = 0 ; j < vn ; j ++) { // (2*ans) x (ans) box
        for (int k = j + 1 ; k < vn && v[k].y - v[j].y < ans ; k ++) {
            double d = v[j].dist_to(v[k]);
            if (ans > d) {
                ans = d;
                a = v[k].ind, b = v[j].ind;
            }
        }
    }
}

int main() {
    scanf("%d", &n);
    for (int i = 0 ; i < n ; i++) {
        P[i].read();
        P[i].ind = i;
    }
    sort(P, P + n);
    closest_pair(P, n);
    printf("%d %d %lf\n", min(a, b), max(a, b), ans);
    return 0;
}
```

### 凸包

Graham-Andrew 方法

- 时间复杂度：$O(N \log N)$

https://informatics.msk.ru/mod/statements/view3.php?chapterid=638

https://informatics.msk.ru/mod/statements/view3.php?id=&chapterid=290

```cpp
#include <iostream>
#include <cstdlib>
#include <cstdio>
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

#define sqr(x) ((x) * (x))

const double pi = acos(-1.0);

struct point {
    double x, y;
};

int n;
vector <point> p, hull;
double ans;

bool cmp(point a, point b) {
    return (a.x < b.x || (a.x == b.x && a.y < b.y));
}

bool eq(point a, point b) {
    return (a.x == b.x && a.y == b.y);
}

bool isCCW(point a, point b, point c) {
    return a.x * (b.y - c.y) + b.x * (c.y - a.y) + c.x * (a.y - b.y) > 0;
}

void setConvexHull(vector <point> p, vector <point> &h) {
    sort(p.begin(), p.end(), cmp);
    p.erase(unique(p.begin(), p.end(), eq), p.end());

    vector <point> up, down;
    point head = p[0], tail = p.back();

    up.push_back(head); down.push_back(head);

    for (int i = 1; i < (int) p.size(); i++) {
        if (i == (int) p.size() - 1 || !isCCW(tail, head, p[i])) {
            while ( (int) up.size() >= 2 && isCCW(up[up.size() - 2], up.back(), p[i]) )
                up.pop_back();
            up.push_back(p[i]);
        }
        if (i == (int) p.size() - 1 || isCCW(tail, head, p[i])) {
            while ( (int) down.size() >= 2 && !isCCW(down[down.size() - 2], down.back(), p[i]) )
                down.pop_back();
            down.push_back(p[i]);
        }
    }

    h.clear();
    for (int i = 0; i < (int) up.size(); i++)
        h.push_back(up[i]);
    for (int i = (int) down.size() - 2; i > 0; i--)
        h.push_back(down[i]);

}

double dist(point a, point b) {
    return sqrt(sqr(a.x - b.x) + sqr(a.y - b.y));
}

double getPerimeter(vector <point> p) {
    double per = 0;

    for (int i = 1; i < (int) p.size(); i++)
        per += dist(p[i - 1], p[i]);
    per += dist(p.back(), p[0]);

    return per;
}

int main() {
    freopen("input.txt","r",stdin);
    freopen("output.txt","w",stdout);

    scanf("%d", &n);

    for (int i = 1; i <= n; i++) {
        point tmp;
        scanf("%lf %lf", &tmp.x, &tmp.y);
        p.push_back(tmp);
    }

    setConvexHull(p, hull);
    ans = getPerimeter(hull);

    printf("%.1lf", ans);

    return 0;
}
```

- 时间复杂度：$O(sort) + O(N)$

```cpp
#include <cstdlib>
#include <cstdio>
#include <algorithm>
#include <vector>
#include <cmath>

using namespace std;

#define sqr(x) ((x) * (x))

const double inf = 1e100, eps = 1e-12;

struct point {
    double x, y;
    void read() {
        scanf("%lf%lf", &x, &y);
    }
    double r() {
        return sqrt(x*x+y*y);
    }
    void print() {
        printf("%lf %lf\n", x, y);
    }
    bool operator<(const point& r) const {
        if (x < r.x) return 1;
        if (x > r.x) return 0;
        return y < r.y;
    }
    point operator-(point& r) {
        point res = {x - r.x, y - r.y};
        return res;
    }
    double slope() {
        if (x == 0.0 && y == 0.0) return -inf;
        if (x == 0.0) return inf;
        return y/x;
    }
    double operator*(const point& r) {
        return x*r.y - y*r.x;
    }
    double dist_to(point& r) {
        return sqrt(sqr(x-r.x)+sqr(y-r.y));
    }
};

point O; // left-most lower point
bool BY_SLOPE(point l, point r) {
    double ls = (l-O).slope(),  rs = (r-O).slope();
    if (ls < rs) return 1;
    if (ls > rs) return 0;
    return l.dist_to(O) < r.dist_to(O);
}
// pre: N >= 0, [p, p + N) - points
vector<point> convex_hull(point *p, int N) {
    if (N <= 2) return vector<point>(p, p + N);
    sort(p, p + N);
    O = p[0];
    sort(p + 1, p + N, BY_SLOPE);
    vector<point> hull;
    for (int i = 0 ; i < N ; i ++) {
        if (i < 3) hull.push_back(p[i]);
        else {
            int sz = hull.size();
            while (sz >= 2 && (p[i] - hull[sz-2])*(hull[sz-1]-hull[sz-2]) >= 0)
                hull.pop_back(), sz --;
            hull.push_back(p[i]);
        }
    }
    return hull;
}// post: convex hull in hull, given in ccw order


vector<point> v;
int  n;
point P[21100];

int main() {
    scanf("%d", &n);
    for (int i = 0 ; i < n ; i ++)
        P[i].read();
    v = convex_hull(P, n);
    double p = 0.0, s = 0.0;
    for (int i = 0 ; i < v.size() ; i ++) {
        p += (v[i]-v[(i+1)%v.size()]).r();
        s += .5*(v[i]*v[(i+1)%v.size()]);
    }
    printf("%e\n%e\n", p, s);
    //printf("%.1lf\n", p);


    return 0;
}
```

## 图

### Bellman-Ford 算法

- 时间复杂度：$O(N \times M)$

https://informatics.msk.ru/mod/statements/view3.php?id=260&chapterid=178

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>

using namespace std;

const int MAXN = 105;
const int INF = 30000;

struct edge {
    int from, to;
    int w;
};

int n, m;
int dist[MAXN];
vector <edge> e;

int main() {
    //assert(freopen("input.txt","r",stdin));
    //assert(freopen("output.txt","w",stdout));

    scanf("%d %d", &n, &m);

    for (int i = 1; i <= m; i++) {
        edge curEdge;
        scanf("%d %d %d", &curEdge.from, &curEdge.to, &curEdge.w);
        e.push_back(curEdge);
    }

    for (int i = 1; i <= n; i++)
        dist[i] = INF;
    dist[1] = 0;

    for (int i = 1; i <= n; i++) {
        bool changed = false;
        for (int j = 0; j < m; j++) {
            int from = e[j].from, to = e[j].to, w = e[j].w;
            if (dist[from] != INF && dist[from] + w < dist[to]) {
                dist[to] = dist[from] + w;
                changed = true;
            }
        }
        if (!changed)
            break;
    }

    for (int i = 1; i <= n; i++)
        printf("%d ", dist[i]);

    return 0;
}
```

### 二部图匹配

Kuhn 算法

- 时间复杂度：$O(N \times M)$

https://informatics.msk.ru/mod/statements/view.php?chapterid=1683

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>

using namespace std;

const int MAXN = 105;

int n, m;
vector <int> g[MAXN];
bool used[MAXN];
int mt[MAXN];
int ans;

bool kuhn(int v) {
    if (used[v])
        return false;
    used[v] = true;
    for (int i = 0; i < (int) g[v].size(); i++) {
        int to = g[v][i];
        if (mt[to] == 0 || kuhn(mt[to])) {
            mt[to] = v;
            return true;
        }
    }
    return false;
}

int main() {
    //assert(freopen("input.txt","r",stdin));
    //assert(freopen("output.txt","w",stdout));

    scanf("%d %d", &n, &m);

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            int can;
            scanf("%d", &can);
            if (can)
                g[i].push_back(j);
        }
    }

    for (int i = 1; i <= n; i++) {
        memset(used, 0, sizeof(used));
        if (kuhn(i))
            ans++;
    }

    printf("%d\n", ans);

    return 0;
}
```

### 桥搜索

- 时间复杂度：$O(M)$

https://codeforces.com/gym/100083

```cpp
#include <iostream>
#include <fstream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
#include <cassert>
#include <utility>
#include <iomanip>

using namespace std;

const int MAXN = 105000;

int n, m;
vector <int> g[MAXN];
vector <int> ind[MAXN];
int tin[MAXN], mn[MAXN];
bool used[MAXN];
vector <int> bridges;
int timer;

void dfs(int v, int par = -1) {
    used[v] = true;
    timer++;
    tin[v] = timer;
    mn[v] = tin[v];

    for (int i = 0; i < (int) g[v].size(); i++) {
        int to = g[v][i];
        if (!used[to]) {
            dfs(to, v);
            if (mn[to] == tin[to]) {
                bridges.push_back(ind[v][i]);
            }
            mn[v] = min(mn[v], mn[to]);
        }
        else if (to != par) {
            mn[v] = min(mn[v], mn[to]);
        }
    }
}

int main() {
    assert(freopen("bridges.in","r",stdin));
    assert(freopen("bridges.out","w",stdout));

    scanf("%d %d", &n, &m);

    for (int i = 1; i <= m; i++) {
        int from, to;
        scanf("%d %d", &from, &to);

        g[from].push_back(to);
        ind[from].push_back(i);

        g[to].push_back(from);
        ind[to].push_back(i);
    }

    for (int i = 1; i <= n; i++)
        if (!used[i])
            dfs(i);

    sort(bridges.begin(), bridges.end());

    printf("%d\n", (int) bridges.size());
    for (int i = 0; i < (int) bridges.size(); i++)
        printf("%d\n", bridges[i]);

    return 0;
}
```

### 

```cpp

```

```cpp

```

```cpp

```

```cpp

```

```cpp

```

```cpp

```

```cpp

```

```cpp

```

```cpp

```

```cpp

```

```cpp

```

```cpp

```

```cpp

```

```cpp

```

```cpp

```

```cpp

```

##  参考资料

- [ADJA/algos: Competitive programming algorithms in C++ - GitHub](https://github.com/ADJA/algos)

