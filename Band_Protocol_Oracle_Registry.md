# Band Protocol Oracle Registry (Acala Network)

## Overview

the current abstraction implemented into Acala's oracle modules has already enabled any DeFi module to connect to any of the available oracle modules that the module wants. However, it is not currently possible for a module that is using an oracle module to switch the module it is using during runtime. Therefore, a hardfork is the only way to go.

To resolve this issue, we would like to propose Band Protocol's implementation of the oracle registry module, the specification of which we believe will enable the runtime-switching of registries mentioned above. 

Band's oracle registry module will have one main functionality; to be the hub of all oracle modules on Acala's network. Its main aim is to enable any DeFi module to be able to connect to any of the available oracle modules through oracle registry module, and to enable those module to more freely switch the oracle module that it is using, even during runtime.

This document only consider phase one.

## Dependency

- module-primitives
  - CurrencyId
- module-support
  - Price
- orml_traits
  - DataProvider

## Types

- trait DataProviderRegistry<Key, Value>
  - get(data_provider_id: &str, key: &Key) -> Option<Value>


## Traits

- Source1: DataProvider<CurrencyId, Price>
- Source2: DataProvider<CurrencyId, Price>
- Source3: DataProvider<CurrencyId, Price>
- ...
- SourceN: DataProvider<CurrencyId, Price>

# Methods

- get(data_provider_id: &str, key: &CurrencyId) -> Option<Price>
    - match data_provider_id
      - "Source1" => Source1::get(&key),
      - "Source2" => Source2::get(&key),
      - "Source3" => Source3::get(&key),
      - ...
      - "SourceN" => SourceN::get(&key),
      - _ => None
