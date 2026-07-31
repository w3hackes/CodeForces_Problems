Problem: An Alternate Way
ID: 2241D
Rating; 1100
Date of encounter: 7/30/2026

Intuition:
Any operation that has an interval length of 2 or more (r-l+1 > 2) then we can split said operation into multiple smaller operations operating on intervals of size 1 and 2. So we only have to consider operations of size 1 and 2

Strategy:

Notice that on the interval [I, I] a_i is incremented by 1 and the interval [I, I+1] a_i is incremented by 1 and a_(I+1) is decremented by 1. 

[Bullet point] We can go from right to left where either a_i < b_i and we increment a_i. If a_i > b_i we can decrease a_i and increase a_(I-1). Eventually, either a_1 > b_1 which yields a NO, or a_1 < b_1 which yields a YES

[Bullet point] An alternate strategy is to use the [prefix sum] (Has a dropdown to the definition of the prefix sum. Notice that either we add a {+1} or a {+1, -1} so the prefix sum either stays the same or increases. So if we define p_i the prefix sum of a on the interval [1,i] and q_i is the prefix sum of b on the interval [1,i] then when p_i <= q_i for all I in {1,2,... n} there is a possible method to equate a to b using the previous strategies method, if p_i > q_i then it's not possible to to equation a to b

Code:

#include <iostream>
#include <vector> 
using namespace std;
 
int main(){
 
    int tt;
    cin >> tt;
 
    while(tt--){
        int n;
        cin >> n;
 
        vector<long long> a(n), b(n);
 
        for(int i = 0; i < n; i++) cin >> a[i];
        for(int i = 0; i < n; i++) cin >> b[i];
        for(int i = 1; i < n; i++) a[i] += a[i - 1];
        for(int i = 1; i < n; i++) b[i] += b[i - 1];
 
        bool is = 1;
        for(int i = 0; i < n; i++) if(a[i] > b[i]) is = 0;
 
        if(is) cout << "YES\n";
        else cout << "NO\n";
    }
 
    return 0;
}
