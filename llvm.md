
## basic tools ##

see [Getting Start](http://llvm.org/docs/GettingStarted.html)

compile like gcc

    clang hello.c -o hello



compilie to assembly (native) codes

    clans -S hello.c




compile to bitcode files

The -emit-llvm option can be used with the -S or -c options to emit an LLVM .ll or .bc file (respectively) for the code.

    clang -O3 -emit-llvm hello.c -c -o hello.bc




`lli` the LLVM JIT

    lli hello.bc




traslate bitcode form to llvm-ir

    llvm-dis < hello.bc | less




translate bitcode form to native machine code

    llc hello.bc -o hello.s



load a pass

still have some problems here :(


    opt -load ../../../Release+Asserts/lib/LLVMHello.so -hello < hello.bc
