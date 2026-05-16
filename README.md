# @io/byteorder 🔄

> **Reading and writing integers in big-endian and little-endian byte order** for Zeta.

Port of the Rust [`byteorder`](https://crates.io/crates/byteorder) crate (v1.5.0) to Zeta.

## Why byteorder matters for BSV

Bitcoin transactions use **little-endian** encoding throughout — amounts, sequence numbers, locktimes, sizes. Blocks use a mix of big-endian and little-endian. This crate provides the building blocks for reading and writing the integer fields in BSV transaction serialization without manual byte shuffling.

## API

```zeta
// Read integers from a byte slice (&[u8])
pub trait ReadBytesExt {
    fn read_u8(&mut self) -> Result<u8>
    fn read_u16_le(&mut self) -> Result<u16>
    fn read_u32_le(&mut self) -> Result<u32>
    fn read_u64_le(&mut self) -> Result<u64>
    fn read_i32_le(&mut self) -> Result<i32>
    fn read_i64_le(&mut self) -> Result<i64>
    fn read_u16_be(&mut self) -> Result<u16>
    fn read_u32_be(&mut self) -> Result<u32>
    fn read_u64_be(&mut self) -> Result<u64>
    fn read_i32_be(&mut self) -> Result<i32>
    fn read_i64_be(&mut self) -> Result<i64>
}

// Write integers to a byte vector (Vec<u8>)
pub trait WriteBytesExt {
    fn write_u8(&mut self, val: u8) -> Result
    fn write_u16_le(&mut self, val: u16) -> Result
    fn write_u32_le(&mut self, val: u32) -> Result
    fn write_u64_le(&mut self, val: u64) -> Result
    fn write_i32_le(&mut self, val: i32) -> Result
    fn write_i64_le(&mut self, val: i64) -> Result
    fn write_u16_be(&mut self, val: u16) -> Result
    fn write_u32_be(&mut self, val: u32) -> Result
    fn write_u64_be(&mut self, val: u64) -> Result
    fn write_i32_be(&mut self, val: i32) -> Result
    fn write_i64_be(&mut self, val: i64) -> Result
}
```

## Examples

```zeta
let mut data = &bytes[..]
let version = data.read_u32_le()!  // tx version (LE)
let inputs_count = data.read_varint()!  // compact size uint

let mut buf = Vec::<u8>::new()
buf.write_u32_le(0x01000000)!  // tx version
buf.write_varint(2)!           // number of inputs
```

## Usage

Add to your `zorb.toml`:

```toml
[dependencies]
"@io/byteorder" = "1.5.0"
```

## License

MIT — free for all use, including commercial and BSV infrastructure.

## Dark Factory Integration

`@io/byteorder` is a foundational dependency of **nour** — the Zeta BSV toolkit. Every integer field in transaction serialization flows through this crate. Correct endianness isn't optional in BSV; it's the difference between a valid and an invalid transaction.
