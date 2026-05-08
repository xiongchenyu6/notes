---
title: "Data Structure"
date: 2021-11-20
---

# stack

# queue

# array

## dynamic array

```cpp
vector<int> v{0, 1, 2, 3, 4, 5, 6};

for (auto it = v.begin(), e = v.end(); it != e; ++it) {
    const int x = *it;
    cout << x << "," << v.capacity() << endl;
    v.push_back(x);
}
```

# list

## reverse list

``` cpp
// a -> b  -> c
Node* a = new Node{1};
Node* b = new Node{2};
Node* c = new Node{3};
a->next = b;
b->next = c;

auto pre = a;
auto cur = a->next;
Node* tmp;
while (cur != nullptr) {
    tmp = cur->next;
    cur->next = pre;
    pre = cur;
    cur = tmp;
}
a->next = nullptr;
while (c != nullptr) {
    cout << c->val << endl;
    c = c->next;
}
```

# heap

## min heap

```cpp
using namespace std;
priority_queue<int, vector<int>, greater<int> > a;
a.push(1);
a.push(2);
a.push(3);
cout << a.top() << endl;
```

```text
1
```

# tree map

```cpp
map<int, int> m;
m.emplace(1, 23);
```

# hash map

```cpp
unordered_map<int, int> hash_map;
hash_map.emplace(1, 23);
cout << hash_map[1] << endl;
```

```text
23
```
