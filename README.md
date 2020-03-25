binary.js
=========

Javascript Binary Reader to read and manipulate binary data.

This library supports:
* little/big endian
* seek and tell
* strings and chars in different encodings
* blobs and chunks
* really fast, because it avoids copying underlaying buffer
* takes care of byteOffset
* chainable :D

# Install

Install with [yarn](https://yarnpkg.com/): `yarn add opusonline-binary.js`

# Usage

### Example

```javascript
let binary = new Binary(16);
binary.setString('hello world');
binary.setByte(33);
binary.skip();
binary.setByte(32);
binary.setChar(':');
binary.setChar('D');
binary.tell();
// gives 16

binary.seek(0).getBytes(4);
// gives [104, 101, 108, 108]
binary.skip(10).getUint16();
// gives 14916

let blob = binary.getBlob('text/plain');
// gives Blob {type: "text/plain", size: 16, …}
```

Same, but chained:

```javascript
new Binary(16)
	.setString('hello world')
	.setByte(33)
	.skip()
	.setByte(32)
	.setChar(':')
	.setChar('D')
	.seek(0)
	.getString();
// gives "hello world :D"
```

## Methods

### new Binary(input[, littleEndian = false])

`input` can be:
* Array
* [TypedArray like DataView or Uint8Array etc.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays)
* ArrayBuffer
* number as the size for a new binary

Binary Object has variables `length`, `position`, `littleEndian`.

### Getter

`getByte`, `getUint8`, `getInt8`, `getUint16`, `getInt16`, `getUint32`, `getInt32`, `getFloat32`, `getFloat64`, `getBytes`, `getChar`, `getString`, `getBlob`, `getChunk`, `getBuffer`, `toString`

### Setter

`setByte`, `setUint8`, `setInt8`, `setUint16`, `setInt16`, `setUint32`, `setInt32`, `setFloat32`, `setFloat64`, `setBytes`, `setChar`, `setString`, `fill`, `copy`, `setLittleEndian`, `setBigEndian`

### Specials

`seek`, `tell`, `skip`

#### getByte() = getUint8()

Returns 1 unsigned byte.

#### setByte(value) = setUint8(value)

Sets value as 1 unsigned byte.

#### getUint16([littleEndian = Binary.littleEndian])

Reads 2 unsigned bytes and returns as 1 number.

If littleEndian is not provided, the property of the constructed binary object is taken.

#### Others

* getInt8() / setInt8(value) 
* getInt16([littleEndian = Binary.littleEndian]) / setInt16(value) 
* getUint32([littleEndian = Binary.littleEndian]) / setUint32(value) 
* getInt32([littleEndian = Binary.littleEndian]) / setInt32(value) 
* getFloat32([littleEndian = Binary.littleEndian]) / setFloat32(value) 
* getFloat64([littleEndian = Binary.littleEndian]) / setFloat64(value)

#### getBytes([length][, copy = false])

Returns a new Uint8Array.

If `length` is not provided, the full length until the end is assumed.

If `copy` is `true` the Uint8Array holds a copy of the ArrayBuffer in the desired length.

#### getChar([encoding = 'binary'])

Returns a character value.

If [`TextDecoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoder) is available you can choose every encoding.
Otherwise `encoding` supports only `utf-8` and `binary` (raw). 

#### getString([length][, encoding = 'binary'])

Returns a string.

If `length` is not provided, the full length until the end is assumed.

If [`TextDecoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoder) is available you can choose every encoding.
Otherwise `encoding` supports only `utf-8` and `binary` (raw). 

#### toString([encoding = 'binary'][, from = 0][, length])

Returns a string.

If `length` is not provided, the full length until the end is assumed.

If [`TextDecoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoder) is available you can choose every encoding.
Otherwise `encoding` supports only `utf-8` and `binary` (raw). 

#### getBlob([type = 'application/octet'])

Returns a new Blob generated from the complete underlaying ArrayBuffer.

#### getChunk(from, to) or getChunk(length)

Returns a new UintArray with a **copy** of the ArrayBuffer in the desired length.

With `from` and `to` the offset is given. With `length` it starts from current position.

#### getBuffer()

Returns the complete underlaying ArrayBuffer.

#### setBytes(input)

`input` can be:
* Array
* [TypedArray like DataView or Uint8Array etc.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays)
* ArrayBuffer

#### setChar(value[, encoding = 'binary'])

Sets a character value at current position.

If [`TextDecoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoder) is available you can choose every encoding.
Otherwise `encoding` supports only `utf-8` and `binary` (raw).

#### setString(string[, encoding = 'binary'])

Sets a string.

If [`TextDecoder`](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoder) is available you can choose every encoding.
Otherwise `encoding` supports only `utf-8` and `binary` (raw).

#### fill(value[, length])

`value` can be
* any number, but will be cut of as uint8
* any character or string, but only first character will be used and cut of as uint8

Fills binary with provided value from current position for `length` bytes.

If `length` is not provided, the full length until the end is assumed.

#### copy(target[, targetFrom = 0][, sourceFrom = 0][, length]) {

Copies given bytes strictly (blind) to `target` binary.

`target` **must** be another binary.

`length` defaults to length of source bytes from `sourceStart`. If source length is higher than target size starting from `targetFrom` an `Error` is thrown.

#### setLittleEndian() / setBigEndian()

Changes binary object to desired mode.

#### seek(offset)

Sets position for next operations on binary object.

#### tell()

Returns current position.

#### skip([length = 1])

Shifts the current operation position by given length.
