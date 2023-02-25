# Kickstarter Smart Contract
- Create a smart contract that requires approvers to approve every spending request
- Refer to [solidity basics](https://github.com/chickensmitten/solidity-basics) for more info
- `web3` is the interface between the smart contract and js framework

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

## Gotchas for returning a list of Requests
- ***Get array of struts one by one***: As of time of writing, Solidity cannot return an array of struts. It has to be done, one by one. code below in solidity smart contract doesn't work
```
function getAllRequests() public view returns (Request[]) {
  return requests;
}

// TypeError: Internal or recursive type is not allowed for public or external function getAllRequests() public view returns (Request[])
```
we can though get the total count with
```
function getRequestsCount() public view returns (uint256) {
  return requests.length;
}
```

## Gotchas with MetaMask complaining that a transaction is going to fail
- ***Transaction going to fail***: Sometimes, MetaMask will complain that a transaction is going to fail due to high gas limit. However, take a closer look because it could be that your Solidity function is going to fail and not because of the high gas fees. For example, `finalizeRequest` failes when not enough approvers are met, yet MetaMask will complain about the high gas limit instead.