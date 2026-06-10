# How to Learn and Remember C++ STL Properly

## Goal

Do **not** try to memorize all STL APIs at once.

The correct way is:

1. Learn **container purpose**
2. Learn **5-10 important APIs**
3. Solve **small problems**
4. Use STL in **real mini projects**
5. Revise with **active recall**

---

# 1. Best Way to Learn STL

## Step 1: Learn by category, not all together

Learn STL in this order:

### Phase 1: Must know first
- `vector`
- `string`
- `pair`
- `map`
- `unordered_map`
- `set`
- `unordered_set`

### Phase 2: For problem solving
- `stack`
- `queue`
- `deque`
- `priority_queue`

### Phase 3: Algorithms
- `sort`
- `reverse`
- `find`
- `count`
- `lower_bound`
- `upper_bound`
- `binary_search`
- `remove`
- `unique`
- `max_element`
- `min_element`

### Phase 4: Numeric + utility
- `accumulate`
- `iota`
- `partial_sum`
- `iterators`
- `tuple`
- `bitset`

---

# 2. What to Remember for Each STL Topic

For every STL thing, remember only these 4 points:

## A. What is it used for?
Example:
- `vector` -> dynamic array
- `map` -> sorted key-value
- `unordered_map` -> fast key-value lookup
- `set` -> unique sorted values
- `stack` -> LIFO
- `queue` -> FIFO
- `priority_queue` -> max/min heap

## B. Time complexity
Example:
- `vector push_back` -> usually `O(1)`
- `map` -> `O(log n)`
- `unordered_map` -> average `O(1)`
- `set` -> `O(log n)`

## C. Most used APIs
Example for `vector`:
- `push_back`
- `pop_back`
- `size`
- `empty`
- `begin/end`
- `insert`
- `erase`

## D. One common interview use
Example:
- `unordered_map` -> frequency count
- `set` -> remove duplicates
- `stack` -> next greater element
- `deque` -> sliding window maximum

---

# 3. Real Reason People Forget STL

People forget STL because they do this:

- read API list
- highlight notes
- move to next topic
- never use it

That does **not** build memory.

You remember STL only when you:

- write code by hand
- repeat same APIs many times
- solve pattern-based questions
- build small programs using them

---

# 4. Best Learning Method

## Method = Learn -> Use -> Revise -> Repeat

### Day format
For one STL topic:

1. Read APIs for 20 minutes
2. Write 10 small examples manually
3. Solve 5 small STL-only questions
4. Revise next day without notes

---

# 5. What to Practice for Each Container

## Vector
Practice:
- insert element
- delete element
- remove duplicates
- sort descending
- rotate array
- find max/min
- use `lower_bound`

## String
Practice:
- split string
- reverse words
- substring extraction
- find pattern
- palindrome
- convert string to number

## Map / Unordered_Map
Practice:
- frequency counter
- first unique character
- group values
- count duplicates
- key existence check

## Set / Unordered_Set
Practice:
- remove duplicates
- intersection
- union
- check existence
- nearest greater/smaller using `lower_bound`

## Stack
Practice:
- balanced parentheses
- next greater element
- postfix evaluation
- min stack

## Queue
Practice:
- BFS
- recent counter
- simulation problems

## Deque
Practice:
- sliding window maximum
- push/pop from both sides

## Priority Queue
Practice:
- kth largest
- top k frequent
- merge k sorted arrays
- Dijkstra

---

# 6. Should You Do Projects?

## Yes, but small STL-focused projects

Big projects are not necessary first.

Do **mini projects** where STL is used heavily.

### Best mini projects for STL learning

## Project 1: Student Record Manager
Use:
- `vector`
- `map`
- `sort`
- `find_if`

Features:
- add student
- delete student
- sort by marks
- search by roll number
- print topper

## Project 2: Word Frequency Analyzer
Use:
- `string`
- `unordered_map`
- `vector`
- `sort`

Features:
- read paragraph
- count words
- print most frequent words
- remove punctuation

## Project 3: Library Book Tracker
Use:
- `map`
- `set`
- `vector`

Features:
- add books
- issue books
- return books
- search book by id
- list available books

## Project 4: Task Scheduler
Use:
- `queue`
- `priority_queue`
- `deque`

Features:
- normal tasks
- high priority tasks
- process tasks in order

## Project 5: Browser History Simulator
Use:
- `stack`

Features:
- visit page
- back
- forward

This is very good for remembering stack usage.

---

# 7. Best Strategy to Remember APIs Permanently

## Rule 1: Keep a 1-page cheat sheet
Write only:
- container name
- purpose
- top APIs
- 2 common patterns

Example:

### `vector`
- dynamic array
- APIs: `push_back`, `pop_back`, `size`, `erase`, `insert`
- patterns:
  - remove using erase-remove
  - sort and binary search

---

## Rule 2: Use active recall
Close notes and ask:

- how to remove element from vector?
- how to find frequency?
- how to get first element >= x in set?
- how to make min heap?
- how to remove duplicates from sorted vector?

If unable to answer, revise that topic again.

---

## Rule 3: Use same API in many questions
Example:
Practice `lower_bound` in:
- vector
- set
- map

Then it becomes natural.

---

## Rule 4: Write from memory
Every 2-3 days, write this from memory:

- `vector` top APIs
- `map` top APIs
- `unordered_map` top APIs
- `set` top APIs
- `stack` use cases
- `priority_queue` syntax

Writing helps memory strongly.

---

# 8. 14-Day STL Learning Plan

## Day 1
- `vector`
- `pair`
- 10 coding examples

## Day 2
- `string`
- substring/search/erase

## Day 3
- `map`
- `unordered_map`
- frequency problems

## Day 4
- `set`
- `unordered_set`
- duplicate/removal/search

## Day 5
- `stack`
- parentheses / next greater

## Day 6
- `queue`
- `deque`
- BFS / sliding window intro

## Day 7
- `priority_queue`
- max heap / min heap / top k

## Day 8
- `sort`
- comparator
- `reverse`
- `find`

## Day 9
- `lower_bound`
- `upper_bound`
- `binary_search`

## Day 10
- `remove`
- `remove_if`
- `unique`
- `erase-remove idiom`

## Day 11
- `accumulate`
- `iota`
- `partial_sum`

## Day 12
- iterators
- `next`
- `prev`
- `distance`
- `back_inserter`

## Day 13
- solve 15 STL-only questions

## Day 14
- build 1 mini project using STL

---

# 9. Best Question Style for STL Learning

Do not start with very hard DSA.

Start with:

- frequency map questions
- sorting custom comparator
- remove duplicates
- top k elements
- sliding window with deque
- stack basic questions
- set lower_bound questions

First learn:
- **which STL to choose**
- **which API to call**

That is more important initially than hard logic.

---

# 10. Interview-Oriented Memory Trick

Remember STL like this:

## Arrays / dynamic data
- `vector`

## Text
- `string`

## Fast lookup
- `unordered_map`
- `unordered_set`

## Sorted lookup
- `map`
- `set`

## LIFO
- `stack`

## FIFO
- `queue`

## Double-ended
- `deque`

## Largest/smallest quickly
- `priority_queue`

## Processing ranges
- `<algorithm>`

## Sum/prefix sum
- `<numeric>`

This mental grouping makes recall easier.

---

# 11. What to Memorize Exactly

You do **not** need all APIs.

Memorize these first.

## `vector`
- `push_back`
- `pop_back`
- `size`
- `empty`
- `begin`
- `end`
- `insert`
- `erase`

## `string`
- `size`
- `substr`
- `find`
- `erase`
- `append`
- `stoi`
- `to_string`

## `map` / `unordered_map`
- `[]`
- `find`
- `count`
- `insert`
- `erase`

## `set`
- `insert`
- `find`
- `count`
- `erase`
- `lower_bound`
- `upper_bound`

## `stack`
- `push`
- `pop`
- `top`
- `empty`

## `queue`
- `push`
- `pop`
- `front`
- `back`

## `deque`
- `push_front`
- `push_back`
- `pop_front`
- `pop_back`
- `front`
- `back`

## `priority_queue`
- `push`
- `pop`
- `top`

## Algorithms
- `sort`
- `reverse`
- `find`
- `count`
- `lower_bound`
- `upper_bound`
- `binary_search`
- `remove`
- `unique`
- `max_element`
- `min_element`

## Numeric
- `accumulate`
- `iota`
- `partial_sum`

---

# 12. Final Advice

## If the goal is interview readiness:
Do this:

1. Learn one STL topic daily
2. Write small examples manually
3. Solve 5 STL-based questions daily
4. Build 1 mini project every week
5. Revise from memory every Sunday

## Best formula
**Read less, code more, revise repeatedly.**

You will remember STL when you use it in:
- questions
- mini projects
- handwritten recall

---

# 13. One Simple Weekly Routine

## Monday to Friday
- 1 STL topic
- 5 examples
- 5 questions

## Saturday
- 1 mini project

## Sunday
- revise all APIs from memory
- no notes for first attempt

---

# 14. Short Answer

## Do you need projects?
**Yes, small projects help a lot.**

## Do you need to memorize all APIs?
**No. Memorize common APIs first.**

## Best way to remember?
**Repeated coding + active recall + mini projects + pattern questions.**

---

# 15. Personal STL Learning Checklist

- [ ] I know what each STL container is used for
- [ ] I know top 5 APIs of each container
- [ ] I know time complexity of common operations
- [ ] I can choose between `map` and `unordered_map`
- [ ] I can choose between `set` and `unordered_set`
- [ ] I know min heap syntax
- [ ] I know erase-remove idiom
- [ ] I know `lower_bound` and `upper_bound`
- [ ] I solved at least 50 STL-based questions
- [ ] I built at least 3 STL mini projects

---

If needed, a **next README** can be made for:
1. **30-day STL roadmap**
2. **STL mini projects list**
3. **STL practice questions by topic**
4. **STL revision notes in 1-page format**

------------------------------------------------------------


# C++ STL Complete API Reference with Examples

## 1. Containers

### Vector (`#include <vector>`)

| API | Description | Example |
|-----|-------------|---------|
| `push_back(val)` | Add element at end | `v.push_back(5);` |
| `emplace_back(args...)` | Construct in-place at end | `v.emplace_back(x, y);` |
| `pop_back()` | Remove last element | `v.pop_back();` |
| `size()` | Number of elements | `int n = v.size();` |
| `empty()` | Check if empty | `if(v.empty()) { }` |
| `resize(n)` | Change size to n | `v.resize(10);` |
| `reserve(n)` | Pre-allocate capacity | `v.reserve(1000);` |
| `clear()` | Remove all elements | `v.clear();` |
| `insert(it, val)` | Insert at position | `v.insert(v.begin() + 3, 99);` |
| `erase(it)` | Remove at position | `v.erase(v.begin() + 2);` |
| `erase(first, last)` | Remove range | `v.erase(v.begin(), v.begin() + 5);` |
| `front() / back()` | First/last element | `int x = v.front(); int y = v.back();` |
| `begin() / end()` | Iterators | `for(auto it = v.begin(); it != v.end(); ++it)` |
| `rbegin() / rend()` | Reverse iterators | `for(auto it = v.rbegin(); it != v.rend(); ++it)` |
| `at(i)` | Access with bounds check | `int x = v.at(5);` throws exception |
| `operator[]` | Access without bounds check | `int x = v[5];` |
| `assign(n, val)` | Fill with n copies | `v.assign(10, 0);` |
| `swap(other)` | Swap contents | `v1.swap(v2);` |
| `shrink_to_fit()` | Release unused memory | `v.shrink_to_fit();` |
| `data()` | Raw pointer to array | `int* ptr = v.data();` |

**Common Patterns:**
```cpp
// Remove even numbers from vector
vector<int> v = {1, 2, 3, 4, 5, 6};
v.erase(remove_if(v.begin(), v.end(), [](int x){ return x % 2 == 0; }), v.end());

// Print vector
for(int x : v) cout << x << " ";

// Find and remove specific element
auto it = find(v.begin(), v.end(), 3);
if(it != v.end()) v.erase(it);
```

---

### String (`#include <string>`)

| API | Description | Example |
|-----|-------------|---------|
| `length() / size()` | String length | `int len = s.length();` |
| `empty()` | Check if empty | `if(s.empty()) { }` |
| `substr(pos, len)` | Extract substring | `string sub = s.substr(2, 4);` |
| `find(str, pos)` | Find first occurrence | `int idx = s.find("ab", 0);` |
| `rfind(str)` | Find last occurrence | `int idx = s.rfind("ab");` |
| `find_first_of(chars)` | First char in set | `int idx = s.find_first_of("aeiou");` |
| `find_last_of(chars)` | Last char in set | `int idx = s.find_last_of("aeiou");` |
| `find_first_not_of(chars)` | First char NOT in set | `int idx = s.find_first_not_of(" ");` |
| `append(str) / +=` | Concatenate | `s.append("world"); s += "!";` |
| `insert(pos, str)` | Insert at position | `s.insert(2, "XX");` |
| `erase(pos, len)` | Remove characters | `s.erase(1, 3);` |
| `replace(pos, len, str)` | Replace substring | `s.replace(0, 2, "hi");` |
| `c_str()` | Get C-string pointer | `const char* cstr = s.c_str();` |
| `stoi(s)` | String to int | `int num = stoi("123");` |
| `stol(s)` | String to long | `long num = stol("999999");` |
| `stof(s) / stod(s)` | String to float/double | `double d = stod("3.14");` |
| `to_string(val)` | Number to string | `string s = to_string(42);` |
| `push_back(ch)` | Append character | `s.push_back('a');` |
| `pop_back()` | Remove last char | `s.pop_back();` |
| `compare(str)` | Lexicographic compare | `if(s1.compare(s2) < 0) { }` |

**Common Patterns:**
```cpp
// Split string by delimiter
string s = "hello,world,test";
vector<string> tokens;
size_t pos = 0;
while((pos = s.find(',')) != string::npos) {
    tokens.push_back(s.substr(0, pos));
    s.erase(0, pos + 1);
}
tokens.push_back(s);

// Convert to lowercase
for(char& c : s) c = tolower(c);

// Trim whitespace from left and right
s.erase(0, s.find_first_not_of(" "));
s.erase(s.find_last_not_of(" ") + 1);

// Check if palindrome
string rev = s;
reverse(rev.begin(), rev.end());
if(s == rev) cout << "Palindrome";
```

---

### Map (`#include <map>`) — Ordered, O(log n)

| API | Description | Example |
|-----|-------------|---------|
| `insert({key, val})` | Insert pair | `m.insert({5, "five"});` |
| `emplace(key, val)` | Construct in-place | `m.emplace(5, "five");` |
| `operator[key]` | Access/insert | `m[5] = "five";` |
| `at(key)` | Access with exception | `string s = m.at(5);` throws if not found |
| `find(key)` | Iterator to element | `auto it = m.find(5); if(it != m.end())` |
| `count(key)` | 0 or 1 | `if(m.count(5)) { }` |
| `contains(key)` | Boolean check | `if(m.contains(5)) { }` (C++20) |
| `erase(key)` | Remove by key | `m.erase(5);` |
| `erase(it)` | Remove by iterator | `m.erase(it);` |
| `lower_bound(key)` | First >= key | `auto it = m.lower_bound(5);` |
| `upper_bound(key)` | First > key | `auto it = m.upper_bound(5);` |
| `equal_range(key)` | Pair of lower/upper | `auto [lo, hi] = m.equal_range(5);` |
| `size() / empty()` | Size check | `int sz = m.size();` |
| `clear()` | Remove all | `m.clear();` |
| `begin() / end()` | Sorted iteration | `for(auto [k, v] : m) { }` |
| `rbegin() / rend()` | Reverse iterators | `auto max_key = m.rbegin()->first;` |

**Common Patterns:**
```cpp
// Frequency counter
map<char, int> freq;
for(char c : "hello") freq[c]++;

// Find all elements with key >= 5
map<int, string> m = {{1, "a"}, {3, "b"}, {5, "c"}, {7, "d"}};
auto it = m.lower_bound(5);
while(it != m.end()) {
    cout << it->first << " " << it->second << "\n";
    ++it;
}

// Range query [3, 7]
auto lo = m.lower_bound(3);
auto hi = m.upper_bound(7);
for(auto it = lo; it != hi; ++it) {
    cout << it->first << "\n";
}

// Group by value
map<string, vector<int>> groups;
for(auto [k, v] : m) groups[v].push_back(k);
```

---

### Unordered_Map (`#include <unordered_map>`) — O(1) average

| API | Description | Example |
|-----|-------------|---------|
| `insert({key, val})` | Insert pair | `um.insert({5, "five"});` |
| `operator[key]` | Access/insert | `um[5] = "five";` |
| `find(key)` | Iterator or end() | `auto it = um.find(5);` |
| `count(key)` | 0 or 1 | `if(um.count(5)) { }` |
| `erase(key)` | Remove by key | `um.erase(5);` |
| `bucket_count()` | Number of buckets | `int buckets = um.bucket_count();` |
| `load_factor()` | Elements per bucket | `float lf = um.load_factor();` |
| `reserve(n)` | Pre-allocate buckets | `um.reserve(1000);` |

**Common Patterns:**
```cpp
// Two-sum problem
unordered_map<int, int> seen;
for(int x : arr) {
    if(seen.count(target - x)) {
        return {seen[target - x], i};
    }
    seen[x] = i;
}

// Anagram grouping
unordered_map<string, vector<string>> groups;
for(string word : words) {
    string sorted_word = word;
    sort(sorted_word.begin(), sorted_word.end());
    groups[sorted_word].push_back(word);
}
```

---

### Set (`#include <set>`) — Ordered unique elements, O(log n)

| API | Description | Example |
|-----|-------------|---------|
| `insert(val)` | Add element | `s.insert(5);` |
| `erase(val)` | Remove element | `s.erase(5);` |
| `find(val)` | Iterator or end() | `auto it = s.find(5);` |
| `count(val)` | 0 or 1 | `if(s.count(5)) { }` |
| `lower_bound(val)` | First >= val | `auto it = s.lower_bound(5);` |
| `upper_bound(val)` | First > val | `auto it = s.upper_bound(5);` |
| `begin() / end()` | Sorted iteration | `int min = *s.begin();` |
| `rbegin()` | Reverse begin | `int max = *s.rbegin();` |
| `size() / empty()` | Size | `bool is_empty = s.empty();` |

**Common Patterns:**
```cpp
// Remove duplicates from array
vector<int> arr = {1, 2, 2, 3, 3, 3};
set<int> unique_elements(arr.begin(), arr.end());

// Find range of elements [3, 7]
set<int> s = {1, 2, 3, 4, 5, 6, 7, 8, 9};
auto lo = s.lower_bound(3);
auto hi = s.upper_bound(7);
for(auto it = lo; it != hi; ++it) cout << *it << " ";

// Check if element exists
if(s.find(x) != s.end()) cout << "Found";

// Intersection of two sets
set<int> s1 = {1, 2, 3, 4};
set<int> s2 = {3, 4, 5, 6};
set<int> intersection;
set_intersection(s1.begin(), s1.end(), s2.begin(), s2.end(),
                 inserter(intersection, intersection.begin()));
```

---

### Multiset (`#include <set>`)

| API | Description | Example |
|-----|-------------|---------|
| `insert(val)` | Add element (allows duplicates) | `ms.insert(5); ms.insert(5);` |
| `count(val)` | Number of occurrences | `int freq = ms.count(5);` |
| `erase(find(val))` | Remove ONE occurrence | `ms.erase(ms.find(5));` |
| `erase(val)` | Remove ALL occurrences | `ms.erase(5);` |
| `equal_range(val)` | Range of equal elements | `auto [lo, hi] = ms.equal_range(5);` |

**Common Patterns:**
```cpp
// Frequency counter with sorted order
multiset<pair<int, char>> freq; // {count, char}
for(char c : "hello") {
    auto it = freq.find({freq.count(c), c});
    if(it != freq.end()) freq.erase(it);
    freq.insert({freq.count(c) + 1, c});
}

// Sliding window median
multiset<int> window;
for(int x : arr) {
    window.insert(x);
    if(window.size() > k) window.erase(window.find(old_element));
    auto mid = window.begin();
    advance(mid, window.size() / 2);
    cout << *mid << "\n";
}
```

---

### Unordered_Set (`#include <unordered_set>`)

| API | Description | Example |
|-----|-------------|---------|
| `insert(val)` | Add element | `us.insert(5);` |
| `find(val) / count(val)` | Check existence | `if(us.count(5)) { }` |
| `erase(val)` | Remove | `us.erase(5);` |

**Common Patterns:**
```cpp
// Check for duplicate within distance k
unordered_set<int> window;
for(int i = 0; i < arr.size(); i++) {
    if(window.count(arr[i])) return true;
    window.insert(arr[i]);
    if(window.size() > k) window.erase(arr[i - k]);
}
return false;
```

---

### Stack (`#include <stack>`)

| API | Description | Example |
|-----|-------------|---------|
| `push(val)` | Add to top | `st.push(5);` |
| `pop()` | Remove top | `st.pop();` |
| `top()` | Access top | `int x = st.top();` |
| `size() / empty()` | Size check | `while(!st.empty()) { }` |

**Common Patterns:**
```cpp
// Check balanced parentheses
bool isBalanced(string s) {
    stack<char> st;
    for(char c : s) {
        if(c == '(') st.push(c);
        else {
            if(st.empty()) return false;
            st.pop();
        }
    }
    return st.empty();
}

// Next greater element
vector<int> nextGreater(vector<int>& arr) {
    stack<int> st;
    vector<int> result(arr.size(), -1);
    for(int i = arr.size() - 1; i >= 0; i--) {
        while(!st.empty() && st.top() <= arr[i]) st.pop();
        if(!st.empty()) result[i] = st.top();
        st.push(arr[// filepath: c:\Users\JhaRagha\Desktop\workspace\personal\embedded_system_guidance\Core Programming\C++ Interview Questions\c++_programming_stl.md

# C++ STL Complete API Reference with Examples

## 1. Containers

### Vector (`#include <vector>`)

| API | Description | Example |
|-----|-------------|---------|
| `push_back(val)` | Add element at end | `v.push_back(5);` |
| `emplace_back(args...)` | Construct in-place at end | `v.emplace_back(x, y);` |
| `pop_back()` | Remove last element | `v.pop_back();` |
| `size()` | Number of elements | `int n = v.size();` |
| `empty()` | Check if empty | `if(v.empty()) { }` |
| `resize(n)` | Change size to n | `v.resize(10);` |
| `reserve(n)` | Pre-allocate capacity | `v.reserve(1000);` |
| `clear()` | Remove all elements | `v.clear();` |
| `insert(it, val)` | Insert at position | `v.insert(v.begin() + 3, 99);` |
| `erase(it)` | Remove at position | `v.erase(v.begin() + 2);` |
| `erase(first, last)` | Remove range | `v.erase(v.begin(), v.begin() + 5);` |
| `front() / back()` | First/last element | `int x = v.front(); int y = v.back();` |
| `begin() / end()` | Iterators | `for(auto it = v.begin(); it != v.end(); ++it)` |
| `rbegin() / rend()` | Reverse iterators | `for(auto it = v.rbegin(); it != v.rend(); ++it)` |
| `at(i)` | Access with bounds check | `int x = v.at(5);` throws exception |
| `operator[]` | Access without bounds check | `int x = v[5];` |
| `assign(n, val)` | Fill with n copies | `v.assign(10, 0);` |
| `swap(other)` | Swap contents | `v1.swap(v2);` |
| `shrink_to_fit()` | Release unused memory | `v.shrink_to_fit();` |
| `data()` | Raw pointer to array | `int* ptr = v.data();` |

**Common Patterns:**
```cpp
// Remove even numbers from vector
vector<int> v = {1, 2, 3, 4, 5, 6};
v.erase(remove_if(v.begin(), v.end(), [](int x){ return x % 2 == 0; }), v.end());

// Print vector
for(int x : v) cout << x << " ";

// Find and remove specific element
auto it = find(v.begin(), v.end(), 3);
if(it != v.end()) v.erase(it);
```

---

### String (`#include <string>`)

| API | Description | Example |
|-----|-------------|---------|
| `length() / size()` | String length | `int len = s.length();` |
| `empty()` | Check if empty | `if(s.empty()) { }` |
| `substr(pos, len)` | Extract substring | `string sub = s.substr(2, 4);` |
| `find(str, pos)` | Find first occurrence | `int idx = s.find("ab", 0);` |
| `rfind(str)` | Find last occurrence | `int idx = s.rfind("ab");` |
| `find_first_of(chars)` | First char in set | `int idx = s.find_first_of("aeiou");` |
| `find_last_of(chars)` | Last char in set | `int idx = s.find_last_of("aeiou");` |
| `find_first_not_of(chars)` | First char NOT in set | `int idx = s.find_first_not_of(" ");` |
| `append(str) / +=` | Concatenate | `s.append("world"); s += "!";` |
| `insert(pos, str)` | Insert at position | `s.insert(2, "XX");` |
| `erase(pos, len)` | Remove characters | `s.erase(1, 3);` |
| `replace(pos, len, str)` | Replace substring | `s.replace(0, 2, "hi");` |
| `c_str()` | Get C-string pointer | `const char* cstr = s.c_str();` |
| `stoi(s)` | String to int | `int num = stoi("123");` |
| `stol(s)` | String to long | `long num = stol("999999");` |
| `stof(s) / stod(s)` | String to float/double | `double d = stod("3.14");` |
| `to_string(val)` | Number to string | `string s = to_string(42);` |
| `push_back(ch)` | Append character | `s.push_back('a');` |
| `pop_back()` | Remove last char | `s.pop_back();` |
| `compare(str)` | Lexicographic compare | `if(s1.compare(s2) < 0) { }` |

**Common Patterns:**
```cpp
// Split string by delimiter
string s = "hello,world,test";
vector<string> tokens;
size_t pos = 0;
while((pos = s.find(',')) != string::npos) {
    tokens.push_back(s.substr(0, pos));
    s.erase(0, pos + 1);
}
tokens.push_back(s);

// Convert to lowercase
for(char& c : s) c = tolower(c);

// Trim whitespace from left and right
s.erase(0, s.find_first_not_of(" "));
s.erase(s.find_last_not_of(" ") + 1);

// Check if palindrome
string rev = s;
reverse(rev.begin(), rev.end());
if(s == rev) cout << "Palindrome";
```

---

### Map (`#include <map>`) — Ordered, O(log n)

| API | Description | Example |
|-----|-------------|---------|
| `insert({key, val})` | Insert pair | `m.insert({5, "five"});` |
| `emplace(key, val)` | Construct in-place | `m.emplace(5, "five");` |
| `operator[key]` | Access/insert | `m[5] = "five";` |
| `at(key)` | Access with exception | `string s = m.at(5);` throws if not found |
| `find(key)` | Iterator to element | `auto it = m.find(5); if(it != m.end())` |
| `count(key)` | 0 or 1 | `if(m.count(5)) { }` |
| `contains(key)` | Boolean check | `if(m.contains(5)) { }` (C++20) |
| `erase(key)` | Remove by key | `m.erase(5);` |
| `erase(it)` | Remove by iterator | `m.erase(it);` |
| `lower_bound(key)` | First >= key | `auto it = m.lower_bound(5);` |
| `upper_bound(key)` | First > key | `auto it = m.upper_bound(5);` |
| `equal_range(key)` | Pair of lower/upper | `auto [lo, hi] = m.equal_range(5);` |
| `size() / empty()` | Size check | `int sz = m.size();` |
| `clear()` | Remove all | `m.clear();` |
| `begin() / end()` | Sorted iteration | `for(auto [k, v] : m) { }` |
| `rbegin() / rend()` | Reverse iterators | `auto max_key = m.rbegin()->first;` |

**Common Patterns:**
```cpp
// Frequency counter
map<char, int> freq;
for(char c : "hello") freq[c]++;

// Find all elements with key >= 5
map<int, string> m = {{1, "a"}, {3, "b"}, {5, "c"}, {7, "d"}};
auto it = m.lower_bound(5);
while(it != m.end()) {
    cout << it->first << " " << it->second << "\n";
    ++it;
}

// Range query [3, 7]
auto lo = m.lower_bound(3);
auto hi = m.upper_bound(7);
for(auto it = lo; it != hi; ++it) {
    cout << it->first << "\n";
}

// Group by value
map<string, vector<int>> groups;
for(auto [k, v] : m) groups[v].push_back(k);
```

---

### Unordered_Map (`#include <unordered_map>`) — O(1) average

| API | Description | Example |
|-----|-------------|---------|
| `insert({key, val})` | Insert pair | `um.insert({5, "five"});` |
| `operator[key]` | Access/insert | `um[5] = "five";` |
| `find(key)` | Iterator or end() | `auto it = um.find(5);` |
| `count(key)` | 0 or 1 | `if(um.count(5)) { }` |
| `erase(key)` | Remove by key | `um.erase(5);` |
| `bucket_count()` | Number of buckets | `int buckets = um.bucket_count();` |
| `load_factor()` | Elements per bucket | `float lf = um.load_factor();` |
| `reserve(n)` | Pre-allocate buckets | `um.reserve(1000);` |

**Common Patterns:**
```cpp
// Two-sum problem
unordered_map<int, int> seen;
for(int x : arr) {
    if(seen.count(target - x)) {
        return {seen[target - x], i};
    }
    seen[x] = i;
}

// Anagram grouping
unordered_map<string, vector<string>> groups;
for(string word : words) {
    string sorted_word = word;
    sort(sorted_word.begin(), sorted_word.end());
    groups[sorted_word].push_back(word);
}
```

---

### Set (`#include <set>`) — Ordered unique elements, O(log n)

| API | Description | Example |
|-----|-------------|---------|
| `insert(val)` | Add element | `s.insert(5);` |
| `erase(val)` | Remove element | `s.erase(5);` |
| `find(val)` | Iterator or end() | `auto it = s.find(5);` |
| `count(val)` | 0 or 1 | `if(s.count(5)) { }` |
| `lower_bound(val)` | First >= val | `auto it = s.lower_bound(5);` |
| `upper_bound(val)` | First > val | `auto it = s.upper_bound(5);` |
| `begin() / end()` | Sorted iteration | `int min = *s.begin();` |
| `rbegin()` | Reverse begin | `int max = *s.rbegin();` |
| `size() / empty()` | Size | `bool is_empty = s.empty();` |

**Common Patterns:**
```cpp
// Remove duplicates from array
vector<int> arr = {1, 2, 2, 3, 3, 3};
set<int> unique_elements(arr.begin(), arr.end());

// Find range of elements [3, 7]
set<int> s = {1, 2, 3, 4, 5, 6, 7, 8, 9};
auto lo = s.lower_bound(3);
auto hi = s.upper_bound(7);
for(auto it = lo; it != hi; ++it) cout << *it << " ";

// Check if element exists
if(s.find(x) != s.end()) cout << "Found";

// Intersection of two sets
set<int> s1 = {1, 2, 3, 4};
set<int> s2 = {3, 4, 5, 6};
set<int> intersection;
set_intersection(s1.begin(), s1.end(), s2.begin(), s2.end(),
                 inserter(intersection, intersection.begin()));
```

---

### Multiset (`#include <set>`)

| API | Description | Example |
|-----|-------------|---------|
| `insert(val)` | Add element (allows duplicates) | `ms.insert(5); ms.insert(5);` |
| `count(val)` | Number of occurrences | `int freq = ms.count(5);` |
| `erase(find(val))` | Remove ONE occurrence | `ms.erase(ms.find(5));` |
| `erase(val)` | Remove ALL occurrences | `ms.erase(5);` |
| `equal_range(val)` | Range of equal elements | `auto [lo, hi] = ms.equal_range(5);` |

**Common Patterns:**
```cpp
// Frequency counter with sorted order
multiset<pair<int, char>> freq; // {count, char}
for(char c : "hello") {
    auto it = freq.find({freq.count(c), c});
    if(it != freq.end()) freq.erase(it);
    freq.insert({freq.count(c) + 1, c});
}

// Sliding window median
multiset<int> window;
for(int x : arr) {
    window.insert(x);
    if(window.size() > k) window.erase(window.find(old_element));
    auto mid = window.begin();
    advance(mid, window.size() / 2);
    cout << *mid << "\n";
}
```

---

### Unordered_Set (`#include <unordered_set>`)

| API | Description | Example |
|-----|-------------|---------|
| `insert(val)` | Add element | `us.insert(5);` |
| `find(val) / count(val)` | Check existence | `if(us.count(5)) { }` |
| `erase(val)` | Remove | `us.erase(5);` |

**Common Patterns:**
```cpp
// Check for duplicate within distance k
unordered_set<int> window;
for(int i = 0; i < arr.size(); i++) {
    if(window.count(arr[i])) return true;
    window.insert(arr[i]);
    if(window.size() > k) window.erase(arr[i - k]);
}
return false;
```

---

### Stack (`#include <stack>`)

| API | Description | Example |
|-----|-------------|---------|
| `push(val)` | Add to top | `st.push(5);` |
| `pop()` | Remove top | `st.pop();` |
| `top()` | Access top | `int x = st.top();` |
| `size() / empty()` | Size check | `while(!st.empty()) { }` |

**Common Patterns:**
```cpp
// Check balanced parentheses
bool isBalanced(string s) {
    stack<char> st;
    for(char c : s) {
        if(c == '(') st.push(c);
        else {
            if(st.empty()) return false;
            st.pop();
        }
    }
    return st.empty();
}

// Next greater element
vector<int> nextGreater(vector<int>& arr) {
    stack<int> st;
    vector<int> result(arr.size(), -1);
    for(int i = arr.size() - 1; i >= 0; i--) {
        while(!st.empty() && st.top() <= arr[i]) st.pop();
        if(!st.empty()) result[i] = st.top();
        st.push(arr[i]);
    }
    return result;
}

// Min stack with O(1) access
stack<pair<int,int>> st; // {value, min_so_far}
st.push({5, 5});
st.push({3, 3});
int current_min = st.top().second;
```

---

### Queue (`#include <queue>`)

| API | Description | Example |
|-----|-------------|---------|
| `push(val)` | Add to back | `q.push(5);` |
| `pop()` | Remove from front | `q.pop();` |
| `front()` | Access front | `int x = q.front();` |
| `back()` | Access back | `int x = q.back();` |
| `size() / empty()` | Size check | `while(!q.empty()) { }` |

**Common Patterns:**
```cpp
// BFS traversal
queue<int> q;
q.push(start);
while(!q.empty()) {
    int node = q.front();
    q.pop();
    for(int neighbor : adj[node]) {
        if(!visited[neighbor]) {
            visited[neighbor] = true;
            q.push(neighbor);
        }
    }
}

// Level order traversal with size trick
queue<TreeNode*> q;
q.push(root);
while(!q.empty()) {
    int levelSize = q.size();
    for(int i = 0; i < levelSize; i++) {
        TreeNode* node = q.front();
        q.pop();
        if(node->left) q.push(node->left);
        if(node->right) q.push(node->right);
    }

### Stack (`#include <stack>`)

| API | Description | Example |
|-----|-------------|---------|
| `push(val)` | Add to top | `st.push(5);` |
| `pop()` | Remove top | `st.pop();` |
| `top()` | Access top | `int x = st.top();` |
| `size() / empty()` | Size check | `while(!st.empty()) { }` |

**Common Patterns:**
```cpp
// Check balanced parentheses
bool isBalanced(string s) {
    stack<char> st;
    for(char c : s) {
        if(c == '(') st.push(c);
        else {
            if(st.empty()) return false;
            st.pop();
        }
    }
    return st.empty();
}

// Next greater element
vector<int> nextGreater(vector<int>& arr) {
    stack<int> st;
    vector<int> result(arr.size(), -1);
    for(int i = arr.size() - 1; i >= 0; i--) {
        while(!st.empty() && st.top() <= arr[i]) st.pop();
        if(!st.empty()) result[i] = st.top();
        st.push(arr[i]);
    }
    return result;
}

// Min stack with O(1) access
stack<pair<int,int>> st; // {value, min_so_far}
st.push({5, 5});
st.push({3, 3});
int current_min = st.top().second;
```

---

### Queue (`#include <queue>`)

| API | Description | Example |
|-----|-------------|---------|
| `push(val)` | Add to back | `q.push(5);` |
| `pop()` | Remove from front | `q.pop();` |
| `front()` | Access front | `int x = q.front();` |
| `back()` | Access back | `int x = q.back();` |
| `size() / empty()` | Size check | `while(!q.empty()) { }` |

**Common Patterns:**
```cpp
// BFS traversal
queue<int> q;
q.push(start);
while(!q.empty()) {
    int node = q.front();
    q.pop();
    for(int neighbor : adj[node]) {
        if(!visited[neighbor]) {
            visited[neighbor] = true;
            q.push(neighbor);
        }
    }
}

// Level order traversal with size trick
queue<TreeNode*> q;
q.push(root);
while(!q.empty()) {
    int levelSize = q.size();
    for(int i = 0; i < levelSize; i++) {
        TreeNode* node = q.front();
        q.pop();
        if(node->left) q.push(node->left);
        if(node->right) q.push(node->right);
    }
```

# C++ STL Practice Pack

## 1. 50 STL Questions Topic-Wise

---

## A. `vector` Questions
1. Insert an element at a given index in a vector.
2. Delete all occurrences of a value using erase-remove idiom.
3. Reverse a vector using `reverse`.
4. Rotate a vector left by `k` positions using `rotate`.
5. Find the maximum and minimum element using STL.
6. Sort a vector in descending order.
7. Remove duplicates from a sorted vector using `unique`.
8. Find the first element `>= x` using `lower_bound`.

---

## B. `string` Questions
9. Reverse a string.
10. Check whether a string is palindrome.
11. Find all occurrences of a substring using `find`.
12. Extract all words from a sentence using `stringstream`.
13. Remove spaces from a string.
14. Convert string to integer and integer to string.
15. Find the longest word in a sentence.
16. Count frequency of each character using STL.

---

## C. `pair` and `tuple` Questions
17. Store `{value, index}` pairs from an array.
18. Sort a vector of pairs by second element.
19. Merge intervals stored as pairs.
20. Use tuple to store `{value, row, col}` for matrix problems.

---

## D. `map` Questions
21. Count frequency of integers using `map`.
22. Print elements in sorted key order.
23. Find the first unique character using `map`.
24. Find the smallest key `>= x` using `lower_bound`.
25. Merge two maps containing frequencies.
26. Count word frequency in a sentence.

---

## E. `unordered_map` Questions
27. Solve two-sum using `unordered_map`.
28. Find the most frequent element in an array.
29. Group anagrams using `unordered_map<string, vector<string>>`.
30. Count duplicates in an array.
31. Find elements appearing more than `n/3` times.
32. Build an index map for fast lookups.

---

## F. `set` and `unordered_set` Questions
33. Remove duplicates from an array using `set`.
34. Find union of two arrays.
35. Find intersection of two arrays.
36. Find the first element `>= x` using `set::lower_bound`.
37. Find nearest smaller and greater element around `x`.
38. Detect duplicates using `unordered_set`.

---

## G. `stack` Questions
39. Check balanced parentheses.
40. Find next greater element.
41. Evaluate postfix expression.
42. Implement min stack.
43. Reverse a string using stack.
44. Remove adjacent duplicates from a string using stack.

---

## H. `queue` and `deque` Questions
45. Implement BFS traversal using `queue`.
46. Print level order traversal of a binary tree.
47. Sliding window maximum using `deque`.
48. Sliding window minimum using `deque`.

---

## I. `priority_queue` Questions
49. Find kth largest element using heap.
50. Find top `k` frequent elements using heap.

---

# 2. How to Use These 50 Questions

For each question, do this:

1. Identify the STL container or algorithm.
2. Write the syntax from memory.
3. Solve the problem in 10-20 lines if possible.
4. Revise the same question after 2 days.

---

# 3. STL Mini Projects with Full Requirements

---

## Project 1: Student Record Manager

### Goal
Practice:
- `vector`
- `map`
- `sort`
- `find_if`

### Features
- Add student
- Delete student by roll number
- Update student marks
- Search student by roll number
- Print all students
- Print topper
- Sort students by marks
- Sort students by name

### Data Model
Each student should have:
- `rollNumber`
- `name`
- `marks`

### STL to Use
- `vector<Student>`
- `map<int, Student>` or `unordered_map<int, Student>`
- `sort`
- `find_if`

### Practice Questions Inside Project
- How will you prevent duplicate roll numbers?
- How will you sort by marks descending?
- How will you find topper efficiently?
- How will you delete a student safely?
- When should you use `map` vs `vector` here?

### Extra Features
- Save topper separately
- Print average marks
- Print pass/fail students
- Search by partial name

---

## Project 2: Word Frequency Analyzer

### Goal
Practice:
- `string`
- `unordered_map`
- `vector`
- `sort`

### Features
- Input a paragraph
- Remove punctuation
- Convert to lowercase
- Split into words
- Count word frequency
- Print all word counts
- Print top 5 frequent words
- Print unique words in sorted order

### STL to Use
- `string`
- `unordered_map<string, int>`
- `vector<pair<string, int>>`
- `sort`

### Practice Questions Inside Project
- How will you clean punctuation?
- How will you split words?
- How will you sort by frequency?
- How will you handle uppercase/lowercase words?
- How will you find unique words?

### Extra Features
- Ignore stop words like `the`, `is`, `a`
- Find longest word
- Find shortest word
- Print words with frequency more than `k`

---

## Project 3: Library Book Tracker

### Goal
Practice:
- `map`
- `set`
- `vector`

### Features
- Add a new book
- Delete a book
- Search by book id
- Search by title
- Issue a book
- Return a book
- Print available books
- Print issued books

### Data Model
Each book should have:
- `id`
- `title`
- `author`
- `isIssued`

### STL to Use
- `map<int, Book>`
- `set<int>` for available or issued ids
- `vector<Book>` for listing

### Practice Questions Inside Project
- How will you prevent duplicate book ids?
- How will you track available books?
- How will you search quickly by id?
- How will you list all books in sorted order?
- Why use `set` for available books?

### Extra Features
- Count total issued books
- Show overdue list
- Print books by author
- Search title substring

---

## Project 4: Task Scheduler

### Goal
Practice:
- `queue`
- `priority_queue`
- `deque`

### Features
- Add normal task
- Add high-priority task
- Execute next task
- View next task
- Cancel task
- Print pending tasks
- Keep execution history

### Data Model
Each task should have:
- `taskId`
- `taskName`
- `priority`
- `timestamp`

### STL to Use
- `queue<Task>` for normal tasks
- `priority_queue<Task>` for urgent tasks
- `deque<Task>` for history or double-ended processing

### Practice Questions Inside Project
- Which task should run first?
- How will you design custom comparator?
- How will you cancel tasks?
- How will you store processed tasks?
- Why use `queue` and `priority_queue` together?

### Extra Features
- Retry failed tasks
- Delay task execution
- Show completed task history
- Count pending urgent tasks

---

## Project 5: Browser History Simulator

### Goal
Practice:
- `stack`

### Features
- Visit new page
- Go back
- Go forward
- Show current page
- Print back history
- Print forward history

### STL to Use
- `stack<string>` for back history
- `stack<string>` for forward history

### Logic
- On visiting new page:
  - push current page to back stack
  - clear forward stack
- On back:
  - move current page to forward stack
  - pop from back stack
- On forward:
  - move current page to back stack
  - pop from forward stack

### Practice Questions Inside Project
- Why are two stacks needed?
- What happens when visiting new page after back?
- How will you handle empty history?
- How will you display current page?
- What is the real-life stack pattern here?

### Extra Features
- Limit history size
- Store timestamps
- Search visited URLs
- Save session

---

## Project 6: Sliding Window Analyzer

### Goal
Practice:
- `deque`
- `vector`
- STL algorithms

### Features
- Input array and window size `k`
- Print sliding window maximum
- Print sliding window minimum
- Print window sum
- Print first negative number in each window

### STL to Use
- `deque<int>`
- `vector<int>`
- `accumulate`

### Practice Questions Inside Project
- Why does `deque` work for max/min window?
- Why store indices instead of values?
- How do you remove out-of-window elements?
- How do you maintain decreasing order?
- What is time complexity?

---

## Project 7: Top-K Dashboard

### Goal
Practice:
- `priority_queue`
- `unordered_map`
- `vector`
- `sort`

### Features
- Read list of numbers or words
- Count frequencies
- Print top `k` frequent items
- Print kth largest element
- Print kth smallest element

### STL to Use
- `unordered_map<int, int>` or `unordered_map<string, int>`
- `priority_queue`
- `vector<pair<int,int>>`

### Practice Questions Inside Project
- When use heap vs sort?
- How to maintain heap of size `k`?
- How to get top `k` frequent items?
- How to design min heap?
- What is complexity difference?

---

# 4. Best Weekly Plan

## Week 1
- Solve Questions 1-10
- Build Project 1

## Week 2
- Solve Questions 11-20
- Build Project 2

## Week 3
- Solve Questions 21-30
- Build Project 3

## Week 4
- Solve Questions 31-40
- Build Project 4

## Week 5
- Solve Questions 41-50
- Build Project 5 or 6

---

# 5. Best Revision Method

For every STL topic, revise in this order:

1. Purpose
2. Syntax
3. Top 5 APIs
4. One problem
5. One mini project use case

Example:
- `unordered_map`
- purpose: fast key-value lookup
- syntax: `unordered_map<int,int> mp;`
- APIs: `[]`, `find`, `count`, `erase`, `insert`
- problem: frequency count
- project use: word frequency analyzer

---

# 6. Final Rule

If you want to remember STL properly:

- do not only read
- write syntax from memory
- solve repeated pattern questions
- build mini projects using same STL again and again

That is the fastest method.

---

If needed next, I can give:
1. **answers/hints for all 50 questions**
2. **full code for 1 STL mini project**
3. **30-day STL roadmap**
4. **1-page STL revision sheet**