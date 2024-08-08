# Library-Linker
Scripts for Static and Dynamic library linking

# Static Library [.a/.lib]

- Compiled during runtime, so no need to worry about lib present on the user machine and no runtime loading cost
- In cases of changes, entire code has to be recompiled

Code flow:
1. Create static library: [lib .c->.o] -> [lib .o->.a]
2. Write source code:	  [main .c->.o] -> [main.o + lib.a -> exe]

gcc -c static_lib.c -o static_lib.o<br>
ar rcs static_lib.a static_lib.o [can add more obj files]<br>
gcc -c main.c -o main.o<br>
gcc -o exe main.o -L. static_lib.a<br>

# Dynamic Library [.so/.dll]

gcc -c -fPIC dynamic_lib.c -o dynamic_lib.o<br>
gcc -shared -o libdynamic_lib.so dynamic_lib.o<br>
gcc -c main.c -o main.o<br>
gcc -o exe main.o -L. -ldynamic_lib<br>
gcc -o exe main.o -L. -ldynamic_lib -Wl,-rpath,. [or] export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:. then step 4<br>
by default the compiler looks for lib in [/lib or /usr/lib], the above command tells the compiler to look for the library in the current directory.<br><br>

-Wl, tells gcc to pass the following options directly to the linker (ld).<br>
-rpath,. sets the runtime library search path to the current directory. -rpath specifies a list of directories to be searched for shared libraries at runtime.<br>
