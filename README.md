# Rust Notebook

My notes on Rust development

<!-- vim-markdown-toc GFM -->

- [Cargo Dependencies](#cargo-dependencies)
  - [External private crate via Git](#external-private-crate-via-git)
  - [Internal via path](#internal-via-path)
- [Current Installation Script](#current-installation-script)
- [Issues](#issues)
  - [Caching](#caching)
- [Tips and Tricks](#tips-and-tricks)
- [Vim Configuration](#vim-configuration)

<!-- vim-markdown-toc -->

## Cargo Dependencies

### External private crate via Git

```rust
[dependencies]
example = { git = "https://gitlab.com/avastmick/example.git" }
example = { git = "https://gitlab.com/avastmick/example.git", branch = "dev" }
example = { git = "https://gitlab.com/avastmick/example.git", tag = "v1.0" }
example = { git = "https://gitlab.com/avastmick/example.git", rev = "v1.1" }
```

### Internal via path

```rust
[dependencies]
example = { path = "example" }
```

```bash
cd repo_name
cargo new example
cd example/src
vim lib.rs
...
```

## Current Installation Script

```bash
curl https://sh.rustup.rs -sSf | sh -s -- -y;
source $HOME/.cargo/env;
mkdir ~/.zfunc && rustup completions zsh > ~/.zfunc/_rustup;
rustup install nightly beta; 
rustup component add rustfmt-preview rls-preview rust-analysis clippy-preview rust-src;
rustup target add arm-unknown-linux-gnueabi arm-unknown-linux-gnueabihf armv7-unknown-linux-gnueabihf;
# Install sccache for caching.
cargo install sccache;
cargo +nightly install racer;
```

## Issues

### Caching

Caching may speed up Rust compile times, however care needs to be taken with where the `$RUSTC_WRAPPER` is exported: 

- `sccache` is used as the value to `$RUSTC_WRAPPER` in your `.bashrc` or `.zshrc`, however if it is not installed and any usage of `rustc` via `cargo` fails.
- To install `sccache` you need to `cargo install sccache` - catch 22!
  - Open a new terminal
  - `export RUSTC_WRAPPER=""`
  - `cargo install sccache`
  - Should compile and install and now the problem is overcome!

## Tips and Tricks

...


## Vim Configuration

My Current Vim settings:

```bash
" Rust Settings: {{{
let g:rustfmt_autosave = 1
" vim-racer
set hidden
let g:racer_cmd =expand('$HOME/.cargo/bin/racer')
let $RUST_SRC_PATH=substitute(system('rustc --print sysroot'), '\n\+$', '', '') . '/lib/rustlib/src/rust/src'
" definitions
au FileType rust nmap gd <Plug>(rust-def)
au FileType rust nmap gs <Plug>(rust-def-split)
au FileType rust nmap gx <Plug>(rust-def-vertical)
au FileType rust nmap <leader>gd <Plug>(rust-doc)
" tagbar setup
let g:tagbar_type_rust = {
  \ 'ctagstype' : 'rust',
  \ 'kinds' : [
      \'T:types,type definitions',
      \'f:functions,function definitions',
      \'g:enum,enumeration names',
      \'s:structure names',
      \'m:modules,module names',
      \'c:consts,static constants',
      \'t:traits',
      \'i:impls,trait implementations',
  \]
\}
" }}}
```
