# Getting Started

## Prerequisites

- `rustup target add wasm32-unknown-unknown`

- `cargo install wasm-gc`

- `cargo new hello --lib`

- `cd hello`

Add the following to the Cargo.toml file

```
[lib]
crate-type = ["cdylib"]
```

## Compile to wasm

`cargo build --target wasm32-unknown-unknown --release`

This will produce a large wasm file

## Optimize the wasm

`wasm-gc target/wasm32-unknown-unknown/release/hello.wasm`

Using [wasm-gc](https://lib.rs/crates/wasm-gc) to garbage collect (remove unnecessary imports / exports / functions etc.)

## Usage @[index.js](./index.html)

```
	<script>
		(async () => {
			let response = await fetch('hello/target/wasm32-unknown-unknown/release/hello.wasm');
			let bytes = await response.arrayBuffer();
			let { instance } = await WebAssembly.instantiate(bytes, {});

			// parsing the input , here 4
			let input = parseInt(document.querySelector('.input').innerHTML);
			// calling the add_one fn from wasm module
			document.querySelector('.answer').innerHTML = instance.exports.add_one(input)
		})();
	</script>
```

> Note: Serve the index.js from a local server to see the result
