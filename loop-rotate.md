loop-rotate pass
================

rotate the loop structure

in file `lib/Transforms/Scalar/LoopRotation.cpp`


### for loop in C code:

    for (i = 0; i < N; ++i) {
        ...
    }


### lowering to:

    i = 0;
    while (true) {
        if (i >= n) break;
        ...
        ++i;
    }

### after rotation:

    i = 0;
    do {

        ++i;
    } while (i < n);


### in IR form:

    i = phi 0, i.next
    pred.i = cmp i, M
    
    (... loop body)
    
    i.next = add i, 1


### after rotation:
    
    i = phi 0, i.next
    
    (... loop body)
    
    i.next = add i, 1
    pred.i = cmp i, M
    

