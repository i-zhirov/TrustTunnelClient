# TrustTunnelClient Redux

> **Unofficial fork.** This project is a personal fork of the
> [TrustTunnel Client](https://github.com/TrustTunnel/TrustTunnelClient).
> It is **not affiliated with or endorsed by the upstream project**, and it is
> **not** an official release channel. Use at your own risk.

TrustTunnelClient Redux keeps the upstream native client library but adds one
**experimental feature that is not present upstream**:

## ✨ Experimental feature: SOCKS5 listener support on Android

The upstream Android adapter cannot run the client in SOCKS listener mode (its
config parser requires a TUN section and the JNI always hands the native client
a TUN file descriptor, so the native `AutoSetup`-only SOCKS listener is never
used). This fork fixes the Android adapter so the client can expose a local
SOCKS5 proxy instead of a TUN device:

- `VpnServiceConfig` — make the `tun` config optional (SOCKS-only configs parse)
- `VpnService` — skip TUN interface creation in SOCKS mode
- `lib.cpp` (JNI) — use `AutoSetup` listener settings when no TUN fd is provided
- `protectSocket` — treat protection as a no-op in SOCKS mode (no TUN to protect)
- `VpnClient` — close the TUN fd if native client creation fails

This is consumed by the app fork
[TrustTunnelFlutterClient-redux](https://github.com/i-zhirov/TrustTunnelFlutterClient-redux)
(see its SOCKS5 proxy mode).

## Branch layout

- `master` — default branch: mirror of the upstream **code** (this fork README is
  shown by default; the upstream README is preserved at
  [`README_UPSTREAM.md`](README_UPSTREAM.md))
- `socks5-proxy-support` — **fork changes**: Android SOCKS listener support and CI

## Upstream documentation

The original upstream README is preserved at
[`README_UPSTREAM.md`](README_UPSTREAM.md) — please refer to it for the full
project description, architecture, and upstream usage instructions.
