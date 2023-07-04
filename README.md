# ENS Executable Functions Wallet Guide

These are interactions the ENS Manager dApp suggests might suggest the user execute.
The aim of this document is to aggregate functions in order to provide transparency in user frontends.

Other handy links:

-   [ENS Docs](https://docs.ens.domains/)
-   [ENS Alpha Docs](https://alpha-docs.ens.domains/)
-   [EIP-137 Ethereum Domain Name Service Specification](https://eips.ethereum.org/EIPS/eip-137)

## Resolvers

Any smart-contract could be a resolver. A Resolver contract is responsible for retrieving the data associated with a name.
The ENS Registry contract stores the address of the resolver contract responsible for a name.

There is however one special resolver, the "public resolver". This resolver is a generalized key-value storage resolver that anyone can use.
It is the default resolver for all names, unless changed by the user through [setting resolver](#setting-resolver).

| Contract          | Address                                                                                                                                                            |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Public Resolver   | [resolver.eth](https://ens.app/resolver.eth) [0x231b0Ee14048e9dCcD1d247744d114a4EB5E8E63](https://etherscan.io/address/0x231b0Ee14048e9dCcD1d247744d114a4EB5E8E63) |
| Public Resolver 2 | [0x4976fb03C32e5B8cfe2b6cCB31c09Ba78EBaBa41](https://etherscan.io/address/0x4976fb03C32e5B8cfe2b6cCB31c09Ba78EBaBa41)                                              |

## ETH Registrar

ETHRegistrarController / ETHRegistrar / Old ETHRegistrar Controller

| Contract                     | Address                                                                                                               |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| ETH Registrar Controller     | [0x253553366da8546fc250f225fe3d25d0c782303b](https://etherscan.io/address/0x253553366da8546fc250f225fe3d25d0c782303b) |
| Old ETH Registrar Controller | [0x283af0b28c62c092c9727f1ee09c02ca627eb7f5](https://etherscan.io/address/0x283af0b28c62c092c9727f1ee09c02ca627eb7f5) |

-   [commit](#commit)
-   [register](#register)
-   [registerWithConfig](#register-with-config)
-   [renew](#renew)

### Commit

| Function                     | Signature  | Implementation                                                                                                                                                  | Example Transaction                                                                                                                       |
| ---------------------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `commit(commitment bytes32)` | 0xf14fcbc8 | [Source Code](https://github.com/ensdomains/ens-contracts/blob/787c5d8f1a99ad14435a65784a7c5ceca1e2575e/contracts/ethregistrar/ETHRegistrarController.sol#L141) | [Example Transaction](https://etherscan.io/tx/0xea772f0f05543cc90e25a19997c0430d82e85331d45e2264603bc3cd2bbff434) registering `meowy.eth` |

> This function is used to commit to a name before registering it. The bytes32 is an opaque hash of the name so the user can't be frontrun. This transaction is generally followed by [registerWithConfig](#register-with-config).

#### Particulars

Generally followed up by [registerWithConfig](#registerwithconfig).

#### To reproduce

Open ENS Manager dApp, type in a name you would like to register, select "Ethereum", and press "Next", "Begin", "Start Timer", "Open Wallet".

### Register

| Function                                                                 | Signature  | Implementation | Example Transaction |
| ------------------------------------------------------------------------ | ---------- | -------------- | ------------------- |
| `register(name string, owner address, duration uint256, secret bytes32)` | 0x85f6d155 |                |                     |

> This function is used to register a name. In contrary to [commit](#commitcommitment-bytes32) this function takes the name as a string and the secret as a bytes32. This function under the hood runs [registerWithConfig](#register-with-config)`(name, owner, duration, secret, address(0), address(0))`.

This transaction has a **payable amount**.

### Register with Config

| Function                                                                                                           | Signature  | Implementation | Example Transaction                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------ | ---------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `registerWithConfig(name string, owner address, duration uint256, secret bytes32, resolver address, addr address)` | 0xf7a16963 |                | [Example Transaction](https://etherscan.io/tx/0x07528c73fe837a339e2f48188278e3f2fadabb8e56c7766ddadd69f3a009b0f5) registering `meowy.eth` |

This function is used to register a name. In contrary to [commit](#commitcommitment-bytes32) this function takes the name as a string and the secret as a bytes32.
This function requires that the user [commit](#commitcommitment-bytes32)ed to this name beforehand.

This transaction has a **payable amount**.

#### Particulars

The [commit](#commitcommitment-bytes32) transaction that preceded it can be found [here](https://etherscan.io/tx/0xea772f0f05543cc90e25a19997c0430d82e85331d45e2264603bc3cd2bbff434).

#### To reproduce

Open ENS Manager dApp, type in a name you would like to register, select "Ethereum", and press "Next", "Begin", "Start Timer", "Open Wallet", execute the [commit](#commitcommitment-bytes32) transaction, and wait 60 seconds. Now press "Open Wallet" again and execute the [registerWithConfig](#register-with-config) transaction.

### Renew

| Function                               | Signature  | Implementation | Example Transaction                                                                                                                                                            |
| -------------------------------------- | ---------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `renew(name string, duration uint256)` | 0xacf1a841 |                | [Example Transaction](https://etherscan.io/tx/0xd17a36d5c2ffd629ef63f443144f389b3600ec8560f5e016b2a56adf87d6eeac) renewing `helgesson.eth` for `31536000` **seconds** (1 year) |

This function is used to renew a name.

This transaction has a **payable amount**.

#### To reproduce

Open ENS Manager dApp, navigate to any name, select "Extend", press "Next", "Open Wallet".

## ReverseRegistrar

| Contract          | Address                                                                                                                                                                  |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Reverse Registrar | [reverse.ens.eth](https://ens.app/reverse.ens.eth) [0xa58E81fe9b61B5c3fE2AFD33CF304c454AbFc7Cb](https://etherscan.io/address/0xa58E81fe9b61B5c3fE2AFD33CF304c454AbFc7Cb) |
| Old               | [0x9062C0A6Dbd6108336BcBe4593a3D1cE05512069](https://etherscan.io/address/0x9062C0A6Dbd6108336BcBe4593a3D1cE05512069)                                                    |
| Old 2             | [0x084b1c3C81545d370f3634392De611CaaBFf8148](https://etherscan.io/address/0x084b1c3C81545d370f3634392De611CaaBFf8148)                                                    |

### Set Name

| Function               | Signature | Implementation | Example Transaction                                                                                               |
| ---------------------- | --------- | -------------- | ----------------------------------------------------------------------------------------------------------------- |
| `setName(name string)` |           |                | [Example Transaction](https://etherscan.io/tx/0x905e106763556d0cb16d1fc11ab13d75cf5b1227e0480098b909bba50c4271b8) |

#### To reproduce

Open ENS Manager dApp, navigate to [/my/settings](https://ens.app/my/settings), select "Change", the name you would like to change it to, "Next", "Start", "Open Wallet".

## Registry

## NameWrapper

The NameWrapper is a contract users can use to "wrap" their name in order to provide it with additional functionality.
This functionality includes creating trustless namewrapper subnames (a seperate ERC721 token).

## Bulk Renewal

| Contract          | Address                                                                                                               |
| ----------------- | --------------------------------------------------------------------------------------------------------------------- |
| Bulk Renewal      | [0xff252725f6122a92551a5fa9a6b6bf10eb0be035](https://etherscan.io/address/0xff252725f6122a92551a5fa9a6b6bf10eb0be035) |
| StaticBulkRenewal | [0xa12159e5131b1eEf6B4857EEE3e1954744b5033A](https://etherscan.io/address/0xa12159e5131b1eEf6B4857EEE3e1954744b5033A) |

### Renew All

| Function                                   | Signature  | Implementation | Example Transaction |
| ------------------------------------------ | ---------- | -------------- | ------------------- |
| renewAll(names string[], duration uint256) | 0xe8d6dbb4 |                |                     |

#### To reproduce

Open ENS Manager dApp, navigate to [/my/names](https://ens.app/my/names), click the Circled Checkmark to enable Multi Selection, select multiple names, press "Extend", "Next", "Next", "Open Wallet"

## DNSRegistrar

### Prove and Claim

| Function                                                       | Signature | Implementation                                                                                                                                       | Example Transaction |
| -------------------------------------------------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `proveAndClaim(name bytes, input DNSSEC.RRSetWithSignature[])` |           | [Source Code](https://github.com/ensdomains/ens-contracts/blob/787c5d8f1a99ad14435a65784a7c5ceca1e2575e/contracts/dnsregistrar/DNSRegistrar.sol#L90) |                     |

### Prove and Claim with Resolver

| Function                                                                                                   | Signature | Implementation                                                                                                                                                    | Example Transaction |
| ---------------------------------------------------------------------------------------------------------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `proveAndClaimWithResolver(name bytes, input DNSSEC.RRSetWithSignature[], resolver address, addr address)` |           | [Contract Implementation](https://github.com/ensdomains/ens-contracts/blob/787c5d8f1a99ad14435a65784a7c5ceca1e2575e/contracts/dnsregistrar/DNSRegistrar.sol#L101) |                     |

Basically what [registerWithConfig](#register-with-config) is to [register](#register), [proveAndClaimWithResolver](#proveandclaimwithresolvername-bytes-input-dnssecrrsetwithsignature-resolver-address-addr-address) is to [proveAndClaim](#proveandclaimname-bytes-input-dnssecrrsetwithsignature).

## Other

ENS Registry With Fallback
[0x00000000000C2E074eC69A0dFb2997BA6C7d2e1e](https://etherscan.io/address/0x00000000000c2e074ec69a0dfb2997ba6c7d2e1e)
