---
layout: rns
title: RIF Token
---

RIF Name Service (RNS) implements the RIF Directory Protocol (RDP) which recommends the usage of the RIF Token to stake at name auctions and also paying for name maintenance rent.

To interact with RNS, it's important to understand the nature of the RIF Token.

| Token Name | RIF |
| Total Supply | 1,000,000,000 RIF |
| Contract Testnet Address | [0xd8c5adcac8d465c5a2d0772b86788e014ddec516](http://explorer.testnet.rsk.co/address/0xd8c5adcac8d465c5a2d0772b86788e014ddec516) (see [RNS Testnet](/RNS-Testnet) section)|
| Contract Mainnet Address | [0x2acc95758f8b5f583470ba265eb685a8f45fc9d5](http://explorer.rsk.co/address/0x2acc95758f8b5f583470ba265eb685a8f45fc9d5) |
| Contract Type | ERC677 |

## ERC677 token standard

An ERC20 token transaction between a regular/non-contract address and contract are two different transactions: You should call approve on the token contract and then call transferFrom on the other contract when you want to deposit your tokens into it.

ERC677 simplifies this requirement and allows using the same transfer function. ERC677 tokens can be sent by calling transfer function on the token contract with no difference if the receiver is a contract or a wallet address, since there is a new way to notify the receive contract of the transfer.

An ERC677 token transfer will be the same as an ERC20 transfer. On the other hand, if the receiver is a contract, then the ERC677 token contract will try to call `tokenFallback` function on receiver contract. If there is no `tokenFallback` function on receiver contract, the transaction will fail.

## RIF transfer methods

- Approve and transfer:
    ```js
    function approve(address _spender, uint256 _value) public returns (bool)
    function transfer(address _to, uint256 _value) public returns (bool)
    ```

- Transfer and call:
    ```js
    function transfer(address _to, uint256 _value, bytes data)
    ```

    **Parameters**
    - `_to: address`: Contract address.
    - `_value: uint256`: Amount of RIF tokens to send.
    - `data: bytes`: 4-byte signature of the function to be executed, followed by the function parameters to be executed with encoded as a byte array.

As explained in the [Registrar](/Architecture/Registrar) section, both **bid submission** and **rent payment** can be executed through the ERC677 transfer methods.
