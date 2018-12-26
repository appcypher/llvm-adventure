### Building LLVM on macOS

1. Go to the folder where you want to build the LLVM source
2. Download LLVM source ➡ `svn co http://llvm.org/svn/llvm-project/llvm/trunk llvm`
3. Move into `llvm/tools` and create folders ➡ `mkdir clang`
4. Move into clang directory and download Clang source ➡ `svn co http://llvm.org/svn/llvm-project/cfe/trunk clang`
6. Move back into the folder where the llvm source is downloaded ➡ `cd ../../`
3. Create a directory where you want to build LLVM files ➡ `mkdir llvm-build`
4. Move into the build folder ➡ `cd llvm-build`; that's where you create and run the shell script.
    ```sh
    #!usr/bin/env sh

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
5. You may not have ninja installed, so if you have [brew](https://brew.sh/) installed (highly recommended), you  need to install it ninja with ➡ `brew install ninja`
6. Customize the `LLVM_*` options according to your needs; they're documented [here](http://llvm.org/docs/CMake.html#llvm-specific-variables)
7. The list of backends for `LLVM_TARGETS_TO_BUILD` is in `$LLVMSRC/CMakeLists.txt`, near `set(LLVM_ALL_TARGETS`.
8. Save the script in a `llvm-build.sh` file
9. Run the script ➡ `sh llvm-build.sh`

Took about 40mins on my MacBook Pro (High Sierra, 2.3 GHz Intel Core i5, 16GB RAM)

Try it out with 
  ```sh
  path/to/llvm-build/bin/clang --target=wasm32 test.c -o test.wasm
  ```

Visit [llvm site](https://llvm.org/docs/GettingStarted.html) for more information.
