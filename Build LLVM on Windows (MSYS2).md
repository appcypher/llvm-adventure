### Build LLVM on Windows (MSYS2)

_This assumes you have [set up MSYS2](/valtron/llvm-stuff/wiki/Set-up-Windows-dev-environment-with-MSYS2)._

It's easier to install LLVM with `pacman` rather than building it yourself: `pacman -S mingw64/mingw-w64-x86_64-llvm`. But if you do want to build it, here's how:

- Using `pacman`, install needed dependecies:
    * `mingw64/mingw-w64-x86_64-make`
    * `mingw64/mingw-w64-x86_64-gcc`
    * `mingw64/mingw-w64-x86_64-cmake
    
- Go to the folder where you want to build the LLVM source
- Download LLVM source from `http://releases.llvm.org/download.html`and unpack the source `.tar.xz` with `tar -xJf [archive]`.
- Move into `llvm/tools` and create folders ➡ `mkdir clang`
- Move into clang directory and download Clang source ➡ `svn co http://llvm.org/svn/llvm-project/cfe/trunk clang`
- Move back into the folder where the llvm source is downloaded ➡ `cd ../../`
- Create a directory where you want to build LLVM files ➡ `mkdir llvm-7.0.1`
- Move into the build folder ➡ `cd llvm-7.0.1`; that's where you create and run the shell script.
    ```sh
    #!usr/bin/env sh

    LLVMSRC="~/Desktop/llvm/llvm-7.0.1.src"
    LLVMINS="~/Desktop/llvm/llvm-7.0.1"


    cmake \
        -G "MinGW Makefiles" \
        -DCMAKE_INSTALL_PREFIX="$LLVMINS" \
        -DCMAKE_BUILD_TYPE="Release" \
        -DLLVM_TARGETS_TO_BUILD="X86;ARM" \
        # -DLLVM_EXPERIMENTAL_TARGETS_TO_BUILD=WebAssembly \ # Only needed in llvm-6.x and below
        -DLLVM_INCLUDE_TESTS=OFF \
        -DLLVM_ENABLE_CXX1Y=ON \
        -DLLVM_ENABLE_ASSERTIONS=ON \
        -DSPHINX_OUTPUT_HTML=OFF \
        -DSPHINX_OUTPUT_MAN=OFF \
        "$LLVMSRC"

    cmake --build .
    cmake --build . --target install
    ```
    
- Customize the `DLLVM_*` options according to your needs. They are documented [here](http://llvm.org/docs/CMake.html#llvm-specific-variables)
- The list of backends for `LLVM_TARGETS_TO_BUILD` is in `$LLVMSRC/CMakeLists.txt`, near `set(LLVM_ALL_TARGETS`.
- Save the script in a `build.sh` file
- Run the script ➡ `sh build.sh`

Try it out with: 
  ```sh
  bin/llvm-7.0.1/bin/clang --target=wasm32 test.c -o test.wasm
  ```

Visit [llvm site](https://llvm.org/docs/GettingStarted.html) for more information.
