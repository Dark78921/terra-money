# Cosmwasm Examples

[![CircleCI](https://circleci.com/gh/terra-project/cosmwasm-contracts/tree/master.svg?style=shield)](https://circleci.com/gh/terra-project/cosmwasm-contracts/tree/master)

This repo is a collection of contracts built with the
[cosmwasm](https://github.com/confio/cosmwasm) framework.
Anyone building on cosmwasm is encouraged to submit their contracts
as a sub-project via a PR.

The organization is relatively simple. The top-level directory is just a placeholder
and has no real code. And we use workspaces to add multiple contracts below.
This allows us to compile all contracts with one command.

## Usage

Sample contracts are available under the `contracts/` directory. You can view the source code under each contract’s `src` subdirectory. Take a look here:

* [maker](./contracts/maker) - Example implementation to show terra-binding for CosmWasm
* [assert limit order](./contracts/assert_limit_order)
* [mask](./contracts/mask)
* [mintable erc20](./contracts/mintable_erc20)
* [roll staking](./contracts/roll_staking)
* [send to burn address](./contracts/send-to-burn-address)

## Development

### Starting a contract

If you want to add a contract, first fork this repo and create a branch for your PR.
I suggest setting it up via [cosmwasm-template](https://github.com/confio/cosmwasm-template):

`cargo generate --git https://github.com/confio/cosmwasm-template.git --name FOO`

Then update the `README.md` to reflect your actual contract (just read the `README.md` in the autogenerated
template - it explains a lot).

### Preparing for merge

Before you merge the code, make sure it builds and passes all tests, both in the package,
and when calling it from the root packages `cargo wasm && cargo test`. This should
show your package is covered by the CI.

There is also quite some useful information in `Development.md` and `Publishing.md` in the newly generated
contract.

You should also prepare a compiled `contract.wasm` before each merge to master.
This is not enforced by the CI (a full build each commit), but should be tested
on merge. See [`cosmwasm-opt`](https://github.com/confio/cosmwasm-opt/blob/master/README.md#usage)
for an explanation of how to make a deterministic build.

```sh
docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.11.3
```