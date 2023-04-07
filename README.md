# Rust and WebAssembly Sandbox
### Demonstration of using WebAssembly.

***I worked with Windows 11 WSL (Ubuntu 20.04), on macOS it should be 
easier to install. For more detailed information about installing 
Rust and Cargo, see the official pages.***

### Install Rust and Cargo
- Rust is a multi-paradigm, high-level, general-purpose programming language. (https://www.rust-lang.org/)
- Cargo: the Rust build tool and package manager

### Installation
https://doc.rust-lang.org/cargo/getting-started/installation.html

#### Windows Subsystem for Linux
`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`

---

### Init Cargo.toml file
`cargo init --name wasm_poc_sandbox --lib`

### Add Wasm BindGen Dependency

`cargo add wasm-bindgen`

Facilitating high-level interactions between Wasm modules and JavaScript.

### Add wasm-pack
`cargo install wasm-pack`

### Add the following to your Cargo.toml file:
```
[lib]
crate-type = ["cdylib", "rlib"]
```

### src/lib.rs
```
use wasm_bindgen::prelude::wasm_bindgen;

#[wasm_bindgen]
pub fn add(a: usize, b: usize) -> usize {
    return a + b
}
```

### Build wasm-pack
`wasm-pack build --release --target web`

### npm package initialization

`npm init`

### Install http-server
`npm i http-server --save`

### Edit package.json add start script
```
"scripts": {
  "start": "http-server -a localhost -p 5200"
}
```

### Create index.js
```
import init from "./pkg/wasm_poc_sandbox.js";
import {add} from '../pkg/wasm_poc_sandbox.js';

function run() {
    console.log(add(5, 5));
}

init().then(run)
```

### Create index.html
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>Wasm Sandbox</title>
    <script type="module" src="./index.js"></script>
</head>
<body>
    Look result into the console.
</body>
</html>
```

---
### Troubleshooting

#### Fix Rust Error “linker ‘cc’ not found” on Linux
`sudo apt-get update`

`sudo apt install gcc cmake`

#### WSL INTERNET Connection
`sudo nano /etc/resolv.conf`
with the change of the nameserver to **8.8.8.8** or **1.1.1.1**