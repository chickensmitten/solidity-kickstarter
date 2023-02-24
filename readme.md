# Kickstarter Smart Contract
- Create a smart contract that requires approvers to approve every spending request
- Refer to [solidit basics](https://github.com/chickensmitten/solidity-basics) for more info

## How the CampaignFactory Contract work
### Variables
- `deployedCampaigns`: address[], addresses of all deployed campaigns

### Functions
- `createCampaign`: deploys a new instance of a Campaign and stores the resulting address
- `getDeployedCampaigns`: returns a list of all deployed campaigns

## How the Campaign Contract work
### Variables
- `manager`: address, address of the person who is managing this campaign
- `minimum contribution`: uint, minimum donation required to be considered a contributor or approver
- `approvers`: mapping, list of addresses for every person who has donated money
- `requests`: request[], list of requests that the manager has created

### Functions
- `campaign`: constructor function that sets the minimumContribution and the owner
- `contribute`: called when someone wants to donate money to the campaign and become an approver
- `createRequest`: called by the manager to create a new spending request
- `approveRequest`: called by each contributor to approve a spending request
- `finalizeRequest`: after a request has gotten enough approvals, the manager can call this to get money sent to the vendor

### Request Struct
- `description`, string, purpose of request
- `amount`, uint, ether to transfer
- `recipient`, address, who gets the money
- `complete`, bool, whether the request is done
- `approvals`, mapping, track who has voted
- `approvalCount`, uint, track number of approvals

## Wrong way to design the approveRequest
- create an `Address[]` of votes. If approver votes yes, store the address in the `Address[]` then increment counter of yes votes or no votes. When approver attempts to vote again, look at the `Address[]` to see if the approver address is already in the array. If it does, then prevent approver from voting.
- Using `Address[]` is a poor approach because of gas cost that linearly increase.
- When working with contracts, avoid using arrays as much as possible. Instead use mapping because search time is constant.
- This is the correct mapping `mapping(address => bool) public approvers;`

## Factory to create solidty contracts
- Use a contract to deploy another contract.
- 