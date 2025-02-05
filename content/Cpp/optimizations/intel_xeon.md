

**![[Pasted image 20230323102013.png]]**
Note: The instruction sets are upward compatible, therefore, applications compiled with:

-   -xAVX can run on Sandy Bridge, Ivy Bridge, Haswell, Broadwell, Skylake, and Cascade Lake processors.
-   -xCORE-AVX2 can run on Haswell, Broadwell, Skylake, and Cascade Lake processors.
-   -xCORE-AVX512 can run _only_ on Skylake and Cascade Lake processors.

---
An update for recent GCC / Xeon.

-   **[Sandy-Bridge-based](http://en.wikipedia.org/wiki/Sandy_Bridge) Xeon** (E3-12xx series, E5-14xx/24xx series, E5-16xx/26xx/46xx series).
    
    `-march=corei7-avx` for GCC < 4.9.0 or `-march=sandybridge` for GCC >= 4.9.0.
    
    This enables the [Advanced Vector Extensions support](http://en.wikipedia.org/wiki/Advanced_Vector_Extensions) as well as the AES and [PCLMUL](https://en.wikipedia.org/wiki/CLMUL_instruction_set) instruction sets for Sandy Bridge. Here's the overview from the GCC i386/x86_64 options page:
    
    > Intel Core i7 CPU with 64-bit extensions, MMX, SSE, SSE2, SSE3, SSSE3, SSE4.1, SSE4.2, AVX, AES and PCLMUL instruction set support.
    
-   **[Ivy-Bridge-based](http://en.wikipedia.org/wiki/Ivy_Bridge_%28microarchitecture%29) Xeon** (E3-12xx v2-series, E5-14xx v2/24xx v2-series, E5-16xx v2/26xx v2/46xx v2-series, E7-28xx v2/48xx v2/88xx v2-series).
    
    `-march=core-avx-i` for GCC < 4.9.0 or `-march=ivybridge` for GCC >= 4.9.0.
    
    This includes the Sandy Bridge (corei7-avx) options while also tacking in support for the new Ivy instruction sets: FSGSBASE, [RDRND](http://en.wikipedia.org/wiki/RdRand) and [F16C](http://en.wikipedia.org/wiki/F16C). From GCC options page:
    
    > Intel Core CPU with 64-bit extensions, MMX, SSE, SSE2, SSE3, SSSE3, SSE4.1, SSE4.2, AVX, AES, PCLMUL, FSGSBASE, RDRND and F16C6 instruction set support.
    
-   **[Haswell-based](http://en.wikipedia.org/wiki/Haswell_(microarchitecture)) Xeon** (E3-1xxx v3-series, E5-1xxx v3-series, E5-2xxx v3-series).
    
    `-march=core-avx2` for GCC 4.8.2/4.8.3 or `-march=haswell` for GCC >= 4.9.0.
    
    From GCC options page:
    
    > Intel Haswell CPU with 64-bit extensions, MOVBE, MMX, SSE, SSE2, SSE3, SSSE3, SSE4.1, SSE4.2, POPCNT, AVX, AVX2, AES, PCLMUL, FSGSBASE, RDRND, FMA, BMI, BMI2 and F16C instruction set support.
    
-   **[Broadwell-based](https://en.wikipedia.org/wiki/Broadwell_%28microarchitecture%29) Xeon** (E3-12xx v4 series, E5-16xx v4 series)
    
    `-march=core-avx2` for GCC 4.8.x or `-march=broadwell` for GCC >= 4.9.0.
    
    From GCC options page:
    
    > Intel Broadwell CPU with 64-bit extensions, MOVBE, MMX, SSE, SSE2, SSE3, SSSE3, SSE4.1, SSE4.2, POPCNT, AVX, AVX2, AES, PCLMUL, FSGSBASE, RDRND, FMA, BMI, BMI2, F16C, RDSEED, ADCX and PREFETCHW instruction set support.
    
-   **[Skylake-based](https://en.wikipedia.org/wiki/Skylake_%28microarchitecture%29) Xeon** (E3-12xx v5 series) and **[KabyLake-based](https://en.wikipedia.org/wiki/Kaby_Lake) Xeon** (E3-12xx v6 series):
    
    `-march=core-avx2` for GCC 4.8.x or `-march=skylake` for GCC 4.9.x or `-march=skylake-avx512` for GCC >= 5.x
    
    [AVX-512](https://en.wikipedia.org/wiki/AVX-512) are 512-bit extensions to the 256-bit Advanced Vector Extensions SIMD instructions.
    
    From GCC options page:
    
    > Intel Skylake Server CPU with 64-bit extensions, MOVBE, MMX, SSE, SSE2, SSE3, SSSE3, SSE4.1, SSE4.2, POPCNT, PKU, AVX, AVX2, AES, PCLMUL, FSGSBASE, RDRND, FMA, BMI, BMI2, F16C, RDSEED, ADCX, PREFETCHW, CLFLUSHOPT, XSAVEC, XSAVES, AVX512F, AVX512VL, AVX512BW, AVX512DQ and AVX512CD instruction set support.
    
-   **[Coffee Lake-based](https://en.wikipedia.org/wiki/Skylake_%28microarchitecture%29) Xeon** (E-21xx): `-march=skylake-avx512`.
    
-   **[Cascade Lake-based](https://en.wikipedia.org/wiki/Cascade_Lake_(microarchitecture)) Xeon** (Platinum 8200/9200 series, Gold 5200/6200 series, Silver 4100/4200 series, Bronze 3100/3200 series): `-march=cascade-lake` (requires [gcc 9.x](https://gcc.gnu.org/gcc-9/changes.html#x86)).
    
    From GCC options page:
    
    > enables MOVBE, MMX, SSE, SSE2, SSE3, SSSE3, SSE4.1, SSE4.2, POPCNT, PKU, AVX, AVX2, AES, PCLMUL, FSGSBASE, RDRND, FMA, BMI, BMI2, F16C, RDSEED, ADCX, PREFETCHW, CLFLUSHOPT, XSAVEC, XSAVES, AVX512F, CLWB, AVX512VL, AVX512BW, AVX512DQ, AVX512CD and AVX512VNNI.
    
    [AVX-512 Vector Neural Network Instructions](https://en.wikichip.org/wiki/x86/avx512vnni) (AVX512 VNNI) is an x86 extension, part of the AVX-512, designed to accelerate convolutional neural network-based algorithms.
    
-   **[Cooper Lake-based](https://en.wikichip.org/wiki/intel/microarchitectures/cooper_lake) Xeon** (Platinum, Gold, Silver, Bronze): `-march=cooperlake` (requires [gcc 10.1](https://gcc.gnu.org/gcc-9/changes.html#x86)).
    
    The switch enables the AVX512BF16 ISA extensions.
    

---

To find out what the compiler will do with the `-march=native` option you can use:

```bash
gcc -march=native -Q --help=target
```


---
sources :
	https://stackoverflow.com/questions/943755/gcc-optimization-flags-for-xeon
	https://www.nas.nasa.gov/hecc/support/kb/recommended-compiler-options_99.html