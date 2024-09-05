Essentially, the purpose of this is to use only pointers for raylib structs, for use in FFI that only allows pointers, and not pass-by-value.

This explores a new direction for some ideas in [raybun](https://github.com/konsumer/raybun) and [raylib-pointers-ideas](https://github.com/konsumer/raylib-pointers-ideas).

```
# build pointer-only raylib
cmake -G Ninja -B build
cmake --build build

# test it in bunjs
bun test.js
```

I also wrapped these in scripts:

```
# build native raylib-pointers (in build/librlptr.[dylib|so|dll])
bun run build

# generate the raylib API JSON (requires build first)
bun run gen:raylib

# generate tools/api.json (requires raylib JSON)
bun run gen:api

# generate src/lib.c from tools/api.json
bun run gen:lib

# generate raylib_bun.js from tools/api.json
bun run gen:bun

# run test.js
bun run test
```

### API info

I also include [api.json](./tools/api.json), which is the same as [raylib's parser output](https://github.com/raysan5/raylib/blob/master/parser/output/raylib_api.json), but includes my additions, and seperates things into categories. You can use this in your own codegen to wrap this lib easier.

```
unwrapped     # original raylib functions that are just exported
wrapped       # exported functions that have been wrapped to use pointers
struct_utils  # size/get/set util-functions for structs

# these are all original fields:
defines
structs
aliases
enums
callbacks
functions # you should probly use wrapped/unwrapped above, since it's the same info (but categorized, with updated types)
```
