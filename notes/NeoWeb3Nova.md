---
timezone: UTC+8
---

# Neo.Yun

**GitHub ID:** NeoWeb3Nova

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
今天核心学习了，monad的gas机制：

## Monad 和 Ethereum 最大区别：Gas Limit 是否会真的收费

这是重点。

在 **Ethereum** 上：

```
实际手续费 = Gas Used × 实际 Gas Price
```

假设你设置：

```
Gas Limit = 100,000
Gas Used = 21,000
```

Ethereum 通常只按 `21,000` 收费，没用完的 gas limit 不扣钱。

但在 **Monad** 上，官方文档写的是：

```
实际手续费 = Gas Limit × Gas Price
```

也就是说，在 Monad 上，如果你把 Gas Limit 设置太高，哪怕实际只用了很少，也可能要按你声明的 Gas Limit 付费。Monad 这样设计，是为了配合它的 **异步执行架构**：区块 leader 在执行交易之前就要构建区块、验证者也会先投票；如果允许用户随便报很高 gas limit 但最后只按 gas used 付费，就容易制造 DoS/资源占用问题。

简单说：

```
Ethereum：Gas Limit 更像“保险额度”，没用完会退。
Monad：Gas Limit 更像“资源预订量”，你报多少就按多少算。
```

## 4\. Monad 为什么要这么设计？

因为 Monad 的目标不是简单复制 Ethereum，而是提高并行执行、高吞吐和异步处理能力。

在 Ethereum 里，执行和区块处理更紧密，实际用了多少 gas 比较容易成为收费依据。

Monad 为了提升性能，采用延迟/异步执行思路：交易先被排进区块，执行结果后面再处理。这样如果仍按实际 Gas Used 收费，用户可以故意设置很大的 Gas Limit，占用区块资源，但最后只执行很少逻辑，成本很低。Monad 按 Gas Limit 收费，就是为了让“占用资源”本身有成本。

这也是为什么在 Monad 上写合约、调钱包、估 gas 时，**Gas Limit 不能像 Ethereum 那样随便给很大 buffer**。官方和生态文章都建议使用 `eth_estimateGas` 作为基准，再加一个适度 buffer，不要过度放大。

## 5\. Base Fee 调整机制也不同

Monad 和 Ethereum 都有动态 Base Fee，但参数不完全一样。

Ethereum 的 Base Fee 会根据上一个区块的 gas 使用量调整，目标区块大小是 gas limit 的一半，Base Fee 每个区块最多上下调整约 12.5%。

Monad 的官方文档说，它的 Base Fee 控制器类似 EIP-1559，但特点是：

```
上涨更慢
下降更快
最低 Base Fee = 100 MON-gwei
Block gas limit = 200M gas
Transaction gas limit = 30M gas
```

Monad 生态技术文章还提到，Monad 的目标利用率是 80%，高于 Ethereum EIP-1559 的 50% 目标，这更适合高吞吐链的资源利用方式。

## 6\. 一句话总结

**Monad 的 Gas 价格模型像 Ethereum：Base Fee + Priority Fee + Max Fee；但收费对象不一样：Ethereum 主要按 Gas Used 收费，Monad 按 Gas Limit 收费。**
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
