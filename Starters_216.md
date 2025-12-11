# 🏆 CodeChef Starters 216 — Achieved Rank 201 in Div2 | Jumped from 1618 ➡️ 1653

## Thrilled to share the results from the Starters 216 on CodeChef today. It was a great test of Number Theory and fast debugging. Managed to solve 4️⃣ out of 6️⃣ problems in the allotted time.

## Link of the questions ➡️ [Codechef](https://www.codechef.com/START216)

### #P1 ➡️ Best Seats

#### Question being very straightforward asked for 2 adjacent seats for Chef and Chefina at the minimum cost. So we just have to find the minimum of 2 adjacent elements of the given array of seats.
```
#include <bits/stdc++.h>
using namespace std;
#define int long long
int32_t main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int t;
    cin >> t;
    while(t--){
        int n;
        cin >> n;
        vector<int> a(n);
        for(int i = 0; i < n; i++) cin >> a[i];
        int mini = INT_MAX;
        for(int i = 1; i < n; i++){
            mini = min(mini, a[i]+a[i-1]);
        }
        cout << mini << "\n";
    }
}
```
#### Time Complexity :- $O(N)$
#### Space Complexity :- $O(N)$

### #P2 ➡️ LIS and LDS

#### The question gave us an array of size n and frequences of i as the elements from 1 to n. We need to find LIS(Longest Increasing Subsequence) and LDS (Longest Decreasing Subsequence) possible alongside both and output its min
#### i.e. min(LIS,LDS).
#### So first I thought of highest freq in the array will be the answer but soon understood that it will give infact max(LIS,LDS) not what i want so just observed that if freq of any element is > 1 then it will be in both LIS and LDS but if some elements have only 1 freq those will make work with pivot if they are odd and different elements will get in LIS and LDS but they both will be equal in both cases.
#### i.e. c2 + (c1+1)/2

```
#include<bits/stdc++.h>
using namespace std;
#define int long long
int32_t main(){
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    int t;
    cin >> t;
    while(t--){
        int n;
        cin >> n;
        int freq1 = 0, freq2 = 0;
        for(int i = 0; i < n; i++){
            int x;
            cin >> x;
            if(x == 1) freq1++;
            else freq2++;
        }
        cout << freq2+(freq1+1)/2 << "\n";
    }
    return 0;
}
```
#### Time Complexity :- $O(N)$
#### Space Complexity :- $O(1)$

### #P3 ➡️ XOR Equal Subtract

#### This problem asks for the maximum size of a subsequence $A'$ where, for all pairs $(A'_i, A'_j)$, the condition $A'_i \oplus A'_j = |A'_i - A'_j|$ holds.This condition is only true if and only if all elements in the subsequence share the same Most Significant Bit (MSB), or if all elements are equal.Therefore, the solution is to find the maximum frequency of elements that fall within the same binary range (i.e., share the same MSB) across the entire input array $A$.
#### Firstly seeing the constraints were too less like 1 <= N <= 100 which allowed me to use n^2 complexity in the code as well. Finding the condition was a crutial part which allowed just to solve the problem.
#### Interstingly I solved this Problem before P2 thinking it to be easier then it.
```
#include<bits/stdc++.h>
using namespace std;
#define int long long
int32_t main(){
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    int t;
    cin >> t;
    while(t--){
        int n;
        cin >> n;
        map<int, int> freq;
        for(int i = 0; i < n; i++){
            int a;
            cin >> a;
            freq[a]++;
        }
        vector<int> uv;
        for(auto const& [val, frq] : freq){
            uv.push_back(val);
        }
        int M = uv.size();
        vector<int> dp(M);
        int max_size = 0;
        for(int i = 0; i < M; i++){
            int c_v = uv[i];
            int c_f = freq[c_v];
            dp[i] = c_f;
            for(int j = 0; j < i; j++){
                int p_v = uv[j];
                if ((c_v & p_v) == p_v){
                    dp[i] = max(dp[i], dp[j] + c_f);
                }
            }
            max_size = max(max_size, dp[i]);
        }

        cout << max_size << "\n";
    }
    return 0;
}
```

#### Time Complexity :- $O(N^2)$
#### Space Complexity :- $O(N)$

 ## If Anyone is able to get is O(NlogN) Time Complexity Then it will be a gentle request to share your solution with us!!


### #P4 ➡️ Minimum Same Difference

#### This counting problem asks for the sum of $f(A)$ over all $K^N$ arrays, where $f(A)$ is the minimum distance between any two equal elements.The core idea is to use complementary counting: calculate the total sum by summing over $d = 1$ to $N-1$, where we count the number of arrays $A$ such that $f(A) \ge d$.
####  Core Principle: It uses complementary counting based on the identity: $\sum f(A) = \sum_{d=1}^{N-1} \text{Count}(\text{arrays where } f(A) \ge d)$.
#### Pre-calculation (c): The variable c computes the number of arrays where all elements are distinct ($P(K, N)$ or $\frac{K!}{(K-N)!}$), which is equivalent to $\text{Count}(f(A) \ge N)$.
#### Main Loop: The loop iterates for the minimum difference $d$ from 1 up to $\min(N, K)$. In each step, it calculates $\text{Count}(f(A) \ge d)$ using dynamic programming/combinatorial logic, which involves placing $d-1$ or $d$ distinct values followed by $N-d+1$ positions that can be filled by $K-d+1$ values, utilizing the $\mathbf{O(\log N)}$ power function for exponentiation.
#### Final Sum: It finds the count of arrays where $f(A)=d$ by subtracting the count of $f(A) \ge d+1$ from $f(A) \ge d$, and sums these exact counts to get the final answer modulo $998244353$.
```
#include<bits/stdc++.h>
using namespace std;
#define int long long
int power(int base, int ep){
    int res = 1;
    base %= 998244353;
    while(ep > 0){
        if(ep % 2 == 1)res = (res * base) % 998244353;
        base = (base * base) % 998244353;
        ep /= 2;
    }
    return res;
}

int32_t main(){
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int t;
    cin >> t;
    while(t--){
        int n,k;
        cin >> n >> k;
        int MOD = 998244353;
        int c = 0;
        if(k >= n){
            c = 1;
            for(int i = 0; i < n; i++){
                c = (c * (k - i)) % MOD;
            }
        }
        int sum = 0;
        int perm = 1;
        int limit = (n < k) ? n : k;
        for(int d = 1; d <= limit; d++){
            int base = k - d + 1;
            int ep = n - d + 1;
            int part = power(base, ep);
            int h = (perm * part) % MOD;
            int term = (h - c + MOD) % MOD;
            sum = (sum + term) % MOD;
            perm = (perm * base) % MOD;
        }
        cout << sum << '\n';
    }
    return 0;
}
```

#### Time Complexity :- $O(NlogN)$
#### Space Complexity :- $O(1)$

## Just concluding here my post-contest work and thank to all those who saw my work and appreciation to all those who are also willing to share theirs. Just enjoy Coding!!!😀😀
