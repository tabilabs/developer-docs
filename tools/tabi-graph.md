# Tabi Graph

### Introduction

Tabi Graph is built using Graph Node, an open-source Rust implementation whose event sources come from the Ethereum blockchain to deterministically update data stores that can be queried through GraphQL endpoints.

### Instructions

1. **Installing the Graph CLI**

On your local computer, run one of the following commands: Use npm:

```
npm install -g @graphprotocol/graph-cli
```

Use yarn:

```
yarn global add @graphprotocol/graph-cli
```

2. **Initialization**

```
graph init --studio <SUBGRAPH_SLUG>
```

3. **Subgraph preparation**

When making changes to Subgraph, three main files will be used:

* List (subgraph.yaml) - The list defines which data sources will be indexed by the subgraph.
* Schema (schema.graphql) - GraphQL schema defines the data retrieved from the subgraph.
* AssemblyScript mapping (mapping.ts) - code that converts data in a data source into entities defined in a schema.

Refer to [The Graph](https://thegraph.com/docs/zh/developing/creating-a-subgraph/)

4. **Deployment**

Once your Subgraph is written, run the following command:

```
graph codegen
graph build
graph deploy <SUBGRAPH_SLUG> --ipfs https://graph.testnet.tabichain.com/ipfs --node https://graph.testnet.tabichain.com/deploy
```

5. **Query**

Query address: https://graph.testnet.tabichain.com/subgraphs/name/ < NAME >

See [GraphQL API for details](https://thegraph.com/docs/zh/querying/querying-best-practices/)

### Example

[https://github.com/graphprotocol/example-subgraph\
](https://github.com/graphprotocol/example-subgraph)

### Reference

[https://github.com/graphprotocol/graph-node?tab=readme-ov-file](https://github.com/graphprotocol/graph-node?tab=readme-ov-file)

[https://thegraph.com/docs/zh/developing/creating-a-subgraph](https://thegraph.com/docs/zh/developing/creating-a-subgraph)

[https://thegraph.com/docs/zh/quick-start\
](https://thegraph.com/docs/zh/quick-start)
