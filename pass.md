## writing LLVM pass ##

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

we can define counters for our pass for `-stats` option

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

----

dot regions
-----------

### a method to generate visual cfg

    opt -dot-regions test.ll


##### HINT: need `Graphviz` and other tools

### a script to call dot (a graphviz util)

convert `.dot` files to png, and give it a tmp name

use GNU eog [Eye of GNOME](https://projects.gnome.org/eog)

    #!/usr/bin/sh
    
    prefix=`awk 'BEGIN {srand() ; print rand()*1000000}'`_tmp_
    dot -Tpng $1 -O
    mv $1.png $prefix-$1.png
    eog -n $prefix-$1.png
    rm $prefix-$1.png



HINTS
-----

- read [LLVM Programmer's Manual](http://llvm.org/docs/ProgrammersManual.html) before
you do anything

- use `C++ STL` and tools under `llvm/ADT`

- use other passes before you write it yourself

- use `-O3` before and after your pass

- use `errs()` to show debug message

```
    #if DEBUG
    #define DEBUG_MSG(X) do { errs() << X ; } while(0)
    #else
    #define DEBUG_MSG(X) {  }
    #endif
```

- take care of types

```
    isa<>       - is something
    cast<>      - cast or assert
    dyn_cast<>  - may return nullptr if fail
```

- use git to do version control

----

## casting

    isa<Constant>(*blah)

    dyn_cast<Instruction>(*blah)

    cast<Instruction>(*blah)

    static_cast<BasicBlock *>(*blah)


## Value classes

# plz read [LLVM Programmer's Manual](http://llvm.org/docs/ProgrammersManual.html) !!

### inheritance diagram

    Value <- User <- Instruction <- PHINode

    include/llvm/IR/Instructions.h


### some PHI node methods

    block_iterator block_begin()
    block_iterator block_end()
    unsigned getNumIncomingValues() const
    Value *getIncomingValue(unsigned i) const
    BasicBlock *getIncomingBlock(unsigned i) const

##### in fact, these are just wrappers for User::op_begin(), User::getOperand()



### some Instruction methods

    BasicBlock *getParent()
    void removeFromParent()
    void eraseFromParent()
    void insertBefore(Instruction *InsertPos)
    void insertAfter(Instruction *InsertPos)
    void moveBefore(Instruction *MovePos)
    unsigned getOpcode() const
    const char *getOpcodeName() const
    bool is/may/has_blahblah()const



### use chain

get Vi's def-use chain

    for (Value::use_iterator i = Vi->use_begin(); e = Vi->use_end();
         i != e; ++i) {
    }

### some User methods

    Value *getOperand(unsigned i) const
    void setOperand(unsigned i, Value *Val)
    Use &getOperandUse(unsigned i)
    unsigned getNumOperands() const


### some Value methods

    Type *getType() const
    LLVMContext &getContext() const
    bool hasName() const
    ValueName *getValueName() const
    void setValueName(ValueName *VN)
    StringRef getName() const
    void setName(const Twine &Name)
    void takeName(Value *V)
    void replaceAllUsesWith(Value *V)
    bool use_empty() const
    use_iterator use_begin()
    use_iterator use_end()
    unsigned getNumUses() const
