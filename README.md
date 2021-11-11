# Jetty native quiche builds
This project is providing the native builds of the Cloudflare Quiche (https://github.com/cloudflare/quiche) library used by the Jetty Project's HTTP/3 implementation.

## Current quiche version
0.10.0

## Current targets
 - Linux amd64
 - Macos ARMv8
 - Macos amd64
 - Windows amd64

## How quiche was checked out and built
```
git clone --recursive https://github.com/cloudflare/quiche 0.10.0
cd 0.10.0
git checkout -b tag-0.10.0 tags/0.10.0
cargo build --features ffi,qlog
```

Resulting library: `ls target/debug/libquiche.(so|dylib)` or `dir target\debug\quiche.dll`