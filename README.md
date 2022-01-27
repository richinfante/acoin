# acoin

acoin is a cryptography-based currency in which balances are not stored on a blockchain. Instead, there exists a decentralized network of validator nodes, which store the current owner(s) of coins. Transactions can exist of three types:
- Transfer
- Merge
- Split

Suppose a user named Alice wanted to send Bob *5 for coffee. She owns a single coin, which has a value of *39. She then takes Bob's public key, and mints the following transaction:

- SPLIT
  - ATTESTATION:
    - Previous attestation signature for the coin, which is stored in validator nodes and is locked with Alice's private key.
    - Alice signs the attestation with the transaction to prove she is sending it.
  - DESTINATIONS:
  - ALICE
    - Coin ID: <Random1>
    - Value: *34.00
  - BOB
    - Coin ID: <Random2>
    - Value: *5.00
  
She sends the attestation to validator nodes, and to bob. Bob asks a random set S_1 of the validator nodes for confirmation that he now owns the <Random2> coin.

Bob can pick S_1 to validate from, to ensure it is a sufficient proportion of nodes. In doing so, he is also propogating his transaction randomly across the network.
  
Alice can do the same for her new coin <Random1>.

Once both parties are satisfied that the coin has not been double-spent, Alice can give Bob their coffee. Since we do not rely on proof-of-work, transactions can easily take as short as 30 seconds to propogate across the network since they consist of a small cryptographic proof.
  
## Validator Attacks

A malicous node can attempt do one of a few things to attempt to attack the network:
- Selective withholding of transactions from the rest of the network
- Selective removal of transactions from memory
  
However, these things are expected in normal operation of the network. When probed for a coin, a validator is expected to return it's attestation. If it forges one, the transaction, it simply won't validate to a client.
  
If a transaction is removed from memory, a client will simply re-send it. This is because in normal operation and onboarding of new nodes, historical coin attestations slowly propogate, so a node may not have all the information needed. However, if it _does_ have a coin attestation for an ID, it can be considered a validator for that coin.

## Client attacks

- Double spending: Alice can try to mint two coins from the same source, one to Alice, and one to Eve, while presenting the coin to Bob. This presents a race condition. Transactions are deterministic, and are based on their transaction ID. The lowest transaction, when in conflict, succeeds. When bob asks about his attestation, He'll recieve an ECONFLICT error, which contains his failed attestation as well as Eve's, which succeeded. He can then decide to not conduct business with alice in the future, and not to tender whichever goods were promised to Eve.
  
## Proof of Destruction
- Potentially implement cryptographic minting from other chanins using proof of destruction.
