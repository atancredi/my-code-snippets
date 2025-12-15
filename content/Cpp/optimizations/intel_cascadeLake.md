(https://www.intel.com/content/www/us/en/docs/vtune-profiler/cookbook/2023-0/compile-portable-optimized-binary.html)

### Ingredients

This section lists the systems and tools used in the creation of this recipe:

-   **Processor:** Intel® Xeon® Processor code named Cascade Lake
-   **Operating System:** Fedora 32
-   **Compilers:**
    -   Intel® C++ Compiler Classic 2021.1.2
    -   GCC version 10.1.1
-   **Analysis Tool:**Intel® VTune™ Profiler 2021.1.2

### Compile Generic Optimized Binary

Compile the binary following the [recommendations](https://software.intel.com/content/www/us/en/develop/documentation/vtune-help/top/set-up-analysis-target/linux-targets/compiler-switches-for-performance-analysis-on-linux-targets.html) from VTune Profiler User Guide ([recommendations for Windows](https://software.intel.com/content/www/us/en/develop/documentation/vtune-help/top/set-up-analysis-target/windows-targets/compiler-switches-for-performance-analysis-on-windows-targets.html)).

**Intel C++ Compiler Classic**

Compile the binary with debug information and -O3 optimization level:

```plain
icc -g -O3 -debug inline-debug-info fma.c -o fma_generic
```

**GNU Compiler Collection**

Compile the binary with debug information and -O2 optimization level:

```plain
gcc -g -O2 fma.c -o fma_generic_O2
```

Check if the code was vectorized using the [HPC Performance Characterization](https://software.intel.com/content/www/us/en/develop/documentation/vtune-help/top/analyze-performance/parallelism-analysis-group/hpc-performance-characterization-analysis.html) analysis type of VTune Profiler.

To do that, run the analysis:

```plain
vtune -c hpc-performance -r fma_generic_O2_hpc ./fma_generic_O2
```

And open the result in VTune Profiler GUI:

```plain
vtune-gui fma_generic_O2_hpc
```

Open the analysis result and see the **Top Loops/Functions with FPU Usage by CPU Time** section of the **Summary** tab:

![](https://www.intel.com/content/dam/docs/us/en/cookbook/2023-0/4D0263C2-DBE7-4BB8-A1FD-65FFD64E51E7-low.png)

The fact that **FP Ops: Scalar** value equals 100% and that the **Vector Instruction Set** column is empty indicates that GCC does not vectorize the code at -O2 optimization level. Use -O2 -ftree-vectorize or -O3 options to enable vectorization.

Compile the fma_generic binary with -O3 optimization level:

```plain
gcc -g -O3 fma.c -o fma_generic
```

### Compile Native Binary

**Compile native binary with the Intel C++ Compiler Classic**

The -xHost option instructs the compiler to generate instructions for the highest instruction set available on the processor performing the compilation. Alternatively, the -x{Arch} option, where {Arch} is the architecture codename, instructs the compiler to target processor features of a specific architecture.

Compile the fma_native binary with -xHost flag:

```plain
icc -g -O3 -debug inline-debug-info -xHost fma.c -o fma_native
```

**Compile native binary with the GNU Compiler Collection**

Compile the fma_native binary with -march=native flag:

```plain
gcc -g -O3 -march=native fma.c -o fma_native
```

If your processor supports the AVX-512 instruction set extension, consider experimenting with the mprefer-vector-width=512 option.

### Compare Generic and Native Binaries

Collect the HPC Performance Characterization analysis data for both binaries:

```plain
vtune -c hpc-performance -r fma_generic_hpc ./fma_generic
```

```plain
vtune -c hpc-performance -r fma_native_hpc ./fma_native
```

Compare these results using the command:

```plain
vtune-gui fma_generic_hpc fma_native_hpc
```

In the VTune Profiler GUI, switch to the **Bottom-Up** tab and set **Loop Mode** to **Functions only**:

![](https://www.intel.com/content/dam/docs/us/en/cookbook/2023-0/19EE0FD6-9397-4EA3-8B5E-89841E015CD5-low.png)

Switch to the **Summary** tab and scroll down to the **Top Loops/Functions with FPU Usage by CPU Time** section:

![](https://www.intel.com/content/dam/docs/us/en/cookbook/2023-0/690F620F-0B65-4627-BDBF-AAFC8937ECDF-low.png)

Observe the **CPU Time** and **Vector Instruction Set** columns.

Consider the performance difference between the generic and the native binary. Decide whether it makes sense to compile a portable binary with multiple code paths.

NOTE:

This sample application was auto-vectorized by the compiler. To investigate vectorization opportunities in your application in depth, try [Intel® Advisor](https://software.intel.com/content/www/us/en/develop/tools/oneapi/components/advisor.html).

### Compile Portable Binary

If the comparison between the generic and native binary shows a performance improvement, for example, if the **CPU Time** was improved, consider compiling a portable binary.

**Compile the portable binary with the Intel C++ Compiler Classic**

Use the [-ax (/Qax for Windows)](https://software.intel.com/content/www/us/en/develop/documentation/cpp-compiler-developer-guide-and-reference/top/compiler-reference/compiler-options/compiler-option-details/code-generation-options/ax-qax.html) option to instruct the compiler to generate multiple feature-specific auto-dispatch code paths for Intel processors.

Compile the fma_portable binary with the -ax option:

```plain
icc -g -O3 -debug inline-debug-info -axCOMMON-AVX512,CORE-AVX2,AVX,SSE4.2,TREMONT,ICELAKE-SERVER fma.c -o fma_portable
```

Refer to the [-ax option help page](https://software.intel.com/content/www/us/en/develop/documentation/cpp-compiler-developer-guide-and-reference/top/compiler-reference/compiler-options/compiler-option-details/code-generation-options/ax-qax.html) for the list of supported architectures.

**Compile the portable binary with the GNU Compiler Collection**

Compare the results for generic and native binaries. If the **CPU Time** was improved and an additional **Vector Instruction Set** was utilized for a specific function in the native binary result, then add the target_clones attribute to this function.

If the function calls other functions, consider adding the flatten attribute to force inlining, since the target_clones attribute is not recursive.

Copy the contents of the fma.c source file to a new file, fma_portable.c, and add the TARGET_CLONE preprocessor macro:

```c
#define TARGET_CLONES __attribute__((flatten,target_clones("default,sse4.2,avx,"\
    "avx2,avx512f,arch=skylake,arch=tremont,arch=skylake-avx512,"\
    "arch=cascadelake,arch=cooperlake,arch=tigerlake,arch=icelake-server")))
```

Refer to the [x86 Options](https://gcc.gnu.org/onlinedocs/gcc/x86-Options.html) page of the GCC manual for the list of supported architectures.

Multiple versions of a function will increase the binary size. Consider the trade-off between performance improvement for each target and code size. Collecting and comparing VTune Profiler results enables you to make data-driven decisions to apply the TARGET_CLONES macro only to the functions that will run faster with new instructions.

Add the TARGET_CLONES macro before the my_fma function definition and init functions and save the changes to fma_portable.c:

```c
TARGET_CLONES
void my_fma(float *a, float *b, float *c, const int size)
```

Compile the fma_portable binary:

```plain
gcc -g -O3 fma_portable.c -o fma_portable
```

### Compare Portable and Native Binaries

To compare the performance of portable and optimized binaries, collect the HPC Performance Characterization data for the fma_portable binary:

```plain
vtune -c hpc-performance -r fma_portable_hpc ./fma_portable
```

Open the comparison in VTune Profiler GUI:

```c
vtune-gui fma_portable_hpc fma_native_hpc
```

![](https://www.intel.com/content/dam/docs/us/en/cookbook/2023-0/DBB764EC-9FCF-4034-BC8A-8FB83AD5A9DF-low.png)

As a result, the portable binary uses the highest instruction set extension available and demonstrates optimal performance on the target system.