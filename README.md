# awesome-parallel-blockchain
Faster, robust, and high-performanced blockchain systems are the cornerstone of tomorrow.

1. `Contributions from the community are welcome; make sure the material you add is worth reading, at least you have read it yourself.`
2. About the Format:
    - for the papers: [Type] [Conference(If any)] Name, Author, Year, Accessible links
        - Type: <font color=#1E88E5>[Academic]</font> Paper that demonstrates more about theoretical algorithms or theoretical analysis.
        - Type: <font color=#00E676>[Engineering]</font> Paper that introduces a new system architecture or practical algorithms
    - for the repos: [Type] Repo Name, Org, Year, Accessible links
        - Type: <font color=#FFB300>[PoC]</font> Basic runnable codebase with some basic functions to evaluate the ideas. 
        - Type: <font color=#F4511E>[Developing]</font> Systems under development that have some basic features but are not yet ready for use in a stable production environment.
        - Type: <font color=#64DD17>[Product]</font> Systems that are still under maintenance are already ready to be used in production environments.
        - Type: <font color=#F50057>[Archive]</font> Non-maintained code base.


## Parallelism and concurrency in brief
### Background
Parallelism and concurrency are two sharp swords that could improve the transaction execution performance of Blockchain and Blockchain-related VMs(i.e., EVM and SVM). 

This knowledge base collects information about state-of-the-art research and engineering works about parallel and concurrent blockchain, blockchain VM, and other related systems, with some relevant examples for better understanding.

**Parallelism** and **concurrency** are two easily confused terms; although they are both used to improve the efficiency of the execution of the system, they differ in meaning and in the scenarios in which they are used.

**Parallelism**, typically refers to multiple processes or threads working on different tasks at the same time. For example, ten threads are executing ten different transactions simultaneously. Or think of ten dogs eating from ten food bowls.

<div align="center"><img src="img/misc/parallel_dogs.png" width="45%">
</div>

**Concurrency**, typically refers to a thread simultaneously working multiple tasks to increase efficiency. For instance, certain operations, like reading data from disk(i.e., sLoad in EVM), might need to wait for a while in order to be completed. In such case, the main thread could move ahead and do other duties, like computation. Then, It returns to continuing execution after the disk's data has been read in complete. Again, think about using a bowl to feed ten dogs, and being in the middle of a meal the dog is chewing on a bone, when you could start by letting him out of the bowl for a while and letting the other dogs eat first.
<div align="center"><img src="img/misc/concurrent_dogs.png" width="45%">
</div>

For further reading: [Difference between Concurrency and Parallelism](https://www.geeksforgeeks.org/difference-between-concurrency-and-parallelism/)

### Core challenges

In general, the core challenge of adopting a parallel or concurrent method is the data race problem, read-write conflict, or data hazard problem. All these terms describe the same issue: different threads or operations are trying to read and modify the same data at the same time. 

Consider a transfer scenario on Ethereum where Alice and Bob both want to transfer 10 ETH to Carl at the same time. Suppose Carl's initial balance is 100 ETH. After these two transactions are completed, Carl's balance should be 120 ETH. Let's consider a fully concurrent scenario where Alice's and Bob's transactions start executing at the same time. The initial balance of Carl that their transactions read would be 100 ETH (see the error?). Eventually, they will update Carl's balance as 110 ETH.

To resolve a data race issue, there are usually three ways:
- Protecting competing data by adding **locks**,
- **Scheduling module** to avoid conflicts,
- **Optimistic approach** that doesn't worry about conflicts but **rolls back** transactions after encounting the conflict issue.

So how do we implement these solutions in the blockchain world? Let's dig in!

### Case Study: Parallel/Concurrent EVM

In the current EVM architecture, the most fine-grained read and write operators are **[sload](https://github.com/ethereum/go-ethereum/blob/81fd1b3cf9c4c4c9f0e06f8bdcbaa8b29c81b052/core/vm/instructions.go#L515-L521)** and **[sstore](https://github.com/ethereum/go-ethereum/blob/81fd1b3cf9c4c4c9f0e06f8bdcbaa8b29c81b052/core/vm/instructions.go#L523-L531)**, which read and write data from the state trie, respectively. So the easiest entry point is how to make sure that different threads don't conflict on these two operators. In fact, there is a special kind of transaction in Ethereum that includes a special structure named [Access list](https://eips.ethereum.org/EIPS/eip-2930), which allows transactions carrying storage addresses that it will read and modify. Therefore, this is a good entry point for implementing scheduling-based concurrent methods.

In terms of system implementations, there are three general forms of Parallel/Concurrent EVM.

1. Multi-threads with one EVM instance.
2. Multi-threads with multi-EMV instances on one node.
3. Multi-threads with multi-EMV instances on multi-nodes (basically, this is system-level sharding).

## Parallelsim and Concurrency in Blockchain

### Research Papers
- <font color=#1E88E5>[Academic]</font> An Empirical Study of Speculative Concurrency in Ethereum
Smart Contracts, 2019, [[Paper]](https://arxiv.org/pdf/1901.01376.pdf)
- <font color=#1E88E5>[Academic]</font> Parallel and Asynchronous Smart Contract Execution, 2021, [[Paper]](https://ieeexplore.ieee.org/document/9477197)
- <font color=#00E676>[Engineering]</font> Block-STM: Scaling Blockchain Execution by Turning Ordering Curse to a Performance Blessing, **Aptos**, 2022, [[Paper]](https://arxiv.org/abs/2203.06871), [[Video]](https://www.youtube.com/watch?v=fK_V9Z1q10U)

### Engineering Repos
- <font color=#FFB300>[PoC]</font> Parallel-go-ethereum, **ABCDELabs**, 2022, [[Codebase]](https://github.com/ABCDELabs/parallel-go-ethereum)
- <font color=#F4511E>[Developing]</font> Evmone-compiler, **MegaETH**, 2023, [[Codebase]](https://github.com/megaeth-labs/evmone-compiler)

### Other Materials

## Parallelsim and Concurrency in DBMS

### Research Papers

### Engineering Repos

### Other Materials
- [Tech Doc] Concurrency Control in PostgreSQL, PostgreSQL, [[Doc link]](https://www.postgresql.org/docs/current/transaction-iso.html)

## Parallelsim and Concurrency in OS

### Research Papers
### Engineering Repos
### Other Materials

## Appendix