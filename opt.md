opt
===

the modular LLVM optimizer and analyzer

## two lind of optimization

- IPO (inter-procedural optimization)

    any variety of code optimization that occurs between procedures, function units (modules).

- LTO (link-time optimization)


----

## usage

#### HINT: `-S` option is useful

- load some optimizations

    `opt -loop-rotate ./test.ll -S -o out.ll`

- load your pass from a `.so` file

    `opt -load mypasslib.so -mypass ./test.ll -S -o out.ll`


- see all passes

    `opt -help | less`


see ref below


## dump pass arguments used

    opt -O3 -debug-pass=Arguments test.ll


## related files:

    lib/Transforms/
    lib/Analysis/
    tools/opt/



Reference
---------


[opt](http://llvm.org/docs/CommandGuide/opt.html)

[Passes](http://llvm.org/docs/Passes.html)

[Writing An LLVM Pass](http://llvm.org/docs/WritingAnLLVMPass.html)

pass.md
