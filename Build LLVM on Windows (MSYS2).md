### Building LLVM with MSYS2

Taken from [valtron/llvm-stuff](https://github.com/valtron/llvm-stuff/wiki/Build-LLVM-with-MSYS2) with small edits.

_This assumes you have [set up MSYS2](/valtron/llvm-stuff/wiki/Set-up-Windows-dev-environment-with-MSYS2)._

It's easier to install LLVM with `pacman` rather than building it yourself: `pacman -S mingw64/mingw-w64-x86_64-llvm`. But if you do want to build it, here's how:

1. Using `pacman`, install:
    * `mingw64/mingw-w64-x86_64-make`
    * `mingw64/mingw-w64-x86_64-gcc`
    * `mingw64/mingw-w64-x86_64-cmake`
2. Download LLVM source and uncompress to folder `$LLVMSRC`
3. Create folder `$LLVMBUILD` and create `build.sh` containing:
    ```sh
    PATH="/c/Windows/system32:/c/msys64/mingw64/bin"

    LLVMSRC="/c/Users/Nypro/Desktop/llvm2/llvm-5.0.1.src"
    LLVMINS="/c/Users/Nypro/Desktop/llvm2/llvm-5.0"

    cmake \
        -G "MinGW Makefiles" \
        -DCMAKE_INSTALL_PREFIX="$LLVMINS" \
        -DCMAKE_BUILD_TYPE="Release" \
        -DLLVM_TARGETS_TO_BUILD="X86;ARM;Mips;PowerPC;AArch64;NVPTX" \
        -DLLVM_EXPERIMENTAL_TARGETS_TO_BUILD=WebAssembly \
        -DLLVM_INCLUDE_TESTS=OFF \
        -DLLVM_ENABLE_CXX1Y=ON \
        -DLLVM_ENABLE_ASSERTIONS=ON \
        -DSPHINX_OUTPUT_HTML=OFF \
        -DSPHINX_OUTPUT_MAN=OFF \
        "$LLVMSRC"

    cmake --build .
    cmake --build . --target install
    ```
4. Customize the paths in `build.sh` accordingly
5. Customize the `LLVM_*` options according to your needs; they're documented [here](http://llvm.org/docs/CMake.html#llvm-specific-variables)
6. The list of backends for `LLVM_TARGETS_TO_BUILD` is in `$LLVMSRC/CMakeLists.txt`, near `set(LLVM_ALL_TARGETS`. **Note:** `CppBackend` was removed in 3.9 :(
7. `cd` into `$LLVMBUILD`
8. `sh build.sh`
