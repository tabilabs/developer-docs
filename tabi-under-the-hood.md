# Tabi Under the hood

Tabi Chain is a modular gaming chain. The core tech stack of Tabi is **Omnicomputing**, featuring:

* **Omni execution layer**: Polymorphism VM with any-vm-compatibility, even works with self-defined runtime(non-Web3 VM).
* **Modular consensus layer**: We use ParallelBFT consensus to propose and organise blocks in parallel. But service can define any consensus mechanism at will.
* **Modular security layer**: Service can choose to use protocols like EigenLayer or Babylon to enable shared security.
* **Service-centric**: dApp or game deploys its standalone service, which is part of the Tabi Chain overall consensus but has its independent and customisable execution layer, data structure, consensus scheme, etc..

<figure><img src=".gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

This document will introduce Omnicomputing in a general and understandable way.

## Service-centric

* A dApp or game can launch its standalone service.
* Service is blockchain-ish but does not necessarily be a blockchain.
* Service has **single-node mode** and **multi-node mode**. Sing-node mode is for faster responsiveness and finality. Multi-node mode compose a more decentralised service.
* Services publish their blocks, which will form the overall consensus of the Tabi Chain.
* Every service is equal. Though there’s a Main Service for Tabi’s economy and ecosystem.

## Network Roles

### Validator

* Elected by Captain Nodes via dPoS
* Validate the Main Service
* Partially validate other services(i.e., only block format, signature verification etc. but not the state)

### Supervisor Node

* Elected by Captain Nodes via dPoS
* A special type of Validator with high performance
* Load consistent Polymorphism VMs for each service
* Re-execute blocks proposed by service node and confirm their state
* Slash the malicious service node by initiate a slash proposal with 2/3 majority vote from all Supervisor Nodes

### Service Node

* A node or a group of nodes can launch a service, with staked assets on Main Service and EigenLayer(if shared security enabled)

### Captain Node

* Elect Validators and Supervisors
* Enable modular security on EigenLayer and Babylon
* Act as members of random shuffling group in Proof of Engagement and other AVS built by Tabi

## Omni Execution Layer - Polymorphism VM

<figure><img src=".gitbook/assets/image 5 (1).png" alt=""><figcaption></figcaption></figure>

Polymorphism VM decouples input data, state transition functions and state output. Thus the execution environment is modular and customisable, and can support any VM or self-defined runtime/VM.

We can simply take it as a statemachine with blockchain-related capabilities. Some major components of the Poly-VM:

* ￼Input Interpreter
* Database
* State Transition Functions
* Transaction Accumulator
* State Accumulator
* Block Proposer

Several examples to demonstrate the flexibility of Poly-VM:

1. **Classic Blockchain VM**: To a EVM developer, though the Polymorphism VM is not directly equal to EVM from module structure perspective, but there’s no difference to use Polymorphism VM as EVM: input data format, computation logic, smart contracts coding, output state etc., everything are all the same. It also applies to SVM, MoveVM and theoretically any VM.
2. **Self-defined Runtime**: For a Web2 developer who has no knowledge about any Web3 VM, they can just compile their databases and functions led to database change(state transition functions) into a binary(or modules) as the State Transition Runtime, with required interfaces to Data Interpreter and Block Proposer. The blockchain-ish service thus establishes.
3. **UTXO**: You can implement UTXO-based system in Poly-VM.
4. **Gas-agnostic & Keyless**: Also in a self-defined runtime, because everything is dependent on the developer, they can remove some concepts like wallet, private keys, gas, etc.. Gasless is relative simple. But it needs more consideration about whether remove wallet account system explicitly (no asymmetric authentication at all) or implicitly (something like account abstraction).

### Web2 Runtime Example

Polymorphism VM is highly customisable, especially for Web2 developers who can port any business logic into Polymorphism VM using languages and frameworks they are familiar with. However, we still need to provide a basic sample and definition that should be universal, highly abstract, and heuristic.

Customising the behaviour of Polymorphism VM can be thought of as creating a state machine, where the part that developers really need to focus on is the State Transition Runtime.

<figure><img src=".gitbook/assets/image 6.png" alt=""><figcaption></figcaption></figure>

The State Transition Runtime can be embedded in the form of binaries or modules. At the same time, Supervisor Nodes will also run the corresponding State Transition Runtime to ensure verifiability.

In a blockchain-like systems, we need to ensure the transparency of inputs, the public visibility of state transitions, and the expressibility of the global state. To meet these requirements, we need to build a state machine runtime with the following characteristics:

1. The World DB contains all user data that needs to be recorded on the blockchain within the application. This data should be valuable and important, hence the necessity for a blockchain-like structure to ensure its availability. Therefore, not all data need to be recorded on the blockchain. For example, in a game, users' chat content can be discarded and thus does not need to be put on the blockchain.
2. It can express the complete world state. In many scenarios, such as in games, this expression does not necessarily imply a high degree of traceability – a simple accumulator would suffice, meaning that data structures like the Merkle tree are not always necessary. However, no matter what data structure representing world state is used, it is crucial that the world state for the World DB can be expressed in a form of summary.
3. Any function that can cause changes in the World DB is called a **State Transition Function** and should be encapsulated in the State Transition Runtime. Any modifications to the World DB outside of runtime should be considered illegal and rejected.
4. The input and output interfaces should meet the design of the Input Interpreter and Block Proposer. This point is relatively simple and will not be detailed here.

Now let’s go through a very simple and naive example.

#### World DB

In practice, a game or application is likely to use more than one database, and these databases may be different. Let's assume that a particular game uses both a relational database and a key-value database.

Here is a simple relational DB:

| UID     | Nonce | Extra Data Field 1 | Extra Data Field N |
| ------- | ----- | ------------------ | ------------------ |
| 1248972 | 52    | …                  | …                  |
| 1248973 | 16    | …                  | …                  |
| …       | …     | …                  | …                  |

1. UID: Represent a unique user, it could be public key or other identifier.
2. Nonce: To prevent replay attacks.
3. Extra Data Field(s): Any type of data.

Here is a simple key-value DB:

```
"0x23417528":"58873",
"0xa9201b73":"9302",
"0xaf82102":"abcde",
...
```

#### State Transition Functions

This is a simple example of a State Transition Function. When this function receives an input from the user, it simply multiplies it by 5 and modifies the data in the relational DB.

```
	input := 6666

    UID := "1248973"
    extraDataField1 := input * 5

    query := `UPDATE relationalDB SET extraDataField1 = $1 WHERE UID = $2;`
    _, err = db.Exec(query, UID, extraDataField1)
```

#### ￼World State Accumulator

We can construct a very simple hash accumulator to represent the world state: `A_s+1 = hash(A_s + hash(query))`

With such a construction, it can be ensured that after any modification to the World DB, there will always be a unique and deterministic state corresponding to that modification operation.

It is important to note that this means each State Transition Function must implement this method. This requirement can be enforced through the use of decorators, interfaces, hooks, or other logics specific to the language being used. Since different languages have different features, the details will not be discussed here.

Update for the KVDB is also in the same process.

#### Summary

Now we have a very simple State Transition Runtime that meets the requirements. Although we may still need additional modules like Account Helper, Bridge, and transaction queues to provide more functionalities. However, this abstraction is already sufficient to clarify the essence for developers and researchers.

## ParallelBFT

### Parallel Consensus

By parallel we mean 3 layers of parallel consensus

1.  **The overall consensus** combined with all blocks of all services produced in parallel and could form a DAG under certain conditions.

    <figure><img src=".gitbook/assets/parallel_1 (2).jpg" alt=""><figcaption></figcaption></figure>
2.  Blocks of one service can be produced from different nodes of the service simultaneously and forming a DAG.

    <figure><img src=".gitbook/assets/image 2 (1).png" alt=""><figcaption></figcaption></figure>
3.  Within each multi-node service, service nodes use **Parallel Hotstuff** consensus scheme produces blocks with parallel pipeline, which is 4x faster than traditional PBFT.

    <figure><img src=".gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

### State Merge

State merge is a necessary concept for DAG-based parallel consensus. In blockchain VM, the "state" refers to the current status of all data stored within the VM. An arrow pointing to the previous block means that the state of this block evolved from the previous state.

<figure><img src=".gitbook/assets/image 4.png" alt=""><figcaption></figcaption></figure>

Traditional linear consensus will use linear state transition while parallel consensus will use DAG structure. This could potentially introduce conflicts, but by reasonably partitioning and locking the resources, conflicts can be minimised to a great extent, significantly enhancing the overall consensus speed.

### Inter-service-operability

<figure><img src=".gitbook/assets/image 3 (3).png" alt=""><figcaption></figcaption></figure>

We have noticed that there is a similar Directed Acyclic Graph (DAG) structure between different services to represent inter-service state merge. Due to the heterogeneity between services, inter-chain state merge is implicit and indirect. Namely, partial state data of service A affects the corresponding state in service B in the own rule of B.

Some one may regard this as cross-chain action but indeed they are quite different:

* Protocols that use block headers to transmit information rely on block headers to ensure the verifiability of information across domains. However, these solutions based on block headers (or other methods like zero-knowledge proofs) require a fixed set of trusted witnesses and are difficult to protect against fork attacks. In Tabi Chain, however, the various services can achieve trustlessness because all services are within a single consensus system.
* Cross-chain bridges are generally passive, that is, after initiating a transaction on Chain A, the witnesses on the bridge on Chain B respond and perform corresponding operations on Chain B. However, this model does not support ok. You may achieve this operation in traditional cross-chain solutions through another set of network roles proactively, but this is also irrelevant to the chain's own nodes and may result in service unavailability and fraud. In Tabi Chain, each service can customise its own reference requirements and proactively access external statuses to change its internal state.

In summary, the inter-service information cross-domain transmission in Tabi boasts better interoperability because it is more flexible and secure.

### Block Finality in Services

* Supervisors will run corresponding Polymorphism VMs for each service. So they have the capability to examine the state in the blocks proposed by services.
* If any block has 2/3 majority votes from Supervisors, it can be regarded as finalised.

### Modular Consensus

1. Single-node mode offers faster response and finality.
2. Multi-node provides a more decentralised network. Developers can also choose from different consensus algorithms for multi-node mode, like Hotstuff, Tendermint, **PoW**, etc.

### Supervisor Nodes Grouping & Rotation

TBD

## Modular Security

Service can launch with dual-stake both on EigenLayer and Tabi to get shared security.

### EigenLayer

TBD

### Babylon

TBD
