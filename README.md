# C Alignment Cheatsheet

```c
struct MixedData { /* After compilation in 32-bit x86 machine */
    char Data1; // 1 byte 
    /* [1 byte padding]: for the following 'short' to be aligned on a 2 byte boundary */
    short Data2; // 2 bytes
    int Data3;  // 4 bytes - largest structure member
    char Data4; // 1 byte
    /* [3 bytes padding]: to make total size of the structure 12 bytes, which is a
       multiple of 4, the largest alignment within the struct */
};
```
## Alignment and padding

### Most important principles

- **Padding is added for the next data to align with its size.**
- **The padding in the end makes sure that the total struct size is a multiple of the biggest alignment (size) of any member.**

> The second principle ensures that, in an array of structs, every struct as well as all its members are still aligned.

```
┌───┬─────────┬─────────────┬───────┬──────┐┌───┬─────────┬─────────────┬───────┬──────┐┌──
│M1 │    P1   │      M2     │  M3   │  P2  ││M1 │    P1   │      M2     │  M3   │  P2  ││...
└───┴─────────┴─────────────┴───────┴──────┘└───┴─────────┴─────────────┴───────┴──────┘└──
 1B      3B          4B        2B      2B    1B      3B          4B        2B      2B   
```

> Without the padding in the end of struct, M2 of the second struct would not be aligned at boundary of 4 byte, but 2 byte.

### More detailed explanation
An array uses the same alignment as its elements, except that a local or global array variable of length at least 16 bytes or a C99 variable-length array variable always has alignment of at least 16 bytes

By default, the compiler aligns class and struct members on their size value: bool and char on 1-byte boundaries, short on 2-byte boundaries, int, long, and float on 4-byte boundaries, and long long, double, and long double on 8-byte boundaries.

The struct's first member always starts from any addresses divisible by struct's own
alignment requirement which is determined by largest member's alignment requirement(here
4, alignmentof int32_t), Because the address of a struct is the same as the address of its
first member. This is different from  normal variables outside the struct. They can start
any addresses divisible  by its own alignment.

## References
- [Explanation from Microsoft](https://learn.microsoft.com/en-us/cpp/cpp/alignment-cpp-declarations?view=msvc-170)
- [Aligment of char array](https://stackoverflow.com/questions/22598992/alignment-of-char-array-struct-members-in-c-standard)
- [Explanation from the point of view of RAM and CPU](https://softwareengineering.stackexchange.com/questions/356232/why-data-alignment-is-used-exactly)
- [Structs are created at aligned address](https://stackoverflow.com/questions/76120271/does-the-compiler-always-place-struct-at-aligned-addresses)
