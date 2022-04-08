# .hotfiles

## Dotfiles

I do not have any dotfiles. Dotfiles are for nerds. 
* [The greatest OS](https://www.microsoft.com/software-download/windows10ISO)
* [Best Editor Ever](https://code.visualstudio.com/Download)

### Instructions
Make sure you have the GOAT installed (Greatest OS All Time). Before you install the BEEV (Best Editor EVer).

## Be like Bob
Ok() jokes aside. Rust works great on all three mayor platforms (Windows, MacOS, Linux). As long you have a terminal you are good to go. No one uses VSC in this modern age. [Neovim](https://github.com/neovim/neovim/wiki/Installing-Neovim), [Helix](https://github.com/helix-editor/helix), [IntelliJ Rust](https://www.jetbrains.com/rust/) or [emacs](https://www.devhour.live/) are the weapons of choice. All this stuff should be familiar to you. 

Let us dive in some supa dupa blazingly hot Rust development. What are the tools you need to speed up your development loop. 


## Tool chain

* [cargo](https://doc.rust-lang.org/cargo/)
* cargo-edit
* cargo-watch
* rust-analyzer

## TL;DR
Installation setup:
```bash
cargo install cargo-edit
```

## Cargo extensions

## cargo edit
Opening your projects `Cargo.toml` and manually add or modifying your dependencies is tedious. Just run a `cargo edit` command in a command line. BOOM! 

Currently available commands:

* [cargo add](#$-cargo-add)
* [cargo rm](#$-cargo-rm)
* [cargo upgrade]
* [cargo set-version]

### cargo add
Add new dependencies to your `Cargo.toml`. When no version is specified, cargo add will try to query the latest version's number from [crates.io](https://crates.io).

**Examples**
```zsh
# Add a specific version
$ cargo add regex@0.1.41 --dev
# Query the latest version from crates.io and adds it as build dependency
$ cargo add gcc --build
# Add a non-crates.io crate
$ cargo add local_experiment --path=lib/trial-and-error/
# Add a non-crates.io crate; the crate name will be found automatically
$ cargo add lib/trial-and-error/
# Add a crates.io crate with a local development path
$ cargo add my_helper --vers=1.3.1 --path=lib/my-helper/
# Add a renamed dependency
$ cargo add thiserror --rename error
```
### `$ cargo rm`
Remove dependencies from your `Cargo.toml`.

**Examples**
```bash
# Remove a dependency
$ cargo rm regex
# Remove a development dependency
$ cargo rm regex --dev
# Remove a build dependency
$ cargo rm regex --build
```
### `$ cargo upgrade`
Upgrade dependencies in your Cargo.toml to their latest versions.

To specify a version to upgrade to, provide the dependencies in the <crate name>@<version> format, e.g. cargo upgrade docopt@~0.9.0 serde@>=0.9,<2.0.

This command differs from cargo update, which updates the dependency versions recorded in the local lock file (Cargo.lock).


**Examples**
```zsh
# Upgrade all dependencies for the current crate
$ cargo upgrade
# Upgrade docopt (to ~0.9) and serde (to >=0.9,<2.0)
$ cargo upgrade docopt@~0.9 serde@>=0.9,<2.0
# Upgrade regex (to the latest version) across all crates in the workspace
$ cargo upgrade regex --workspace
# Upgrade all dependencies except docopt and serde
$ cargo upgrade --exclude docopt serde
```

### `$ cargo set-version`
Set the version in your Cargo.toml.

**Examples**
```zsh
# Set the version to the version 1.0.0
$ cargo set-version 1.0.0
# Bump the version to the next major
$ cargo set-version --bump major
# Bump version to the next minor
$ cargo set-version --bump minor
# Bump version to the next patch
$ cargo set-version --bump patch
```
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
