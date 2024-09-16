# Prerequisites

Developed on Windows with:
- [Visual Studio Build Tools](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=buildtools)
  (in the Visual Studio Installer,
   select also "Desktop development with C++" on top of "MSBuild tools");
- [Clang LLVM](https://clang.llvm.org/) (Windows MSVC);
- [Ninja](https://ninja-build.org/) if you want an easy way to build the executables;
- [Alacritty](https://alacritty.org/) if you want smoother terminal display.

# Build

```sh
ninja donut.exe
```

Or check the "Build" section in [the README in the `a1k0n` sub-directory](a1k0n/README.md).

# Run

On Windows (at least) there is a problem with the cursor being plastered everywhere.

A solution is to use [Alacritty](https://alacritty.org/) to launch a dedicated terminal
 with the cursor configured to be the same color as the background:

 ```sh
alacritty -o "window.dimensions.columns=80" -o "window.dimensions.lines=25" -o "colors.cursor.cursor='CellBackground'" -e "donut"
 ```

# Clean

## Prettify: go back to the recipe

A few steps are needed to get the code in a readable state,
 that is to say, to "dedonutize the donut":
- format the code,
- remove all the "comments",
- remove all extra semicolons.

In the following subsections, we detail how it can be done step-by-step using the Clang toolkit,
 but a Ninja target is provided to directly get a prettified version of the code:

```sh
ninja donut_dedonutized.c
```

This version can then be compiled and run with:

```sh
ninja run-donut_dedonutized
```

### Format the code

To format the code, we can of course use [ClangFormat](https://clang.llvm.org/docs/ClangFormat.html).

```sh
clang-format a1k0n/donut.c > donut_remodeled.c
```

### Remove the comments

To strip all the "comments",
 we can use the [Clang preprocessor](https://clang.llvm.org/docs/CommandGuide/clang.html#cmdoption-E)
 (see "[Clang preprocessor to strip comments from C++ files](https://stackoverflow.com/questions/28944174/clang-preprocessor-to-strip-comments-from-c-files)" on Stack Overflow).

```sh
clang -E -P donut_remodeled.c -o donut_remodeled_and_slimmed.c
```

### Remove the extra semicolons

To remove all the extra semicolons, it's a little bit more complex,
 because semicolons are delicate to handle,
 the C `for (;;)` forever loop being one of the reasons
 (see "[What does "for(;;)" mean?](https://stackoverflow.com/questions/4894120/what-does-for-mean)" on Stack Overflow).

Fortunately, the detection of extra semicolons has been added to [Clang-Tidy](https://clang.llvm.org/extra/clang-tidy/),
 mainly for semicolons outside of a function with [`-Wextra-semi`](https://clang.llvm.org/docs/DiagnosticsReference.html#wextra-semi-stmt),
 but also for empty expression statements with [`-Wextra-semi-stmt`](https://clang.llvm.org/docs/DiagnosticsReference.html#wextra-semi-stmt).

However, Clang-Tidy is by default even more capricious about unsafe code than the Clang compiler,
 therefore, not only the same warnings/errors have to be disabled (see the "Build" section in [the README in the `a1k0n` sub-directory](a1k0n/README.md)),
 but also the deprecated use of `memset` has to be confirmed.

Also, Clang-tidy works in-place, therefore the source file has to be copied before being processed.

```sh
copy donut_remodeled_and_slimmed.c donut_dedonutized.c
clang-tidy --extra-arg=-Wno-implicit-function-declaration --extra-arg=-Wno-implicit-int -checks=-clang-analyzer-security.insecureAPI.DeprecatedOrUnsafeBufferHandling --extra-arg=-Wextra-semi-stmt -fix donut_dedonutized.c --
```

(For the `--` of the last command line, see "[How to use and configure clang-tidy on windows?](https://stackoverflow.com/questions/52710180/how-to-use-and-configure-clang-tidy-on-windows)" on Stack Overflow.)

### Test the dedonutized version

```sh
clang -Wno-implicit-function-declaration -Wno-implicit-int donut_dedonutized.c -o donut_dedonutized.exe
donut_dedonutized
```
