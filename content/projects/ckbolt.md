---
name: CKBolt
url: https://ckbolt.texlabs.org/
tagline:
  en: Swap BTC on Lightning for CKB, RUSD or cWBTC on Fiber, and back, with both legs locked to one payment hash.
  zh: 在闪电网络的 BTC 与 Fiber 上的 CKB、RUSD 或 cWBTC 之间双向兑换，两边都锁定在同一个支付哈希上。
categories: [Payment, Fiber]
links:
  x: CKBoltNetwork
  docs: https://ckbolt.texlabs.org/app/guide
github: Tech-Expansion/CKBolt
testnet: true
verified: 2026-11-21
listed: 2026-11-21
---

# What it proves

Lightning and Fiber can trade both ways without trusting the operator. In a web page, a visitor swaps test BTC on Lightning for CKB, RUSD or cWBTC on Fiber, or the reverse: any Lightning wallet pays by QR code or invoice, and the Fiber side is a node that runs in the browser. Both legs are hold invoices on one SHA-256 payment hash, so the hub pays out first and collects only with the preimage that payout reveals, or the payer is refunded. The same page pays a Lightning invoice, a Fiber invoice or a CKB address, and asks for a payment from either network.

# How it uses CKB

The hub runs LND next to fnn and makes its Fiber hold invoices with `hash_algorithm: sha256`, so one preimage settles both legs; RUSD and cWBTC move in UDT channels. The visitor's node is fiber-js (WASM) in the browser, funded from a CKB wallet such as JoyID through CCC, and the hub opens an inbound channel to it so it can receive. The hub sizes every payout from the timelocks on both sides, and closes a payout channel early when waiting could let the payee claim on chain after the Lightning hold ends.

# Before mainnet

It runs on Mutinynet and CKB testnet with test money only, and it has not been audited. On mainnet the hub would write a free option on every quote and lock capital in inbound channels nobody uses; the repository's [mainnet design](https://github.com/Tech-Expansion/CKBolt/blob/main/ckbolt-hub/docs/mainnet-design.md) prices the lock time, closes idle channels and ties limits to cost. Token channels opened by a visitor's own node stay off until Fiber reports each channel's commitment delay ([nervosnetwork/fiber#1765](https://github.com/nervosnetwork/fiber/pull/1765)).
