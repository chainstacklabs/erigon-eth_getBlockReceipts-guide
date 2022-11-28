# Erigon | eth_getBlockReceipts guide
Erigon is becoming a more common Ethereum client, and this repo describes the ```eth_getBlockReceipts``` method which is available by querying a node running Erigon as a client.

For an in-depth comparison and inner working of **Geth** and **Erigon**, see: [Ethereum clients—Geth and Erigon](https://chainstack.com/ethereum-clients-geth-and-erigon/).

Calling ```eth_getBlockReceipts``` will return the receipts of all the transactions in a specified block displaying the transactions details. This method can be useful when you want to retrieve information about all the transactions in a block in one go— instead of calling multiple different methods.

You can call this RPC method using ```cURL```; ```web3.py``` or ```web3.js``` do not support it at the moment.

## cURL

cURL is a command-line tool used send and receive data.

Most operating systems have cURL available by default. Run this code in a command prompt using admin privileges to verify if it is installed on your system.

``` sh
curl -h
```

### cURL POST request syntax

In the JSON RPC methods case, we are working with a JSON body, and the general form of a cURL command for making a POST request with a JSON body is as follows:

```sh
curl -X POST 'URL'
     -H "Content-Type: application/json" 
     --data "{JSON data}" 
```
Where:
  * -X: HTTP method to use when communicating with the server.
  * -H: HTTP headers to send to the server with a POST request.
  * --data: Data to be sent to the server using a POST request.

### Use Postman to run cURL requests

> **Note** 
> You need access to an endpoint from a node running Erigon for this specific example.

[Postman](https://www.postman.com/) is a platform used by developers to test APIs. It has a nice graphical interface, and makes cURL requests easy to test.

1. Click on the "Import" button on the left side of the workspace.
1. Select "Raw text" and paste one of the code examples.
1. Replace ```'ERIGON_NODE_URL'``` with your own HTTP endpoint URL.
1. Click on "Continue" and then on "Import".
1. Now you have the code imported in the workplace. Click the "Send" button to subtmit the request and receive a response.

> After the cURL code has been imported for the first time, you can go in the "Body" tab and modify the data to send different requests without having to import the entire code again.

## Code example

### Parameters required

The block that you want to query to retrieve the transaction details. 

`quantity or tag` - Integer block number, or the string ```'latest'```, ```'earliest'``` or ```'pending'```. See the [default block parameter](https://eth.wiki/json-rpc/API#the-default-block-parameter). 

``` sh
curl -X POST 'ERIGON_NODE_URL' \
-H 'Content-Type: application/json' \
--data '{"method":"eth_getBlockReceipts","params":["14985726"], "jsonrpc":"2.0","id":1}'
```

Or

``` sh
curl -X POST 'ERIGON_NODE_URL' \
-H 'Content-Type: application/json' \
--data '{"method":"eth_getBlockReceipts","params":["latest"], "jsonrpc":"2.0","id":1}'
```

### Returns
Returns an object with the receipts (details) of all the transactions in that block. 

```Object``` - An object with the following fields:

- ```ID(integer number)``` - json-rpc ID.

- ```jsonrpc(string)``` - json-rpc version.

- ```result(object)``` - an object with the following fields:

  - ```nextPageIndex(integer number)``` - next page (if exists, else blank).

  - ```data(array of objects, defined below)``` - sorted in ascending order by block number.

```Transaction receipt``` - An object with the following fields::

- ```blockHash(hex string)``` - block hash.

- ```blockNumber(hex string)``` - block number.

- ```contractAddress(hex string)``` - contract address.

- ```cumulativeGasUsed(hex string)``` - cumulative gas used.

- ```effectiveGasPrice(hex string)``` - effective gas price.

- ```from(hex string)``` - from.

- ```gasUsed(hex string)``` - gas used.

- ```logs(array of log info object)``` - logs array.

- ```logsBloom(hex string)``` - logs bloom.

- ```root(hex string)``` - root.

- ```status(hex string)``` - status.

- ```to(hex string)``` - to address.

- ```transactionHash(hex string)``` - transaction hash.

- ```transactionIndex(hex string)``` - transaction index.

The request will return a result like the one below. 

> Usually, there are many transactions in a block, the block number ```14985726``` I use in the example holds 40 transactions, and I just placed a few of them here to see how the result will look. 

```sh
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": [
        {
            "blockHash": "0xf0d77748e875fc8a704e35323f65b5e3f5fea87909a40e785e63ea2fdaa57927",
            "blockNumber": "0xe4a9fe",
            "contractAddress": null,
            "cumulativeGasUsed": "0xb8b0",
            "effectiveGasPrice": "0xe87547000",
            "from": "0x0031e147a79c45f24319dc02ca860cb6142fcba1",
            "gasUsed": "0xb8b0",
            "logs": [
                {
                    "address": "0xdac17f958d2ee523a2206206994597c13d831ec7",
                    "topics": [
                        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                        "0x000000000000000000000000e6445715c82158f5271fbe2a69b143614729cbb2",
                        "0x000000000000000000000000ffec0067f5a79cff07527f63d83dd5462ccf8ba4"
                    ],
                    "data": "0x000000000000000000000000000000000000000000000000000000000cc8b480",
                    "blockNumber": "0xe4a9fe",
                    "transactionHash": "0x5d2378f4a5ad9f5d102032b5ac3d5f460193610bbb870ae466f31e834120f682",
                    "transactionIndex": "0x0",
                    "blockHash": "0xf0d77748e875fc8a704e35323f65b5e3f5fea87909a40e785e63ea2fdaa57927",
                    "logIndex": "0x0",
                    "removed": false
                }
            ],
            "logsBloom": "0x00000000000000000000000008000000000000000000000000000000000000004000000000000000000000000000010000000000000000000000000000400000000000000001000000000008000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000010080000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020000",
            "status": "0x1",
            "to": "0xe6445715c82158f5271fbe2a69b143614729cbb2",
            "transactionHash": "0x5d2378f4a5ad9f5d102032b5ac3d5f460193610bbb870ae466f31e834120f682",
            "transactionIndex": "0x0",
            "type": "0x0"
        },
        {
            "blockHash": "0xf0d77748e875fc8a704e35323f65b5e3f5fea87909a40e785e63ea2fdaa57927",
            "blockNumber": "0xe4a9fe",
            "contractAddress": null,
            "cumulativeGasUsed": "0x17611",
            "effectiveGasPrice": "0x9285ac371",
            "from": "0xeca2e2d894d19778939bd4dfc34d2a3c45e96456",
            "gasUsed": "0xbd61",
            "logs": [],
            "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
            "status": "0x0",
            "to": "0x4cb18386e5d1f34dc6eea834bf3534a970a3f8e7",
            "transactionHash": "0x23fc7e85dde0861f51fc77ab5dbb6c498088498fde211a15f7543f053d7641e5",
            "transactionIndex": "0x1",
            "type": "0x2"
        },
        {
            "blockHash": "0xf0d77748e875fc8a704e35323f65b5e3f5fea87909a40e785e63ea2fdaa57927",
            "blockNumber": "0xe4a9fe",
            "contractAddress": null,
            "cumulativeGasUsed": "0x374d2",
            "effectiveGasPrice": "0x891737000",
            "from": "0xbfdabd4f1bf6f7d7220906d6f858a5e222597e12",
            "gasUsed": "0x1fec1",
            "logs": [
                {
                    "address": "0x4e4a47cac6a28a62dcc20990ed2cda9bc659469f",
                    "topics": [
                        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                        "0x00000000000000000000000093c212b82c41dc99ba8ff5b21e03946da567ae6f",
                        "0x000000000000000000000000bfdabd4f1bf6f7d7220906d6f858a5e222597e12"
                    ],
                    "data": "0x000000000000000000000000000000000000000006eaa0efdb9b7b4391f4d83d",
                    "blockNumber": "0xe4a9fe",
                    "transactionHash": "0x3e2b5584d629393eb9130d363e63e8809025d9fc36658830e1bcdd7b1f4160fe",
                    "transactionIndex": "0x2",
                    "blockHash": "0xf0d77748e875fc8a704e35323f65b5e3f5fea87909a40e785e63ea2fdaa57927",
                    "logIndex": "0x1",
                    "removed": false
                },
                {
                    "address": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
                    "topics": [
                        "0xe1fffcc4923d04b559f4d29a8bfc6cda04eb5b0d3c460751c2402c5c5cc9109c",
                        "0x00000000000000000000000068b3465833fb72a70ecdf485e0e4c7bd8665fc45"
                    ],
                    "data": "0x00000000000000000000000000000000000000000000000010a741a462780000",
                    "blockNumber": "0xe4a9fe",
                    "transactionHash": "0x3e2b5584d629393eb9130d363e63e8809025d9fc36658830e1bcdd7b1f4160fe",
                    "transactionIndex": "0x2",
                    "blockHash": "0xf0d77748e875fc8a704e35323f65b5e3f5fea87909a40e785e63ea2fdaa57927",
                    "logIndex": "0x2",
                    "removed": false
                },
                {
                    "address": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
                    "topics": [
                        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                        "0x00000000000000000000000068b3465833fb72a70ecdf485e0e4c7bd8665fc45",
                        "0x00000000000000000000000093c212b82c41dc99ba8ff5b21e03946da567ae6f"
                    ],
                    "data": "0x00000000000000000000000000000000000000000000000010a741a462780000",
                    "blockNumber": "0xe4a9fe",
                    "transactionHash": "0x3e2b5584d629393eb9130d363e63e8809025d9fc36658830e1bcdd7b1f4160fe",
                    "transactionIndex": "0x2",
                    "blockHash": "0xf0d77748e875fc8a704e35323f65b5e3f5fea87909a40e785e63ea2fdaa57927",
                    "logIndex": "0x3",
                    "removed": false
                },
                {
                    "address": "0x93c212b82c41dc99ba8ff5b21e03946da567ae6f",
                    "topics": [
                        "0xc42079f94a6350d7e6235f29174924f928cc2ac818eb64fed8004e115fbcca67",
                        "0x00000000000000000000000068b3465833fb72a70ecdf485e0e4c7bd8665fc45",
                        "0x000000000000000000000000bfdabd4f1bf6f7d7220906d6f858a5e222597e12"
                    ],
                    "data": "0xfffffffffffffffffffffffffffffffffffffffff9155f10246484bc6e0b27c300000000000000000000000000000000000000000000000010a741a462780000000000000000000000000000000000000000000000018b5826c0cedd442dfadc000000000000000000000000000000000000000000519d9cde069bdad938552bfffffffffffffffffffffffffffffffffffffffffffffffffffffffffffcbf79",
                    "blockNumber": "0xe4a9fe",
                    "transactionHash": "0x3e2b5584d629393eb9130d363e63e8809025d9fc36658830e1bcdd7b1f4160fe",
                    "transactionIndex": "0x2",
                    "blockHash": "0xf0d77748e875fc8a704e35323f65b5e3f5fea87909a40e785e63ea2fdaa57927",
                    "logIndex": "0x4",
                    "removed": false
                }
            ],
            "logsBloom": "0x00000000000000000000000000000000001000000000000002000000000000000000000000000000000080000000000002000000180020000000000000000000000000000000000800000008000000000000000000000000000000008000000000000000000000000400000000000000000000000000000000004010000800000000000000000002000800000000000000000001000000000000000000000000000000000000000000800002000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000100000000000000200000001000000000000000080000000000000000400000004000000000",
            "status": "0x1",
            "to": "0x68b3465833fb72a70ecdf485e0e4c7bd8665fc45",
            "transactionHash": "0x3e2b5584d629393eb9130d363e63e8809025d9fc36658830e1bcdd7b1f4160fe",
            "transactionIndex": "0x2",
            "type": "0x0"
        },
```

As you can see, this request will return multiple transactions receipt, in fact, the receipts for all the transactions in the block. This is possible querying a node running **Geth** as well, but you would need to use mulitple RPC methods and make a script using ```web3.py``` or ```web3.js```.
