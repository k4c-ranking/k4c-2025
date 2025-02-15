# k4c-2025

Kang Chiao CodeCombat Cup (K4C) is an internal programming contest organized by the Computer Science Honor Society (CSHS), dedicated to promoting CS and fostering inclusivity.

## Disclaimer

**We sincerely apologize for underestimating the problemset difficulty.** While we aimed to create an engaging and inclusive contest, some problems (C, D, E, K1 in particular) proved to be more challenging than expected. Again, we appreciate your effort and persistence. If we have the opportunity to hold a similar event in the future, this experience will serve as a good reference.


## Notice
Feel free to read through the solutions of problems you missed during the contest, or even try them out in your free time!


## A - !(Hello World)

Expected Difficulty: ⭐

<details>
    <summary>Hint 1</summary>

Just print anything other than hello world :)
</details>

<details>
	<summary>Tutorial</summary>

This problem is a giveaway to teams that show up. To pass, your program just needs to compile and output anything (other than hello world).
</details>

<details>
	<summary>Implementation (C++)</summary>

```c++
/* pA model full solution */

#include <bits/stdc++.h>
using namespace std;

int main() {
    cout << "Anything would work";
    return 0;
}
```
</details>


## B - Sum Random Problem

Expected Difficulty: ⭐

<details>
	<summary>Hint 1</summary>

Use a `for` loop to handle multiple test cases.
</details>

<details>
	<summary>Hint 2</summary>

For each test case, use another `for` loop with two variables, one being the rolling sum.
</details>

<details>
	<summary>Hint 3</summary>

For C++/Java: Mind the maximum value `int` can handle.
</details>

<details>
	<summary>Tutorial</summary>

This problem tests your proficiency with input/output operations and `for` loops. In addition, it introduces you the general problem format.

By using a rolling variable to keep track of the rolling sum, no arrays are needed. Also note that you can (and should) print answers to each test case **on the fly**.

**C++/Java:** note that the sum could reach $10^5 \cdot 10^9 = 10^{14}$, which exceeds the limits of a 32-bit integer (`int`). To get full credit, you should use a 64-bit integer data type (`long` in Java / `long long` in C++) to store the answer.
</details>

<details>
	<summary>Implementation (C++)</summary>

```c++
/* pB model full solution */

#include <bits/stdc++.h>
using namespace std;


void solve() {
    int n;
    cin >> n;

    // Use a rolling variable to keep track of the sum (no need for array)
    // As the answer could reach +-10^14, `long long` should be used
    long long sum = 0;
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        sum += x;
    }

    cout << sum << '\n';
}


int main() {
    cin.tie(0)->sync_with_stdio(0);

    // Boilerplate code for multiple test cases
    // (available in contest templates)
    int t;
    cin >> t;
    while (t--) solve();

    return 0;
}
```
</details>


## C - Nonclassic $3n+1$

Expected Difficulty: ⭐⭐

<details>
	<summary>Hint 1</summary>

What data type is most appropriate for storing `n`?
</details>

<details>
	<summary>Hint 2</summary>

Use a string.
</details>

<details>
	<summary>Hint 3</summary>

How would *you* quickly tell if a number is odd or even?
</details>

<details>
	<summary>Tutorial</summary>

This problem requires proficiency in loops and strings.
A beginner pitfall is converting the string $n$ into an integer to determine parity, especially in Python. This would likely lead to TLE (Time Limit Exceeded) or RE (Runtime Error). To receive full credit, you should check only if the last digit of $n$ is odd or even.
</details>

<details>
	<summary>Implementation (C++)</summary>

```c++
/* pC model full solution */

#include <bits/stdc++.h>
using namespace std;


void solve() {
    string n;  // using a string to store `n` is most appropriate
    int k;
    cin >> n >> k;

    for (int i = 0; i < k; i++) {
        // directly converting `n` to int would result in overflow (in C++ and Java)
        // note that it is sufficient to check the last digit of `n` for parity
        int last_digit = n.back() - '0';
        if (last_digit % 2 == 0) {
            n = n.substr(0, (n.size() + 1) / 2);
        } else {
            n = n + n + n + '1';
        }
    }

    cout << n << '\n';
}


int main() {
    cin.tie(0)->sync_with_stdio(0);

    // solve multiple test cases
    int t;
    cin >> t;
    while (t--) solve();

    return 0;
}
```
</details>


## D - DAG

Expected Difficulty: ⭐⭐

<details>
	<summary>Hint 1</summary>

If any three consecutive numbers are neither arithmetic nor geometric, you can immediately answer `NO`. This obvious fact might make implementation easier.
</details>

<details>
	<summary>Hint 2</summary>

Pay attention to the lower & upper bounds of $a_i$.
</details>

<details>
	<summary>Hint 3</summary>

This problem can be solved by using integer operations (addition & multiplication). Other methods might fail on tricky test cases.
</details>

<details>
	<summary>Tutorial</summary>

This problem seems simple, but might be tricky to AC. The underlying motive is to showcase how the imprecision of floating point numbers could lead to errors. Below are some erroneous approaches (C++):

```c++
/* Goal: determine if a, b, c is geometric */
int a, b, c;

// RE or WA
if (a / b == b / c)
    // Division by zero could happen
    
// WA
if (b != 0 && c != 0 && a / b == b / c)
    // Actually performs integer division, which auto rounds down

// WA
if (b != 0 && c != 0 && (double)a / b == (double)b / c)
    // double is not precise enough
    // Use distance <= (small constant) instead of ==
    // when dealing with floating point numbers

// AC or WA
const long double EPS = 1e-18;
if (b != 0 && c != 0 && abs((long double)a / b - (long double)b / c) < EPS)
    // Risky, might fail on edge cases (also depends on compiler)
```

So, is there a better way that circumvents precision issues altogether? Perhaps by only using integer operations?
Well, the key is to observe that:

$$\frac{a}{b} = \frac{b}{c} \Leftrightarrow ac = b^2 \; (b, c \ne 0)$$
</details>

<details>
	<summary>Implementation (C++)</summary>

```c++
/* pD model full solution */

#include <bits/stdc++.h>
using namespace std;

/* Returns if (x, y, z) form an arithmetic sequence */
bool is_ap(int x, int y, int z) {
    return x + z == y + y;
}

/* Returns if (x, y, z) form a geometric sequence */
bool is_gp(int x, int y, int z) {
    return y != 0 && (long long)x * z == (long long)y * y;
}

bool verdict() {
    int n;
    cin >> n;
    vector<int> a(n);
    for (int &i: a) cin >> i;  // I used range-based for loops for brevity

    for (int i = 1; i < n-1; i++) {  // i represents the index of the middle term
        if (!(is_ap(a[i-1], a[i], a[i+1])     // If any three consecutive terms are neither arithmetic
            || is_gp(a[i-1], a[i], a[i+1])))  // or geometric
            return false;  // the answer must be "NO"
    }

    return true;
}

void solve() {
    cout << (verdict() ? "YES": "NO") << '\n';
}

int main() {
    cin.tie(0)->sync_with_stdio(0);

    // Solve multiple test cases
    int t;
    cin >> t;
    while (t--) solve();
    return 0;
}
```
</details>


## H1 - The Lazy Problem (easy version)

Expected Difficulty: ⭐⭐

<details>
	<summary>Hint 1</summary>

Try Googling "[C++/Python/Java] read until EOF"
</details>

<details>
	<summary>Hint 2</summary>

To process $\text{SORT}$ operations, implement a custom comparator returning `order[char1] < order[char2]`.
</details>

<details>
	<summary>Hint 3</summary>

Don't overthink it &mdash; just do exactly what the problem says.
</details>

<details>
	<summary>Tutorial</summary>

Aside from $Q$ not being explicitly given, there's really no tricks to the easy version of this problem. Just follow the instructions and your solution should pass in $\mathcal O(Q N \log N)$ time.
</details>

<details>
	<summary>Implementation (C++)</summary>

```c++
/* pH1 model full solution */

#include <bits/stdc++.h>
using namespace std;

// Global variables
const string ORDER = "qwertyuiopasdfghjklzxcvbnm";
int rnk[256];  // Using a hashmap (like unordered_map) is also okay
string s;
int n;


bool cmp(const char &a, const char &b) {  // Custom comparator to process SORT operations
    return rnk[a] < rnk[b];
}

void op_ins() {
    int type; string t;
    cin >> type >> t;

    int pos[3] = {0, n / 2, n};
    s.insert(pos[type + 1], t);
}

void op_repl() {
    char x, y;
    cin >> x >> y;

    for (char &c: s)  // I used reference-based for loop for brevity
        if (c == x)
            c = y;
}

void op_rev() {
    reverse(s.begin(), s.end());
}

void op_sort() {
    sort(s.begin(), s.end(), cmp);
}

void solve() {
    cin >> s;
    string op;
    while (cin >> op) {  // Read until input is exhausted
        n = s.size();
        if (op == "INS")  op_ins();
        if (op == "REPL") op_repl();
        if (op == "REV")  op_rev();
        if (op == "SORT") op_sort();
    }
    cout << s << '\n';
}

int main() {
    // Fast input & output is recommended for this problem
    cin.tie(0)->sync_with_stdio(0);

    // Preprocess string sorting order
    for (int i = 0; i < (int)ORDER.size(); i++)
        rnk[ORDER[i]] = i;

    solve();
    return 0;
}
```
</details>


## I - Square Distance

Expected Difficulty: ⭐⭐⭐⭐

<details>
	<summary>Hint 1</summary>

Consider any $x$. To compute $f(x)$ using the formula, checking $n$ one-by-one is too slow. Can you narrow it down to $2$ candidates?
</details>

<details>
	<summary>Hint 2</summary>

Try brute-forcing answers to small inputs. Notice any patterns?
</details>

<details>
	<summary>Hint 3</summary>

Under what condition is the answer trivially $0$?
</details>

<details>
	<summary>Tutorial</summary>

To efficiently compute $f(x)$ for some $x$, notice that $|x - n^2|$ obtains a minimum of $0$ at $n = \sqrt x$. It follows that the nearest integer values are $\lfloor \sqrt x \rfloor$ and $\lceil \sqrt x \rceil$. Therefore, comparing $|x - n^2|$ at these values suffices.

There is another challenge. For small inputs like subtask 1, directly computing $f(p)$ for each subarray works. Unfortunately, that is too slow for large inputs up to $10^6$. Still, observing small inputs may lead to patterns that generalize. This is a common technique in programming problems of any difficulty.

Let $\text{ans}$ be the answer for some $[l, r]$. After some experimenting, one might notice that $[l, r] \text{ contains a perfect square} \Rightarrow \text{ans} = 0$. One might also hypothesize that the converse holds (see Notes). Moreover, $\text{length} \ge 4 \Rightarrow \text{ans} \le 1$ (see Notes).

These two conditions lead to a conclusion: for long arrays, checking for perfect squares (single elements) suffices. For short arrays, the original brute-force approach can be used.
</details>

<details>
	<summary>Implementation (C++)</summary>

```c++
/* pI model full solution */

#include <bits/stdc++.h>
using namespace std;

long long square(int x) {
    return (long long)x * x;
}

int f(long long x) {
    // It suffices to check two candidates
    // the square of integers closest to sqrt(x)
    int y = (int)sqrtl(x);  // sqrtl is more reliable than sqrt
    return min(x - square(y), square(y+1) - x);
}

int answer() {
    int l, r;
    cin >> l >> r;

    int x = sqrtl(r);  // sqrtl is more reliable than sqrt
                    // alternatively, one can preprocess square roots of all 1 <= x <= 10^6

    /* If the interval contains a perfect square x
    Selecting the subarray [x, x] will achieve a minimum of 0 */
    if (l <= square(x) && square(x) <= r) return 0;

    /* The product of any four consecutive integers is always
    1 less than a perfect square */
    if (r - l >= 3) return 1;

    /* Otherwise, if the interval has a length of 3 or less
    brute-force checking suffices */
    int res = f(l);
    for (int i = l; i <= r; i++) {
        long long product = 1LL;
        for (int j = i; j <= r; j++) {
            product *= j;
            res = min(res, f(product));
        }
    }

    return res;
}

void solve() {
    cout << answer() << '\n';
}

int main() {
    cin.tie(0)->sync_with_stdio(0);

    // Solve multiple test cases
    int t;
    cin >> t;
    while (t--) solve();
}
```
</details>


## K1 - Math 101 (easy version)

Expected Difficulty: ⭐

<details>
	<summary>Hint 1</summary>

You should never print $-1$.
</details>

<details>
	<summary>Hint 2</summary>

Look for a trivial solution.
</details>

<details>
	<summary>Hint 3</summary>

Find the probability of me getting a gf. No, not $-1$.
</details>

<details>
	<summary>Tutorial</summary>

Problems may be deceivingly hard or simple. While often overlooked, trivial solutions &mdash; even if only applicable to a subproblem &mdash; can be really helpful. In this case, $0$ will always work, as it is a multiple of any integer (by definition) and a palindrome.
And before you ask, yes, the examples are intended to trick you :P
</details>

<details>
    <summary>Implementation (C++)</summary>

```c++
/* pK1 model full solution */

#include <bits/stdc++.h>
using namespace std;

void solve() {
    cout << 0 << '\n';  // 0 is a palindrome and a multiple of any number
}

int main() {
    cin.tie(0)->sync_with_stdio(0);

    // Solve multiple test cases
    int t;
    cin >> t;
    while (t--) solve();
    return 0;
}
```
</details>

## Notes
### I - Square Distance
**Lemma 1.** If $r - l \ge 3$, then $\text{ans}[l, r] \le 1$.

This is because the product of four consecutive integers is always $1$ less than a perfect square.

$$\begin{align}
0 \times 1 \times 2 \times 3 = 0 = 1^2 - 1 \\
1 \times 2 \times 3 \times 4 = 24 = 5^2 - 1 \\
2 \times 3 \times 4 \times 5 = 120 = 11^2 - 1 \\
3 \times 4 \times 5 \times 6 = 360 = 19^2 - 1 \\
\dots
\end{align}$$

**Proof** (by no means required to solve this problem)

$$\begin{align}
n(n+1)(n+2)(n+3) & = \left(n(n+3)\right) \left((n+1)(n+2)\right) \\
 & = (n^2 + 3n)(n^2 + 3n + 2) \\
 & = \left((n^2 + 3n + 1) - 1\right) \left((n^2 + 3n + 1) +1\right) \\
 & = (n^2 + 3n + 1)^2 - 1 \\
\end{align}$$


**Lemma 2.** $\text{ans}[l, r] = 0$ if and only if $[l, r]$ contains a perfect square.

The forward implication is obviously true, as selecting the perfect square as the subarray will result in $f(p) = 0$. However, showing the converse is way harder. A rigorous but out-of-scope proof is given [here](https://www.renyi.hu/~p_erdos/1939-03.pdf) (by no means required to solve this problem).
