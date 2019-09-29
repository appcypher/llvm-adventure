# LLVM ON WINDOWS

- Install the following software and make sure you enable *adding to PATH* during their installations.
    - Git Bash for GNU utils like `tar`
    - Visual Studio Build Tools
    - Visual Studio Community 2019
    - Python 
    - CMake 


- Download llvm, lld and clang source from the LLVM download page
- Untar the source 

    ```bash
    tar -xJf llvm-8.0.0.src.tar.xz
    tar -xJf cfe-8.0.0.src.tar.xz
    tar -xJf lld-8.0.0.src.tar.xz
    ```

- Create a directory for CMake to unbundle your project

    ```
    mkdir llvm-8.0.0
    ```

- Rename the untarred clang and lld source to `clang` and `lld` respectively
- Move the `lld` and `clang` folders to `llvm-8.0.0.src/tools` folder
- Open CMake GUI app and add the path to source and build.
- Click on the `configure` button to select the type of project
- Select `Visual Studio XX 2019` .
- This should run configure and show all the options available 
- You may want to change some of the options. 
    - Note your `CMAKE_INSTALL_PREFIX`.  Default path should be `C:\Program File(x86)\LLVM`, you may want to change that to your the folder you created for CMake, `llvm-8.0.0/`
    - I usually change the `LLVM_TARGETS_TO_BUILD` from `all` to the following.

        ```
        X86;ARM;Mips;AArch64;WebAssembly;PowerPC
        ```

- Also pass `-Thost=x64` to cmake commandline (Optional ?)
- With the various options adjusted, you can click on `Generate Project`.
- In the build folder, you should see `.vcxproj`  files. Open the `INSTALL.vcxproj` one with Visual Studio Community 2019.
- In Visual Studio, click `Build`.


