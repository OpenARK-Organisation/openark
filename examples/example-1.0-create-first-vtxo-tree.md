# Example 1.0

## Alice, Bob, Carol, Dave, and Eve Create a VTXO Tree

In this part of the example, the users join a new ARK round run by Steve.

### Actors in this scenario

- Alice (User)
- Bob (User, service-role in this scenario)
- Carol (User)
- Dave (User)
- Eve (User, service-role in this scenario)
- Steve (ASP)

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

    Note over Steve,Eve: Steve signs and broadcasts the VTXO root (on-chain), then announces round start
    Steve->>Alice: new_round_start
    Steve->>Bob: new_round_start
    Steve->>Carol: new_round_start
    Steve->>Dave: new_round_start
    Steve->>Eve: new_round_start
```

1. Steve issues `new_round_initiate`.
- This informs everyone that a new round is starting.

2. Alice, Bob, Carol, Dave, and Eve reply with `new_round_join`.
- Here, users declare what to onboard, offboard, and transition.

![Tree Created](../exemple-1.0.1-tree-created.svg)

3. Steve issues `new_round_vtxo_tree_proposal`.
- Steve sends the VTXO tree, forfeit tree, and recycle transactions for review and signing.

4. Alice, Bob, Carol, Dave, and Eve issue `new_round_vtxo_tree_accept`.
- Users return signatures for the proposed tree elements.

![Tree Signed](../exemple-1.0.2-tree-signed.svg)

5. Steve issues `new_round_prepare_start`.
- Steve sends the VTXO root and related data that must be signed before round start.

6. Alice, Bob, Carol, Dave, and Eve issue `new_round_start_prepared`.
- Users return the required preparation signatures (forfeit/recycle/onboarding) tied to the proposed root.

![Tree Prepared](../exemple-1.0.3-root-signed-and-deposited.svg)

7. Steve issues `new_round_start`.
- The root transaction is deposited on-chain and the round officially starts.
