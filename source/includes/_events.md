Events API
===========
There are a variety of "events" that are emitted by the Augur contracts.  Each event is triggered by a user doing something on Augur, such as submitting an [Initial Report](#initial-report), [Finalizing](#finalized-market) a [Market](#market), [Filling](#fill-order) an [Open Order](#open-order), etc. For a full list, please refer to the [Event Types](#event-types) section.

<aside class="notice">The Events API described here should not be confused with the (unrelated) concept of "events" that <a href="#reporter">Reporters</a> <a href="#report">Report</a> on. These concepts are sufficiently different that the context should always make clear which kind of "event" is being discussed. However, if this documentation is ever ambiguous about this, feel free to drop us a note at <a href="mailto:hugs@augur.net">hugs@augur.net</a>, <a href="https://www.reddit.com/r/augur">Reddit</a>, or <a href="https://twitter.com/AugurProject">Twitter</a>!</aside>

The augur.js Events API includes event listeners, which provide notifications of events that are currently happening. The functions `augur.events.startAugurNodeEventListeners`, `augur.events.startBlockchainEventListeners`, and `augur.events.startBlockListeners` can be used to listen for incoming events. 

The function `augur.events.getAllAugurLogs` can be used to get an array of all Augur-related events that have been logged in the past.

<aside class="success">Events are retrieved either via push notification (if connected via WebSocket) or by polling the Ethereum node (if using HTTP RPC).  If polling, augur.js will check for new events every 6 seconds.</aside>

Events Functions
-----------
```javascript
augur.events.getAllAugurLogs({
  fromBlock: 1703565
}, function(error, allAugurLogs) {
  console.log(allAugurLogs); 
});
// example output:
[
  {
    [0: {
      "target": "0x913da4198e6be1d5f5e4a40d0667f70c0b5430eb",
      "value": "100000000000000000000000",
      "address": "0x6e968fe21894a35ba59ee8ec6f60ea0ddc3a59e5",
      "removed": false,
      "transactionHash": "0x78d918bce994b637bac7e80711b8d46bb2aea066bac26bdaf01afc0d93d6e309",
      "transactionIndex": 0,
      "logIndex": 0,
      "blockNumber": 23,
      "blockHash": "0x803c4f62e4ee742cc2f120de4c55a2ed1528b65521afcc4d7a8d5d7693c46688",
      "contractName": "LegacyReputationToken",
      "eventName": "Mint"
      },
      ...
    ]
  }
  ...
]

// No example for `augur.events.hashEventAbi`

augur.events.startAugurNodeEventListeners({
  TokensTransferred: function(error, result){
    console.log("A new TokensTransferred event has occurred: ", result); 
  }
}, function() {
  console.log("Started Augur Node event listeners!");
});
// example output:
"Started Augur Node event listeners!"
// example output after a TokensTransferred event occurs:
"A new TokensTransferred event has occurred:"
{
  "transactionHash": "0xbcb517796168347d92ef9448c8aec6f3112dfd5a41ebd9de0c097927cb01ca6b",
  "logIndex": 1,
  "sender": "0x8fa56abe36d8dc76cf85fecb6a3026733e0a12ac",
  "recipient": "0x40485264986740c8fb3d11e814bd94cf86012d29",
  "token": "0x13fa2334966b3cb6263ed56148323014c2ece753",
  "value": "0.001",
  "blockNumber": 1600771
}

augur.events.startBlockchainEventListeners(
  {
    Augur: {
      TokensTransferred: function(result) { console.log("A new TokensTransferred event has occurred:", result); }
    }
  }, 
  1490000, 
  function() {
    console.log("Setup is complete!");
  }
); 
// example output: 
"Starting blockstream at block  1490000"
"Setup is complete!"
// example output when a TokensTransferred event is detected: 
"A new TokensTransferred event has occurred:"
{
  "universe": "0x9c666216fe333c78aeb8aec56eb9ca17e6cd15c9",
  "token": "0x32714b2b26c8c96e7a4e63ee9523c71a03948683",
  "from": "0x913da4198e6be1d5f5e4a40d0667f70c0b5430eb",
  "to": "0xcfdd3f6dded3cbc2ba2d341337914b62e4cfe1de",
  "value": "1000000000000000",
  "tokenType": "0",
  "market": "0x0000000000000000000000000000000000000000",
  "address": "0x16300ddb8db4d1870f05036f339b444b7fbbc3a6",
  "removed": false,
  "transactionHash": "0x18a0eb4da2bb1eb7632402754fa8b534052af6018199a09b2fa81aeaf858747f",
  "transactionIndex": 0,
  "logIndex": 1,
  "blockNumber": 13765,
  "blockHash": "0x892774223a7473ddf387318be1351808ca71446b47538298cc9f37c85eb031c6",
  "contractName": "Augur",
  "eventName": "TokensTransferred"
}

augur.events.startBlockListeners({
  onAdded: function(block) {
    console.log("New block added!", block);
  }, 
  onRemoved: function(block) {
    console.log("Block removed!", block);
  }
});
// example output:
"true"
// example output after a block is added to the Ethereum blockchain:
"New block added!"
{
  "difficulty": "0x2",
  "extraData": "0xd783010703846765746887676f312e392e32856c696e75780000000000000000e76b9e0b0711c73221a771804a8c61f01d1ebf0cf6c8bafb17f93e671d38944422c9d3503d5e9ea9763907af5dd1064256b5bdb320f9c5247708a486dfd4292b01",
  "gasLimit": "0x6a9bc4",
  "gasUsed": "0x337bf",
  "hash": "0xaae5645ad53b51f85c55971ffcbfad7c5e44a940794fac1fab6fd93560c5055b",
  "logsBloom": "0x00000000040000000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000020000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000011000000000000000000000",
  "miner": "0x0000000000000000000000000000000000000000",
  "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "nonce": "0x0000000000000000",
  "number": "0x1877a7",
  "parentHash": "0xcb656990d8b7993183bed724f1af4b74d8a5e9ab1e6f7abdb3e5209504272cd2",
  "receiptsRoot": "0x4a0b60d86057241ca030d4ebe75df43ca7857ad30a542c21db8876fa7f6c836b",
  "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  "size": "0x5b4",
  "stateRoot": "0xd13fadf3b2edc48c2597900aa0bda6a97d17f4e907161eb061e34821b6067af0",
  "timestamp": "0x5a5e3023",
  "totalDifficulty": "0x2d6a4a",
  "transactions": [
    "0xce2fa17a608bfad0f1520e307b16f4bffbe1d8b6ec399fc5af7f8b6ab3327e1d",
    "0xcd54f9e1335e723448690876794664f21a270984f0f34153e891577df181c3d9",
    "0x776fbb3f73dee8f40361b06f642406909551d67e146d3e80ffe5a6fdf6f087a9",
    "0xc2efc2f39bf738cfbbf00f7d6ee89ecb6edaac2d0e0c1ae6a495786daf68b168",
    "0x422095f55a36c1f752328a82873a5724fde7c29b49b545a27762037303cac71f",
    "0x439b923062e2d23aa672a09ff221afdb81b6d0622fc29981ee742702a3157524"
  ],
  "transactionsRoot": "0x1b1b70c302dc90d27075af34bfb4bd42bb505822570b5f8131cfcc8cf0368396",
  "uncles": []
}

augur.events.stopAugurNodeEventListeners(
  function() {
    console.log("Stopped Augur Node event listeners!");
  }
);
// example output:
"Unsubscribed from 1e08901c-0797-49f2-b13f-e688e5695905"
"Stopped Augur Node event listeners!"

augur.events.stopBlockchainEventListeners();
// example output: 
"true"

augur.events.stopBlockListeners();
// example output:
"true"
```
### augur.events.getAllAugurLogs(p, callback)

Returns all Augur event logs on the Ethereum blockchain within a certain block range, sorted by `blockNumber` and `logIndex`.

Note: Depending on how many event logs there are to be retrieved, this function can take a long time to run.

#### **Parameters:**

* **`p`** (Object) Parameters object.
    * **`p.fromBlock`**  (number) &lt;optional> Block number to start looking up logs (default: `augur.constants.AUGUR_UPLOAD_BLOCK_NUMBER`). (Note: While this parameter is optional, specifying a `fromBlock` is recommended, since this function will take much longer to run if it has to scan every block in the Ethereum blockchain.)
    * **`p.toBlock`**  (number) &lt;optional> Block number where the log lookup should stop (default: current block number).
* **`callback`** (function) Called when all data has been received and parsed.

#### **Returns:**

* (Array.&lt;[AugurEventLog](#AugurEventLog)>) Array of AugurEventLog objects found on the Ethereum blockchain, sorted by `blockNumber` and `logIndex`.

### augur.events.hashEventAbi(eventAbi)

Generates a set of JavaScript bindings for the Solidity ABI passed in.

#### **Parameters:**

* **`eventAbi`** (Object) Parameters object.

#### **Returns:**

(string) 32-byte hexadecimal hash of the `eventAbi`.

### augur.events.startAugurNodeEventListeners(eventCallbacks, onSetupComplete)

Begins listening for events emitted by an [Augur Node](#augur-node).

#### **Parameters:**

* **`eventCallbacks`** (Object.&lt;function()>) Callbacks to fire when events are received, keyed by event name.
* **`onSetupComplete`** (function) &lt;optional> Called when all listeners are successfully set up.

### augur.events.startBlockchainEventListeners(eventCallbacks, startingBlockNumber, onSetupComplete)

Begins listening for events emitted by the Ethereum blockchain.

#### **Parameters:**

* **`eventCallbacks`** (Object.&lt;function()>) &lt;optional> Callbacks to fire when events are received, keyed by contract name and event name.
* **`startingBlockNumber`** (number) &lt;optional> Block number at which to start listening for blockchain events.
* **`onSetupComplete`** (function) &lt;optional> Called when all listeners are successfully set up.

### augur.events.startBlockListeners()

Start listening for blocks being added/removed from the Ethereum blockchain.

#### **Parameters:**

* **`blockCallbacks`** (Object) Parameters object. 
    * **`blockCallbacks.onAdded`** (function) &lt;optional> Callback to fire when new blocks are received.
    * **`blockCallbacks.onRemoved`** (function) &lt;optional> Callback to fire when blocks are removed.

#### **Returns:**

*  (boolean) `true` if listeners were successfully started, or `false` otherwise.

### augur.events.stopAugurNodeEventListeners(callback)

Removes all active listeners for events emitted by Augur Node.

#### **Parameters:**

* **`callback`** (function) &lt;optional> 

### augur.events.stopBlockchainEventListeners()

Removes all active listeners for events emitted by the Ethereum blockchain.

#### **Returns:**

*  (boolean) `true` if listeners were successfully stopped, or `false` otherwise.

### augur.events.stopBlockListeners()

Stop listening for blocks and block removals.

#### **Returns:**

*  (boolean) `true` if listeners were successfully stopped, or `false` otherwise.

Event Types
-----------
The following table shows the different types of events that Augur's smart contracts log to the Ethereum blockchain. In this table, the `Contract` field refers to the Solidity contract in which the event is defined ([source code](https://github.com/AugurProject/augur-core) / [contract addresses](https://github.com/AugurProject/augur-contracts/blob/master/addresses.json)), and and the `Data (indexed)` field describes which event fields are indexed on the Ethereum blockchain.

Label                     | Contract                                                                                   | Event Description | Data (indexed) | Data (non-indexed)
------------------------- | ------------------------------------------------------------------------------------------ | ----------------- | -------------- | ------------------
<a name="Approval"></a>Approval                  | [ERC20](https://github.com/AugurProject/augur-core/blob/master/source/contracts/libraries/token/ERC20.sol) | `spender` has been approved to spend `value` amount of ERC20 tokens on behalf of `owner`. | owner, spender | value
<a name="Burn"></a>Burn                      | [VariableSupplyToken](https://github.com/AugurProject/augur-core/blob/master/source/contracts/libraries/token/VariableSupplyToken.sol) | `value` amount of `target`'s tokens have been burned (i.e., completely destroyed).  | target | value
<a name="CompleteSetsPurchased"></a>CompleteSetsPurchased | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | The Ethereum address `account` purchased `numCompleteSets` in `market` of `universe`. | universe, market, account | numCompleteSets
<a name="CompleteSetsSold"></a>CompleteSetsSold | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | The Ethereum address `account` sold `numCompleteSets` in `market` of `universe`. | universe, market, account | numCompleteSets
<a name="DisputeCrowdsourcerCompleted"></a>DisputeCrowdsourcerCompleted | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | The Ethereum contract address `disputeCrowdsourcer` for `market` in `universe` filled the [Dispute Bond](#dispute-bond) required to [Challenge](#challenge) `market`'s [Tentative Outcome](#tentative-outcome). | universe, market | disputeCrowdsourcer 
<a name="DisputeCrowdsourcerContribution"></a>DisputeCrowdsourcerContribution | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | The Ethereum address `reporter` [Staked](#dispute-stake) `amountStaked` [REP](#rep) on the [Outcome](#outcome) for `disputeCrowdsourcer` in `market` of `universe`. | universe, reporter, market | disputeCrowdsourcer, amountStaked
<a name="DisputeCrowdsourcerCreated"></a>DisputeCrowdsourcerCreated | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | The Ethereum contract address `disputeCrowdsourcer` with [Payout Set](#payout-set) `payoutNumerators` and [Dispute Bond](#dispute-bond) size `size` was created in `market` of `universe`. | universe, market | disputeCrowdsourcer, payoutNumerators, size
<a name="DisputeCrowdsourcerRedeemed"></a>DisputeCrowdsourcerRedeemed | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | `reporter` redeemed `amountRedeemed` [REP](#rep) and received `repReceived` additional REP, plus `reportingFeesReceived` [attoETH](#atto-prefix), from `disputeCrowdsourcer` (with `payoutNumerators`) in `market` of `universe`. | universe, reporter, market | disputeCrowdsourcer, amountRedeemed, repReceived, reportingFeesReceived, payoutNumerators
<a name="EscapeHatchChanged"></a>EscapeHatchChanged      | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | In the event that the Augur development team must [halt](#developer-mode) Augur, normal functionality will be suspended, and users will be able to withdraw any ETH/REP funds they have in Augur throught the [TradingEscapeHatch](#trading-escape-hatch-tx-api) contract. Once the system is no longer halted, regular functionality will resume, and the TradingEscapeHatch contract's functionality will be disabled. Whenever the TradingEscapeHatch is toggled, the `EscapeHatchChanged` event will be logged, with the `isOn` parameter representing whether it is enabled or disabled. | | isOn
<a name="FeeWindowCreated"></a>FeeWindowCreated          | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | The Ethereum contract address `feeWindow` has created a new [Fee Window](#fee-window) (with ID `id` and running from `startTime` to `endTime`) in `universe`. | universe | feeWindow, startTime, endTime, id
<a name="FeeWindowRedeemed"></a>FeeWindowRedeemed | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | `reporter` redeemed `amountRedeemed` [REP](#rep) and received `reportingFeesReceived` [attoETH](#atto-prefix) from `feeWindow` in `market` of `universe`. | universe, reporter, feeWindow | amountRedeemed, reportingFeesReceived
<a name="InitialReporterRedeemed"></a>InitialReporterRedeemed | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | `reporter` redeemed `amountRedeemed` [REP](#rep) and received `repReceived` additional REP, plus `reportingFeesReceived` [attoETH](#atto-prefix), from `market` of `universe`. | universe, reporter, market | amountRedeemed, repReceived, reportingFeesReceived, payoutNumerators
<a name="InitialReportSubmitted"></a>InitialReportSubmitted | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | `reporter` has submitted an [Initial Report](#initial-report) for the `market` in `universe` with `amountStaked` [REP](#rep) staked on the [Outcome](#outcome) `payoutNumerators`. If `reporter` is the [Designated Reporter](#designated-reporter) (as opposed to the [First Public Reporter](#first-public-reporter)), `isDesignatedReporter` is set to true. | universe, reporter, market | amountStaked, isDesignatedReporter, payoutNumerators
<a name="InitialReporterTransferred"></a>InitialReporterTransferred | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | Ownership of an InitialReporter contract was transferred between Ethereum addresses `from` and `to` for `market` in `universe`. | universe, market | from, to
<a name="MarketCreated"></a>MarketCreated             | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | `marketCreator` has created a `marketType` `market` with `outcomes` and `topic` in `universe` for a price of `marketCreationFee` and a price range of `minPrice` to `maxPrice`. Additional information about `market` can be found in `description` and `extraInfo`. | topic, universe, marketCreator | description, extraInfo, market, outcomes, marketCreationFee, minPrice, maxPrice, marketType
<a name="MarketFinalized"></a>MarketFinalized           | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | The [Outcome](#outcome) of `market` in `universe` is now considered final. | universe, market |  
<a name="MarketMigrated"></a>MarketMigrated           | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | `market` was migrated from `originalUniverse` to `newUniverse`. | market, originalUniverse, newUniverse |  
<a name="MarketMailboxTransferred"></a>MarketMailboxTransferred  | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | Ownership of Market Creator Mailbox `mailbox` for `market` in `universe` was transferred from `from` address to `to` address. | universe, market, mailbox | from, to
<a name="MarketParticipantsDisavowed"></a>MarketParticipantsDisavowed   | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | The DisputeCrowdsourcers belonging to `market` in `universe` has been disavowed. | universe, market | 
<a name="MarketTransferred"></a>MarketTransferred  | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | Ownership of `market` in `universe` was transferred from `from` address to `to` address. | universe, market | from, to
<a name="Mint"></a>Mint                      | [VariableSupplyToken](https://github.com/AugurProject/augur-core/blob/master/source/contracts/libraries/token/VariableSupplyToken.sol) | `value` amount of brand new tokens were created for `target`. | target | value
<a name="OrderCanceled"></a>OrderCanceled             | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | `sender`'s [Order](#order) (with ID `orderId` and type `orderType`) for the Share Token at address `shareToken` was canceled in `universe`, and `sender` was refunded either `tokenRefund` in [attoETH](#atto-prefix) or `sharesRefund` Share Tokens. | universe, shareToken, sender | orderId, orderType, tokenRefund, sharesRefund
<a name="OrderCreated"></a>OrderCreated              | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | `creator` placed an `orderType` [Order](#order) with `orderId` in `universe` for `amount` of `shareToken` at `price` in the Trade Group `tradeGroupId`. `creator` put up either `moneyEscrowed` or `sharesEscrowed`. | creator, universe, shareToken  | orderType, amount, price, moneyEscrowed, sharesEscrowed, tradeGroupId, orderId
<a name="OrderFilled"></a>OrderFilled               | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | [Order](#order) `orderId` in `universe` for Share Token at address `shareToken` was filled by `filler` in Trade Group `tradeGroupId`. The [Order Creator](#order-creator) escrowed `numCreatorShares` or `numCreatorTokens` in [attoETH](#atto-prefix), and `filler` put up `numFillerShares` or `numFillerTokens` in attoETH. `amountFilled` attoShares were filled, and `marketCreatorFees` and `reporterFees` (denominated in attoETH) were spent to pay the [Market Creator](#market-creator) and [Reporters](#reporter). | universe, shareToken | filler, orderId, numCreatorShares, numCreatorTokens, numFillerShares, numFillerTokens, marketCreatorFees, reporterFees, amountFilled, tradeGroupId
<a name="ReportingParticipantDisavowed"></a>ReportingParticipantDisavowed   | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | The `reportingParticipant` (can be either a DisputeCrowdsourcer or an InitialReporter) belonging to `market` in `universe` has had its `fork` (or `forkAndRedeem`) function called on it.  | universe, market | reportingParticipant
<a name="TimestampSet"></a>TimestampSet              | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | The timestamp used by Augur was set/updated to `newTimestamp`. This event will be logged whenever `TimeControlled.setTimestamp` or `TimeControlled.incrementTimestamp` is called by the owner of the [TimeControlled](#time-controlled-tx-api) contract. |  | newTimestamp
<a name="TokensBurned"></a>TokensBurned              | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | Burned (i.e., completely destroyed) `amount` of `target`'s [attotokens](#atto-prefix) in `market` (if applicable) of `universe`. The destroyed tokens were of type `tokenType`, whose contract address is `token`. (Token types are as follows: ReputationToken = 0, ShareToken = 1, DisputeCrowdsourcer = 2, FeeWindow = 3, FeeToken = 4. FeeWindow tokens are [Participation Tokens](#participation-token). FeeTokens are only used internally for accounting purposes so that Stake holders can get Fees for every [Fee Window](#fee-window) in which they have Stake.) | universe, token, target | amount, tokenType, market
<a name="TokensMinted"></a>TokensMinted              | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | Created `amount` brand new [attotokens](#atto-prefix) for `target` in `market` (if applicable) of `universe` . The minted tokens are of type `tokenType`, whose contract address is `token`. (Token types are as follows: ReputationToken = 0, ShareToken = 1, DisputeCrowdsourcer = 2, FeeWindow = 3, FeeToken = 4. FeeWindow tokens are [Participation Tokens](#participation-token). FeeTokens are only used internally for accounting purposes so that Stake holders can get Fees for every [Fee Window](#fee-window) in which they have Stake.) | universe, token, target | amount, tokenType, market
<a name="TokensTransferred"></a>TokensTransferred         | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | `value` amount of the tokens in `market` (if applicable) of `universe` have been transferred between account `from` and `to`. The transferred tokens are of type `tokenType`, whose contract address is `token`. (Token types are as follows: ReputationToken = 0, ShareToken = 1, DisputeCrowdsourcer = 2, FeeWindow = 3, FeeToken = 4. FeeWindow tokens are [Participation Tokens](#participation-token). FeeTokens are only used internally for accounting purposes so that Stake holders can get Fees for every [Fee Window](#fee-window) in which they have Stake.) | universe, token, from | to, value, tokenType, market
<a name="TradingProceedsClaimed"></a>TradingProceedsClaimed    | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | `sender` has collected trading profits from the Share Token at address `shareToken` in [Finalized Market](#finalized-market) `market` in `universe`. `sender` had `numShares` Share Tokens, `numPayoutTokens` paid out, and a balance of `finalTokenBalance`. | universe, shareToken, sender | market, numShares, numPayoutTokens, finalTokenBalance
<a name="Transfer"></a>Transfer | [ERC20Basic](https://github.com/AugurProject/augur-core/blob/master/source/contracts/libraries/token/ERC20Basic.sol) | Transferred `value` [attotokens](#atto-prefix) between accounts `from` and `to`. | from, to | value
<a name="UniverseCreated"></a>UniverseCreated           | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | `childUniverse` has been created from `parentUniverse` where `payoutNumerators` and `invalid` represent the Payout Set of the Forking Market in `parentUniverse` and whether it was determined to be an Invalid Market. | parentUniverse, childUniverse | payoutNumerators, invalid
<a name="UniverseForked"></a>UniverseForked            | [Augur](https://github.com/AugurProject/augur-core/blob/master/source/contracts/Augur.sol) | A [Market](#market) in `universe` has had its [Tentative Outcome](#tentative-outcome) [Challenged](#challenge) with a [Dispute Bond](#dispute-bond) greater than the [Fork Threshold](#fork-threshold), which has caused `universe` to [Fork](#fork). | universe |