cmake
========

## Basic ##

required: `cmake >= 2.8`

### usage###

    mkdir build
    cd build
    cmake llvm_source_dir
    cmake llvm_source_dir -Dflags
    cmake llvm_source_dir -Uflags



flags are stored in `CMakeCache.txt`, you can edit it to change flags


## Frequently-used CMake variables ##

- CMAKE_BUILD_TYPE:STRING

    Sets the build type for make based generators. Possible values are Release, Debug, RelWithDebInfo and MinSizeRel. On systems like Visual Studio the user sets the build type with the IDE settings.

- CMAKE_INSTALL_PREFIX:PATH

    Path where LLVM will be installed if “make install” is invoked or the “INSTALL” target is built.

- LLVM_LIBDIR_SUFFIX:STRING

    Extra suffix to append to the directory where libraries are to be installed. On a 64-bit architecture, one could use -DLLVM_LIBDIR_SUFFIX=64 to install libraries to /usr/lib64.

- CMAKE_C_FLAGS:STRING

    Extra flags to use when compiling C source files.

- CMAKE_CXX_FLAGS:STRING

    Extra flags to use when compiling C++ source files.

- BUILD_SHARED_LIBS:BOOL

    Flag indicating is shared libraries will be built. Its default value is OFF. Shared libraries are not supported on Windows and not recommended in the other OSes.



## Examples ##

**(some values are omitted)**

### cmake values ###

    //CXX compiler.
    CMAKE_CXX_COMPILER:FILEPATH=/usr/bin/c++
    
    //Flags used by the compiler during all build types.
    CMAKE_CXX_FLAGS:STRING=
    
    //Flags used by the compiler during debug builds.
    CMAKE_CXX_FLAGS_DEBUG:STRING=-g
    
    //Flags used by the compiler during release minsize builds.
    CMAKE_CXX_FLAGS_MINSIZEREL:STRING=-Os -DNDEBUG
    
    //Flags used by the compiler during release builds (/MD /Ob1 /Oi
    // /Ot /Oy /Gs will produce slightly less optimized but smaller
    // files).
    CMAKE_CXX_FLAGS_RELEASE:STRING=-O3 -DNDEBUG
    
    //Flags used by the compiler during Release with Debug Info builds.
    CMAKE_CXX_FLAGS_RELWITHDEBINFO:STRING=-O2 -g -DNDEBUG
    
    
    
    
    //C compiler.
    CMAKE_C_COMPILER:FILEPATH=/usr/bin/cc
    
    //Flags used by the compiler during all build types.
    CMAKE_C_FLAGS:STRING=
    
    //Flags used by the compiler during debug builds.
    CMAKE_C_FLAGS_DEBUG:STRING=-g
    
    //Flags used by the compiler during release minsize builds.
    CMAKE_C_FLAGS_MINSIZEREL:STRING=-Os -DNDEBUG
    
    //Flags used by the compiler during release builds (/MD /Ob1 /Oi
    // /Ot /Oy /Gs will produce slightly less optimized but smaller
    // files).
    CMAKE_C_FLAGS_RELEASE:STRING=-O3 -DNDEBUG
    
    //Flags used by the compiler during Release with Debug Info builds.
    CMAKE_C_FLAGS_RELWITHDEBINFO:STRING=-O2 -g -DNDEBUG
    
    
    
    
    //Flags used by the linker.
    CMAKE_EXE_LINKER_FLAGS:STRING=' '
    
    //Flags used by the linker during debug builds.
    CMAKE_EXE_LINKER_FLAGS_DEBUG:STRING=
    
    //Flags used by the linker during release minsize builds.
    CMAKE_EXE_LINKER_FLAGS_MINSIZEREL:STRING=
    
    //Flags used by the linker during release builds.
    CMAKE_EXE_LINKER_FLAGS_RELEASE:STRING=
    
    //Flags used by the linker during Release with Debug Info builds.
    CMAKE_EXE_LINKER_FLAGS_RELWITHDEBINFO:STRING=
    
    
    
    
    //Enable/Disable output of compile commands during generation.
    CMAKE_EXPORT_COMPILE_COMMANDS:BOOL=OFF
    
    //Install path prefix, prepended onto install directories.
    CMAKE_INSTALL_PREFIX:PATH=/usr/local
    
    
    
    
    //Path to a program.
    CMAKE_LINKER:FILEPATH=/usr/bin/ld
    
    //Path to a program.
    CMAKE_MAKE_PROGRAM:FILEPATH=/usr/bin/make
    
    
    
    
    //Flags used by the linker during the creation of modules.
    CMAKE_MODULE_LINKER_FLAGS:STRING=' '
    
    //Flags used by the linker during debug builds.
    CMAKE_MODULE_LINKER_FLAGS_DEBUG:STRING=
    
    //Flags used by the linker during release minsize builds.
    CMAKE_MODULE_LINKER_FLAGS_MINSIZEREL:STRING=
    
    //Flags used by the linker during release builds.
    CMAKE_MODULE_LINKER_FLAGS_RELEASE:STRING=
    
    //Flags used by the linker during Release with Debug Info builds.
    CMAKE_MODULE_LINKER_FLAGS_RELWITHDEBINFO:STRING=
    
    
    
    
    
    //Value Computed by CMake
    CMAKE_PROJECT_NAME:STATIC=LLVM
    
    
    //If true, cmake will use relative paths in makefiles and projects.
    CMAKE_USE_RELATIVE_PATHS:BOOL=OFF


### LLVM values ###

- LLVM_TARGETS_TO_BUILD:STRING

    Semicolon-separated list of targets to build, or all for building all targets. **Case-sensitive**.
    
    Defaults to **all**.

    Example: `-DLLVM_TARGETS_TO_BUILD="X86;PowerPC".`



- LLVM_BUILD_TOOLS:BOOL

    Build LLVM tools.
    
    Defaults to **ON**.
    
    Targets for building each tool are generated in any case. You can build an tool separately by invoking its target. 
    
    For example, you can build `llvm-as` with a makefile-based system executing make `llvm-as` on the root of your build directory.



- LLVM_INCLUDE_TOOLS:BOOL

    Generate build targets for the LLVM tools. 
    
    Defaults to **ON**.
    
    You can use that option for disabling the generation of build targets for the LLVM tools.


- LLVM_BUILD_EXAMPLES:BOOL

    Build LLVM examples.
    
    Defaults to **OFF**.
    
    Targets for building each example are generated in any case. See documentation for `LLVM_BUILD_TOOLS` above for more details.


- LLVM_INCLUDE_EXAMPLES:BOOL

    Generate build targets for the LLVM examples.
    
    Defaults to **ON**.
    
    You can use that option for disabling the generation of build targets for the LLVM examples.


- LLVM_BUILD_TESTS:BOOL

    Build LLVM unit tests.
    
    Defaults to **OFF**.
    
    Targets for building each unit test are generated in any case. You can build a specific unit test with the target UnitTestNameTests (where at this time UnitTestName can be ADT, Analysis, ExecutionEngine, JIT, Support, Transform, VMCore; see the subdirectories of unittests for an updated list.) It is possible to build all unit tests with the target UnitTests.



- LLVM_INCLUDE_TESTS:BOOL

    Generate build targets for the LLVM unit tests.
    
    Defaults to **ON**.
    
    You can use that option for disabling the generation of build targets for the LLVM unit tests.


- LLVM_APPEND_VC_REV:BOOL

    Append version control revision info (svn revision number or Git revision id) to LLVM version string (stored in the PACKAGE_VERSION macro). For this to work cmake must be invoked before the build.
    
    Defaults to **OFF**.



- LLVM_ENABLE_THREADS:BOOL

    Build with threads support, if available.
    
    Defaults to **ON**.


- LLVM_ENABLE_ASSERTIONS:BOOL

    Enables code assertions.
    
    Defaults to **OFF** if and only if `CMAKE_BUILD_TYPE` is Release.


- LLVM_ENABLE_PIC:BOOL

    Add the `-fPIC` flag for the compiler command-line, if the compiler supports this flag. Some systems, like Windows, do not need this flag.
    
    Defaults to **ON**.


- LLVM_ENABLE_WARNINGS:BOOL

    Enable all compiler warnings.
    
    Defaults to **ON**.


- LLVM_ENABLE_PEDANTIC:BOOL

    Enable pedantic mode. This disable compiler specific extensions, is possible.
    
    Defaults to **ON**.


- LLVM_ENABLE_WERROR:BOOL

    Stop and fail build, if a compiler warning is triggered.
    
    Defaults to **OFF**.


- LLVM_BUILD_32_BITS:BOOL

    Build 32-bits executables and libraries on 64-bits systems. This option is available only on some 64-bits unix systems.
    
    Defaults to **OFF**.


- LLVM_TARGET_ARCH:STRING

    LLVM target to use for native code generation. This is required for JIT generation. It defaults to “host”, meaning that it shall pick the architecture of the machine where LLVM is being built. If you are cross-compiling, set it to the target architecture name.


- LLVM_TABLEGEN:STRING

    Full path to a native TableGen executable (usually named tblgen). This is intended for cross-compiling: if the user sets this variable, no native TableGen will be created.


- LLVM_LIT_ARGS:STRING

    Arguments given to lit. make check and make clang-test are affected.
    
    By default, `-sv --no-progress-bar` on Visual C++ and Xcode, `-sv` on others.


- LLVM_LIT_TOOLS_DIR:PATH

    The path to GnuWin32 tools for tests. Valid on Windows host.
    
    Defaults to ""
    
    then Lit seeks tools according to %PATH%. Lit can find tools(eg. grep, sort, &c) on `LLVM_LIT_TOOLS_DIR` at first, without specifying GnuWin32 to %PATH%.


- LLVM_ENABLE_FFI:BOOL

    Indicates whether LLVM Interpreter will be linked with Foreign Function Interface library. If the library or its headers are installed on a custom location, you can set the variables `FFI_INCLUDE_DIR` and `FFI_LIBRARY_DIR`.
    
    Defaults to **OFF**.


- LLVM_EXTERNAL_{CLANG,LLD,POLLY}_SOURCE_DIR:PATH

    Path to {Clang,lld,Polly}‘s source directory
    
    Defaults to **tools/{clang,lld,polly}**.
    
    {Clang,lld,Polly} will not be built when it is empty or it does not point valid path.


- LLVM_USE_OPROFILE:BOOL

    Enable building OProfile JIT support.
    
    Defaults to **OFF**


- LLVM_USE_INTEL_JITEVENTS:BOOL

    Enable building support for Intel JIT Events API.
    
    Defaults to **OFF**


- LLVM_ENABLE_ZLIB:BOOL

    Build with `zlib` to support compression/uncompression in LLVM tools.
    
    Defaults to `ON`.


- LLVM_USE_SANITIZER:STRING

    Define the sanitizer used to build LLVM binaries and tests. Possible values are Address, Memory and MemoryWithOrigins.
    
    Defaults to **""** (empty string)).


## CMakeLists.txt ##

a file to tell cmake how to do configuration for you.

for instance, you can add some flags to the compiler or change some macro flags.

you need CMakeLists.txt in each directories.

### some examples ##

    remove_definitions( -fno-exceptions )      # enable exceptions (it's evil)
    add_definitions( -fexceptions )

    add_definitions( -DAMD_HSAIL )             # add some macro flags

    add_subdirectory(AsmParser)                # add subdirectories of the project
    add_subdirectory(libHSAIL)
    add_subdirectory(InstPrinter)
    add_subdirectory(TargetInfo)

## Reference ##

[LLVM cmake](http://llvm.org/docs/CMake.html)


