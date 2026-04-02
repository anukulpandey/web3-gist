# Web3 Session GIST

## Prerequisites

1. GIT Bash - [click me to download](https://git-scm.com/install/windows)
2. Node.js - [click me to download](https://nodejs.org/en/download)

## Download Now

1. Foundry - [click me to download](https://getfoundry.sh/) - run this inside git bash
   Foundry comes with a tool called Anvil, which we will use to run a local blockchain network

2. Metamask - [click me to download](https://chromewebstore.google.com/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?hl=en)

## Configuring local blockchain

Start a local chain using anvil

```
anvil
```

## Getting started with smart contracts

- Visit [Remix IDE](https://remix.ethereum.org/?nomobileredirect)

- NETWORK DETAILS
-    url: http://127.0.0.1:8545
-    coin: PANEER
-    chain id: 31337
-    network : Aman network

- Paste this in a new file named `HelloWorld.sol`

  ```
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.20;
  
  contract Greeter{
      string public message;
  
      constructor(){
          message ="hello world!";
      }
  
     function setGreeting(string calldata _greeting) public{
      message = _greeting;
      }
  
      function getGreeting() view external returns(string memory){
          return message;
      }
  }
  ```

- Deploy Voting Contract now by pasting this in a new file called `Voting.sol`

  ```
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.20;
  
  contract Voting {
  
      struct Candidate {
          string name;
          uint256 voteCount;
      }
  
      address public owner;
      bool public votingOpen;
  
      mapping(address => bool) public hasVoted;
      Candidate[] private candidates;
  
      event VoteCast(address indexed voter, uint256 indexed candidateId);
      event VotingStatusChanged(bool open);
  
      modifier onlyOwner() {
          require(msg.sender == owner, "Not authorized");
          _;
      }
  
      constructor(string[] memory _candidateNames) {
          owner = msg.sender;
          votingOpen = true;
  
          for (uint256 i = 0; i < _candidateNames.length; i++) {
              candidates.push(Candidate(_candidateNames[i], 0));
          }
      }
  
      function vote(uint256 _candidateId) external {
          require(votingOpen, "Voting closed");
          require(!hasVoted[msg.sender], "Already voted");
          require(_candidateId < candidates.length, "Invalid candidate");
  
          hasVoted[msg.sender] = true;
          candidates[_candidateId].voteCount++;
  
          emit VoteCast(msg.sender, _candidateId);
      }
  
      function closeVoting() external onlyOwner {
          votingOpen = false;
          emit VotingStatusChanged(false);
      }
  
      function getCandidatesCount() external view returns (uint256) {
          return candidates.length;
      }
  
      function getCandidate(uint256 _id) external view returns (string memory, uint256) {
          Candidate memory c = candidates[_id];
          return (c.name, c.voteCount);
      }
  
      function getAllCandidates() external view returns (Candidate[] memory) {
          return candidates;
      }
  }
  ```

- Download frontend

  ```
  cd ~/Desktop
  git clone https://github.com/anukulpandey/voting-dapp
  ```

- Open this folder in VS Code.
- Run

  ```npm install```

  ```npm run dev```

- Replace `0x5FbDB2315678afecb367f032d93F642f64180aa3` with your contract address in `voting-dapp/src/Voting.json`
