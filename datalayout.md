Data layout
========

in the very beginging of an LLVM assembly code (.ll files), we can see the definition of the machine datalayout of the target code

## For instance ##


    ; ModuleID = 'hello.c'
    target datalayout = "e-p:64:64:64-i1:8:8-i8:8:8-i16:16:16-i32:32:32-i64:64:64-f32:32:32-f64:64:64-v64:64:64-v128:128:128-a0:0:64-s0:64:64-f80:128:128-n8:16:32:64-S128"


the terms are seperated by `-`, and it means


    E:                   target lays out data in little-endian form
    p:64:64:64           64-bit pointers with 64-bit alignment
    
    i1:8:8               i1 is 8-bit aligned
    i8:8:8               i8 is 8-bit aligned
    i16:16:16            i16 is 16-bit aligned
    i32:32:32            i32 is 32-bit aligned 
    i64:64:64            i64 is 64-bit aligned 
    
    f32:32:32            f32(float) is 32-bit aligned
    f64:64:64            f62(doubl)e is 64-bit aligned
    
    v64:64:64            64-bit vector is 64-bit aligned
    v128:128:128         128-bit vector is 128-bit aligned
    
    a0:0:64              aggregates are 64-bit aligned
    s0:64:64             the alignment for a stack object of a given bit s<size>:<abi>:<preferred>
    f80:128:128          quad is 128-bit aligned
    n8:16:32:64          a set of native integer widths for the target CPU in bits
    S128                 natural alignment of the stack is 128 bits


## Reference ##
[LLVM type system](http://llvm.org/docs/LangRef.html#type-system)
[LLVM data-layout](http://llvm.org/docs/LangRef.html#data-layout)

