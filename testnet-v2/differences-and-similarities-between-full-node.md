# Differences and Similarities Between Full Node

**1. Full NodeFunctions and Features:**

* **Complete Data Storage:** A Full Node downloads and verifies the entire blockchain history, including all transactions and states.
* **Data Provider:** It serves other nodes by responding to data queries and validating blocks and transactions.
* **High Resource Requirements:** Requires significant storage, computing power, and a stable internet connection.
* **High Security:** With the full blockchain history, it plays a vital role in preventing forks and ensuring network integrity.

**Role:**

* Full nodes act as the **backbone of the network**, ensuring data availability and consistency.
* They don't participate in consensus unless configured as validators but are essential for network health.

***

**2. Captain NodeFunctions and Features:**

* **Governance Node for the Tabi Incentive Mechanism:** Captain Nodes play a key role in shaping the TabiChain ecosystem by deciding which games are launched and thriving within the chain.
* **Revenue Distribution Power:** Through voting, Captain Node holders influence the percentage of revenue sharing allocated to different games and projects.
* **Mining Power:** Captain Nodes allow holders to participate in **PoW-based mining**, collectively earning up to 40% of the $Tabi supply over three years.
* **Node Levels and Tiers:** 5 distinct levels, each with multiple tiers. Benefits remain consistent within a tier, but mining commencement times can vary.
* **Low Technical Requirements:** Requires minimal technical knowledge or cloud hosting to manage.

**Role:**

* Captain Nodes **do not participate in consensus** like validators but focus on **mining and governance**.
* They are responsible for **economic influence** within the ecosystem by deciding revenue flows, and they receive **airdrops and in-game benefits** from TabiChain projects.

***

**3. ValidatorFunctions and Features:**

* **Core Consensus Node:** Validators are key participants in the consensus mechanism (Tendermint in Cosmos-based chains) and are responsible for **proposing and validating blocks**.
* **Staking and Rewards:** Validators stake Tabi tokens and earn inflation rewards (4% annual inflation distributed to stakers).
* **Slashing Mechanism:** Validators can face slashing penalties if they act maliciously or go offline for an extended period.
* **High Resource Requirements:** Requires high computational power and uptime to ensure block production and maintain network security.

**Role:**

* Validators are **critical to the consensus layer**, maintaining the blockchain's security, validating blocks, and ensuring network stability.
* They also participate in **governance** by voting on network proposals.

***

**Comparison Table**

| Node Type    | Data Storage             | Consensus Participation | Resource Requirements | Primary Use Case                                  |
| ------------ | ------------------------ | ----------------------- | --------------------- | ------------------------------------------------- |
| Full Node    | Complete blockchain data | Optional                | High                  | Data validation, storage, service for other nodes |
| Captain Node | No full data storage     | No                      | Low                   | Governance, PoW-based mining, revenue sharing     |
| Validator    | Complete blockchain data | Yes                     | Very High             | Consensus, block production, governance           |

***

**Summary**

1. **Full Nodes** ensure network integrity by storing and validating all blockchain data. They provide essential services to other nodes by responding to queries and maintaining data consistency.
2. **Captain Nodes** play a key **governance and PoW-based mining role** in TabiChain, influencing which projects thrive and deciding revenue sharing. They are suitable for users without deep technical expertise.
3. **Validators** are responsible for **block production and consensus**, with high technical requirements and the need for continuous operation.

**On Tabichain:**

* **Validators** ensure network security and stability, participating in staking and governance to support the chain’s core infrastructure.
* **Captain Nodes** act as **economic and governance drivers**, mining rewards and influencing the chain’s ecosystem growth.
* **Full Nodes** form the **data backbone**, supporting data availability and consistency across the network.
