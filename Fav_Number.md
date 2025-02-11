# Fav Number - CTF Write-up

## Category: Misc (200 points)

### Description
Help me remember my favourite number.

**Challenge Connection:**  
`nc favno.ctf.dscjssstuniv.in 9100`

---

## Solution

Upon connecting to the server at `nc favno.ctf.dscjssstuniv.in 9100`, we were asked to guess a **favourite number** within the range **[1, 10^100]**. The server provided feedback after each guess, indicating whether our guess was **smaller or higher** than the target number.

### Initial Thought
Applying **Binary Search** seemed like an obvious choice, as it would allow us to find the number in approximately **log2(10^100) ≈ 333 attempts**, making it efficient.

However, there were two major challenges:
1. **Time Constraint** – The connection had a strict time limit, meaning we had to find the number quickly.
2. **Randomized Number** – The favourite number changed on every new connection, making it impossible to carry over previous guesses.

### Approach
1. **Binary Search** was used to systematically narrow down the range.
2. **Python Automation was too slow**, so we switched to **C++**, which provided faster execution and allowed us to complete the guessing within the given time limit.

### C++ Implementation
We implemented an automated binary search using C++ for faster execution. Here’s a snippet of the approach:

```cpp
cpp_int low = 1, high = cpp_int("1" + string(100, '0'));
cpp_int mid = (low + high) / 2;
write(socket, buffer(mid.str() + "\n"), error);
```

This allowed us to efficiently search the number within the given time limit.

### Final Flag
```
dscctf{maths2_prolly_fun_lollz!!!}
```

---

