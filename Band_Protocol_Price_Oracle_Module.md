# Band Protocol Price Oracle Module (Acala Network)

## Overview

This proposal introduces an implementation of Band Protocol's price oracle module to the Acala Network. The module will act as the main hub from which other modules in the network can query the price information of various tokens and cryptocurrecy from Band Protocol oracle.

The inclusion of this module would first and foremost be complementary to Acala's currently implemented oracle module. With the introduction of `OracleRegistry`, this module will serve as an option for any DeFi module to access oracle data, allowing it to use Band oracle data as another reference datapoint or as a standalone dependency. Anyone looking to acquire the price data can rely on Band's off-chain oracle to handle the data queries and aggregation logic, and they themselves would simply have to submit the proof generated on BandChain to the module in order to add new datapoint to the Acala's Network. 

We believe that the addition of such price oracle will have a net positive on Acala's DeFi ecosystem as a whole.

Implementation-wise, the price oracle module will have three main functionalities:

1. Validate data relayed from the BandChain and store the price data to state
2. Expose a function that allows other modules to read price information from the store
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
  - // TODO: this crate is not available -- we're working with Chorus One and Interlay team on this
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
