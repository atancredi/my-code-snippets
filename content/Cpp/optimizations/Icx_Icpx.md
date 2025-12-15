# Resources
https://www.intel.com/content/www/us/en/docs/dpcpp-cpp-compiler/developer-guide-reference/2023-0/ax-qax.html
https://www.intel.com/content/www/us/en/docs/cpp-compiler/developer-guide-reference/2021-8/o-001.html
https://www.intel.com/content/www/us/en/docs/dpcpp-cpp-compiler/developer-guide-reference/2023-0/optimization-options.html
https://www.intel.com/content/www/us/en/docs/dpcpp-cpp-compiler/developer-guide-reference/2023-0/alphabetical-option-list.html
---
## Turn On Inlining

Use the -ip or -ipo flags.

Using -ip enables additional interprocedural (IP) optimizations for single-file compilation. One of these optimizations enables the compiler to perform inline function expansion for calls to functions defined within the current source file.

Using -ipo enables multi-file IP optimizations between files. When you specify this option, the compiler performs inline function expansion for calls to functions defined in separate files.

## Parallelize Your Code

The -qopenmp option handles OMP directives and -parallel looks for loops to parallelize.