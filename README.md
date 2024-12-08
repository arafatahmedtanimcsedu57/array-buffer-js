# ArrayBuffer

## Introduction

> Array Buffer is part of the Binary Data API introduced in ECMAScript 6 which is commonly used to handle Binary Data such as files, images, audio, and any other raw binary data buffer.
It doesn’t provide a method to manipulate data. Instead, you need to use Typed Arrays or DataView to interpret and manipulate data.

## Constructor

The  ArrayBuffer()  constructor creates ArrayBuffer objects.

```JS
const buffer = new ArrayBuffer(<number>)
```

When constructing an Array Buffer, the JavaScript engine requests a block of <number>  bytes from the System’s Memory. This allocation occurs in the heap, a region of memory used for dynamically allocated objects in most programming languages. Allocation for ArrayBuffer is guaranteed to be contiguous in memory.

For example:
If the starting address is 0x1000, the memory addresses for an 8-byte ArrayBuffer would look like this:

|Address    |    Content |
|-----------|------------|
|0x1000     |     Byte 1 |
|0x1001     |     Byte 2 |
|0x1002     |     Byte 3 |
|0x1003     |     Byte 4 |
|0x1004     |     Byte 5 |
|0x1005     |     Byte 6 |
|0x1006     |     Byte 7 |
|0x1007     |     Byte 8 |

So if we replace <number> by 8 then the outcome will be

```JS
const buffer = new ArrayBuffer(8); // Creates a buffer with 8 bytes
console.log(buffer.byteLength); // Output: 8
```

### Example with Sequential Access

Here's how we can observe the sequential nature of memory in practice:

```JS
const buffer = new ArrayBuffer(8);
const view = new Uint8Array(buffer); // 1-byte view for each element

// Fill the buffer with data
for (let i = 0; i < view.length; i++) {
  view[i] = i + 1; // Assign values 1, 2, 3, ..., 8
}

console.log(view); // Output: Uint8Array(8) [1, 2, 3, 4, 5, 6, 7, 8]
```

## Manipulation

Unfortunately/fortunately JS doesn’t provide the pointer. So, we can't manipulate ArrayBuffer directly using the memory address, and it doesn’t provide a method to manipulate data.
Instead, you need to use Typed Arrays or DataView to interpret and manipulate data.

### Using Typed Arrays

```JS
const buffer = new ArrayBuffer(8); // Create an 8-byte buffer
const view = new Int32Array(buffer); // Create a view for 32-bit integers

view[0] = 42; // Store the value 42 in the first 4 bytes (32 bits)
view[1] = 256; // Store 256 in the next 4 bytes

console.log(view[0]); // Output: 42
console.log(view[1]); // Output: 256
console.log(view.length); // Output: 2 (because it's an Int32Array)
```

### Using DataView

```JS
const buffer = new ArrayBuffer(8); // Create an 8-byte buffer
const view = new DataView(buffer); // Create a DataView for arbitrary manipulation

view.setUint8(0, 255); // Store 255 in the first byte
view.setUint16(1, 65535); // Store 65535 in the second and third bytes (big-endian by default)
view.setFloat32(4, 3.14); // Store 3.14 starting at the 5th byte

console.log(view.getUint8(0)); // Output: 255
console.log(view.getUint16(1)); // Output: 65535
console.log(view.getFloat32(4)); // Output: 3.14
```

Array Buffer reads its bits from right to left

```JS
const buffer = new ArrayBuffer(8);
const view = new Int8Array(buffer);
view[0] = 1;  //00000001  first 8 bits
view[1] = 1;  // 00000001 second 8 bits
console.log(view[0]); // 1
console.log(view[1]); // 1

const view2 = new Int16Array(buffer); // Here we change the access method for buffer
console.log(view2[0]); // 0000000100000001 = 257
console.log(view2[1]); // 0
```

One interesting question is whether **overflow** happens in ArrayBuffer.
Answer is **NO**

JavaScript doesn’t crash or corrupt memory when you attempt out-of-bounds access. Instead:

* Read Access: Returns undefined.
* Write Access: Does nothing silently.

Example:

```JS
Copy code
const buffer = new ArrayBuffer(8);
const view = new Uint8Array(buffer);

view[10] = 42; // No error, but it does nothing
console.log(view[10]); // Output: undefined
```

## Conclusion

Mastering ArrayBuffer empowers developers to handle raw binary data with precision and efficiency. Whether we're processing file I/O, implementing custom binary protocols, or working with performance-critical applications like WebAssembly and multimedia processing, ArrayBuffer serves as an essential tool. Its seamless integration with TypedArrays, DataViews, and Web APIs ensures that JavaScript remains capable of tackling even the most demanding binary data tasks. By leveraging ArrayBuffer, developers can unlock new possibilities in performance optimization and advanced data manipulation.