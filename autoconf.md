autoconf
========

LLVM use autoconf for configuration

## don't try to do some hacks in autoconf ...

## read the README.TXT three times ...

    Upgrading autoconf
    =============================================================================== 
    
    If you are in the mood to upgrade autoconf, you should:
    
     1. Consider not upgrading.
     2. No really, this is a hassle, you don't want to do it.
     3. Get the new version of autoconf and put it in <SRC>
     4. configure/build/install autoconf with --prefix=<PFX>
     5. Run autoupdate on all the m4 macros in llvm/autoconf/m4
     6. Run autoupdate on llvm/autoconf/configure.ac
     7. Regenerate configure script with AutoRegen.sh
     8. If there are any warnings from AutoRegen.sh, fix them and go to step 7.
     9. Test, test, test.


----

## some reasons maybe you want to modify configure

    - adjust JIT options
    - write new targets (--enable-targets, TARGETS_TO_BUILD)

## if you want to do some Makefile hacks, plz go to `Makefile.rules`, not here
