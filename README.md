Compile with the following flags:
```
-D__minux -D_WASI_EMULATED_SIGNAL -D_WASI_EMULATED_GETPID \
  -DSQLITE_OMIT_FLOATING_POINT -DSQLITE_OMIT_TRACE -DSQLITE_NOHAVE_SYSTEM -DSQLITE_OMIT_JSON \
  -DSQLITE_THREADSAFE=0 -DSQLITE_OMIT_LOAD_EXTENSION -DSQLITE_TEMP_STORE=3 \
  shell.c sqlite3.c -lc-printscan-no-floating-point -lwasi-emulated-signal -lwasi-emulated-getpid \
  -o [name]
```
