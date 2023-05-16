## SmartWeave Contract Interoperability 

This guide is intended to outline the various known proposals to achieve interoperability for SmartWeave contracts.

### Known Methods

#### Foreign Call Protocol v1 (FCPv1)

FCPv1 was the first proposal to solve for interoperability within the SmartWeave environment. It worked by placing a message in a specific array within a contract state, called an `outbox`, that would then be read by another contract attempting to receive the message. In order to do this, the second contract would need to evaluate up to the latest state of the desired contract to determine what was in the outbox, and then it would evaluate by recursively calling its own `handle` function with the outbox message used as an input and the corresponding contract as the caller.

**Known implementations and examples**
- [Original POC gist](https://gist.github.com/t8/e5485597c479b5c949aee580b7feeab2)
- [Revised proposal draft](https://www.notion.so/Foreign-Call-Protocol-Specification-61e221e5118a40b980fcaade35a2a718)
- [Implementation within Verto's CLOB Contract](https://github.com/useverto/contracts/tree/main/src/clob)

#### Foreign Call Protocol v2 (FCPv2)

FCPv2 leverages the same theory used to pass messages from one contract to another as FCPv1, but it improved the functionality and developer experience of v1 by utilizing Warp Contracts' `internalWrites` feature. This way a contract no longer needed to manually evaluate the state of another contract to read the outbox; instead, Warp's execution environment handles this automatically when querying for contract interactions to evaluate.

**Known implementations and examples**
- [Proposal](https://specs.g8way.io/?tx=iXHbTuV7kUR6hQGwNjdnYFxxp5HBIG1b3YI2yy7ws_M)
- [Implementation within AFTR Market Contracts](https://github.com/aftrmarket/aftr-contracts)
- [Implementation within Verto Flex](https://github.com/useverto/flex)
- Implementation within FACTS Protocol?
- Implementation within NOW/atomic tokens?

#### Hashed Timelock Contract (HTLC)

HTLC requires the use of temporary locks that are engaged by each participant. A lock can be described as anything that guarantees the validity of an agreed upon action for a specific time frame. The responsibility of evaluating the contract state in order to know if a lock is valid, including its timeout, can be delegated to a trusted third party or client side implementation.

**Known implementations and examples**
- [Proposal](https://arweave.app/tx/LO3owpsedIst_QHDsR_XtlMoBaWWRVtYRLFXYrU-jnA)
- [Proposal for atomic assets](https://arweave.net/oIrgzZt3UBW9ct_WzIVTxO6XO1w1AJKJEsrncJJ2IAI)
- [Implementation within RareWeave](https://rareweave.store/)

### Compare / Contrast

| Name                                                          | **Foreign Call Protocol v1 (FCPv1)** | **Foreign Call Protocol v2 (FCPv2)** | **Hashed Timelock Contract (HTLC)** |
|---------------------------------------------------------------|--------------------------------------|--------------------------------------|-------------------------------------|
| Require state replay of corresponding contract                |                   ✅                  |                   ✅                  |                                     |
| Require trust to be placed on a counterparty                  |                                      |                                      |                  ✅?                 |
| Dependant on/integrated within smart contract environment     |                   ✅                  |                   ✅                  |                                     |
| Performance based on execution speed and transaction fetching |                   ✅                  |                   ✅                  |                                     |
| Performance based on timelock                                 |                                      |                                      |                  ✅                  |
| Avoids data availability constraints                          |                                      |                                      |                  ✅                  |
| Known implementations                                         |                   1                  |                  >5                  |                  1                  |

