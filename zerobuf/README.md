# Overview

This is zerobuf 0.5 for rumprun, a zero-copy, zero-serialize, zero-hassle protocol buffers [HBPVIS/ZeroBuf](https://github.com/HBPVIS/ZeroBuf). Zerobuf requires Servus [HBPVIS/Servus](https://github.com/HBPVIS/Servus).
The test platform is running Ubuntu 16.04 and the compilation options include ```-std=c++17```.

## Maintainer

* Bela Berde, bela.berde@gmail.com
* Github: @kvart2006

## Instructions
Use the included``Makefile`` by typing ```make```.

## Examples

The schema compiler, ```zerobufCxx.py```, is located in 
```
/path_to/rumprun-packages/zerobuf/ZeroBuf/bin/
```

To compile a schema file, called in this case ```aschema.fbs```, type:
```
/path_to/rumprun-packages/zerobuf/ZeroBuf/bin/zerobufCxx.py -o=. aschema.fbs
``` 

Example1: (compiling the schema file number 1) 
```
/root/git/rumprun-packages/zerobuf/ZeroBuf/bin/zerobufCxx.py -o=. schema1.fbs.fbs
```

In order to run an example application on rumprun, you have to provide the paths to the zerobuf include directory located in rumprun-packages/pkgs/include, and to the ```libServus``` and ```libZeroBuf``` libraries during compilation. Both are located in rumprun-packages/pkgs/lib: 

```
$ x86_64-rumprun-netbsd-gcc <filename>  -I <path-to-zeromq-package>/include \
-L <path-to-zeromq-package>/lib -lpthread -lServus -lZeroBuf -o <filename-output>
```
Example (for testing schema1.fbs):
Rumprun compile step:
```
<path-to-zeromq-package>/lib = /root/git/rumprun-packages/pkgs/lib
<path-to-zeromq-package>/include = /root/git/rumprun-packages/pkgs/include
```
leads to:
```
x86_64-rumprun-netbsd-g++ -std=c++17 schema1_test.cpp schema1.cpp \
/root/git/rumprun-packages/zerobuf/ZeroBuf/Servus/servus/serializable.cpp \
-I/root/git/rumprun-packages/pkgs/include/zerobuf \
-I/root/git/rumprun-packages/pkgs/include/servus \
-I/root/git/rumprun-packages/pkgs/include \
-L/root/git/rumprun-packages/pkgs/lib \
-lpthread -lServus -lZeroBuf -o schema1-rr
```
Rumprun baking step:
```
rumprun-bake hw_generic schema1-rr.bin schema1-rr
```
Rumprun running step for qemu:
```
rumprun qemu -i schema1-rr.bin
```

### Example 1 - schema1
A simple key-value record:
```
table KeyValue {
  intvalue:    int;
  doublevalue: double;
}
```

### Example 2 - schema2
A record for 125 doubles:
```
table DoubleTable {
  doublearray: [double:125];
}

```
Use the following for compiling the example2 code:
```
x86_64-rumprun-netbsd-g++ -std=c++17 schema2_test.cpp schema2.cpp \
/root/git/rumprun-packages/zerobuf/ZeroBuf/Servus/servus/serializable.cpp \
-I/root/git/rumprun-packages/pkgs/include/zerobuf \
-I/root/git/rumprun-packages/pkgs/include/servus \
-I/root/git/rumprun-packages/pkgs/include \
-L/root/git/rumprun-packages/pkgs/lib \
-lpthread -lServus -lZeroBuf -o schema2-rr
```
and for baking the example2:
```
rumprun-bake hw_generic schema2-rr.bin schema2-rr
```
Finally, the Rumprun running step for qemu:
```
rumprun qemu -i schema2-rr.bin
