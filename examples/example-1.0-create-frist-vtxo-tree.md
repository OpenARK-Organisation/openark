# Example 1.0

## Alice, Bob, Carol, Dave, and Eve create a VTXO tree

In this part of the example, the users join a new ARK round run by Steve.

```mermaid
sequenceDiagram
    autonumber
    participant Steve as Steve (ASP)
    participant Alice as Alice (User)
    participant Bob as Bob (User)
    participant Carol as Carol (User)
    participant Dave as Dave (User)
    participant Eve as Eve (User)

    Note over Steve,Eve: Invite state
    Steve->>Alice: new_round_initiate (round_id, params)
    Steve->>Bob: new_round_initiate (round_id, params)
    Steve->>Carol: new_round_initiate (round_id, params)
    Steve->>Dave: new_round_initiate (round_id, params)
    Steve->>Eve: new_round_initiate (round_id, params)

    Alice->>Steve: new_round_join (onboard/offboard/transition)
    Bob->>Steve: new_round_join (onboard/offboard/transition)
    Carol->>Steve: new_round_join (onboard/offboard/transition)
    Dave->>Steve: new_round_join (onboard/offboard/transition)
    Eve->>Steve: new_round_join (onboard/offboard/transition)

    Note over Steve,Eve: Confirm state 
    Steve->>Alice: new_round_vtxo_tree_proposal (vtxo-tree, forfeit-tree, recycle txs)
    Steve->>Bob: new_round_vtxo_tree_proposal (vtxo-tree, forfeit-tree, recycle txs)
    Steve->>Carol: new_round_vtxo_tree_proposal (vtxo-tree, forfeit-tree, recycle txs)
    Steve->>Dave: new_round_vtxo_tree_proposal (vtxo-tree, forfeit-tree, recycle txs)
    Steve->>Eve: new_round_vtxo_tree_proposal (vtxo-tree, forfeit-tree, recycle txs)

    Alice->>Steve: new_round_vtxo_tree_accept (signatures)
    Bob->>Steve: new_round_vtxo_tree_accept (signatures)
    Carol->>Steve: new_round_vtxo_tree_accept (signatures)
    Dave->>Steve: new_round_vtxo_tree_accept (signatures)
    Eve->>Steve: new_round_vtxo_tree_accept (signatures)

    Note over Steve,Eve: Fund state
    Steve->>Alice: new_round_prepare_start 
    Steve->>Bob: new_round_prepare_start 
    Steve->>Carol: new_round_prepare_start 
    Steve->>Dave: new_round_prepare_start 
    Steve->>Eve: new_round_prepare_start 

    Alice->>Steve: new_round_start_prepared (signatures: forfeit/recycle/onboarding)
    Bob->>Steve: new_round_start_prepared (signatures: forfeit/recycle/onboarding)
    Carol->>Steve: new_round_start_prepared (signatures: forfeit/recycle/onboarding)
    Dave->>Steve: new_round_start_prepared (signatures: forfeit/recycle/onboarding)
    Eve->>Steve: new_round_start_prepared (signatures: forfeit/recycle/onboarding)

    Note over Steve, Eve: Steve signs/broadcasts vtxo-root (on-chain) and announces round start
    Steve->>Alice: new_round_start 
    Steve->>Bob: new_round_start 
    Steve->>Carol: new_round_start 
    Steve->>Dave: new_round_start 
    Steve->>Eve: new_round_start 
```

1. Steve issues a `new_round_initiate`
    1. Informs everybody that a new round is starting.
2. Alice, Bob, Carol, Dave, and Eve reply with `new_round_join`
    1. Here the Users declare what to onboard, what to offboard, and what to transition.

![Tree Created](../exemple-1.0.1-tree-created.svg)

3. Steve issues a `new_round_vtxo_tree_proposal`
    1. Here Steve sends out the VTXO tree for signing.
4. Alice, Bob, Carol, Dave, and Eve issue a `new_round_vtxo_tree_accept`
    1. Here the Users return the signed nodes.

![Tree Signed](../exemple-1.0.2-tree-signed.svg)

5. Steve issues a `new_round_prepare_start`
    1. Here Steve sends out the new root for signature, binding the tree to the root.
6. Alice, Bob, Carol, Dave, and Eve issue a `new_round_start_prepared`.
    1. Here all Users accept the tree.

![Tree Prepared](../exemple-1.0.3-root-signed-and-deposited.svg)

7. Steve issues a `new_round_start`.
    1. The root is deposited and the round starts. This is the official start of the round.
