
## basic tools ##

see [Getting Start](http://llvm.org/docs/GettingStarted.html)

compile like gcc

    clang hello.c -o hello



compilie to assembly (native) codes

    clang -S hello.c




compile source code to bitcode files / LLVM assembly

The `-emit-llvm` option can be used with the -S or -c optionsto emit an LLVM .ll or .bc file (respectively) for the code.

    clang -c -emit-llvm hello.c -o hello.bc
    clang -S -emit-llvm hello.c -o hello.ll




`lli` the LLVM JIT (llvm interpreter & dynamic compiler)

    lli hello.bc




`llvm-dis` traslate bitcode form to llvm-ir (llvm .bc -> .ll disassembler)

    llvm-dis < hello.bc
    llvm-dis < hello.bc | less



`llvm-as` translate llvm-ir form to bitcode form (llvm .ll -> .bc assembler)

    llvm-as hello.ll



`llc` translate bitcode form to native machine code (llvm system compiler)

    llc hello.bc -o hello.s
    llc -march=arm hello.bc -o hello.s
    llc -march=mips hello.bc -o hello.s
    llc -march=x86 hello.bc -o hello.s
    llc -march=x86-64 hello.bc -o hello.s




load a pass

    opt -load $LLVM_SOURCE_DIR/Release+Asserts/lib/LLVMHello.so -hello < hello.bc
    opt -load ../../../Release+Asserts/lib/LLVMHello.so -hello < hello.bc
