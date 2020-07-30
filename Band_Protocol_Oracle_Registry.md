# Decentralized Oracle Registry (Acala Network)

## Overview

With the current abstraction, any DeFi module deployed on Acala has access to the `Oracle` module to query the price data retrieved from external sources. Although this works well in a short-term, the lack of incentives for the whitelisted data feeders may become a barrier of entry for new DeFi projects that require new asset prices to operate. Not only having one oracle module will make Acala prone to centralization and data manipulation, but also denial-of-service attacks that may cripple the DeFi functionalities or, in worst case, result in loss of the user funds. Different oracle models and providers are needed on Acala's Network to broaden the data availability and security of the DeFi modules as a whole.

Therefore, we would like to propose the specification for an `OracleRegistry` module, a standardized registry which functions as a hub of all oracle modules on Acala's network. It implements `DataProviderRegistry` trait and allows a collection of `DataProvider` to be assigned and then freely utilized by any DeFi module.

Example Implementation: https://github.com/bandprotocol/band-integration-acala/pull/4/files

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
