# Mining

## Block Reward

## How to Seal Block

其中一个关键的函数是`miner/worker.go`中的`fillTransactions()`函数。这个函数会从 transaction pool 中的 Pending 的 transactions 选取若干的交易并且将他们按照 Gas Price 和 Nonce 的顺序进行排序形成新的 tx set 并传递给`commitTransactions()`函数。在`fillTransactions()`函数会首先处理 Local Pool 中的交易，然后再处理从网络中接受到的远程交易。

目前 Block 中的 Transaction 的顺序就是由`fillTransactions()`函数通过调用`types/transaction.go`中的`NewTransactionsByPriceAndNonce()`来决定。

也就说如果我们希望修改 Block 中 Transaction 的打包顺序和从 Transaction Pool 选择 Transactions 的策略的话，我们可以通修改`fillTransactions()`函数。

`commitTransactions()`函数的主体是一个 for 循环体。在这个 for 循环中，函数会从 txs 中不断拿出头部的 tx 进行调用`commitTransaction()`函数进行处理。在 Transaction 那一个 Section 我们提到的`commitTransaction()`函数会将成功执行的 Transaction 保存在`env.txs`中。
