# Curve Leverage Lending Bot CosmWasm smart contract

This is a CosmWasm smart contract to manage Curve Leverage Lending Bot smart contract on EVM chain written in Vyper.

Users can create a Curve Leverage Lending Bot by depositing their tokens into a Vyper smart contract with options for leverage on the EVM chain.

A scheduler or script fetches events from the Vyper smart contract and runs `repay_bot` function with the parameters via Compass-EVM on high risk or expiration.

The Vyper smart contract will then repay the bot.

## ExecuteMsg

### RepayBot

Run `repay_bot` function on Vyper smart contract.

| Key                        | Type            | Description                     |
|----------------------------|-----------------|---------------------------------|
| bots                       | DynArray[address, MAX_SIZE] | Array of bot addresses |
| callbackers                | DynArray[address, MAX_SIZE] | Array of callbacker addresses |
| callback_args              | DynArray[DynArray[uint256,5], MAX_SIZE] | Nested array of callback arguments for each bot |

### CreateBot

Create and start a new bot with the provided parameters.

| Key               | Type                                 | Description                             |
|-------------------|--------------------------------------|-----------------------------------------|
| swap_infos        | DynArray[SwapInfo, MAX_SIZE]         | Array of SwapInfo structures            |
| collateral        | address                              | Address of the collateral token         |
| debt              | uint256                              | Amount of debt                          |
| N                 | uint256                              | Number of intervals for repayment      |
| callbacker        | address                              | Address of the callbacker contract      |
| callback_args     | DynArray[uint256, 5]                 | Array of callback arguments             |
| leverage          | uint256                              | Leverage amount                         |
| deleverage_percentage | uint256                          | Percentage for deleveraging            |
| health_threshold  | uint256                              | Health threshold for automatic repayment|
| expire            | uint256                              | Expiration time                         |
| number_trades     | uint256                              | Number of trades to perform             |
| interval          | uint256                              | Time interval between trades            |

### Update*

Run `update_*` function on Vyper smart contract to update this contract address data in the Vyper contract.

| Key | Type | Description |
|-----|------|-------------|
| new_* | address/uint256 | New value for the respective setting |

## QueryMsg

### State

Get the state of a bot.

| Key  | Type   | Description      |
|------|--------|------------------|
| bot  | address | Bot address      |

#### Response

| Key          | Type     | Description                  |
|--------------|----------|------------------------------|
| state        | uint256[4] | Array representing bot state |

### Health

Get the health of a bot.

| Key  | Type   | Description      |
|------|--------|------------------|
| bot  | address | Bot address      |

#### Response

| Key          | Type     | Description                  |
|--------------|----------|------------------------------|
| health       | int256 | Health value of the bot       |

## Structs

### SwapInfo

| Key           | Type              | Description                             |
|---------------|-------------------|-----------------------------------------|
| route         | address[11]       | Array of addresses for swap route       |
| swap_params   | uint256[5][5]     | Parameters for the swap                 |
| amount        | uint256           | Amount to swap                          |
| expected      | uint256           | Expected return amount                  |
| pools         | address[5]        | Addresses of the pools involved in swap |

### BotInfo

| Key                     | Type           | Description                           |
|-------------------------|----------------|---------------------------------------|
| depositor               | address        | Address of the depositor              |
| collateral              | address        | Address of the collateral token       |
| amount                  | uint256        | Amount of collateral                  |
| debt                    | uint256        | Amount of debt                        |
| N                       | uint256        | Number of intervals for repayment    |
| leverage                | uint256        | Leverage amount                       |
| deleverage_percentage   | uint256        | Percentage for deleveraging           |
| health_threshold        | uint256        | Health threshold for automatic repayment|
| expire                  | uint256        | Expiration time                       |
| remaining_count         | uint256        | Remaining count of operations         |
| interval                | uint256        | Time interval between trades          |

This structure gives you a comprehensive update to reflect the new features and structures introduced in the code, such as the `DynArray` type used for dynamic array handling and the specific structures for bot creation and management.
