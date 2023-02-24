# Kickstarter Smart Contract
- Create a smart contract that requires approvers to approve every spending request

## How the Campaign contract work
### Variables
- `manager`: address, address of the person who is managing this campaign
- `minimum contribution`: uint, minimum donation required to be considered a contributor or approver
- `approvers`: address[], list of addresses for every person who has donated money
- `requests`: request[], list of requests that the manager has created

### Functions
- `campaign`: constructor function that sets the minimumContribution and the owner
- `contribute`: called when someone wants to donate money to the campaign and become an approver
- `createRequest`: called by the manager to create a new spending request
- `approveRequest`: called by each contributor to approve a spending request
- `finalizeRequest`: after a request has gotten enough approvals, the manager can call this to get money sent to the vendor
