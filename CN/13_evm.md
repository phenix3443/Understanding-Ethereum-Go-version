# EVM in Practice

## General

EVM 是 Ethereum 中最核心的模块，也可以说是 Ethereum 运行机制中的灵魂模块。

如果我们抛开 Blockchain 的属性，那么 EVM 可以类比 JVM（或者其他的类似的 Virtual Machine)。

EVM，由 Stack，Program Counter，Gas available，Memory，Storage，Opcodes 组成。

## EVM Instructions

关于 EVM 的指令，我们首先关注会与 StateDB 交互，甚至会引发 Disk I/O 的指令。这些指令包括：
>`Balance`,`Sload`,`Sstore`, `EXTCODESIZE`,`EXTCODECOPY`,`EXTCODEHASH`,`SELFDESTRUCT`,`LOG0`,`LOG1`,`LOG2`,`LOG3`,`LOG4`,`KECCAK256`。

其中`LOG0`,`LOG1`,`LOG2`,`LOG3`,`LOG4`在本质上都调用了同一个底层函数`makeLog`。

## EVM Trace

我们知道，在以太坊中有两种类型的交易，

1. Native 的 Ether 的转账交易。
2. 调用合约函数的交易。调用合约的交易，本质上是实行了一段函数代码，由于是图灵完备的，这算代码可以任意的运行。作为用户，我们只需要知道 Transaction 的最终的运行结果（最终修改的 Storage 的结果）。但是对于开发人员，我们需要了解交易运行的最终结果，我们还需要了解在 Transaction 执行过程中的一些中间状态来方便 debug 和调优。

为了满足开发人员的这种需求，go-ethereum 提供了 EVM Tracing 的模块，来 trace Transaction 执行时 EVM 中一些值的情况。

目前，这种 EVM 的 Trace 是以 transaction 执行时的调用的 opcode 单位开展的。EVM 每执行一个指令，都会将当前 Stack，Memory，Storage，Transaction 剩余的 Gas 量，当前执行的指令的信息输出出来。

### EVM Logger
