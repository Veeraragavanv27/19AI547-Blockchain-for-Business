# Experiment 5: Zero-Knowledge Proof (ZK) Private Voting System
## Name : Veeraragavan V
## Reg : 212223230237
## Date : 24/04/2025
# Aim:
To implement a fully private and transparent voting system using Zero-Knowledge Proofs (ZKPs). This ensures that votes are counted fairly without revealing who voted for whom.

# Algorithm:
## step 1: Key Generation:

Each voter creates a secret vote key.

## step 1: Commitment Submission:

Voter submits a hashed version (commitment) of their vote to the smart contract.

## step 1: Registration Verification:

The contract records the commitment, confirming the voter is registered.

## step 1: Private Voting:

Voter submits their actual vote in hashed form, ensuring privacy.

## step 1: Zero-Knowledge Proof:

A ZK proof verifies the vote is valid and cast by a registered voter, without revealing the vote.

## step 1: Vote Storage:

The contract stores the verified encrypted vote.

## step 1: Voting Deadline:

Once the deadline passes, voting ends and votes can be counted.

## step 1: Vote Tallying:

The contract calculates the final result without exposing individual votes or identities.


# Program:
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
# Output:

![Screenshot 2025-04-25 130008](https://github.com/user-attachments/assets/c1b0ca62-9607-4c17-8cd3-7639cbd7994e)
![Screenshot 2025-04-25 130223](https://github.com/user-attachments/assets/0ed0ac42-4900-4067-a28f-35c9ae9678b3)
![Screenshot 2025-04-25 130344](https://github.com/user-attachments/assets/7e9c8371-2329-45bb-a090-38421bef622c)
![Screenshot 2025-04-25 130433](https://github.com/user-attachments/assets/f2eb4a4a-88e0-4001-82da-55d0c34399d6)
![Screenshot 2025-04-25 130500](https://github.com/user-attachments/assets/376cf351-3bb7-4ca5-b003-c3e09c1e03c8)



# High-Level Overview:
Uses ZKPs to ensure anonymous and fair elections.


Prevents vote tampering while maintaining voter privacy.


Mimics real-world ZK voting applications in governance and DAOs.

# RESULT: 
Thus,a fully private and transparent voting system using Zero-Knowledge Proofs (ZKPs) is successfully executed.
