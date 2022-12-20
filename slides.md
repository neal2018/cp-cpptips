---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# use UnoCSS
css: unocss
---

# Some Tips for Coding in C++ in Competitive Programming
press space to continue


<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/neal2018/cp-cpptips" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# IO

<br/>

Improve the speed of `std::cin`
```cpp
ios::sync_with_stdio(false);
cin.tie(nullptr);
```
(Or in oneline)
```cpp
cin.tie(nullptr)->sync_with_stdio(false);
```

Note: can not use `scanf` after this

Output with `endl` is slow, use `\n` instead

Output with given precision
```cpp
cout << fixed << setprecision(20);
cout << 1.0 / 3.0 << '\n';
```

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# `std::is_sorted`
<br/>

check whether is sorted in non-descending order
```cpp
if (std::is_sorted(v.begin(), v.end())) {
  // do something
}
```
or non-ascending order
```cpp
if (std::is_sorted(v.begin(), v.end(), greater<>())) {
  // do something
}
```

Example: [https://codeforces.com/contest/1670/problem/A](https://codeforces.com/contest/1670/problem/A)

---

# `cond ? x : y` can return a reference
```cpp
vector<int> a(n), even, odd;
for (auto& x : a) cin >> x;
for (auto& x : a) ((x & 1) ? odd : even).push_back(x);
```

Example: [https://codeforces.com/contest/1638/problem/B](https://codeforces.com/contest/1638/problem/B)

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# `std::max` can receive more than 2 arguments
<br/>

NO:
```cpp
int ans = max(a, max(b, c));
```

YES:
```cpp
int ans = max({a, b, c});
```

Same for `std::min`

Example: [https://codeforces.com/contest/1244/problem/B](https://codeforces.com/contest/1244/problem/B)

---

# `std::minmax`
<br/>

`std::minmax` returns a pair of the minimum and maximum
```cpp
auto [mini, maxi] = minmax(u, v);
```

It is pretty useful if you want to store an unordered pair in a set
```cpp
set<pair<int, int>> s;
s.insert(minmax{u, v});
```
<br/>

Be careful: this code does not work as one might expect
```cpp
tie(u, v) = minmax(u, v);
```

Example: [https://codeforces.com/contest/1700/problem/E](https://codeforces.com/contest/1700/problem/E)

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# reverse
<br/>

```cpp
string s;
cin >> s;
string rev_s(s.rbegin(), s.rend());
```

Example: [https://codeforces.com/contest/1758/problem/A](https://codeforces.com/contest/1758/problem/A)

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# check whether contains repeated element
<br/>

```cpp
vector a = {1, 1, 4, 5, 1, 4};
if (set(a.begin(), a.end()).size() == a.size()) {
  // do something
}
```

Example: [https://codeforces.com/contest/1742/problem/B](https://codeforces.com/contest/1742/problem/B)

---

# iterate each segments with the same element
<br/>

```cpp
vector a = {1, 1, 1, 2, 2, 3} // [1, 1, 1], [2, 2], [3]
int n = int(a.size());

for (int i = 0, j; i < n; i = j){
    for (j = i; j < n && a[j] == a[i];) j++;
    // do something
}
// [i, j): [0, 3) [3, 5) [5, 6)
```

Example: [https://codeforces.com/contest/1430/problem/D](https://codeforces.com/contest/1430/problem/D)

---
layout: two-cols
---

# lambda

```cpp
// keep new name unvisited to outside
int limit = [&]{
  int l = 0, r = n;
  while (l < r) {
    int m = (l + r) / 2;
    if (check(m)){
      r = m;
    } else {
      l = m + 1;
    }
  }
  return l;
}();
```
```cpp
// can be used in `if`
if ([&]{
  for (auto& x : a) {
    if (check(x)) return false;
  }
}()){
  cout << "YES\n";
} else {
  cout << "NO\n";
}
```

::right::

<div p-5>

```cpp
// use as goto
[&] {
  for (auto& row : a) {
    for (auto& x : row) {
      if (check(x)) return;
    }
  }
  cout << "-1\n";
}();
```

```cpp
// can be passed around
auto multiplier = [](int x) { return x * x; };
auto adder = [](int x) { return x + x; };

auto solve = [&](auto operator){
  // ...
}

solve(f), solve(g);
```
```cpp
// recursion
function<void(int)> dfs = [&](int node){
  for (auto& child : g[node]) dfs(child);
};
```
</div>

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# `std::iota`
<br/>

```cpp
vector a = {1, 1, 4, 5, 1, 4};
vector<int> order(a.size());
iota(order.begin(), order.end(), 0);

// handy when sorting 
// and wanting to reserve the original array
sort(order.begin(), order.end(), [&](int i, int j){
  return a[i] < a[j];
});
```

Example: [https://codeforces.com/contest/1689/problem/E](https://codeforces.com/contest/1689/problem/E)


---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# `std::lower_bound`
<br/>

```cpp
set<int> s = {1, 2, 3, 4, 5};
```

`s.lower_bound(x);` and `lower_bound(s.begin(), s.end(), x);` are different!

The first one is O(log n), while the second one is O(n).

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# `std::accumulate`
<br/>

```cpp
vector<int> a(100000, 100000);
long long sum = accumulate(a.begin(), a.end(), 0ll);
```
Be careful with the type of the initial value!

Be careful with overflow!


---

# `floor_div`
<br/>

```cpp
int floor_div(int a, int b) { return a / b - ((a ^ b) < 0 && a % b != 0); }
```

Be careful with negative numbers!

Example: Kickstart 2022 Round E, Problem D

---

# print `__int128`
<br/>

Maybe useless though...

```cpp
using ll = long long;
void print(__int128 x) {
  constexpr ll P = 1e18;
  if (x < P) {
    cout << ll(x) << "\n";
  } else {
    cout << ll(x / P) << setw(18) << setfill('0') << ll(x % P) << "\n";
  }
}
```

---

# BFS
<br/>

`std::queue` is slow...

```cpp
vector<int> q = {0}, nq; // init
for (int step = 0; q.size(); swap(q, nq), nq.clear(), step++) {
  for (auto& x : q) {
    for (auto& y : g[x]) {
      // do something
      nq.push_back(y);
    }
  }
}
```

---

# generate random integers

<br/>

Do not use `rand()`...

```cpp
mt19937_64 rng(chrono::steady_clock::now().time_since_epoch().count());

int x = rng() % 100; // [0, 99]
```

---

# gcc built-in bit functions

<br/>

They are handy and fast!

```cpp
__builtin_popcount(x); // count number of 1s
__builtin_clz(x); // count number of leading 0s
__builtin_ctz(x); // count number of trailing 0s
__builtin_parity(x); // count number of 1s mod 2

// unsigned long long version
__builtin_popcountll(x);
```

---

# `erase` vs `extract` in `std::multiset`/`std::multimap`

<br/>

If you want to erase all elements with the same key, use `erase`.

```cpp
multiset<int> s = {1, 2, 3, 3, 3};
s.erase(3); // {1, 2}
```

If you want to erase only one element, use `extract`.

```cpp
multiset<int> s = {1, 2, 3, 3, 3};
s.extract(3); // {1, 2, 3, 3}
```

---
# `std::unique`
<br/>

Remove duplicates by sorting and `std::unique`.

```cpp
vector<int> a = {1, 1, 4, 5, 1, 4};
sort(a.begin(), a.end());
a.erase(unique(a.begin(), a.end()), a.end());
```
---

# `std::stable_sort`

<br/>

Sometimes faster than `std::sort`, but occupies more memory.

```cpp
stable_sort(v.begin(), v.end());
```

---

# `std::vector<bool>`
<br/>

Best practice: Do not use it.

```cpp
vector<bool> a = {true, false, true};
// ???
for (auto x : a) x = true;
for (auto& x : a) x = true;
for (auto&& x : a) x = true;
```

---

# scope
<br/>

If you want to use a same name twice, or just want to make the code more readable, you can make a scope explicitly.

```cpp
{
  int direction = 1;
  // do something
}

{
  int direction = -1;
  // do something
}
```

---

# `std::find`
<br/>

```cpp
vector<int> a = {1, 1, 4, 5, 1, 4};
// find the first 4
auto it_first = find(a.begin(), a.end(), 4);
// find the last 4
auto it_last = find(a.rbegin(), a.rend(), 4).base();
```

---

# Tips:
might subjective

- Prefer local variables to global variables
- Be careful with the lifetime of local variables
- Prefer lambda to normal function. Capture the local variables!
- Prefer `std::vector` and `std::array` to C-style array (`int a[100000]`)
- Prefer `std::string` to C-style string (`char s[100000]`)
- Prefer indexing starting from 0, not 1
- Prefer `std::array` to `std::vector` when the size is small and fixed, like a 2-by-2 matrix
- Prefer `std::array` to `std::pair`/`std::tuple` when the types are the same
- gcc: compile with `-D_GLIBCXX_DEBUG -D_GLIBCXX_ASSERTIONS`
  - it helps to throw exceptions when meeting some UB, like accessing out-of-bound elements
- In Linux, use sanitizer: compile with `-fsanitize=address -fsanitize=undefined` etc
- Precompile your header files (like `bits/stdc++.h`) to speed up compilation
