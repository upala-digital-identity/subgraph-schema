# Upala subgraph

## Requirements for the subgraph

#### Requests from DB ([repo](https://github.com/upala-digital-identity/db))

When writing data:
- Get pool manager for the pool address
- Get scoreBundle timestamp by hash

When reading data:
- Get array of deleted roots since [latestDeletedRootTimestamp]
- Get array of base score updates since [latestBaseScoreUpdateTimestamp]

#### Requests from group manager ([repo](https://github.com/upala-digital-identity/group-manager))
- When updating scores "is this bundleHash deleted"
- When updating base score "what is the current base score for the pool address"

- TBD Commitments

#### Requests from multipassport ([repo](https://github.com/upala-digital-identity/multipassport))
When populating multipassport:
- Get UpalaID for the provided address (search owners and delegates of the ID)
- Get owner and delegates for the Upala ID"
- Get pool metadata by address
- Get poolType (poolFactoryAddress) by pool address

#### Requests from unique-human-lib ([repo](https://github.com/upala-digital-identity/unique-human-lib))
- Get approved pools for the dapp address
- Get poolType (poolFactoryAddress) by pool address

#### Housekeeping
Data needed for the subgraph itself.

All spawned pools are registered in Upala.sol with an emitted event in order to create Data source templates for the subgraph.
Same for DApps
Same for poolFactories

## Contracts with events
- [Upala](https://github.com/upala-digital-identity/upala/blob/master/contracts/protocol/upala.sol)
- [Pool and pool factory](https://github.com/upala-digital-identity/upala/blob/master/contracts/pools/signed-scores-pool.sol)
- [DApp lib and example DApp](https://github.com/upala-digital-identity/upala/blob/master/contracts/mockups/dapp.sol)

## Schema (under developement)

#### Users 

    type UpalaID @entity {
        id: ID!  # user Upala ID
    }

    type Delegate @entity {
        id: ID!  # delegate Ethereum address
        isOwner: Boolean!  # owner is delegate too (per protocol)
        upalaID: UpalaID!
    }

#### Pools 

    type Pool @entity {
        # on pool creation
        id: ID!  # pool address (also used for subgraph housekeeping???)
        poolFactoryAddress: Bytes!  # is used to select pool ABI
        
        # monitors ownership at creation and on OwnershipTransferred
        owner: Bytes!
        
        # base score
        baseScore: BitInt!  # uint256
        # timestamp is needed for db housekeeping (is derived from event)
        baseScoreTimestamp: BitInt!  # uint256

        metadata: String  # metadata json
    }

    type ScoreBundle @entity {
        id: ID!  # score bundle hash (and root if Merkle tree)
        timestamp: BitInt!  # uint256
        isDeleted: Boolean!
        pool: Pool!
    }

#### DApps 

TODO - check [entity-relationships](https://thegraph.com/docs/developer/create-subgraph-hosted#entity-relationships)

    type DApp @entity {  # need many-to-many relation?
        id: ID!  # dapp address
        approvedPools: [Pool]  # todo one-to-many
    }

## Query subgraph

Example querry to a subgraph - https://github.com/graphprotocol/ethdenver-dapp/blob/bcf0daf564825bb50e7072fa470d94bbce178db5/src/App.js#L29

## Questions 

Need standardazed events for pools??
Does it makes sense to store protocol settings in the subgraph?