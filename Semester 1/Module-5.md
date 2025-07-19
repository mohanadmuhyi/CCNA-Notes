# 🧮 CCNA - Module 5: Number Systems

## 🎯 Objective  
Understand how to convert between binary, decimal, and hexadecimal number systems — and why they matter in networking (especially IP & MAC addressing).

---

## 5.1 ⚙️ Binary Number System

### 🔢 Basics:
- **Binary** = base-2 → uses only `0` and `1`
- **Decimal** = base-10 → uses digits `0` to `9`
- **IPv4 addresses** are based on binary — 32 bits, written in 4 groups (octets) of 8 bits

Each **octet = 8 bits = 1 byte**, and the whole address looks like this:

```
Binary : 11000000.10101000.00001011.00001010  
Decimal: 192     .168     .11      .10
```

---

### 🧠 Positional Notation in Decimal
Each position represents a power of 10:

| Position | Value  |
|----------|--------|
| 10³      | 1000   |
| 10²      | 100    |
| 10¹      | 10     |
| 10⁰      | 1      |

> Example: `1234 = 1×1000 + 2×100 + 3×10 + 4×1`

---

### 🧠 Positional Notation in Binary

Each bit has a positional value (right to left) as powers of 2:

| Bit Pos | 7  | 6  | 5  | 4  | 3  | 2  | 1  | 0  |
|---------|----|----|----|----|----|----|----|----|
| Value   |128 | 64 | 32 | 16 | 8  | 4  | 2  | 1  |

> Example: Binary `11000000` = `1×128 + 1×64 = 192`

---

### 📥 Convert Binary to Decimal

1. Multiply each bit by its positional value  
2. Add the results

Example:
```
11000000 → 1×128 + 1×64 = 192  
10101000 → 1×128 + 1×32 + 1×8 = 168  
00001011 → 1×8 + 1×2 + 1×1 = 11  
00001010 → 1×8 + 1×2 = 10  
```

✅ Result: **192.168.11.10**

---

### 📤 Convert Decimal to Binary

Steps:
1. Start with leftmost value (128) and check if your number ≥ it  
2. If yes → put 1 and subtract  
3. If no → put 0 and move to next lower value  
4. Repeat down to 1

Example: Convert 168  
```
168 ≥ 128 → 1 (168-128 = 40)  
40 < 64 → 0  
40 ≥ 32 → 1 (40-32 = 8)  
8 < 16 → 0  
8 = 8 → 1 (8-8 = 0)  
Remaining → 0s  
→ 168 = 10101000
```

---

### 📌 IPv4 Addressing
- IPv4 = 32-bit address used by routers and computers
- Humans read it in **dotted decimal**, but devices use **binary**

---

## 5.2 🧪 Hexadecimal Number System

### 🔢 Basics:
- **Hexadecimal** = base-16  
- Digits: `0–9` and `A–F` (A = 10, B = 11, ..., F = 15)  
- **1 hex digit = 4 bits**
- Used in:
  - **IPv6** addresses  
  - **MAC addresses**

---

### 💡 IPv6 and Hex
- IPv6 = 128-bit address  
- Every **4 bits** becomes 1 hex digit → total of **32 hex digits**  
- Grouped into **8 blocks** (hextets) separated by colons:

Example:
```
2001:0db8:0000:0042:0000:8a2e:0370:7334
```

---

### 🔁 Decimal to Hexadecimal

Steps:
1. Convert decimal to 8-bit binary  
2. Split binary into groups of 4 bits (nibbles)  
3. Convert each nibble to hex

Example: Decimal `168`  
→ Binary: `10101000`  
→ Groups: `1010` = A, `1000` = 8  
✅ Result: **A8**

---

### 🔁 Hexadecimal to Decimal

Steps:
1. Convert each hex digit to 4-bit binary  
2. Combine into 8-bit binary  
3. Convert binary to decimal

Example: Hex `D2`  
→ D = `1101`, 2 = `0010` → Binary = `11010010`  
→ Decimal = `210`

---

## 🧠 Module Summary

| Number System | Base | Digits Used         | Used In                  |
|---------------|------|---------------------|---------------------------|
| Binary        | 2    | 0, 1                | IPv4, Subnetting         |
| Decimal       | 10   | 0–9                 | IP in human-readable form|
| Hexadecimal   | 16   | 0–9, A–F            | IPv6, MAC addresses      |

---

## ✅ Recap & Tips

- Binary is the native language of devices  
- **1 byte = 8 bits**  
- **1 hex digit = 4 bits**  
- Practice both directions:  
  - Decimal ↔ Binary  
  - Binary ↔ Hex  
  - Decimal ↔ Hex  

---

