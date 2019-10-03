# S5IOF (Scheme 5 In One File) - A Minimal R5RS Scheme System
                         
S5IOF is a portable interpreter for a subset of the Scheme programming language. 
It can be built from a single C source file [s5iof.c](https://raw.githubusercontent.com/false-schemers/s5iof/master/s5iof.c); 
there are no OS- or hardware-specific parts, no compiler-specific tricks, no dependency on platform-specific building tools. There is no distributive to install: just compile the file with your favorite C compiler, link it with the standard C runtime libraries and be done with it. For some platforms, precompiled binaries are available (please see [releases](https://github.com/false-schemers/s5iof/releases)).

## Installation

There is no installation to speak of; just grab the file and compile it with your favorite C compiler.

Here's how you can compile S5IOF on a unix box:

```
gcc -o s5iof -O3 -DNDEBUG s5iof.c -lm
```

Please note that some compilers may issue hundreds of warnings; we recommend to add `-Wno-parentheses-equality` for
Clang and `-D_CRT_SECURE_NO_WARNINGS` for Windows headers (unless you want to hear that `fopen`
is no longer a reasonable way to open files). Some compilers link `<math.h>` library automatically, some require explicit 
option like `-lm` above. 

If you specify high optimization settings as in the example above, be ready for long compilation times. But it's usually worth the wait: the executable is smaller and faster (which is important because the speed was traded for code size).

The resulting interpreter has no dependencies and can be run from any location.


## Scheme Compatibility

S5IOF is true to basic Scheme principles -- it features precise garbage collector, supports proper tail recursion, `call/cc`, `dynamic-wind`, multiple return values, and has a hygienic macro system. It is mostly R5RS-compatible, but it has the following known limitations and deviations from the standard:

  *  fixnums are limited to 24 bits, flonums are doubles
  *  no support for bignums/rational/complex numbers
  *  `eval` ignores its optional second argument; environment functions are stubs
  *  some run-time errors such as zero division and fixnum overflows trigger asserts in C code unless `NDEBUG` is defined during compilation
  * `transcript-on` and `transcript-off` are not implemented

SIOF supports some common extensions:

  *  `error` (not based on exceptions)
  *  `file-exists?`, `delete-file`, `rename-file`
  *  `exit`, `abort`, `reset`, `command-line`
  *  `get-environment-variable`, `system`, `current-jiffy`, `jiffies-per-second` 


## Origins

S5IOF's original code is written in [#F](https://github.com/false-schemers/sharpF), a language for building Scheme-like
systems. Its #F source code can be found there in `examples` directory:

[s5iof.sf](https://raw.githubusercontent.com/false-schemers/sharpF/master/examples/s5iof.sf)

S5IOF's on-the-fly compiler is derived from Marc Feeley's Scheme Interpreter (see `gambit/bench/src/scheme.scm` in the [Gambit Scheme repository](https://github.com/gambit/gambit)). Hygienic macroexpander is derived from Al Petrofsky's alexpander v1.65 (please see the #F source file for the original copyright notice). Supporting library code comes from #F's [LibS library](https://raw.githubusercontent.com/false-schemers/sharpF/master/lib/libs.sf).

## Family

Please see [S4IOF](https://github.com/false-schemers/s4iof) for a smaller system without hygienic macros, [SIOF](https://github.com/false-schemers/siof) for the latest (and biggest) single-file scheme implementation.
