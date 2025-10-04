<div align="center">
  <img height="170x" src="https://cdn.discordapp.com/attachments/1423078506786455674/1424123189088227519/ChatGPT_Image_Oct_4_2025_08_50_37_PM.png?ex=68e2cd93&is=68e17c13&hm=37f8526e29218f1800f29d72c388c0fdcd48cedd615d512e5518bd8e836680dd&" />

  <h1>Bode</h1>

  <p>
    <strong>Unified Cross-Chain Program Framework</strong>
  </p>

  <p>
    <a href="https://Bode-lang.com"><img alt="Tutorials" src="https://img.shields.io/badge/docs-tutorials-blueviolet" /></a>
    <a href="https://discord.gg/Bode"><img alt="Discord Chat" src="https://img.shields.io/discord/889577356681945098?color=blueviolet" /></a>
    <a href="https://opensource.org/licenses/Apache-2.0"><img alt="License" src="https://img.shields.io/github/license/Bode/Bode?color=blueviolet" /></a>
  </p>
</div>

## The Vision

Bode realizes Anatoly Yakovenko's vision of a truly unified blockchain ecosystem. By bridging Solana and BNB Chain, Bode enables developers to create tokens and deploy programs across both networks using a single, unified API. Write once, deploy everywhere.

## What is Bode?

Bode is a groundbreaking framework that connects Solana and BNB Chain, providing developers with seamless tools for writing cross-chain programs and creating tokens on both networks simultaneously.

- **Unified API**: One codebase deploys to both Solana and BNB Chain
- **Cross-Chain Token Creation**: Create tokens on both networks with a single command
- **Rust eDSL**: Familiar Solana-style development experience
- **[IDL](https://en.wikipedia.org/wiki/Interface_description_language) specification**: Generate clients for both chains
- **TypeScript package**: Type-safe clients from IDL for multi-chain interaction
- **CLI and workspace management**: Complete cross-chain application development

Bode is the first framework to truly unify Solana and BNB Chain development.

> [!NOTE]
> Bode brings the best of both worlds: Solana's speed and BNB Chain's massive ecosystem. If you're familiar with Anchor, Truffle, or web3.js, you'll feel right at home with Bode's unified approach to cross-chain development.

## Key Features

- **Single API, Dual Deployment**: Write your program once, deploy to both Solana and BNB Chain
- **Unified Token Standard**: Create tokens that work seamlessly across both networks
- **Cross-Chain State Management**: Synchronize state between Solana and BNB Chain
- **Developer Experience**: Familiar Anchor-like syntax with cross-chain superpowers

## Getting Started

For a quickstart guide and in-depth tutorials, see the [Bode book](https://book.Bode-lang.com) and the [Bode documentation](https://Bode-lang.com).

To jump straight to examples, go [here](https://github.com/Bode/Bode/tree/master/examples). For the latest Rust and TypeScript API documentation, see [docs.rs](https://docs.rs/Bode-lang) and the [typedoc](https://www.Bode-lang.com/docs/clients/typescript).

## Packages

| Package                 | Description                                              | Version                                                                                                                          | Docs                                                                                                            |
| :---------------------- | :------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------- |
| `Bode-lang`           | Rust primitives for writing cross-chain programs         | [![Crates.io](https://img.shields.io/crates/v/Bode-lang?color=blue)](https://crates.io/crates/Bode-lang)                     | [![Docs.rs](https://docs.rs/Bode-lang/badge.svg)](https://docs.rs/Bode-lang)                                |
| `Bode-spl`            | CPI clients for SPL and BEP programs                     | [![crates](https://img.shields.io/crates/v/Bode-spl?color=blue)](https://crates.io/crates/Bode-spl)                          | [![Docs.rs](https://docs.rs/Bode-spl/badge.svg)](https://docs.rs/Bode-spl)                                  |
| `Bode-client`         | Rust client for Bode cross-chain programs              | [![crates](https://img.shields.io/crates/v/Bode-client?color=blue)](https://crates.io/crates/Bode-client)                    | [![Docs.rs](https://docs.rs/Bode-client/badge.svg)](https://docs.rs/Bode-client)                            |
| `@Bode/sdk`           | TypeScript client for Bode programs                    | [![npm](https://img.shields.io/npm/v/@Bode/sdk.svg?color=blue)](https://www.npmjs.com/package/@Bode/sdk)                     | [![Docs](https://img.shields.io/badge/docs-typedoc-blue)](https://Bode.github.io/Bode/ts/index.html)        |
| `@Bode/cli`           | CLI to support building and managing cross-chain apps    | [![npm](https://img.shields.io/npm/v/@Bode/cli.svg?color=blue)](https://www.npmjs.com/package/@Bode/cli)                     | [![Docs](https://img.shields.io/badge/docs-typedoc-blue)](https://Bode.github.io/Bode/cli/commands.html)    |

## Note

- **Bode is in active development, so all APIs are subject to change.**
- **This code is unaudited. Use at your own risk.**

## Examples

Here's a cross-chain counter program that deploys to both Solana and BNB Chain, where only the designated `authority` can increment the count on both networks:

```rust
use Bode_lang::prelude::*;

declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS");

#[program]
mod counter {
    use super::*;

    pub fn initialize(ctx: Context<Initialize>, start: u64) -> Result<()> {
        let counter = &mut ctx.accounts.counter;
        counter.authority = *ctx.accounts.authority.key;
        counter.count = start;
        Ok(())
    }

    pub fn increment(ctx: Context<Increment>) -> Result<()> {
        let counter = &mut ctx.accounts.counter;
        counter.count += 1;
        Ok(())
    }
}

#[derive(Accounts)]
pub struct Initialize<'info> {
    #[account(init, payer = authority, space = 48)]
    pub counter: Account<'info, Counter>,
    pub authority: Signer<'info>,
    pub system_program: Program<'info, System>,
}

#[derive(Accounts)]
pub struct Increment<'info> {
    #[account(mut, has_one = authority)]
    pub counter: Account<'info, Counter>,
    pub authority: Signer<'info>,
}

#[account]
pub struct Counter {
    pub authority: Pubkey,
    pub count: u64,
}
```

### Creating Cross-Chain Tokens

```bash
# Create a token on both Solana and BNB Chain with one command
Bode token create --name "MyToken" --symbol "MTK" --networks solana,bnb

# Deploy to both chains
Bode deploy --target all
```

For more, see the [examples](https://github.com/Bode/Bode/tree/master/examples)
and [tests](https://github.com/Bode/Bode/tree/master/tests) directories.

## Architecture

Bode uses a unified runtime that translates your program logic into native operations for both Solana and BNB Chain. The framework handles:

- **Cross-chain account management**: Seamless state synchronization
- **Token bridging**: Automatic token creation and management across both chains
- **Transaction routing**: Intelligent routing to the appropriate network
- **Unified wallet integration**: Single wallet interface for both chains

## Roadmap

- [ ] Enhanced cross-chain messaging
- [ ] Native DEX integration
- [ ] Advanced bridge mechanics
- [ ] Support for additional chains
- [ ] Cross-chain NFT standards

## License

Bode is licensed under [Apache 2.0](./LICENSE).

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in Bode by you, as defined in the Apache-2.0 license, shall be
licensed as above, without any additional terms or conditions.

## Contribution

Thank you for your interest in contributing to Bode!
Please see the [CONTRIBUTING.md](./CONTRIBUTING.md) to learn how.

## The Future is Cross-Chain

Bode represents the future that Anatoly envisioned: a world where blockchain networks work together seamlessly, where developers aren't constrained by chain boundaries, and where users experience the best of multiple ecosystems through a single, unified interface.

### Thanks ❤️

<div align="center">
  <a href="https://github.com/Bode/Bode/graphs/contributors">
    <img src="https://contrib.rocks/image?repo=Bode/Bode" width="100%" />
  </a>
</div>



