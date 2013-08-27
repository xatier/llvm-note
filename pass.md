## wrting LLVM pass ##

My codes can be cloned from [This github repo](https://github.com/xatier/llvm-pass-exercise1)

more Information and Reference: The Official LLVM documentation [Writing An LLVM PAss](http://llvm.org/docs/WritingAnLLVMPass.html)

### Makefile ###

we need `-fPIC` to compile an shared library

We don't need `-fno-rtti` [RTTI](http://en.wikipedia.org/wiki/Run-time_type_information)

RTTI: We can't use  `typeid` and `dynamic_cast` without it

### code details ###


- debug

`#define DEBUG_TYPE "mypass"`

this is used for the `-debug` `-debug-only=xxx` options

[see](http://llvm.org/docs/ProgrammersManual.html#fine-grained-debug-info-with-debug-type-and-the-debug-only-option)


----

- statistic

`STATISTIC(myPassCounter, "my pass");`

we can defind counters for our pass for `-stats` option

then we `++myPassCounter;` in our transformations

[see](http://llvm.org/docs/ProgrammersManual.html#the-statistic-class-stats-option)


----

- write a function pass

we inherit from the class `FunctionPass` and print out the function name when we find a function via our pass

`return false` It should return true if the module was modified by the transformation and false otherwise.


    // function pass example
    struct myPass : public FunctionPass {
      static char ID;
      myPass() : FunctionPass(ID) {}
  
      // runOn
      virtual bool runOnFunction(Function &F) {
        ++myPassCounter;
        errs() << "my pass: (runOn) '" << F.getName() << "'\n";
        return false;
      }
    };

[see](http://llvm.org/docs/doxygen/html/classllvm_1_1FunctionPass.html)


----

- loop pass

`BasicBlockPass`

    virtual bool doInitialization(Function &F)
    virtual bool runOnBasicBlock(BasicBlock &BB)
    virtual bool doFinalization(Function &F)

the steps here are prepare-transform-cleanup

1. doInitialization - Virtual method overridden by BasicBlockPass subclasses to do any necessary per-function initialization.

2. runOnBasicBlock - Virtual method overriden by subclasses to do the per-basicblock processing of the pass.

3. doFinalization - Virtual method overriden by BasicBlockPass subclasses to do any post processing needed after all passes have run.

## Tom: don't write loop passes

[see](http://llvm.org/docs/doxygen/html/classllvm_1_1BasicBlockPass.html)


----

- iterating a Function

we can use `Function::iterator` to iterator all basic blocks in a function

    for (Function::iterator i = F.begin(), e = F.end(); i != e; ++i) {
      errs() << "sized " << i->size() << "\n";
      errs() << "---> " << *i << "\n";
    }

[see](http://llvm.org/docs/ProgrammersManual.html#iterating-over-the-basicblock-in-a-function)


----

- iterating a Function

likewise, we can use the similar `BasicBlock::iterator` to iterator over a basicblock

    for (BasicBlock::iterator i = BB.begin(), e = BB.end(); i != e; ++i) {
      errs() << "--> " << *i << "\n";
    }

[see](http://llvm.org/docs/ProgrammersManual.html#iterating-over-the-instruction-in-a-basicblock)



- declare the instance of our passes

in the end, we need to registe the passes to llvm (pass_name / description of the pass)

    char myPass::ID = 0;
    static RegisterPass<myPass> X("mypass1", "my first LLVM Pass");
    
    char myPass2::ID = 0;
    static RegisterPass<myPass2> XX("mypass2", "my second LLVM Pass");
    
    char myPass3::ID = 0;
    static RegisterPass<myPass3> XXX("mypass3", "my third LLVM Pass");
    
    char myPass4::ID = 0;
    static RegisterPass<myPass4> XXXX("mypass4", "my 4th LLVM Pass");
