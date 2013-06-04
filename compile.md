## compiling llvm ##


the following scripts
    # number of processors
    NP="8"
    
    # fetch the tarball
    wget http://llvm.org/releases/3.2/$PKGNAME.src.tar.gz
    tar zxvf $PKGNAME.src.tar.gz
    
    # we need absolute path
    LLVM_BUILD_DIR="`pwd`/build"
    mkdir -p $LLVM_BUILD_DIR 
    cd $PKGNAME.src/
    ./configure --enable-optimized --prefix=$LLVM_BUILD_DIR
    
    # build
    export REQUIRES_RTTI=1
    make -j $NP install > ../$PKGNAME-make.log 2>&1




./build/bin/llc --version
LLVM (http://llvm.org/):
  LLVM version 3.2svn
  Optimized build with assertions.
  Built Jun  4 2013 (15:29:06).
  Default target: x86_64-unknown-linux-gnu
  Host CPU: corei7

  Registered Targets:
    arm      - ARM
    cellspu  - STI CBEA Cell SPU [experimental]
    cpp      - C++ backend
    hexagon  - Hexagon
    mblaze   - MBlaze
    mips     - Mips
    mips64   - Mips64 [experimental]
    mips64el - Mips64el [experimental]
    mipsel   - Mipsel
    msp430   - MSP430 [experimental]
    nvptx    - NVIDIA PTX 32-bit
    nvptx64  - NVIDIA PTX 64-bit
    ppc32    - PowerPC 32
    ppc64    - PowerPC 64
    sparc    - Sparc
    sparcv9  - Sparc V9
    thumb    - Thumb
    x86      - 32-bit X86: Pentium-Pro and above
    x86-64   - 64-bit X86: EM64T and AMD64
    xcore    - XCore

