# Get started

This guide will quickly take you through how to initialize, create, and deploy your subgraph with OKC's Subgraph service.

By deploying the subgraph, you can explore the subgraph GraphQL API by issuing queries and viewing the schema.

OKC Subgraph currently provides the Hosted Service. If you want to deploy a subgraph on it, please send your application to  [subgraph@okx.com](mailto:subgraph@okx.com), and provide the information below:

| **Information**                                          | **Sample**                                  |
| ------------------------------------------------------------ | ------------------------------------------- |
| Name of project                                              | OKC Test Project                            |
| Official website (if any)                                    | https://www.okx.com/cn/oktc/subgraph        |
| Github (if any)                                              | https://github.com/okx-subgraph/oktc-subgraph-example           |
| Type of project                                              | DeFi，GameFi，NFT...                        |
| Introduction to project                                      | Okctestproject is an AMM swap               |
| Uses of the Subgraph service                                 | Query business data and present it to users |
| Special requirements for Subgraph services (e.g., QPS, please specify if any) | QPS > 100                                   |
| Way of contacting you (please provide Telegram account and email) | @okctestproject                             |
| Expected subgraph name                                       | okctestproject-statistics-v1                |

# How to deploy OKTC Subgraph

## Develop Subgraph

### Code specification

Refer to https://github.com/okc-subgraph/subgraph-example/blob/main/docs/Developing.md

### Version requirement

-  "@graphprotocol/graph-cli": "^0.20.1"
- "@graphprotocol/graph-ts": "^0.20.1"
- "npm": "^7.20.5"

### Block height requirement

- The query block height can't be lower than 13,400,000
- Sample：https://github.com/okc-subgraph/subgraph-example

## Upload Subgraph code to Github

Special notes

- If the code repository is a private repository, you need to add your OKC Github account subgraph@okx.com. (This step isn't required for public code repositories)
- Before uploading the subgraph code, please make sure it's been compiled

## Deploy the Subgraph on the Hosted Service

OKC will help you with the deployment

## With the Subgraph deployed, you can query the Graph

After the subgraph is deployed, it takes some time to synchronize the block height before it can be queried.

# Sample Queries

- Query URL ：https://www.okx.com/okc/subgraph-hosted/name/<Subgraph_name>
- Query Playground：https://www.okx.com/okc/subgraph-hosted/name/<Subgraph_name>/graphql

Please replace `<Subgraph_name>` in the example with your own Subgraph name

## Example

### Specification definition of GraphQL language

```Shell
query [operationName]([variableName]: [variableType]) {
  [queryName]([argumentName]: [variableName]) {
    # "{ ... }" express a Selection-Set, we are querying fields from `queryName`.
    [field]
    [field]
  }
}
```

While the list of syntactic do's and don'ts is long, here are the essential rules to keep in mind when it comes to writing GraphQL queries:

- Each `queryName` must only be used once per operation.
- Each `field` must be used only once in a selection (we cannot query `id` twice under `token`)
- Some `field`s or queries (like `tokens`) return complex types that require a selection of sub-field. Not providing a selection when expected (or providing one when not expected - for example, on `id`) will raise an error. 
- Any variable assigned to an argument must match its type.
- In a given list of variables, each of them must be unique.
- All defined variables must be used.

Failing to follow the above rules will end with an error from the Graph API.

### The anatomy of a GraphQL query

Unlike REST API, a GraphQL API is built upon a Schema that defines which queries can be performed.

For example, a query to get a block using the `GetBlocks` query will look as follows:

```Shell
query GetBlocks($id: ID!) {
  blocks(id: $id) {
    id
    number
    timestamp
  }
}
```

It will return the following predictable JSON response ( *when passing the correct* *`$id `**variable value* ):

```Shell
{
    "data": {
        "blocks": [
            {
                "id": "...",
                "number": "...",
                "timestamp": "..."
            }
        ]
    }
}
```

#### GraphQL API

- When querying a collection, the `orderBy `parameter can be used to sort by specific properties. `orderDirection `can be used to specify the sort
- When querying a collection, you can use the `first `parameter to page from the beginning of the collection. The `skip `parameter can be used to skip entities and pagination.
- When querying a collection, you can use the `where `parameter to filter different properties

example:

```Shell
query GetBlocks {
  blocks(
    first: 10
    orderBy: id
    orderDirection: desc
    skip: 10
    where: {number_gt: "17007257"}
  ) {
    id
    number
    timestamp
  }
}
```

For more features (such as time-travel queries, fulltext search queries), please refer to https://thegraph.com/docs/zh/querying/graphql-api/

#### Combine multiple queries

Your application may need to query data for multiple entities, and you can send multiple queries in the same GraphQL request as follows:

```Shell
query GetInfo {
  blocks(
    first: 10
    orderBy: id
    orderDirection: desc
    where: {id: "0xffffc9887a687234b1076e3ccfb19d58b19746795ae13a5eea19286d6fc34f0b"}
  ) {
    id
    number
    timestamp
  }
  submits(first: 10, orderBy: id, orderDirection: asc) {
    id
    from
    amount
  }
}
```

### Graph Explorer

Using Graph Explorer will help you query data more easily

1. Click "Explorer" in the upper left corner to expand

![img](https://github.com/okc-subgraph/subgraph-example/blob/main/docs/images/Explorer.png)

2. Select the query entity and filter

![img](https://github.com/okc-subgraph/subgraph-example/blob/main/docs/images/Filter.png)

3. Click this button to get results

![img](https://github.com/okc-subgraph/subgraph-example/blob/main/docs/images/query.png)

### more

Query from the application can refer to: https://thegraph.com/docs/zh/querying/querying-from-an-application/

# FAQ

## What is a subgraph?

A subgraph is a custom API built on blockchain data. Subgraphs are queried using the GraphQL query language and are deployed to a Graph node using the Graph CLI. Once deployed and published, subgraphs are available to be queried by subgraph consumers.

## Are there requirements for subgraph code？

- "@graphprotocol/graph-cli": "^0.20.1"
- "@graphprotocol/graph-ts": "^0.20.1"
- "npm": "^7.20.5"

For more code specifications, refer to https://github.com/okc-subgraph/subgraph-example/blob/main/docs/Developing.md

## Is it possible to deploy subgraphs with the same name？

No, duplicate subgraph names aren't allowed.

## Can I delete my subgraph?

Yes. You may contact the OKC support team to do that for you.

## Does the deployed subgraph support modifications or upgrades？

Yes. You may contact the OKC support team for further help.

## What are the restrictions when querying the subgraph?

There will be a query limit. If you encounter limited flow or no response to the query, please contact the OKC support team.

## Do I need to pay for querying the subgraph?

Free for now.
