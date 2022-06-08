# Bloom Filter in Ethereum

Bloom Filter 是一种可以快速检索的工具。Bloom Filter 本身由是一个长度为 m 的 bit array，k 个不相同的 hash 函数和源 dataset 组成。具体的说，Bloom Filter 是由 k 个不同的 hash function 将源 dataset hash 到 m 位的 bit array 构成。通过 Bloom Filter，我们可以快速检测出，一个 data 是不是在源 dataset 中（O(k) time）。

Bloom Filter 不保证完全的正确性。In other word, Bloom Filter 会出现 false positive 的结果。但是，如果一个被检索的 data，在 bloom filter 中得到了 false 的反馈，那他一定不在源 data 之中。

Bloom Filter 是 Ethereum 的核心功能之一。具体的代码是实现是在“core/types/bloom9.go”文件之中。

在文件的起始位置，定义了两个常量 BloomByteLength 和 BloomBitLength，大小分别是 256 和 8.

在 Ethereum 中的 Bloom Filter 是一个长度为 256 的 byte 数组组成的。

    Bloom represents a 2048 bit bloom filter
    type Bloom [BloomByteLength]byte

Ethereum 中 Bloom Filter 使用的 SHA Hash Function.

基本的思想是，使用三个 value 的值来判断 log 是否存在。
首先对 data 使用 SHA function 进行求值。选择 hash 后的
这三个 value 的选择 [0,1],[2,3],[4,5] 的值对 2048 取模得到目标位置的下标，并把这几个位置设为 1.

对待判断的 log 进行相同的操作。