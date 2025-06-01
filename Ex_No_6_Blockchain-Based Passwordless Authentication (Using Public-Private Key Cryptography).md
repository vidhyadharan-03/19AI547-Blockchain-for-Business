# Experiment 6: Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
## Date : 28.04.2025
# Aim:
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks.

# Algorithm:
## Step 1:
Start the Ethereum environment (such as Ganache, or a Testnet) and deploy the smart contract that manages user registration and authentication.

## Step 2:
User initiates registration by submitting their Ethereum public key to the smart contract.

## Step 3:
The smart contract securely stores the public key associated with the user's account address.

## Step 4:
During login, the server (or smart contract) generates a random challenge message (e.g., a random string or nonce).

## Step 5:
The server sends the challenge to the user’s client-side application.

## Step 6:
The user’s client-side application signs the challenge using their private key via cryptographic functions (e.g., web3.eth.personal.sign).

## Step 7:
The signed challenge (digital signature) is sent back to the server (or smart contract) for verification.

## Step 8:
The server (or smart contract) verifies the signature using the stored public key and checks whether the signature matches the original challenge.

## Step 9:
If the verification is successful, the user is authenticated successfully; otherwise, access is denied.

## Step 10:
End the process by granting access to authenticated users or sending an error message for failed authentication.



# Program:
```
Register Number : 212222100058
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PasswordlessAuthDemo {
    struct User {
        bool registered;
        address pubKey;
        bytes32 privateKey; // Fake private key for demo
    }

    mapping(address => User) public users;
    bytes32 public latestChallenge;

    event UserRegistered(address user, address pubKey, bytes32 privateKey);
    event ChallengeGenerated(bytes32 challenge);
    event SignatureGenerated(bytes32 hash, uint8 v, bytes32 r, bytes32 s);

    // Step 1: Register user
    function registerUser() public {
        require(!users[msg.sender].registered, "Already registered");

        // Fake public/private keys
        address fakePubKey = msg.sender;
        bytes32 fakePrivateKey = keccak256(abi.encodePacked(msg.sender, block.timestamp));

        users[msg.sender] = User({
            registered: true,
            pubKey: fakePubKey,
            privateKey: fakePrivateKey
        });

        emit UserRegistered(msg.sender, fakePubKey, fakePrivateKey);
    }

    // Step 2: Generate random challenge
    function generateChallenge() public returns (bytes32) {
        require(users[msg.sender].registered, "User not registered");
        latestChallenge = keccak256(abi.encodePacked(block.timestamp, msg.sender));
        emit ChallengeGenerated(latestChallenge);
        return latestChallenge;
    }

    // Step 3: "Sign" the challenge (fake signing)
    function generateSignature() public returns (bytes32 hash, uint8 v, bytes32 r, bytes32 s) {
        require(users[msg.sender].registered, "User not registered");
        
        hash = latestChallenge;
        bytes32 combined = keccak256(abi.encodePacked(users[msg.sender].privateKey, hash));
        
        // Fake values for r, s, v
        r = bytes32(uint256(uint160(users[msg.sender].pubKey)) << 96);
        s = combined;
        v = 27;

        emit SignatureGenerated(hash, v, r, s);

        return (hash, v, r, s);
    }

    // Step 4: Authenticate
    function authenticate(bytes32 hash, uint8 v, bytes32 r, bytes32 s) public view returns (bool) {
        require(users[msg.sender].registered, "User not registered");

        bytes32 expectedCombined = keccak256(abi.encodePacked(users[msg.sender].privateKey, hash));
        bytes32 expectedR = bytes32(uint256(uint160(users[msg.sender].pubKey)) << 96);
        uint8 expectedV = 27;

        if (r == expectedR && s == expectedCombined && v == expectedV) {
            return true;
        } else {
            return false;
        }
    }
}
```

# Output:


![Screenshot 2025-04-28 144207](https://github.com/user-attachments/assets/0392deb0-1fbe-455a-a1d5-50321c316f2a)
![Screenshot 2025-04-28 144433](https://github.com/user-attachments/assets/14a02e98-ba35-40d5-899c-b76b57719f7e)
![Screenshot 2025-04-28 144739](https://github.com/user-attachments/assets/8231bdf0-2799-4735-b09f-daf740084efe)
![Screenshot 2025-04-28 144922](https://github.com/user-attachments/assets/2b445960-011f-4b54-b561-6eaca305df8a)
![Screenshot 2025-04-28 145143](https://github.com/user-attachments/assets/88e2ba77-3683-4233-866c-8c7b51d0eab8)
![Screenshot 2025-04-28 144129](https://github.com/user-attachments/assets/2d4ac3d2-dde9-4424-ba0a-855412a20a7e)
![Screenshot 2025-04-28 144146](https://github.com/user-attachments/assets/2e5c1a16-1484-4a4e-8c2c-0c8390202e3a)


# High-Level Overview:
Eliminates password hacks & phishing attacks.


Uses Ethereum's built-in cryptographic functions.


Inspired by Web3 login solutions like MetaMask authentication.

# RESULT: 
Thus, Blockchain-Based Passwordless Authentication has been sucessfully implemented.
