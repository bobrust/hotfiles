# dotfiles

## Dependencies
* [The greatest OS](https://www.microsoft.com/software-download/windows10ISO)
* [Best Editor Ever](https://code.visualstudio.com/Download)

## Instructions
Make sure you have the GOAT installed (Greatest OS All Time). Before you install the BEEV (Best Editor EVer).

## Tool chain

rust-analyzer

### Faster Linking
When looking at the inner development loop, we are primarily looking at the performance of incre- mental compilation - how long it takes cargo to rebuild our binary after having made a small change to the source code.
A sizeable chunk of time is spent in the linking phase - assembling the actual binary given the outputs of the earlier compilation stages.
The default linker does a good job, but there are faster alternatives depending on the operating system you are using:
• lld on Windows and Linux, a linker developed by the LLVM project; • zld on MacOS.
To speed up the linking phase you have to install the alternative linker on your machine and add this configuration file to the project:

```toml
# .cargo/config.toml
# On Windows
# ```
# cargo install -f cargo-binutils
# rustup component add llvm-tools-preview
# ```
[target.x86_64-pc-windows-msvc]
rustflags = ["-C", "link-arg=-fuse-ld=lld"]
[target.x86_64-pc-windows-gnu]
rustflags = ["-C", "link-arg=-fuse-ld=lld"]
# On Linux:
# - Ubuntu, `sudo apt-get install lld clang`
# - Arch, `sudo pacman -S lld clang`
[target.x86_64-unknown-linux-gnu]
rustflags = ["-C", "linker=clang", "-C", "link-arg=-fuse-ld=lld"]
# On MacOS, `brew install michaeleisel/zld/zld`
[target.x86_64-apple-darwin]
rustflags = ["-C", "link-arg=-fuse-ld=/usr/local/bin/zld"]
[target.aarch64-apple-darwin]
rustflags = ["-C", "link-arg=-fuse-ld=/usr/local/bin/zld"]
```
There is ongoing work on the Rust compiler to use lld as the default linker where possible - soon enough this custom configuration will not be necessary to achieve higher compilation performance!11

- [cargo edit](https://github.com/killercup/cargo-edit)
- [cargo watch](https://watchexec.github.io/#cargo-watch)

### Cargo Watch:
```zsh
$ cargo watch
$ cargo watch -- cargo test
$ cargo watch -x clippy -x "test --exact ice_cream"
```
