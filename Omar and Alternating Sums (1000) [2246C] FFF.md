# Omar and the Alternating Sum

* **ID:** 2246C  
* **Date:** July 21, 2026  
* **Status:** FFF  

---

## Intuition

Notice that in order to achieve an alternating sum of $0$, unless $-1$ is present in the chosen subset, we must select an **even number** of elements from each equal-value group so that they cancel each other out. If $-1$ is present, we instead require a pair of positive integers that differ by $1$ (i.e., $p$ and $p + 1$).

<details>
<summary><b>Proof: Why strictly increasing positive subsets cannot sum to 0</b></summary>

Assume our subset has length $N$ such that $a_i < a_{i+1}$ with $a_i \ge 1$ (no $-1$ present).

* **If $N$ is even:** The alternating sum expands to:
  $$\sum_{k=1}^{N/2} (a_{2k-1} - a_{2k})$$
  Since $a_{2k-1} < a_{2k}$, every term $(a_{2k-1} - a_{2k}) < 0$. A summation of strictly negative values cannot equal $0$.

* **If $N$ is odd:** The alternating sum expands to:
  $$a_1 + \sum_{k=1}^{(N-1)/2} (a_{2k+1} - a_{2k})$$
  Since $a_{2k+1} > a_{2k}$, every difference is strictly positive ($> 0$). Since $a_1 \ge 1$, the total sum $S \ge 1 > 0$.

Thus, no non-empty subset of strictly distinct positive integers can sum to $0$.
</details>

---

## Strategy

We split the problem into two distinct cases depending on whether $-1$ is present in the input array.

### 1. Array Does Not Contain $-1$

Let $d$ represent the number of distinct elements in the input array, and $N$ be the total length. 

<details>
<summary><b>Explanation: Parity distribution of subsets</b></summary>

For any group of equivalent elements of length $\ell$, exactly half of all possible subsets have an even size ($2^{\ell-1}$). 

To see why: consider choosing any configuration for the first $\ell - 1$ elements ($2^{\ell-1}$ ways). The choice for the final $\ell$-th element is forced: we either include it (if we need to flip parity) or exclude it (to preserve parity). Thus, exactly $2^{\ell-1}$ subsets have an even size.
</details>

Multiplying this across all $d$ distinct groups gives the total valid configurations:

$$\prod_{i=1}^{d} 2^{\ell_i - 1} = 2^{\sum (\ell_i - 1)} = 2^{N - d}$$

### 2. Array Contains $-1$

Since the array is sorted, $-1$ is present if and only if $a_1 = -1$. We consider two scenarios for the count of $-1$ elements selected:

* **Even number of $-1$'s:** Their signs cancel out completely, yielding the base case of $2^{N - d}$ valid choices.
* **Odd number of $-1$'s:** They contribute a net $-1$ to the sum. To balance this out to $0$, we must pick two positive elements $p$ and $p + 1$ that both appear an odd number of times.

<details>
<summary><b>Proof: Cancellation to {-1, p, p+1}</b></summary>

Since all other groups contribute an even number of elements, their internal parity signs cancel out completely. The simplified effective set reduces to $\{-1, p, p+1\}$, whose alternating sum is:

$$-1 + (p + 1) - p = 0$$
</details>

If $L$ is the number of adjacent pairs $(p, p+1)$ present in the array, each valid pair provides an additional set of $2^{N-d}$ configurations. Combined with the even case, the total answer is:

$$2^{N - d} \cdot (L + 1)$$

---

## Code

```cpp
#include <iostream>
#include <vector>

using namespace std;

const int MOD = 1e9 + 7;

// Helper to compute (base^exp) % MOD
long long power(long long base, long long exp) {
    long long res = 1;
    base %= MOD;
    while (exp > 0) {
        if (exp % 2 == 1) res = (res * base) % MOD;
        base = (base * base) % MOD;
        exp /= 2;
    }
    return res;
}

int solver(const vector<int>& arr, int N) {
    int diff = 0;
    int pairs = 0;

    for (int i = 0; i < N - 1; i++) {
        if (arr[i] < arr[i + 1]) {
            diff++;

            // Ensure both are positive numbers before checking adjacent pair
            if (arr[i] >= 1 && arr[i + 1] - arr[i] == 1) {
                pairs++;
            }
        }
    }

    // Total distinct values d = diff + 1
    // Power needed is N - d = N - 1 - diff
    long long exp = N - 1 - diff;
    long long ans = power(2, exp);

    // Since array is sorted, -1 is present iff arr[0] == -1
    if (arr[0] == -1) {
        ans = (ans * (pairs + 1)) % MOD;
    }

    return ans;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int T;
    cin >> T;
    while (T--) {
        int N;
        cin >> N;
        vector<int> arr(N);
        for (int i = 0; i < N; i++) {
            cin >> arr[i];
        }

        cout << solver(arr, N) << "\n";
    }

    return 0;
}
