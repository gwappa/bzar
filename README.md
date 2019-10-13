# bzar (bare zlib array)
A platform-independent archive format for numeric arrays, built on top of `zlib` and `json`.

## Reference implementations

### Python implementation

- [bzar-python](https://github.com/gwappa/bzar-python): a python reference implementation.

### Future plans

I plan to provide a Java implementation as well.

## Specification of "bare-zlib array"

This data format is intended for a structure of a multidimensional numeric array that:

- is used in conjunction with the file system.
- is simple, compact and reusable in as many environments as possible.
- can be used directly during data acquisition.

On the other hand, this format itself is not intended for containing several numeric array or any other data types in a hierarchy within it. This feature is expected to be implemented outside of the format e.g. as a file system. By doing so, it becomes easier to manage individual arrays under version-control.

### overall structure

```
zlib-compressed numeric array
-----
metadata dict in JSON
-----
length of metadata dict, in int64 big-endian
```

### keys for metadata dict

The metadata dictionary must form a valid JSON mapping.

dict must contain:

- datatype (float32, float64, int8, int16, int32, int64, char, bool)
- shape

In addition, some keys are expected in some specific cases (it will be otherwise ignored even when present):

- endianness, in case of multibyte datatypes
- order, in case of multi-dimensional arrays

you can contain other information as well, but there is no guarantee that it is preserved across multiple read-write operations: there are cases, for example, when the author information must be or must not be preserved. It would not be a good idea to restrict the usage to either case. Rather, the users are encouraged to read/write on their own responsibility.
