# SSH Public-Key Authentication

SSH public-key authentication works through asymmetric cryptography. Here's how it enables passwordless authentication between two servers:

## Key Components

**Public Key**: A mathematically derived key that's shared openly. It's placed on the remote server you want to access.

**Private Key**: A secret key kept secure on your local machine. Never shared with anyone.

These keys are mathematically linked—data encrypted with the public key can only be decrypted with the private key, and vice versa.

## The Passwordless Authentication Flow

1. **Key Generation**: You generate a key pair on your local machine (e.g., using `ssh-keygen`). The private key stays local, the public key gets copied to the remote server's `~/.ssh/authorized_keys` file.

2. **Connection Initiation**: When you connect to the remote server, SSH initiates a connection and the server presents a challenge (typically by sending a random value).

3. **Proof of Ownership**: Your SSH client signs this challenge using your private key, creating a cryptographic signature. The client sends this signature to the server.

4. **Verification**: The server uses the public key (from `authorized_keys`) to verify the signature. If the signature is valid, it proves you possess the corresponding private key without ever transmitting it.

5. **Access Granted**: If verification succeeds, you're authenticated and granted access—no password needed.

## Why This is Secure

- Your private key never leaves your machine
- The server never needs to know your private key
- Each authentication attempt uses a new challenge, preventing replay attacks
- The cryptographic signatures are mathematically unique to both the challenge and your private key

This is more secure than passwords because:
- No credentials transmitted over the network
- Immune to brute-force password attacks
- You can disable password authentication entirely on the server
- Keys can have passphrases for additional protection
