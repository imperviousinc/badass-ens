# badass-ens

How we created [badass.domains](https://badass.domains)

# How we set up ENS for decentralized subdomains on our [Handshake](https://handshake.org) TLD .badass

## Why do this?

ENS uses the orphan .eth TLD which is not anchored in any root zone.  The .eth TLD is the ISO for Ethiopia, so it probably won't be included in the ICANN root zone.  It is blacklisted on Handshake, so it could be used on Handshake with a plugin.  Other "decentralized" domains like Unstoppable Domains (.crypto or .zil) and Namecoin (.bit) also use orphan TLDs that are not anchored in any root zone.  If they decide and are able to get the TLD in the ICANN root zone, it is centralized and maganed by ICANN.

By forking ENS and using it on a TLD that is anchored in a decentralized root zone (Handshake), we create truly decentralized subdomains.

## Set up

1. Deploy ENS contracts to Ethereum

The [badass repo](https://github.com/imperviousinc/badass) contains the fork of ENS we deployed for .badass.  You can then [verify the contract source code on etherscan](https://etherscan.io/address/0x36fc69f0983E536D1787cC83f481581f22CCA2A1#code).  Once the code is verified, you can interact with the contracts directly on etherscan, and test, for example, registering a subdomain.

2. Deploy Subgraphs

We forked the [ENS subgraph repo](https://github.com/ensdomains/ens-subgraph) and changed the contract addresses, for example like [this line](https://github.com/ensdomains/ens-subgraph/blob/add243cb35cbca26b4f91a835642aa2db6bc05ce/subgraph.yaml#L13).

3. Launch custom ENS app

We [forked the ens-app](https://github.com/imperviousinc/ens-app) and modified it to work with our deployed contracts from step #1 above.  This allows for an easier to use web UI for registering and managing .badass subdomains.

4. [hsd](https://github.com/handshake-org/hsd) plugin to resolve domains

In order to resolve .badass domains, you will need to run `hsd` with a plugin that resolves ENS using [HIP-0005](https://github.com/handshake-org/HIPs/pull/10).  We have created [handover](https://github.com/imperviousinc/handover) for this.

5. Remove trust

The  .eth version of ENS has [keyholders](https://www.reddit.com/r/ethereum/comments/5z3agy/ens_root_keyholders_announced/), so it is not truly decentralized.  In order to make .badass truly badass (and provably decentralized) we have set the owner of the ENS contract to a burn address 0x0. We used the following geth command for this:

```
ENSInstance.setOwner("0x0000000000000000000000000000000000000000","0x0000000000000000000000000000000000000000", {from: eth.accounts[0], gasPrice: 125000000000})
```

We have also done the same with the .badass TLD on Handshake, making it unable to be updated, but renewable by anyone, using the following:

```
OPTYPE
0x08 // RENEW
OPEQUALVERIFY
```

You can see an [implementation of this here](https://github.com/handshake-org/hsd/pull/567).

## Notes

Our app is the first implementation of [EIP-1185](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1185.md) (DNS on ENS)

## Contributors

HUGE thanks to [Kiba Gateaux](https://github.com/kibagateaux) and [Matt Zipkin](https://github.com/pinheadmz) for their help!
Also thanks to all of those who have contributed to open source code for ENS and Handshake!
