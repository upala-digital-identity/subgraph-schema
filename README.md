# Upala subgraph

## Requirements for the subgraph

#### Requests from DB ([repo](https://github.com/upala-digital-identity/db))

When writing data:
- Get group manager for the group address
- Get scoreBundle timestamp by hash

When reading data:
- Get array of deleted roots since [latestDeletedRootTimestamp]
- Get array of base score updates since [latestBaseScoreUpdateTimestamp]

#### Requests from group manager ([repo](https://github.com/upala-digital-identity/group-manager))
- When updating scores "is this bundleHash deleted"
- When updating base score "what is the current base score for the group address"

- TBD Commitments

#### Requests from multipassport ([repo](https://github.com/upala-digital-identity/multipassport))
When populating multipassport:
- Get UpalaID for the provided address (search owners and delegates of the ID)
- Get owner and delegates for the Upala ID"
- Get group metadata by address

#### Requests from unique-human-lib ([repo](https://github.com/upala-digital-identity/unique-human-lib))
- Get approved pools for the dapp address

#### Housekeeping
Data needed for the subgraph itself.
All spawned contracts must be registered in Upala.sol with an emitted event in order to create Data source templates for the subgraph.

Need standardazed events for pools??

## Schema (under developement)

    type UpalaID @entity {
        id: ID!  # user Upala ID
    }

    type Delegate @entity {
        id: ID!  # delegate Ethereum address
        isOwner: Boolean!  # owner is delegate too (per protocol)
        upalaID: UpalaID!
    }

    type Group @entity {
        id: ID!  # group address (also used for subgraph housekeeping???)
        owner: Bytes!  # onwer's Ethereum address
        metadata: String  # metadata json
    }

    type ScoreBundle @entity {
        id: ID!  # score bundle hash (and root if Merkle tree)
        timestamp: BitInt!  # uint256
        isDeleted: Boolean!
        group: Group!
    }

    type BaseScore @entity {  # needed just for db housekeeping to work!
        id: ID!  # some ID?? 
        value: BitInt!  # uint256
        timestamp: BitInt!  # uint256
        group: Group!
    }

    type DApp @entity {  # need many-to-many relation
        id: ID!  # dapp address
        group: Group!
    }

## Query subgraph

Example querry to a subgraph - https://github.com/graphprotocol/ethdenver-dapp/blob/bcf0daf564825bb50e7072fa470d94bbce178db5/src/App.js#L29