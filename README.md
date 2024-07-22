# Generate ZK-Proof via Nexus zkVM

### install dependencies
```
sudo apt update && sudo apt upgrade
sudo apt install cmake build-essential -y

# install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"

# install the RISC-V target
rustup target add riscv32i-unknown-none-elf

# Install the Nexus zkVm
cargo install --git https://github.com/nexus-xyz/nexus-zkvm cargo-nexus --tag 'v0.2.0' --force
```

### create a nexus project
```
cargo nexus new nexus-projectV2
cd nexus-projectV2
```

### add the program code to the ./src/main.rs file
```
echo '#![cfg_attr(target_arch = "riscv32", no_std, no_main)]

fn fib(n: u32) -> u32 {
    match n {
        0 => 0,
        1 => 1,
        _ => fib(n - 1) + fib(n - 2),
    }
}

#[nexus_rt::main]
fn main() {
    let n = 7;
    let result = fib(n);
    assert_eq!(result, 13);
}' > ./src/main.rs
```

### run the program
```
cargo nexus run
```

### generate a proof for the rust program
```
cargo nexus prove
```

### Save Proof
**Save the proof in your local devie via Filezila or Termius, also can download the file from your Server with scp command if your OS is linux
```
scp -r root@your-server-ip:/root/nexus-projectV2/nexus-proof .
```
