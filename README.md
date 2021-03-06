# CommonFactory
Package with helper scripts to deploy a Common DAO using Arc `DAOFactory`.

# Usage
Run: `npm install --save @daostack/commonfactory`

Import the `commonfactory` methods: `getForgeOrgData, getSetSchemesData` and use them to generate call data to Arc `DAOFactory` methods.
```javascript
const { getForgeOrgData, getSetSchemesData } = require('@daostack/commonfactory');

// ...
const forgeOrg = await daoFactory.methods.forgeOrg(
      ...getForgeOrgData({
        DAOFactoryInstance,
        orgName,
        founderAddresses: [web3.eth.accounts.wallet[0].address],
        tokenDist: [0],
        repDist: [100]
      })
    ).send();

// ...
const setSchemes = await daoFactory.methods.setSchemes(
      ...getSetSchemesData({
        DAOFactoryInstance,
        avatar: avatarAddress,
        votingMachine,
        joinAndQuitVoteParams: "0x1000000000000000000000000000000000000000000000000000000000000000",
        fundingRequestVoteParams: "0x1100000000000000000000000000000000000000000000000000000000000000",
        schemeFactoryVoteParams: "0x1110000000000000000000000000000000000000000000000000000000000000",
        fundingToken: "0x0000000000000000000000000000000000000000",
        minFeeToJoin: 100,
        memberReputation: 100,
        goal: 1000,
        deadline,
        metaData: 'metadata'
    })
  ).send();

//...
```

# API
### - `getForgeOrgData`: Prepares the parameters for calling DAOFactory `forgeOrg`.
### Parameters:
    - `DAOFactoryInstance`: The DAOFactory address
    - `orgName`: The Common name
    - `founderAddresses`: Array of founders addresses
    - `repDist`: Array of reputation per founder (array must be the same length as `founderAddresses`)

### - `getSetSchemesData`: Prepares the parameters for calling DAOFactory `setSchemes`.
### Parameters:
    - `DAOFactoryInstance`: The DAOFactory address
    - `avatar`: The address of the Avatar (created by the `forgeOrg` contract call)
    - `votingMachine`: Genesis Protocol address
    - `joinAndQuitVoteParams`: Hash of the Genesis Protocol parameters to use for the `JoinAndQuit` scheme (must be registered in the GenesisProtocol contract before)
    - `fundingRequestVoteParams`: Hash of the Genesis Protocol parameters to use for the `FundingRequest` scheme (must be registered in the GenesisProtocol contract before)
    - `schemeFactoryVoteParams`: Hash of the Genesis Protocol parameters to use for the `SchemeFactory` scheme (must be registered in the GenesisProtocol contract before)
    - `fundingToken`: Token address to use in the `JoinAndQuit` and `FundingRequest` schemes (`0x0000000000000000000000000000000000000000` for the native token of the network used)
    - `minFeeToJoin`: Minimum fee to join the Common (wei value)
    - `memberReputation`: Reputation to give to each member joining
    - `goal`: Funding goal of the Common (wei value)
    - `deadline`: Deadline to reach the Common funding goal (UNIX epoch)
    - `metaData`: IPFS hash to the Common parameters
