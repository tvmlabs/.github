## Quick Start

Learn playground, documentation, make your first request and integrate

Let's start with observing API in sandbox playground.
Learn how to read API documentation.
Then move to making an api request with curl.
And integrate it with [tvm-sdk](https://github.com/tvmlabs/tvm-sdk-js).

### Playground
Go to GraphQL playground https://ackinacki-testnet.tvmlabs.dev/graphql
​

Insert this query in the left part. 

```graphql
query{
blockchain{
    account(address:"-1:3333333333333333333333333333333333333333333333333333333333333333"){
      info{
        balance(format:DEC)
        address
      }
    }
  }
}
```

Now click play button and you will see the result:

![image](https://github.com/tvmlabs/.github/assets/39991771/f9a62664-4793-49cf-9f8c-6f95ee4f7b31)

### Documentation

Click on the button "schema" on the right. You will see the API documentation with all available fields. Try to make your own query now.

<img width="1553" alt="image" src="https://github.com/tvmlabs/.github/assets/39991771/918d1b26-74c5-4ec4-8ad3-2a3015a221f6">

### Request with curl

```curl
curl --location --request POST 'https://ackinacki-testnet.tvmlabs.dev/graphql' \
--header 'Content-Type: application/json' \
--data-raw '{"query":"query($address: String!){\n  blockchain{\n    account(address:$address){\n      info{\n        balance(format:DEC)\n      }\n    }\n  }\n}","variables":{"address":"0:3333333333333333333333333333333333333333333333333333333333333333"}}'
```

### Request with SDK (JavaScript)

```js
const {TonClient} = require("@tvmsdk/core");
const {libNode} = require("@tvmsdk/lib-node");

TonClient.useBinaryLibrary(libNode)

const client = new TonClient({
    network: {
        endpoints: [
            "https://ackinacki-testnet.tvmlabs.dev/graphql"
        ],
    },
});

(async () => {
    try {
        // Get account balance. 
        const query = `
            query {
              blockchain {
                account(
                  address: "${address}"
                ) {
                   info {
                    balance(format: DEC)
                  }
                }
              }
            }`
        const {result}  = await client.net.query({query})
        console.log(`The account balance is ${result.data.blockchain.account.info.balance}`);
        client.close();
    }
    catch (error) {
        console.error(error);
    }
}
)()
```


