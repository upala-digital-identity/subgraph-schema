Storage:

Example querry to a subgraph - https://github.com/graphprotocol/ethdenver-dapp/blob/bcf0daf564825bb50e7072fa470d94bbce178db5/src/App.js#L29

# Requests from DB

## When writing data: 
Get group manager for the group address
Get root timestamp by roothash

## When readng data:
Get array of deleted roots since [latestDeletedRootTimestamp]
Get array of base score updates sinse "latestBaseScoreUpdateTimestamp]

# Housekeeping
All spawned contracts must be registered in Upala.sol with an emitted event in order to create Data source templates for the subgraph.

Need standardazed events for pools?? 

# Requests from manager
When updating scores "is this bundleHash deleted"
When updating base score "what is current base score for the group address"


# Requests from multipassport
When populating multipassport "what is the UpalaID for this address (search owners and delegates of the ID)"