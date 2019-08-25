<!-- COURSE REPO -->
https://github.com/15Dkatz/cryptochain
`
<!-- BLOCKCHAIN -->
Distributed ledger that stores data that is shared across all nodes in a network.

Each block acts as storage unit. It is given a data field that stores data. Each BLOCK is given an unique value that is hashed and is generated from a hashed/crypto function. Gives out a unique output for every different input.
 
 Includes last hashed block which is key to creating a chain. Thanks to this linkage, it becomes a blockchain.

 Use it because it is fair, transparent and SECURE

 <!-- LEDGER -->
 A record keeping book that records txn's in an organisation
 Blockchain stores yxn's that is distributed.
 Ledger is shared with everyone in network.
 By connecting to the network, everyone get's a record and will receive same updates.

<!-- SECURITY -->
No central point of failure in the netowrk.

<!-- CRYPTOCURRENCY -->
Do levergae the blockchain but is its own tech - where the blockchain is one of the pieces. 

3 pieces:
1. Blockchain - ledger. Txn's that all can see. Preventing fraud, the blockchain uses cryptography that allows users to genrate unique signatures that allows them to prove ownership of the currency. Each person must stamp a txn with their signature and this is bases on 2 cryptographic keys:
- 1 private
- 1 public

2. Wallets - objects that stores public and private keys of the user's assets. 
PUBLIC key is used as an address fore the wallet. Allows others to send currency to you.
PRIVATE key - helps wallets generate signatures that make these txn's official.

3. Mining - miners do the work of adding txn's to the netowrk. 
Miners take a group of unconfirmed txn's and use them as the data to be officially recorded in a new block in the chain.
In order for the miner to earn this right, they must solve a complex problem on the chain - proof of work - which is time consuming & expensive. When solved, they get the reward of creating the block with all the confirmed txn's and is rewarded with crypto. 

Other miners receive confirmation of the solution which is easily verified and recognised - miners then include the new block into the chain. 

<!-- BLOCKS -->
1. timeStamp - JS date object down to the millisecond
2. lastHash - Refers to hash of the block that came before
3. data - txn's or any other kind of data
4. hash - of the new block, generated from all its data

<!-- GENESIS BLOCK -->
Hardcoded with dummy values to allow lastHash function to work for all following blocks

*********************************
<!-- SECTION 3: THE BACKEND -->
*********************************
<!-- Overall functionality -->
1. 1st block is the Genesis block
2. Ability to add block to the chain at the end of its own chain
3. New block is based on last block before it.

<!-- Blockchain class -->
1. Create blockchain class
2. Ensure it takes into account the genesis block
3. Ensure it can add a block to the blockchain
4. Add data into the last block of the blochain

<!-- Validate the blockchain -->
1. Inspecting a blockchain and ensuring everything is correct and authentic:
- right fields [timestamp, data, lastHash, hash]
- the lastHash field of each block needs to reference the hash of the block that became before. THIS IS WHAT CREATES THE LINKS
- the blockHash needs to be VALID. 

2. Hashing
- Take all fields in a block and generate the fields ourselves
- Check if generated hash is equal to hash contained in block

*Returns a Boolean*

<!-- Chain Replacement -->
The blockchain should be able to replace the chain with new a NEW chain of blocks as long as the new chain is longer and the new chain is deemed to be valid.

WHY?
1. A blockchain works better with multiple instances of the chain.
2. When multiple chains interract - this is the blockchain network.
3. The goal is they all come to an agreement on the longest chain. This is when all nodes come to an agreement and everyone has an equal stake on validating it.

<!-- Hash in MineBlock -->
SHA-256 hash
Want to make sure that minedblock hash value is equal to the cryptohash function call of the mineblock timestamp, the last block hash and the data. These are all the fields that are going into the new block

<!-- Replace Chain -->
Call blockchain.replace and input a new array as a blockchain
To check if original blockchain instance is actually replaced or not
We need to create a new instance called newChain which will be the new, replacement chain

<!-- Proof of Work -->
- Spend computational power to mine a block - CPU time
- Deters attackers from rewriting data - very expensive to try

Honest - any peer can submit a new chain. Adding a block which is valid to the chain will be accepted by all peers on the network. Just have to add one block.

Malicious - changing an existing block amends its hash and therefore all hashes before also get corruped. Have to rewrite all blocks which is expensive. 

Hashcash - created in 1997 and originally devleoped to avoid email spanning and attacks on web servers.

At any point in the system, there will be a difficulty level. Depending on this difficulty, miners will have to find the hash value for this block that matches the hash difficulty in order to add a block.

For this matching - they have to find the amount of corresponding zero's of a hash that is aligned to the difficulty - Level 6 = 000000eb293b10ca3de8. If you try to find it, it becomes exponentially more difficult as the amount fo zeros rise. So in order to solve POW, a miner will have to generate massive amounts of hash's in order to find the target value. 

A miner will regenerate new hashes for the same block based on the block data as well as an adjusting value called the 'Nonce'. By continuously adjusting this Nonce value, they will eventually find one that matches the difficulty.

The Nonce value starts at Zero '0' and increments updards until a nonce is used as a matching value to the amount of zeros according to the difficulty. 

PROOF OF WORK SUMMARISED:
Finding a Nonce that unlocks a hash that meets the difficulty requirement is Proof of Work. Nonce = number used once.

<!-- Difficulty and the Nonce Values -->
&
<!-- Adding Proof of Work Protocol -->
Demand that a block's hash start with a certain amount of zeros

Add 2 props to the Block class in block.js
1. Nonce - adjusted value that allows a miner to cr5eate new hashes based on the block data, until the difficulty requirement is satisfied.

2. Difficulty number

- block.test.js

*Nonce is initially set in the genesis block in config.js
*Going forward, the difficulty can be set on the block that came before it.
The first block will share difficulty of '3'

<!-- Dynamic Difficulty & Mine Rate -->
1. More miners, means more attempts at find valid hashes
2. New blocks added at quicker pace
3. Eventually so many miners would mean insufficient blockchain

Thus adjust the difficulty rate to equal a set mining rate.
- Added as a variable into the block
- Add a global time value called the mine_rate which determines the rate blocks must be added to the chain

<!-- SYSTEM STRUCTURE -->
1. Check timestamp of new block
2. Compare timestamp to last block
3. Look for difference and if < mine rate, then it was too easy and thus raise the difficulty rate by 1.
4. If difference > then mined too slowly and thus lower the difficulty by 1.
5. The average should then be set to the predetermined mine rate.

- Add mine_rate to config.js

<!-- What if difficulty gets to 0 or below? -->
Possible if a handful of blocks are mined quickly
Difficulty gets lowered each time and can go towards zero
CANNOT GO to ZERO! Given a negative number (difficulty) the do while loop would error out.
Therefore, we need to add safeguard to never let it go to zero.
Set a lower limit of 1
Always demand a hash as at least one zero in front of it

<!-- Hashing - Hash difficulty in bits or hex? -->
Our system uses Hex
Bitcoin checks for leading zero bits

<!-- Preventing Difficulty Jumps -->
Prevent hackers from lowering the difficulty rate and thus, being able to manipulate the next block.
1. blockchain.test.js - add test for invalid chain - where a block contains a JUMPED difficulty.

<!-- Blockchaoin API and Network API -->
1. Nodes work through a common API that exposes common methods
2. API's are a set of callable methods
MAIN METHODS
- Read the blockchain and its data
- Write to the blockchain
3. Use Express.js that allows to create a web-server that handles the calls/HTTP requests
4. We use a GET request to read BC and POST to write info
- API will support the network
- All nodes will read each other's data and add blocks to the chain. Multiple servers on the network.

*Setup Express API
Allow a front-end app to interact with back-end
Allow other BC backends to work with each other
Install npm i express

<!-- API -->
1. GET requests to view the blockchain
2. POST requests to add a 

<!-- Real-Time Messaging Network through PubSub -->
Nodes find out status of the network.
Broadcast real-time updates through the entire network using Pub/Sub
(Rather than maintaining a list of peers through api calls)
Publish/Subscriber
1. Publisher - broadcast messages on channels
2. Subscriber - concerned about new messages on the channel/s that they are subscribed to. Not worried about who senders are. 
A publishes only concerned about b/c messages on channel concerned about. 

This is very scalable and can be a plethora of channels.

Node - better to go through OubSub to its concerned channels.

BLOCKCHAIN channel - all nodes in network.
INDIVIDUAL NODE - then sends its update to the BLOCKCHAIN channel and this data will be contained in a message which allows other nodes to see this data via the BC channel. All subscribed to that channel. 

redis - use it for its nice Pubub capabilities. Any tech that has PubSub can be initiated in - if its methodologies are the same.

PubNub - works with NodeJS
- Create pubsub class in project

<!-- Broadcast to the Network -->
Broadcast addition of block to the entire network
All subscribed instances will get that data and then decide validity and then update their own chains.
- What is the true array blockchain state!
- Utilise PubSub

 #WALLETS, KEYS & TRANSACTIONS
 1. Digital Wallets
 - Core object with a balance field - what an individual has collected from the BC.

 2. Keys
- Public & Private Keys
- Only wallet owner has access to the private key
- Everyone on the BC can see the value in the wallet

#Private Key - allows individual to create unique digital signatures for every exchange. They need to sign the txn to make it official. 

#Public Key - allows others on the BC to verify those digital signatures. 

The wallet's public & private jey are generated in such a way that the combination of both the SIGNATURE and PUBLIC key can only be deemed valid if that signature was generated by that wallet's private key.

 3. Public Address
 Identical to the wallet's public key
 With a wallet's address, ither's can send currency to that address.

 4. Transactions
 Information behind the movement of data between addresses in a BC.
 #2 Primary aspects:
 i. INPUT - provides details of the original sender and includes:
 a. timestamp
 b. Balance of sender
 c. Signature of the sender for the txn
 d. Sender's public key

 ii. OUTPUT - provides details of 
 a. Amount sender wants to send for the txn
 b. Receiver's address

 c. Output of currency sent to the sender him/herself. #WHY? 
 - Specifies how much currency the sender should have after the txn is complete. Is calculated by taking sender amount less txn (Remaining balance output)

 #Digital Signatures
 - Unique signatures are based on the private & public key.
 - These are unique strings of numbers
 - With keys, a user has an ability to sign data using encrypted hash values.
 - The hash values are genrated using the combination of txn data itself and the private key info of the individual.
 - Any difference will generate a complete different hash value

Anyone can then use the public key of that txn to verify that signature.
The public key is used to decrypt the signature and see the data behind it. 

 #Process of verifying txn's
 If the decrypted data does not equal the original data presented by the txn, the txn is invalid due to either:
 1. The original data was tampered with after the data was signed by the sender. 
 2. The signature was generated by a private key that does not correspond to the publick key of the person who wants to action the txn.

 Either way, it is invalid. 

 #3 INGREDIENTS TO CREATE Blockchain-powered Cryptocurrencies
 
 1. It must contain Wallet objects
 2. It must be bale to generate keys for digital signatures and verification
 3. There must be txn objects to contain the movement of funds bewteen users. 

<!-- Public Key Functionality -->
 Cryptographic function that works alongside the Private key
 The Public Key address that works with a private-public key pair
 *use module - elliptic (Node package) - uses elliptic cryptography
* Centers around idea that is is infeasible to calculate the answer to something that is secret upon an elliptic curve.
* Focus on private-public key pair

*ECC*
1. Trapdoor function
- Take a given value (A) to get to another value, (B) - very easy
But if you want to get back A, it's very difficult.
- A popular function is RSA, which uses prime number factorisation. To get back to the 2 variables if very difficult.

2. ECC vs RSA - levels of security
- ECC
256bit

- RSA
3072bit

3. How does ECC work?
- Symmetrical on the x-axis
- A line always hits 3 points (if starting point is on the graph)

<!-- ECC in BChain -->
Added to index.js within /util

<!-- Sign data and verify signatures -->
- Ensure wallet can sign data
- Ensure wallet can verify signature of other wallet. We need to accept transactions with proper signatures via other wallet's private keyt.
- A proper signature, based off of a public key can only be created with the key-pair object that contains the underlying private key. If anyone is modified, the derived data will not match the proposed data.

1. Add tests to wallet/index.js

<!-- Allow wallets to conduct transactions -->
*at this point we have wallets that have balances, public keys & the ability to sign data; now we want to create transaction objects to capture exchange of currency between wallets and signed by sender's wallet.

1. create transaction.test.js in /wallet
2. create transaction.js in /wallet

<!-- Validate a Transaction -->
Add a valid transaction method that returns a boolean value
Depending on whether we should trust the output data and the input object
Place as a static method in the transaction class

*in transaction.test.js

<!-- Wallet Create Transaction -->
index.js/wallet
- Confirm the own wallet instance as the sender wallet

<!-- Update Transactions with multiple outputs -->
- Add update function in transaction.test.js
- Adds an update to the recipient's balance within the output map
- The remaining output of the sender must be subtracted from the sender's wallet
- The input needs to be re-signed by the sender's wallet

*What is amount sent, exceeds the sender's balance??
- cover this in transaction.test.js under update()
- determine if the amount specified exceeds the outputAmount designated in the sender wallet's publicKey

<!-- TRANSACTION POOL STRUCTURE -->
- A txn pool is data structure that contains all the data from the wallets in the network
- It can be an array or map with key-value structure

*It needs to contain 3 things:
1. Unique set of transactions
2. It can update existing stored transactions
3. Rewrite multiple transactions

Every node will have its own instance of the transaction pool
Need to ensure that they are all syncronised
Enables miners to get accurate list of txn's to add to new block

<!-- API to Transaction Pool -->
Allow transaction to be created through API
When a user wants to send funds, they make a POST request
When the transaction is created, it will be added to the growing transaction pool

1. /index.js
Require new transaction pool class and implement functions
Implement cleaner error handling )(try, catch)

*Transaction Updates in the API

<!-- Get Transaction Pool Map -->
Add a GET request for a user to get the data from the transactionPool map
/index.js

<!-- Broadcast a transaction -->
We need broadcast txns as soon as they are made via the API
/index.js --> pubsub --> add transactionPool
/app/pubsub --> add transactionPool

<!-- Syncronise chains -->
use syncChains in /index.js

<!-- RECAP -->
Created the core transaction pool with an ability to set transactions.

Handled transaction through the API, and added API-generated transactions to the pool.

Made sure invalid transactions did not go to the pool.

Handled updates to transactions through the API.

Exposed a new API method to read the transaction pool data.

Broadcasted transactions as they occurred through the API.

Synced the transaction pool map as a network-dependent object like the blockchain.

<!-- MINE TRANSACTIONS -->
1. A transaction miner - will do the job of mining new blocks with new txns data. The txns data comes from the txn pool. Miners do the work and pay the cost for computational power. 

2. The miners receive a reward for doing this work - a special transaction with only an output - to the miner's wallet. The input will have a unqiue transaction input that all nodes validate.

3. Ensures new txns are recorded via an incentive-based POW system.

<!-- Functionality -->
In order for a miner to add a block to the chain.

1. Grab all the VALID transactions in the pool. Need to distinguish valid txns.

2. Generate a miner's reward - which is included in the miner's block
- Key is miner's public key
- Input for txn is more complex.
- Standard input consists of an address, signature, timestamp and amount (wallet balance)
 The input is not generated by a wallet. We need to have hard-coded values for the input - config.js
 REWARD_INPUT
 - Add Reward Transaction to transaction class in /transaction.js
 

3. Do the CPU work to find a valid hash
4. Broadcas the updated blockchain to the entire network to be validated by nodes
5. Clear the txn pool - the miner only needs to clear the pools of its own node. to prevent other nodes from clearing pools.

Also:
Add extra- validation of to ensure txn's cannot be included, if it is already in the BC's history.

<!-- Transaction Miner Class -->
app/transaction-miner.js
This will hold a mine transaction function to properly add a block of a txn pool's data to the block.

<!-- Clear Transaction Pool -->
Add clear function in transaction-pool.js (and test function as well)

However, it may clear the transaction pool that hasn't been received by the blockchain yet

Thus, we need to implement a clearing function that checks if txn's exist in the chain and if they do, then they wipe them.

This function will be called Clear Blockchain Transactions in transaction-pool.js (also add test)

The Clear Blockchain function is what Peers will call when they accept the Blockchain to be replaced. It won't wipe away unaccounted txn's in the local pool

<!-- MINE TRANSACTION ENDPOINT -->
Add mins-transactions endpoint (GET request) to allow the requestor to mine a block of transactions.

1. It grabs the transactions
2. Makes a reward
3. Adds a block consistinng of those transactions
4. Broadcasts that block
5. Then clears the local app's transactions pool

However, the only transaction pool that clears is the one that the request was made to!

We need to ensure that all transaction pools are cleared of transactions that have been placed on to the blockchain

To do this, we call clearBlockchainTransactions method from the transaction pool once a blockchain has been successfully replaced.

<!-- clearBlockchainTransactions --> *as per above
Add a new parameter to the replaceChain function called 'onSuccess' in /blockchain/index.js

<!-- Blockchain Based Wallet Balances -->
We need to calculate accurate balances in wallets, based on the transactions in the blockchain

Initially, everyone will start with the same balance. 

Once a wallet starts receiving, it adds to balance and negates from the receiver

METHOD:
1. Scan the entire txn history from recent to Genesis block
2. If the wallet has made a txn, the balance is the Output of the wallet.
3. This txn is a record of official balance for the wallet

If the wallet is the RECIPIENT for 1 or more txn's then:
1. Wallet adds any Output amount SENT to their wallet after the txn
2. This output gets added to the overall balance

What if wallet conducts another txn?
1. The output will be the balance
2. Once the wallet makes a txn, it adds-up all the outputs to make its balance
3. But we cannot double count it!

<!-- Calculate Wallet Balance Function/Algorithm -->
1. Balance, at any time, can be calculated by summing all outputs you own.
2. This is done by checking EACH transaction in the blockchain history
3. If the wallet public key matches the output recipient, then add that amount to the blockchain balance.
4. Other functions added via test-driven development (complex methods that take into account most variables throughout project.)
/wallet/index.test.js
/wallet/index.js

<!-- Calculate a Balance BEFORE a transaction -->
Before it makes a transaction
1. Pass a chain array to the createTransaction method
wallet/index.test.js
wallet/index.js

<!-- Wallet Balance From Recent Transaction -->
Ensure an accurate balance.

Need to move away from using all outputs from all transactions to calculate the balance. This could add incorrect outputs, if they have previously been used.

The fix is to ensure the wallets only sum balances from outputs starting from the block that the wallet used for its recent transaction.

*If an address has already made a transaction, the balance should be based on the output of that address, within that transaction.

Only outputs from within that block and after, can be added to that balance.

wallet/index.test.js
wallet/index.js

calculateBalance method

<!-- Wallet Info Request - API -->
Accurate balances of wallet via API request that includes balance
main/index.js
walletinfo request

<!-- VALIDATE TRANSACTION BLOCKS -->
1. We already have isValidChain() Method. 
- Makes sure Genesis block is correct
- LastHash reference is correct
- The block difficulty matches the hash leading 0's
- The generated hash matches the hash

BUT we need to check the data field is real.
- A user could create a block with fraudulent data. 
- The hash can be valid in the block, even if the data is 'evil'
- As long as the data generates a correct hash in-line with the method above, that block will be deemed valid in the chain.

The solution is to run a method - validTransactionData()
- Checks if data is valid according to rules:

RULES
1. transaction inputs and amounts

Each transaction in the block must be correctly formatted - validTransaction(). Also checks if signature is valid. And mining rewards and their value are valid.

2. One mining reward per block

3. Valid input amounts according to the BC's history. 

4. Block must not have multiple identical transactions. No duplicates.

<!-- VALIDATE TRANSACTION DATA -->
*Add to the transaction class
/blockchain/index.test.js
/blockchain/index.js - validTransactionData

<!-- VALIDATE INPUT BALANCES -->
/blockchain/index.test.js
/blockchain/index.js
- invalid data - input (balance) is too high
- invalid data - block does not contain multiple, identical txn's

<!-- PREVENT DUPLICATE TRANSACTIONS IN BLOCKS -->
*The block should have a set of unique txn's when it comes in - without any duplicates
/blockchain/index.test.js
/blockchain/index.js - validTransactionData

* Create a const transactionSet()
Use this to determine if all txn's in the set are unique and if not, return false and an error.

<!-- VALIDATE TXN CHAIN -->
/blockchain/index.test.js
/blockchain/index.js - replaceChain
We want the chain to be replaced when all txn's are legit.

<!-- BACKEND SUMMARY -->
Created the core transaction miner class to capture how miners should add transactions to the blockchain.

Added the ability to grab valid transactions from the transaction pool.

Added a way to clear blockchain transactions to ensure that only unique transaction objects could be recorded.

Added a mining transactions endpoint to enable transaction mining through the API.

Cleared recorded transactions on a successful replacement of the blockchain.

Calculated the wallet balance based on the blockchain history.

Applied these wallet balances whenever conducting a new transaction.

Exposed the wallet information including the public key and balance through the API.

Validated transaction data incoming into the blockchain.

Validated incoming transaction input balances.

Prevented duplicate transactions from appearing in a block’s data.

Validated the entire transaction itself when accepting new user-contributed blockchains.

<!-- SUMMARY OF EACH FILE -->

1. blockchain/block.js

The Block class has a 'difficulty' & 'nonce' fields to determine how quickly new blocks can be created.

Genesis block has hard-coded values to ensure the chain can get going.

To create a block, we have a 'hash' method from util/crypto-hash and this method will take in any #  of inputs, sort them, stringify and track any changes and creates unique hash on this. This allows us to link blocks together in a blockchain class.

2. blockchain/index.js

Every block's last-hash value must be equal to the last block. This creates the links between each block.

There are 2 checks:

i. isValidChain - the last hash of each block must be valid. 
ii. The generated hash field, based on all the block fields must be in-line with a validatedHash

These 2 checks make sure a chain is valid. The data has not been tampered with. 

replaceChain:
- If incoming chain is longer than current stored blockchain array (and it is valid), then it must be replaced with the incoming one.
- All nodes will agreen on the longest one. The longer the BC, the stronger the system.
- The POW system helps prevent a 51% hack. 

3. index.js (API)
- GET req to get the blocks
- POST req to mine a new block to the BC
- Conduct txn's to the BC
- PubSub broadcasts (blocks being added etc.)

4. PubSub
For passing messages between servers
Has channels where:
- Subscribers listen for messages
- Publishers broadcast messages
BLOCKCHAIN and TRANSACTION channels.
*In main/index.js - we added Peers to generate peers. Allows multiple instances of the BC application.

5. wallet/index.js - Overall Cryptocurrency
Wallet class for users to interact with each other with the currency. 
Hold private and public keys. The public key is used as an address to receive the currency.
The private key is used to generate unique signaures in the txn's.

CalculateBalance method:
- The balance at any point in time is the output amount for that wallet at its most recent txn.
- In addition to any output amounts it received in the BC history, after that most recent txn.

6. wallet/transaction.js - Transactions
Exchange of currency between 2 wallets.,
Input field - official signature from senderWallet
Output field - values in the txn (recipient) and senderWallet balance after txn.

7. wallet/transaction-pool.js
Set txn's
Update them
Replace the entire txn map if it needs to

7. app/transaction-miner.js
Gets valid txn's from the pool
Has rewardTransaction method to reward miner in finding the validHash that consists of the valid txns
Adds block to the BC
Broadcasts the chain
Clears the local txn pool

8. blockchain/index.js

Validate txns - validTransactionData method
- No duplicates
- Valid shape overall - input sig's and outputMap should be formatted correctly.
- The MINING_REWARD value should be legit
- The input balances must be correct according to the BC history
- No duplicates of txn's in the block

*******************************************************************************

<!-- FRONTEND -->
React.js Blockchain Explorer

MVC Pattern - Models, Views, Controllers

Views - React.js
- Component structure
- Splits app into components that define own html structure
- Dynamic functionality
- State - each component maintains its own state. This is a data structure that is continuously updated as the user interacts with the web app. Based on how the state is updated, it determines what to display to the user. 

- Life-cycle methods: these fire-up at different phases of the application. LCM's allows the app to fire-up the code when a user enters, exits the app (or other times).

- Virtual DOM - in memory version of the app that React uses to update the presentation. This allows it to maintain user-side updates. 

<!-- Life Cycle Methods -->
Work around a component mounting to the DOM - Doc Obj Model
<App /> Component will mount through componentWillMount()

<!-- All files through to a browser? -->
Send all the Apps to one file through Bundlers to one JS file

Transpilers - allow one to code with modern JS features. It will sub a newer syntax that the browser can handle

Use PARCEL Bundler - fast, clean and quick config

***************************************************************************************

FRONTEND CRYPTOCURRENCY

<!-- Display INPUTS & OUTPUTS -->

1. From Field - Alias for the input
See who made the transaction

2. Outputs - 2 Fields
- Recipient/s
- Amount/s received in the txn

Real-time view of the Transaction Pool

<!-- Conduct Transaction -->
- Through the API/TRANSACT endpoint

<!-- Provide a way to mine transactions -->
- Add new block of txn's

<!-- RECAP -->
Created toggle-able transaction displays.

Built a reusable frontend transaction component.

Added routing for a multi-page application that renders different content based on the url.

Supported the conducting of transactions through a form.

Posted transactions through the fetch API.

Visualized the transaction pool - and added real-time polling capabilities.

Added a way to mine a block through the frontend.