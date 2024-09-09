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
ninja
```

Or check the "Build" section in [the README in the `a1k0n` sub-directory](a1k0n/README.md).

# Run

On Windows (at least) there is a problem with the cursor being plastered everywhere.

A solution is to use [Alacritty](https://alacritty.org/) to launch a dedicated terminal
 with the cursor configured to be the same color as the background:

 ```sh
alacritty -o "window.dimensions.columns=80" -o "window.dimensions.lines=25" -o "colors.cursor.cursor='CellBackground'" -e "donut"
 ```
