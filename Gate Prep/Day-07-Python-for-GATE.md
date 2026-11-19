# GATE DA · Day 7 of 20
## Python for GATE: predicting the output, plus Big-O basics

*About a 30-minute read. Answers to the practice and recall questions are in the collapsible section at the bottom. Every snippet was run in Python 3, and the outputs shown are the real ones.*

---

## 1. How GATE asks this

Programming, DSA and Algorithms is about **16 marks** a paper. The Python part usually looks like this:
- **NAT:** "What is printed?" where the answer is a number: a sum, a count, the return value of a recursive function, or the number of function calls.
- **MCQ:** four candidate outputs for a 6–12 line snippet.
- **MSQ:** "which statements about this code are true?" or "which of these snippets print X?"

GATE's code favours **aliasing, default arguments, slicing, // and % with negatives, closures and recursion.** It doesn't test library trivia. You have no interpreter in the exam, so trace by hand: write down each variable and update it line by line.

---

## 2. The concepts, each with a snippet

### 2.1 Names, objects, mutability and aliasing

A variable is a **name bound to an object**. Assignment never copies; it only binds another name to the same object.

- **Mutable:** list, dict, set, and your own class instances.
- **Immutable:** int, float, str, tuple, frozenset, bool, None.

```python
a = [1, 2, 3]; b = a; b.append(4)
print(a)                 # [1, 2, 3, 4]
t = (1, [2, 3]); t[1].append(4)
print(t)                 # (1, [2, 3, 4])
```

A tuple can't be reassigned element by element, but a list **inside** a tuple can still change.

**`x = x + [4]` vs `x += [4]`:**
- `x = x + [4]` builds a **new** list and rebinds x.
- `x += [4]` **extends the same list in place.**

```python
x=[1,2,3]; y=x; x=x+[4]; print(y)   # [1, 2, 3]
x=[1,2,3]; y=x; x+=[4];   print(y)   # [1, 2, 3, 4]
```

For immutables (int, str, tuple), += always makes a new object.

### 2.2 Default mutable arguments

Default values are evaluated **once**, when the `def` line runs, and then shared by every call.

```python
def f(x, acc=[]):
    acc.append(x); return acc
print(f(1))      # [1]
print(f(2))      # [1, 2]   same list as before!
print(f(3, []))  # [3]      fresh list passed in
```

The usual fix is `acc=None`, then `if acc is None: acc = []` inside the function.

### 2.3 Shallow vs deep copy

**Shallow copies** make a new outer list but share the inner objects. These are all shallow:
- `x[:]`
- `list(x)`
- `x.copy()`
- `copy.copy(x)`

```python
import copy
x = [[1,2],[3,4]]; s = x[:]; d = copy.deepcopy(x)
s[0].append(9); s.append([5])
print(x)   # [[1, 2, 9], [3, 4]]   inner list shared, outer append not
print(d)   # [[1, 2], [3, 4]]      fully independent
g = [[0]*3]*3; g[0][0] = 1
print(g)   # [[1, 0, 0], [1, 0, 0], [1, 0, 0]]   three refs to ONE row
```

`[[0]*3]*3` is the classic grid trap. Use `[[0]*3 for _ in range(3)]` instead.

### 2.4 Slicing

The syntax is `seq[start:stop:step]`. Start is included, stop is excluded, and **slicing never raises IndexError.**

```python
s = "GATEDA2027"
s[2:6]      # 'TEDA'
s[-4:]      # '2027'
s[::-1]     # '7202ADETAG'
s[1:8:3]    # 'AD0'   indices 1, 4, 7
s[-1:-5:-1] # '7202'
s[5:2]      # ''      empty, no error
L = [0,1,2,3,4,5]; L[1:3] = [9]
print(L)    # [0, 9, 3, 4, 5]   slice assignment can change the length
```

**Defaults:**
- With a positive step, the defaults are start 0 and stop len.
- With a **negative step**, the defaults are start = last index and stop = before index 0.

So `x[::-2]` starts at the end. And `x[10:]` gives `[]`, but `x[10]` raises IndexError.

### 2.5 Arithmetic: /, //, %, **

```python
7//2, -7//2      # 3, -4     // FLOORS toward −∞, not toward 0
7%3, -7%3, 7%-3  # 1, 2, -2  % takes the sign of the DIVISOR
7/2, 10/5        # 3.5, 2.0  / always returns a float
2**3**2          # 512       ** is right-associative: 2**(3**2)
-2**2, (-2)**2   # -4, 4     ** binds tighter than unary minus
int(-3.7)        # -3        int() truncates toward 0
round(2.5), round(3.5)   # 2, 4   banker's rounding: ties go to even
```

The identity **a == (a//b)*b + a%b** always holds. Use it to check yourself with negative numbers.

**Precedence, high to low:**
1. `**`
2. unary `-`
3. `* / // %`
4. `+ -`
5. comparisons
6. `not`
7. `and`
8. `or`

### 2.6 Strings (immutable)

```python
s = "banana"
s.replace("a","o",2)   # 'bonona'   only the first 2 replaced
s.find("na")           # 2          returns -1 if not found; .index raises instead
s.count("an")          # 2          non-overlapping matches
s.split("a")           # ['b', 'n', 'n', '']   trailing empty string!
"-".join(["a","b"])    # 'a-b'
3*'ab'                 # 'ababab'
```

`s[0] = 'B'` raises a TypeError, because strings are immutable. String methods return **new** strings; the original stays the same.

### 2.7 Comprehensions

```python
[x*x for x in range(6) if x%2]          # [1, 9, 25]
{x%3 for x in range(10)}                # {0, 1, 2}   a set, so no duplicates
{k:v for k,v in zip("abc", range(3))}   # {'a': 0, 'b': 1, 'c': 2}
[(i,j) for i in range(2) for j in range(i,3)]
# [(0, 0), (0, 1), (0, 2), (1, 1), (1, 2)]   loops nest left to right
```

`(x for x in …)` with round brackets is a **generator**, not a tuple. `sum(x for x in range(5))` gives 10.

### 2.8 Scope: LEGB, global, nonlocal

Python looks a name up in this order:
1. **L**ocal
2. **E**nclosing function
3. **G**lobal (module)
4. **B**uilt-in

**Assigning to a name anywhere inside a function makes it local for the whole function.** Reading it before that assignment raises UnboundLocalError. To rebind an outer name, declare it `global` or `nonlocal` first.

```python
x = 10
def outer():
    x = 20
    def inner():
        nonlocal x      # rebinds outer's x, not the global one
        x += 1
        return x
    return inner
fn = outer(); print(fn(), fn(), x)   # 21 22 10
```

The closure keeps outer's x alive between calls, and the global x is untouched.

### 2.9 Closures and late binding

A closure captures **variables, not values.** The value is looked up **when the function is called.**

```python
fs = [lambda: i for i in range(3)]
print([h() for h in fs])        # [2, 2, 2]   all see the final i
fs = [lambda i=i: i for i in range(3)]
print([h() for h in fs])        # [0, 1, 2]   default captures the value now
```

### 2.10 Recursion tracing

```python
def r(n):
    if n <= 1: return 1
    return r(n-1) + r(n-2)
# r(0..5) = 1, 1, 2, 3, 5, 8.  Calls made by r(5): 15
```

The call count follows C(n) = 1 + C(n−1) + C(n−2), with C(0) = C(1) = 1. That gives 1, 1, 3, 5, 9, 15.

**Head vs tail position:** code before the recursive call runs on the way **down**. Code after it runs on the way **back up**.

```python
def p(n):
    if n == 0: return
    print(n, end=" "); p(n-1); print(n, end=" ")
p(3)    # 3 2 1 1 2 3
```

**Digit tricks:**
- `n % 10` is the last digit and `n // 10` drops it. `f(n) = n%10 + f(n//10)` is a digit sum.
- `n % 2` with `n // 2` gives the binary digits. `f(13)` counting `n%2 + f(n//2)` returns 3, the number of 1-bits in 1101.

### 2.11 Generators and yield

A function with `yield` returns a lazy iterator. Each `next()` resumes it from the last `yield`. **A generator can only be used once.**

```python
def gen(n):
    for i in range(n):
        yield i*i
g = gen(4)
print(next(g), next(g), list(g), list(g))   # 0 1 [4, 9] []
```

The second `list(g)` is empty because the generator is already exhausted. `range`, `map`, `filter`, `zip` and `enumerate` are lazy too. In Python 3 they don't return lists.

### 2.12 *args and **kwargs

```python
def h(a, b=2, *args, **kw): return (a, b, args, kw)
h(1)                # (1, 2, (), {})
h(1, 3, 4, 5, k=6)  # (1, 3, (4, 5), {'k': 6})
```

**Parameter order:** positional, then defaults, then `*args`, then keyword-only, then `**kwargs`.
- `*args` collects the extra positional arguments into a **tuple.**
- `**kw` collects the extra keyword arguments into a **dict.**
- At a call site, `f(*lst)` unpacks a list and `f(**d)` unpacks a dict.

### 2.13 is vs ==, truthiness, short-circuiting

- `==` compares **values.**
- `is` compares **identity** (the same object).
  - `[1,2] == [1,2]` → True.
  - `[1,2] is [1,2]` → False.
  - Always compare None with `is None`.
- Small ints (−5 to 256) and some strings are cached, so `is` may *happen* to return True for them. Never rely on it.

**Falsy values:** `0`, `0.0`, `''`, `[]`, `{}`, `set()`, `None`, `False`. Everything else is truthy, including `'False'` and `[0]`.

**`and` / `or` return one of their OPERANDS, not True/False:**
- `a or b` → a if a is truthy, else b.
- `a and b` → a if a is falsy, else b.
- They stop as soon as the answer is known (short-circuit).

```python
0 or "x"          # 'x'
[] and 5          # []
3 and 4           # 4
"" or [] or 0     # 0    the last one, since all are falsy
not []            # True
1 < 3 > 2         # True  chained: (1<3) and (3>2)
```

### 2.14 Dicts, sorting, zip, enumerate, map, filter

- **Dicts keep insertion order** (since Python 3.7). Updating an existing key **doesn't move it.**
- `d.get(k, default)` never raises. `d[k]` raises KeyError.

```python
d = {"b":2, "a":1}; d["c"] = 3; d["b"] = 9
list(d)            # ['b', 'a', 'c']
w = ["kiwi","fig","banana","apple"]
sorted(w, key=len)                     # ['fig', 'kiwi', 'apple', 'banana']
sorted(w, key=lambda s:(-len(s), s))   # ['banana', 'apple', 'kiwi', 'fig']
```

- `sorted()` returns a **new** list. `list.sort()` sorts **in place and returns None**, so `x = x.sort()` sets x to None.
- **Sorting is stable:** items with equal keys keep their original order.
- To sort descending by one field and ascending by another, use a **tuple key** with a minus sign.

```python
list(zip([1,2,3], "ab"))             # [(1, 'a'), (2, 'b')]  stops at the shortest
list(enumerate("ab", 1))             # [(1, 'a'), (2, 'b')]
list(map(lambda x: x*2, [1,2]))      # [2, 4]
list(filter(None, [0,1,"",2]))       # [1, 2]   None keeps only truthy items
```

### 2.15 Classes and inheritance

```python
class A:
    n = 0                                  # class attribute, shared by all instances
    def __init__(self): A.n += 1; self.v = 1
    def show(self): return "A" + str(self.v)
class B(A):
    def __init__(self): super().__init__(); self.v = 2
    def show(self): return "B" + super().show()
o = B(); print(o.show(), A.n, isinstance(o, A))   # BA2 1 True
```

- Method lookup goes **instance → class → parent classes** (the MRO).
- `self.v` is per instance. `A.n` is shared by all instances.
- `super()` calls the parent's version, but `self` is still the B object. That's why it prints `A2`, not `A1`.
- Assigning `self.n = …` creates an **instance** attribute that hides the class one. It doesn't change `A.n`.

### 2.16 Complexity basics

**The notations:**
- **O(g):** an upper bound. f ≤ c·g for large n.
- **Ω(g):** a lower bound.
- **Θ(g):** both at once, a tight bound.
- Example: 3n² + 5n is Θ(n²). It's also O(n³), but not Θ(n³).

**Growth order:** 1 < log n < √n < n < n log n < n² < n³ < 2ⁿ < n! < nⁿ.

**Loop patterns:**
- Two nested loops to n: n².
- Halving i each time (`i //= 2`): log n.
- Inner loop running from i to n: Σ(n − i) ≈ n²/2, which is still Θ(n²).
- Inner `j *= 2` inside an outer loop to n: n log n.

**Python operation costs:**

| Operation | Cost |
|---|---|
| list `append`, `pop()` (end), index `L[i]`, `len` | O(1) (append is amortised) |
| list `insert(0,x)`, `pop(0)`, `x in L`, `remove`, `index` | **O(n)** |
| slice `L[a:b]` | O(b − a) |
| `sorted` / `.sort` (Timsort) | O(n log n), O(n) if nearly sorted |
| dict/set get, set, `in`, delete | **O(1) average**, O(n) worst case |
| string `+` in a loop | O(n²) in total; use `''.join` |
| `collections.deque` appendleft/popleft | O(1) |

---

## 3. Six fully traced "predict the output" examples

**Example 1: default list plus aliasing**
```python
def add(item, bag=[]):
    bag.append(item)
    return bag
b1 = add(1); b2 = add(2); b3 = add(3, [])
print(b1, b2, b3, b1 is b2)
```
**Trace:**
- The default list D is created once, when `def` runs.
- `add(1)` appends to D, so D = [1], and b1 is D.
- `add(2)` appends to the **same** D, so D = [1, 2], and b2 is D. b1 changes too.
- `add(3, [])` uses a fresh list, so b3 = [3].

**Output:** `[1, 2] [1, 2] [3] True`

**Example 2: floor division and modulo with negatives**
```python
print(-17//5, -17%5, 17//-5, 17%-5, 2**-1, 9**0.5, 5//2*2+5%2)
```
**Trace:**
- −17/5 = −3.4, which floors to **−4**. Then −17 − (−4·5) = **3**, positive like the divisor.
- 17/−5 = −3.4, which floors to **−4**. Then 17 − (−4·−5) = **−3**, negative like the divisor.
- `2**-1` = **0.5** (a float).
- `9**0.5` = **3.0** (a float, even though it's a whole number).
- `5//2*2 + 5%2` = 2·2 + 1 = **5**. This is the a == (a//b)·b + a%b identity.

**Output:** `-4 3 -4 -3 0.5 3.0 5`

**Example 3: closure counters (nonlocal)**
```python
def make():
    count = 0
    def step(k=1):
        nonlocal count
        count += k
        return count
    return step
s1 = make(); s2 = make()
print(s1(), s1(5), s2(), s1())
```
**Trace:**
- Each `make()` call creates its **own** `count`. So s1 and s2 are independent.
- s1() → 1. s1(5) → 6. s2() → 1 (s2's own counter). s1() → 7.

**Output:** `1 6 1 7`

Without `nonlocal`, `count += k` raises UnboundLocalError.

**Example 4: recursion with a step of 2**
```python
def g(n):
    if n <= 0: return 0
    return n + g(n-2)
print(g(7), g(8))
```
**Trace:**
- g(7) = 7 + g(5) = 7 + 5 + g(3) = … = 7 + 5 + 3 + 1 + g(−1) = **16**.
- g(8) = 8 + 6 + 4 + 2 + g(0) = **20**.
- The base case is `n <= 0`, so odd n stops safely at −1. With `== 0`, odd n would recurse forever, until it hits RecursionError.

**Output:** `16 20`

**Example 5: changing a list while iterating over it**
```python
L = [2, 4, 6, 8]
for x in L:
    if x % 2 == 0:
        L.remove(x)
print(L)
```
**Trace** (the for loop walks an internal index):
- Index 0: x = 2. Remove it → [4, 6, 8].
- Index 1: x = **6** (4 was skipped because it shifted into index 0). Remove it → [4, 8].
- Index 2: past the end, so the loop stops.

**Output:** `[4, 8]`

Every item was even, yet half survive. The fix is to loop over a copy, `for x in L[:]`, or use a comprehension.

**Example 6: class attribute vs instance attribute**
```python
class C:
    items = []
    count = 0
    def add(self, x):
        self.items.append(x)
        self.count += 1
a = C(); b = C(); a.add(1); b.add(2)
print(C.items, a.count, b.count, C.count)
```
**Trace:**
- `self.items.append` **changes** the one shared class list. So C.items = [1, 2].
- `self.count += 1` reads C.count (0), then **assigns** to `self.count`. That creates an instance attribute equal to 1 on a, and separately on b.
- C.count stays 0.

**Output:** `[1, 2] 1 1 0`

**Bonus: sorting and generators.** Given `scores = {"ana":82, "raj":91, "li":82, "bo":77}`:
- `sorted(scores, key=lambda k:(-scores[k], k))[:3]` → `['raj', 'ana', 'li']`. Highest score first; the tie at 82 is broken alphabetically.
- With `g = (k for k,v in scores.items() if v>80)`:
  - `next(g)` → 'ana'.
  - `len(list(g))` → **2** (raj and li remain).

---

## 4. Common traps

1. **Assignment never copies.** `b = a` is the same object. `a[:]` copies one level only. Use `deepcopy` for nested structures.
2. **Mutable default arguments** persist between calls.
3. **`[[0]*n]*m`** creates m references to one row.
4. **`x += [...]` changes the list in place. `x = x + [...]` rebinds x.** Inside a function, `L += [1]` changes the caller's list, but `L = L + [2]` doesn't.
5. **`//` floors and `%` follows the sign of the divisor.** −7//2 = −4, not −3. `int()` truncates instead.
6. **`2**3**2 = 512`** (right-associative), and **`-2**2 = -4`.**
7. **`/` always returns a float.** `10/5` is 2.0, and it prints as `2.0`.
8. **Late-binding lambdas** in a loop all see the final value. Fix it with a default argument, `i=i`.
9. **Generators run once.** A second `list(g)` gives `[]`.
10. **`list.sort()` returns None.** `x = x.sort()` sets x to None.
11. **Changing a list while looping over it** skips elements.
12. **`and`/`or` return operands,** not bools. `[] or 0` → 0.
13. **`self.attr += …` on a class attribute** creates an instance attribute, unless the class attribute is a mutable object changed in place (like `append`).
14. **`is` vs `==`:** never use `is` to compare values. Small ints being cached is an implementation detail.
15. **Dict update keeps the key's position.** Delete and re-insert it to move it to the end.
16. **Recursion base cases:** check that odd or even inputs actually reach the base case.
17. **`round(2.5)` = 2** (banker's rounding).

---

## 5. Cheat card

**Mutability**
- Mutable: list, dict, set, objects. Immutable: int, float, str, tuple.
- Shallow copies: `[:]`, `list()`, `.copy()`. Deep copy: `copy.deepcopy`.

**Arithmetic**
- `//` floors. `%` takes the divisor's sign. a = (a//b)·b + a%b.
- `/` gives a float. `**` is right-associative and binds tighter than unary minus. `int()` truncates.

**Slicing**
- `[start:stop:step]`, with stop excluded and no IndexError.
- With a negative step, the defaults run from the end. `s[::-1]` reverses.

**Scope**
- LEGB lookup. Assigning inside a function makes the name local. `global` rebinds a module name; `nonlocal` rebinds an enclosing one.
- Closures capture variables, not values, so they bind late.

**Functions**
- Defaults are evaluated once. `*args` is a tuple, `**kw` is a dict.
- Generators are lazy and single-pass. `next()` resumes after the last `yield`.

**Truthiness and operators**
- Falsy: 0, '', [], {}, set(), None, False.
- `a or b` / `a and b` return an operand and short-circuit.
- Chained comparisons: `a < b < c`.

**Dicts and sorting**
- Dicts keep insertion order. `d.get(k, default)` never raises.
- `sorted(…, key=…)` returns a new list and is stable; `.sort()` returns None. Use a tuple key with a minus sign for mixed sort order.
- zip stops at the shortest input. map, filter, zip and range are lazy.

**Classes**
- Lookup goes instance → class → parent. Class attributes are shared. `super()` keeps the same `self`.

**Costs**
- list: append and pop() are O(1). insert(0), pop(0) and `in` are O(n).
- dict/set: `in`, get and set are O(1) on average. Sorting is O(n log n).
- Growth: 1 < log n < n < n log n < n² < 2ⁿ < n!.
- Θ is a tight bound, O an upper bound, Ω a lower bound.

---

## 6. Practice

**Q1 (MCQ).** What does this print?
```python
def m(L):
    L += [1]
    L = L + [2]
    return L
z = [0]; print(m(z), z)
```
(a) `[0, 1, 2] [0, 1, 2]`
(b) `[0, 1, 2] [0, 1]`
(c) `[0, 1, 2] [0]`
(d) `[0, 2] [0]`

**Q2 (NAT).** How many times is `r` called in total, including the first call, when `r(5)` runs?
```python
def r(n):
    if n <= 1: return 1
    return r(n-1) + r(n-2)
```

**Q3 (MCQ).** What does this print?
```python
x = [1, 2, 3, 4, 5]
print(x[::-2], x[-2:1:-1], x[10:])
```
(a) `[5, 3, 1] [4, 3] []`
(b) `[5, 3, 1] [4, 3, 2] []`
(c) `[1, 3, 5] [4, 3] IndexError`
(d) `[5, 3] [4, 3] []`

**Q4 (NAT).** What does `print(k(98765))` print?
```python
def k(n):
    return n if n < 10 else k(n // 10) + n % 10
```

**Q5 (MCQ).** What does this print?
```python
fs = []
for i in range(3):
    fs.append(lambda x: x + i)
print([q(10) for q in fs])
```
(a) `[10, 11, 12]`  (b) `[12, 12, 12]`  (c) `[10, 10, 10]`  (d) NameError

**Q6 (MSQ).** Which statements are TRUE in Python 3?
(a) After `t = (1, [2]); t[1] += [3]`, a TypeError is raised, but t is now `(1, [2, 3])`
(b) `sorted(lst)` returns None and sorts lst in place
(c) Average-case lookup `key in d` for a dict d is O(1)
(d) `-7 // 2` evaluates to `-3`

### Recall: Day 6

**R1.** What is the coefficient of x² in the Maclaurin series of cos x?

**R2.** f(x) = x³ − 3x on [0, 3]. What is its global maximum?

**R3.** Gradient descent on f(x) = (x − 3)² with η = 0.5, starting from x₀ = 0. What is x₁?

---

**Next, Day 8:** data structures. Stacks (including infix to postfix), queues, linked lists, tree traversals, BSTs, heaps and hash tables with probing.

---

<details>
<summary><b>Answers</b> (click to open)</summary>

**Q1: (b).**
- `L += [1]` changes the caller's list in place, so z = [0, 1].
- `L = L + [2]` makes a new local list [0, 1, 2], which is what gets returned. z stays [0, 1].

**Q2: 15.** The call count C(n) = 1 + C(n−1) + C(n−2), with C(0) = C(1) = 1. So the counts are 1, 1, 3, 5, 9, 15.

**Q3: (a).**
- `x[::-2]` takes indices 4, 2, 0.
- `x[-2:1:-1]` starts at index 3 and stops before index 1, giving [4, 3].
- An out-of-range slice returns [] instead of raising an error.

**Q4: 35.** It's a digit sum: 9 + 8 + 7 + 6 + 5.

**Q5: (b).** The closures capture the variable i, not its value. When the lambdas run, i = 2, so each returns 10 + 2.

**Q6: (a), (c).**
- (a) is true: `+=` first extends the list in place, then the reassignment into the tuple fails.
- (b) is false: that describes `.sort()`. `sorted` returns a new list.
- (c) is true.
- (d) is false: −7 // 2 = −4, because // floors.

**Recall**
- **R1:** −1/2.
- **R2:** f(3) = 18. The critical point x = 1 is a local min with f(1) = −2, and f(0) = 0.
- **R3:** x₁ = 0 − 0.5·2(0 − 3) = 3. η = 1/(2a) reaches the minimum in one step.

</details>
