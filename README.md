# mining
btc mining 
# This is a simple python code for btc mining
# Disclaimer: This is not a real mining program and it does not guarantee any profit or success
# This is only for educational and entertainment purposes

import hashlib
import time

# Define the target difficulty (number of leading zeros in the hash)
difficulty = 4

# Define the maximum nonce value (2^32 - 1)
max_nonce = 4294967295

# Define the reward for finding a valid block (currently 6.25 BTC)
reward = 6.25

# Define the genesis block (the first block in the blockchain)
genesis_block = {
    "index": 0,
    "timestamp": 1231006505,
    "transactions": [
        {
            "sender": "Satoshi Nakamoto",
            "recipient": "Hal Finney",
            "amount": 10
        }
    ],
    "nonce": 2083236893,
    "hash": "000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f",
    "previous_hash": None
}

# Initialize the blockchain with the genesis block
blockchain = [genesis_block]

# Define a function to create a new block
def create_block(transactions, previous_hash):
    # Initialize the block index, timestamp and nonce
    index = len(blockchain)
    timestamp = int(time.time())
    nonce = 0

    # Loop until a valid hash is found or the nonce reaches the maximum value
    while nonce <= max_nonce:
        # Concatenate the block data into a string
        block_data = str(index) + str(timestamp) + str(transactions) + str(nonce) + str(previous_hash)

        # Compute the hash of the block data using SHA-256 algorithm
        block_hash = hashlib.sha256(block_data.encode()).hexdigest()

        # Check if the hash meets the target difficulty
        if block_hash.startswith("0" * difficulty):
            # If yes, return a new block with the valid hash and nonce
            return {
                "index": index,
                "timestamp": timestamp,
                "transactions": transactions,
                "nonce": nonce,
                "hash": block_hash,
                "previous_hash": previous_hash
            }

        # If no, increment the nonce and try again
        nonce += 1

    # If the nonce reaches the maximum value, raise an exception
    raise Exception("No valid hash found")

# Define a function to mine a new block
def mine_block():
    # Get the last block in the blockchain
    last_block = blockchain[-1]

    # Create a dummy transaction for testing purposes
    # In reality, this would be a list of verified transactions from the network
    transaction = {
        "sender": "Alice",
        "recipient": "Bob",
        "amount": 1
    }

    # Create a new block with the transaction and the last block's hash
    new_block = create_block([transaction], last_block["hash"])

    # Append the new block to the blockchain
    blockchain.append(new_block)

    # Print the new block details and the reward
    print(f"New block mined: {new_block}")
    print(f"Reward: {reward} BTC")

# Mine 10 blocks for testing purposes
for i in range(10):
    mine_block()
