# push_swap

## Overview

`push_swap` is a project that involves sorting a list of integers using a limited set of operations on two stacks (`a` and `b`). The goal is to sort the integers in ascending order on stack `a` (smallest on top) while minimizing the number of operations performed.

The program takes a list of integers as input (either as separate arguments or space-separated in quotes) and outputs a sequence of operations to sort the stack. The efficiency is measured by the number of operations required:
- For 3 numbers: ≤ 3 operations
- For 5 numbers: ≤ 12 operations
- For 100 numbers: < 700 operations (5 points) to < 900 (4 points)
- For 500 numbers: < 5500 operations (5 points) to < 7000 (4 points)

## Allowed Operations

The program uses only the following operations:

| Operation | Description                          | Effect on stacks                          |
|-----------|--------------------------------------|-------------------------------------------|
| `sa`      | Swap a                               | Swap the first two elements of stack `a` |
| `sb`      | Swap b                               | Swap the first two elements of stack `b` |
| `ss`      | Swap a + Swap b                      | Perform `sa` and `sb` simultaneously      |
| `pa`      | Push a                               | Take the first element from `b` and put it on top of `a` |
| `pb`      | Push b                               | Take the first element from `a` and put it on top of `b` |
| `ra`      | Rotate a                             | Shift all elements of `a` up by 1 (top becomes bottom) |
| `rb`      | Rotate b                             | Shift all elements of `b` up by 1         |
| `rr`      | Rotate a + Rotate b                  | Perform `ra` and `rb` simultaneously      |
| `rra`     | Reverse rotate a                     | Shift all elements of `a` down by 1 (bottom becomes top) |
| `rrb`     | Reverse rotate b                     | Shift all elements of `b` down by 1       |
| `rrr`     | Reverse rotate a + Reverse rotate b  | Perform `rra` and `rrb` simultaneously    |

## Chosen Algorithm: Radix Sort

The main sorting algorithm used is **radix sort** (LSD - Least Significant Digit), chosen for its efficiency on large inputs:

### Why Radix Sort?
- **Efficiency**: O(n × k) where `k` is the number of bits (≈ log₂(n) after normalization), resulting in very few operations even for 500 numbers.
- **Predictability**: Deterministic and consistent performance across inputs.
- **Stack-friendly**: Naturally uses `pb`, `pa`, and rotations (`ra`, `rb`) to partition and reassemble the stack.
- **Scalability**: Handles both medium (6-50) and large (100–500) inputs well.
- **Intuitiveness**: Bit-by-bit processing is intuitive, straightforward to code, and easy to debug/maintain within project constraints.

### How It Works
1. **Normalization**: Input values are mapped to indices (0 to n-1) based on their sorted order. This reduces the bit range from 32 (full `int`) to ≈ log₂(n) (e.g., 9 bits for 500 numbers).
2. **Bit-by-bit Sorting**:
   - For each bit position (from LSB to MSB):
     - Elements with bit = 0 are pushed to stack `b`.
     - Elements with bit = 1 are rotated in stack `a`.
   - After processing all elements for the current bit, push everything back from `b` to `a`.
3. **Small Cases**: For ≤ 5 elements, a specialized hard-coded routine is used to minimize operations.

## Usage

### Compilation
```bash
make
```

### Running
```bash
./push_swap [numbers...]
# Example:
./push_swap 3 1 5 2 4
# Or with quoted input:
./push_swap "3 1 5 2 4"
```
