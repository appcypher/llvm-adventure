### Building LLVM on Mac

1. Go to the folder where you want to build the LLVM source
2. Download LLVM source ➡ `svn co http://llvm.org/svn/llvm-project/llvm/trunk llvm`
3. Create a directory where you want to build LLVM files and use this bash script to build the source. For example,  ➡ `mkdir llvm-build`
    ```bash
    LLVMSRC="~/Desktop/Builds/llvm"
    LLVMINS="~/Desktop/Builds/llvm-build"

    cmake \
        -G "Ninja" \
        -DCMAKE_INSTALL_PREFIX="$LLVMINS" \
        -DCMAKE_BUILD_TYPE="Release" \
        -DLLVM_TARGETS_TO_BUILD="X86;ARM" \
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
4. You may not have ninja installed, so if you have [brew](https://brew.sh/) installed (highly recommended), you  need to install it ninja with ➡ `brew install ninja`
5. Customize the `LLVM_*` options according to your needs; they're documented [here](http://llvm.org/docs/CMake.html#llvm-specific-variables)
6. The list of backends for `LLVM_TARGETS_TO_BUILD` is in `$LLVMSRC/CMakeLists.txt`, near `set(LLVM_ALL_TARGETS`.
7. Save the script in a `llvm-build.sh` file
8. Run the script ➡ `sh llvm-build.sh`

Took about 40mins on my MacBook Pro (High Sierra, 2.3 GHz Intel Core i5, 16GB RAM)

Visit for [llvm site](https://llvm.org/docs/GettingStarted.html) more information.
