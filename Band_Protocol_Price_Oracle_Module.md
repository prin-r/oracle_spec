# Band Protocol Price Oracle Module (Acala Network)

## Overview

This proposal introduces an implementation of Band Protocol's price oracle module to the Acala Network. The module will act as the main hub from which other modules in the network can query the price information of various tokens and cryptocurrecy from.

The inclusion of this module would first and foremost remove the need for Acala to build their own oracle module, or otherwise to provide incentives for other to do the same. Instead, anyone looking to acquire the price data can rely on Band's implementation to handle the complex mechanics, and they themselves would simply have to submit the proof they receive from BandChain to the module. This signficantly reduces the workload on the modules that are looking to use the pirce data.

We also believe that the addition of such price oracle will have a net positive on Acala's DeFi ecosystem as a whole. The availability of such a price feed could greatly increase the functionalities of many modules, as well as make those that are previously infeasible feasible.

Implementation-wise, the price oracle module will have three main functionalities:

1. Store price data to state
2. Expose a function that allows other modules to read values from the store
3. Maintain a set of BandChain's validators

This document only consider phase one.

## Dependencies

- module-primitives
  - CurrencyId
- module-support
  - Price
- orml_traits
  - DataProvider
- proof_verification
  - // TODO: this crate is not avaliable, so we will create one
- obi
  - // TODO: make this crate support non-std

## Type

- type VotingPower = FixedU128
- type struct Config
  - time_tolerance: u64
  - oracle_script_id: u64
  - multiplier: u64
- type struct Req
  - client_id: Vec\<u8\>
  - oracle_script_id: u64
  - calldata: Vec\<u8\>
  - ask_count: u64
  - min_count: u64
- type struct Res
  - client_id: Vec\<u8\>
  - request_id: u64
  - ans_count: u64
  - request_time: u64
  - resolve_time: u64
  - resolve_status: u8
  - result: Vec\<u8\>
- type struct BandPacket
  - req: Req
  - res: Res

## Trait

- type Event: From<Event> + Into\<\<Self as system::Trait\>::Event\>
- type OracleKey = CurrencyId;
- type OracleValue = Price;

## Storages

- Validators: map AccountId => Option\<VotingPower\>
- TotalVotingPower: VotingPower
- Config: Config
- Values: map hasher(twox_64_concat) CurrencyId => Option\<Res\>

## Dispatchables

- feed_value(origin, proof: Vec\<u8\>) -> DispatchResult
- set_validator(origin, proof: Vec\<u8\>) -> DispatchResult

## Methods

- get(key: &CurrencyId) -> Option\<Price\>
