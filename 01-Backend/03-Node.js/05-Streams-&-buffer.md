# Streams & Buffer

## 1. Stream

A **Stream** processes data in small chunks instead of loading everything into memory.

Useful for large files, videos, and network data.

### Types

- **Readable** → Read data
- **Writable** → Write data
- **Duplex** → Read + Write
- **Transform** → Read + Write + Modify data

**Remember:**  
Stream = Process data chunk by chunk.

---

## 2. Backpressure

Backpressure happens when the **producer is faster than the consumer**.

Example:

    Fast Producer → Buffer → Slow Consumer

When the buffer is full:

    write() → false
    ↓
    Stop temporarily
    ↓
    drain
    ↓
    Continue

**Remember:**

- `highWaterMark` → Buffer limit/threshold
- `write() === false` → Slow down
- `drain` → Continue writing

---

## 3. Buffer

A **Buffer** stores raw binary data as bytes.

Used for:
- Files
- Images/videos
- Network data
- Streams

Example:

    "Hello" → Buffer → Bytes

**Remember:**  
Buffer = Raw binary data.

---

## 4. Encoding

Encoding is the process of changing raw information or data into a specific format or code so it can be easily stored, sent, or understood

Example:

    Computers change letters and symbols into numbers or binary code like UTF-8 so systems can read them.

---

## 5. Large File Processing

Don't load a huge file completely into memory.

Use:

    Large File
       ↓
    Readable Stream
       ↓
    Chunks/Buffers
       ↓
    Processing
       ↓
    Storage

Example: Use `createReadStream()` for a 10 GB file.

---

## 6. pipe() vs pipeline()

### `pipe()`

Connects one stream to another.

    readStream.pipe(writeStream)

### `pipeline()`

Connects multiple streams and provides better **error/completion handling**.

**Remember:**

    pipe()     → One connection
    pipeline() → Complete stream chain