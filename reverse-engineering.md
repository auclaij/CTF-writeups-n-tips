## Reverse is not my cup of tea

### strings
Search the binary ```strings -a ./program```

search for strings of different formats:
```
-e s -> single-7-bit-byte characters (default)
-e S -> single-8-bit-byte characters
-e b -> 16-bit bigendian
-e l -> 16-bit littleendian
-e B -> 32-bit bigendian
-e L -> 32-bit littleendian
```
strings -e b ./program

### ELF
Writable/executable sections are of interest? ```readelf --sections ./program```

Display program headers, including section to segment mapping: ```readelf -l ./program```

Dump ELF information: ```readelf -a ./program```
