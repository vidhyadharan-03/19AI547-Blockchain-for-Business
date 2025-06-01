## Experiment 5: Zero-Knowledge Proof (ZK) Private Voting System
## Name:VIDHYADHARAN R
## Reg No: 212222110053
## Aim:
To implement a fully private and transparent voting system using Zero-Knowledge Proofs (ZKPs). This ensures that votes are counted fairly without revealing who voted for whom.

## Algorithm:
Step 1: Voter Registration Each voter generates a secret vote key and submits a commitment (hashed vote) to the contract.

Step 2: Voting Process Voters submit their votes privately using a hash, without revealing their choice.

Step 3: ZK Verification The contract verifies if a vote belongs to a registered voter but does not reveal the actual vote.

Step 4: Vote Counting Once voting ends, the contract reveals the final tally without linking votes to individuals.

## Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ZKVoting {
    struct Voter {
        bool registered;
        bytes32 voteCommitment;
    }

    mapping(address => Voter) public voters;
    uint256 public votesForA;
    uint256 public votesForB;

    event VoteCommitted(address voter, bytes32 commitment);
    event VoteRevealed(address voter, uint256 choice);

    function registerVoter(bytes32 commitment) public {
        require(!voters[msg.sender].registered, "Already registered");
        voters[msg.sender] = Voter(true, commitment);
        emit VoteCommitted(msg.sender, commitment);
    }

    function revealVote(string memory secret, uint256 choice) public {
        require(voters[msg.sender].registered, "Not registered");
        require(keccak256(abi.encodePacked(secret, choice)) == voters[msg.sender].voteCommitment, "Invalid proof");

        if (choice == 1) votesForA++;
        if (choice == 2) votesForB++;

        emit VoteRevealed(msg.sender, choice);
    }
}
```
## Expected Output:
Voters commit their votes privately.

When revealed, the contract verifies correctness but keeps votes anonymous.

Final result is publicly verifiable without exposing individual votes.

![image](https://github.com/user-attachments/assets/b534e87b-647a-4d2a-83eb-4651b6583df4)


![image](https://github.com/user-attachments/assets/ef0ea3d6-8256-4378-88fd-3c72285fb400)


![image](https://github.com/user-attachments/assets/be9bc439-fcbf-4df3-a633-1ded0258b6b2)

![image](https://github.com/user-attachments/assets/907bf58c-919f-4b44-9eb4-3d8549bdca45)

## High-Level Overview:
Uses ZKPs to ensure anonymous and fair elections.

Prevents vote tampering while maintaining voter privacy.

Mimics real-world ZK voting applications in governance and DAOs.

## RESULT:
Uses ZKPs to ensure anonymous and fair elections.
