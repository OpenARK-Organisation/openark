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
    participant S as Steve (ASP)
    participant R as Nostr Relay
    participant A as Alice (User)
    participant B as Bob (User)
    participant C as Carol (User)
    participant D as Dave (User)
    participant E as Eve (User)

    Note over S,R: Invite phase
    S->>R: new_round_initiate (kind:150, t=new_round_initiate, round_id, params)
    R-->>A: deliver event
    R-->>B: deliver event
    R-->>C: deliver event
    R-->>D: deliver event
    R-->>E: deliver event

    A->>R: new_round_join (kind:150, t=new_round_join, intents)
    B->>R: new_round_join (kind:150, t=new_round_join, intents)
    C->>R: new_round_join (kind:150, t=new_round_join, intents)
    D->>R: new_round_join (kind:150, t=new_round_join, intents)
    E->>R: new_round_join (kind:150, t=new_round_join, intents)
    R-->>S: deliver join events

    Note over S,R: Confirm phase
    S->>R: new_round_vtxo_tree_proposal (t=new_round_vtxo_tree_proposal)
    R-->>A: proposal
    R-->>B: proposal
    R-->>C: proposal
    R-->>D: proposal
    R-->>E: proposal

    A->>R: new_round_vtxo_tree_accept (t=new_round_vtxo_tree_accept, sigs)
    B->>R: new_round_vtxo_tree_accept (t=new_round_vtxo_tree_accept, sigs)
    C->>R: new_round_vtxo_tree_accept (t=new_round_vtxo_tree_accept, sigs)
    D->>R: new_round_vtxo_tree_accept (t=new_round_vtxo_tree_accept, sigs)
    E->>R: new_round_vtxo_tree_accept (t=new_round_vtxo_tree_accept, sigs)
    R-->>S: deliver accept events

    Note over S,R: Fund phase
    S->>R: new_round_prepare_start (t=new_round_prepare_start)
    R-->>A: prepare
    R-->>B: prepare
    R-->>C: prepare
    R-->>D: prepare
    R-->>E: prepare

    A->>R: new_round_start_prepared (t=new_round_start_prepared, sigs)
    B->>R: new_round_start_prepared (t=new_round_start_prepared, sigs)
    C->>R: new_round_start_prepared (t=new_round_start_prepared, sigs)
    D->>R: new_round_start_prepared (t=new_round_start_prepared, sigs)
    E->>R: new_round_start_prepared (t=new_round_start_prepared, sigs)
    R-->>S: deliver prepared events

    Note over S: S signs and broadcasts VTXO root on-chain
    S->>R: new_round_start (t=new_round_start, round_txid)
    R-->>A: round started
    R-->>B: round started
    R-->>C: round started
    R-->>D: round started
    R-->>E: round started
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
