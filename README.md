# Jetty native quiche builds
This project is providing the native builds of the Cloudflare Quiche (https://github.com/cloudflare/quiche) library used by the Jetty Project's HTTP/3 implementation.

## Current quiche version
0.23.5

## Current targets
 - Linux x86-64
 - Linux aarch64
 - Macos x86-64
 - Macos aarch64
 - Windows x86-64

## How Quiche was checked out and built
```
git clone --recursive https://github.com/cloudflare/quiche 0.23.5
cd 0.23.5
git checkout -b tag-0.23.5 tags/0.23.5
cargo build --features ffi,qlog
```

Resulting library: `ls target/debug/libquiche.(so|dylib)` or `dir target\debug\quiche.dll`
