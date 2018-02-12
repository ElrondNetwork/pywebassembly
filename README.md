WARNING: This is an early demo in need of a review.


We closely follow "WebAssembly Specification, Release 1.0, Dec 13, 2017". We implement Chapter 5, along with necessary parts of Chapters 2 and 4. The resulting recursive-descent parser takes a .wasm file and builds a syntax tree out of Python tuples, lists, and dicts. In addition, this file includes an inverse of a canonical form of each function, allowing mapping a syntax tree back to a .wasm file. Functions from the spec are named spec_<func>(...) and their inverses are named spec_<func>_inv(...).




Some related tools we have built:

1) Metering injector. Before each sequence of unbranching instructions, a metering function call is injected. The metering function adds to the current cycle count, and checks whether we have exceeded a pre-set limit, when we  and which errors after exceeding a specified number of cycles.

Before and after metering (see fibonacci.wast and fibonacci_metered.wast for more):
```
  (func (;0;) (type 0) (param i32) (result i32)
    (local i32 i32 i32)
    i32.const 1
    set_local 1
    block  ;; label = @1
      get_local 0
      i32.const 1
      i32.lt_s
      br_if 0 (;@1;)
      i32.const 1
      set_local 3
      i32.const 0
      set_local 2
      loop  ;; label = @2
        get_local 2
        get_local 3
        i32.add
        set_local 1
        get_local 3
        set_local 2
        get_local 1
        set_local 3
        get_local 0
        i32.const -1
        i32.add
        tee_local 0
        br_if 0 (;@2;)
      end
    end
    get_local 1)


  (func (;0;) (type 0) (param i32) (result i32)
    (local i32 i32 i32)
    i32.const 1
    set_local 1
    block  ;; label = @1
      i32.const 4
      call 1
      get_local 0
      i32.const 1
      i32.lt_s
      br_if 0 (;@1;)
      i32.const 5
      call 1
      i32.const 1
      set_local 3
      i32.const 0
      set_local 2
      loop  ;; label = @2
        i32.const 13
        call 1
        get_local 2
        get_local 3
        i32.add
        set_local 1
        get_local 3
        set_local 2
        get_local 1
        set_local 3
        get_local 0
        i32.const -1
        i32.add
        tee_local 0
        br_if 0 (;@2;)
      end
    end
    get_local 1)
```


2) i64 Arguments. Because JavaScript does not support 64-bit integers, we to allow passing i64 arguments to WebAssembly as two i32s. Helper code is available in both JavaScript and WebAssembly.

3) Arbitrary Precision Integers. We developed JavaScript code to take an arbitrary sized integer stored as a string, and pass it to WebAssembly as a sequence of i32s. Similarly, a sequence of i32s exported by WebAssembly can be read into a JavaScript string representing the integer.


TODO:
1) Floating point canonicalization code injector.
2) Interpretter as described in chapter 4.
3) Support text format as described in chapter 6.












