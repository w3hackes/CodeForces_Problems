# An Alternate Way

* **ID:** 2241D
* **Rating:** 1100
* **Date:** July 30, 2026

---

## Intuition

Any operation that has an interval length of $2$ or more ($r - l + 1 > 2$) can be split into multiple smaller operations operating on intervals of size $1$ and $2$. Thus, we only need to consider operations of size $1$ and $2$.

---

## Strategy

Notice that:
* On interval $[i, i]$, $a_i$ is incremented by $1$.
* On interval $[i, i + 1]$, $a_i$ is incremented by $1$ and $a_{i+1}$ is decremented by $1$.

* We can process from right to left:
  * If $a_i < b_i$, we increment $a_i$.
  * If $a_i > b_i$, we decrease $a_i$ and increase $a_{i-1}$.
  * Eventually, either $a_1 > b_1$ (which yields `NO`) or $a_1 \le b_1$ (which yields `YES`).

<details>
<summary><b>Explanation: Prefix Sum Approach</b></summary>

Notice that either we add {+1} or {+1, -1}, so the prefix sum either stays the same or increases. 

If we define $p_i$ as the prefix sum of $a$ on the interval $[1, i]$ and $q_i$ as the prefix sum of $b$ on the interval $[1, i]$:
* When $p_i \le q_i$ for all $i \in \{1, 2, \dots, n\}$, there is a valid sequence of operations to equate $a$ to $b$.
* If $p_i > q_i$ for any $i$, it is impossible to transform $a$ to $b$.
</details>

---

## Code

```cpp
#include <iostream>
#include <vector> 
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int tt;
    cin >> tt;

    while (tt--) {
        int n;
        cin >> n;

        vector<long long> a(n), b(n);

        for (int i = 0; i < n; i++) cin >> a[i];
        for (int i = 0; i < n; i++) cin >> b[i];
        for (int i = 1; i < n; i++) a[i] += a[i - 1];
        for (int i = 1; i < n; i++) b[i] += b[i - 1];

        bool is_possible = true;
        for (int i = 0; i < n; i++) {
            if (a[i] > b[i]) {
                is_possible = false;
                break;
            }
        }

        if (is_possible) cout << "YES\n";
        else cout << "NO\n";
    }

    return 0;
}
```
