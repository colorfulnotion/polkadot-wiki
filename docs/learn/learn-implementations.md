---
title: Polkadot Implementations
description: Explore the various Polkadot implementations and their role in the Web3 ecosystem.
---

Polkadot is the flagship protocol of the [Web3 Foundation](https://web3.foundation/), and while
Polkadot can be defined as a protocol, a network, or, a type of infrastructure, it best serves to be
an ecosystem. For true decentralization, there should be multiple implementations of Polkadot. Even
being a _layer 0_ protocol that attempts to build an interconnected, interoperable and secure Web3
ecosystem, Polkadot is a complex piece of software, and its formal implementation depends on being
built on top of a tech stack.

> This page will focus on implementations of **Polkadot's underlying infrastructure** (i.e. runtime,
> host).

## A Wasm-based Meta Protocol

Polkadot uses WebAssembly ([Wasm](https://webassembly.org/)) as a "meta-protocol". This allows for the use of
any programming language that can be interpreted or compiled into Wasm - being the driver for
Polkadot's multiple implementations.

### Parity Technologies: A [Rustic Vision for Polkadot](https://github.com/paritytech/polkadot-sdk/tree/master/polkadot)

[Parity Technologies](https://www.parity.io/) is often in the spotlight for its core development of
Polkadot, and while this is true, Parity Polkadot also serves to be the
[Rust](https://www.rust-lang.org/) client. Parity Tech has a rustic vision for Polkadot through the
use of their main product, [Substrate](https://docs.polkadot.com/develop/parachains/intro-polkadot-sdk/#substrate). Substrate can also be used for
different chains and different networks, but in the case of Polkadot, Substrate acts as the tech
stack that is used to implement Polkadot's sharded heterogeneous multi-chain model.

> Parity Tech focuses on blockchain infrastructure for the decentralised web, where they initially
> offered an Ethereum client (Parity Ethereum). Parity Tech was hired by the Web3 Foundation to
> foster the development of the first implementation of Polkadot.

Polkadot can support parachains that are not built on Substrate, In particular, as long as the state
transition function (STF) of a shard is abstracted into Wasm, the validators on the network can
execute the STF within a Wasm environment.

> Note: chains can also be built on Substrate and are not required to be deployed onto Polkadot.
> More on Polkadot's architecture is available on the [Architecture](learn-architecture.md) page.

With this in mind, we can point to some other implementations of Polkadot. Having different
implementations inherently promotes the decentralization of the technology and progresses it in a
meaningful way. Other implementations of Polkadot that exist, many of whom have received a
[grant](../general/grants.md) from the Web3 Foundation, are in programming languages like Go, C++,
and JavaScript, which are all languages that can be compiled in Wasm.

As stated in the Soramitsu grant announcement:

    It is critically important to have multiple implementations of the Polkadot protocol for a number
    of reasons, including decentralization, knowledge dispersion, and better definitions of the
    protocol... Multiple implementations of Polkadot improves network resilience and adds to the
    decentralization of the network. The governance of the network is more democratized when multiple
    teams build clients that run the nodes in the network.

## Alternative Implementations

### ChainSafe Systems: [Gossamer](https://github.com/ChainSafe/gossamer#a-go-implementation-of-the-polkadot-host)

**Gossamer** is a Go implementation being built by
[ChainSafe Systems](https://github.com/ChainSafeSystems), a blockchain R&D firm based in Toronto,
Canada that is also building an Eth2.0 Serenity client. They were awarded a grant from the Web3
Foundation.

### SORAMITSU: [Kagome](https://github.com/soramitsu/kagome#intro)

**Kagome** is a C++ implementation of the Polkadot Host being built by
[Soramitsu](https://soramitsu.co.jp/), a Japanese digital identity company that previously developed
[Hyperledger Iroha](https://iroha.tech). They were awarded a grant from the Web3 Foundation and
released the first version of Kagome in April 2020. As part of the process, they also released a
[libp2p](https://github.com/soramitsu/libp2p-grpc) networking layer in C++.

### Polkadot-JS Project: [Polkadot-JS](https://github.com/polkadot-js)

**Polkadot-JS** is a [JavaScript client](https://github.com/polkadot-js/client) and offers a
collection of tools, interfaces, and libraries for Polkadot and Substrate.

### Other implementations that have received grants

- [Golkadot](https://github.com/opennetsys/golkadot)
- [Polkadot in Java](https://github.com/polkadot-java)

While the ecosystem continues to grow rapidly, the continued development of alternative
implementations will only strengthen Polkadot. Consider becoming a contributor to the ecosystem,
and learn about the how you can receive a [grant](../general/grants.md) for your development.
