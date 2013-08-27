Loop
====

initialization, some conditions, jump back, just like your life



terms
-----

### header

    The entrance of the loop

    BlockT *getHeader () const


### pre-header

    A loop has a preheader if there is only one edge to the header of the
    loop from outside of the loop.
    If this is the case, the block branching to the header of the loop
    is the preheader node.

    BlockT *getLoopPreheader () const


### latch

    A latch block is a block that contains a branch back to the header.

    BlockT *getLoopLatch () const


### exiting block

    The block which brings you out the loop.

    BlockT *getExitingBlock () const


### block iterator

    // check for other basicblocks                                              
    for (Loop::block_iterator i = L->block_begin(),
         e = L->block_end(); i != e; ++i) {
        DEBUG_MSG("looking at" << *static_cast<BasicBlock *>(*i) << "\n");
    }


### parent loop

    the outer loop of a nested loop pair.

    LoopT *getParentLoop () const


### depth

    The nesting level of this loop.
    An outer-most loop has depth 1, for consistency with loop depth values
    used for basic blocks, where depth 0 is used for blocks not inside any
    loops.

    unsigned llvm::LoopBase< BlockT, LoopT >::getLoopDepth() const


### canonical induction variable

    An integer recurrence that starts at 0 and increments by one each time through the loop.


    PHINode * Loop::getCanonicalInductionVariable() const



### simplify form

    The LoopSimplify form transforms loops to, which is sometimes called normal form.

    bool Loop::isLoopSimplifyForm() const

    in file: lib/Analysis/LoopInfo.cpp

        bool Loop::isLoopSimplifyForm() const {                                         
            // Normal-form loops have a preheader, a single backedge, and all of their    
            // exits have all their predecessors inside the loop.                         
            return getLoopPreheader() && getLoopLatch() && hasDedicatedExits();           
        }


Reference
---------

[llvm::LoopBase](http://llvm.org/docs/doxygen/html/classllvm_1_1LoopBase.html)

[llvm::loop](http://llvm.org/docs/doxygen/html/classllvm_1_1Loop.html)

