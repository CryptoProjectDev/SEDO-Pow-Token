# SEDO-Pow-Token
Sedo Coin
[Deployed on ROPSTEN Ethereum](https://ropsten.etherscan.io/address/0x3c3f4afc4ae44a5486dfd5cdc1712fada97fbea4)
[Deployed on MAINNET Ethereum](https://etherscan.io/address/0x0f00f1696218eaefa2d2330df3d6d1f94813b38f)


![SEDO_small](http://sedocoin.org/wp-content/uploads/2018/10/logo_blue_240.png)


This is intended to be the on of the first Merge-Mined token as a proof of concept for new merge mined tokens, and as the creation of a non-forgable, scarce, digital asset powered by the Ethereum blockchain.

the idea is that the miner uses https://github.com/0xbitcoin/mint-helper/blob/master/contracts/MintHelper.sol to call mint() and then mergeMint() in the same transaction

 
 #### An ERC20 token that is mined using PoW through a SmartContract 
  
  * 1.000.000 pre-mine 
  * No ICO
  * 50,000,000 tokens total
  * Difficulty target auto-adjusts with PoW hashrate
  * Rewards decrease as more tokens are minted 
  * Compatible with all services that support ERC20 tokens
  
     
 #### How does it work?
 
Typically, ERC20 tokens will grant all tokens to the owner or will have an ICO and demand that large amounts of Ether be sent to the owner.   Instead of granting tokens to the 'contract owner', all 0xbitcoin tokens are locked within the smart contract initially.  These tokens are dispensed, 25 at a time, by calling the function 'mint' and using Proof of Work, just like mining bitcoin.  Here is what that looks like: 


     function mint(uint256 nonce, bytes32 challenge_digest) public returns (bool success) {

       
        uint reward_amount = getMiningReward();

        
        bytes32 digest =  keccak256(challengeNumber, msg.sender, nonce );

         
        if (digest != challenge_digest) revert();

        //the digest must be smaller than the target
        if(uint256(digest) > miningTarget) revert();
     

         uint hashFound = rewardHashesFound[digest];
         rewardHashesFound[digest] = epochCount;
         if(hashFound != 0) revert();  //prevent the same answer from awarding twice

        balances[msg.sender] = balances[msg.sender].add(reward_amount);

        tokensMinted = tokensMinted.add(reward_amount);

        //set readonly diagnostics data
        lastRewardTo = msg.sender;
        lastRewardAmount = reward_amount;
        lastRewardEthBlockNumber = block.number;
        
        //start a new round of mining with a new 'challengeNumber'
         _startNewMiningEpoch();

          Mint(msg.sender, reward_amount, epochCount, challengeNumber );

       return true;

    }
 
 ----------
 
#### HOW TO TEST

Deployed on Ropsten: 
0x3c3f4afc4ae44a5486dfd5cdc1712fada97fbea4

npm install -g ethereumjs-testrpc  (https://github.com/ethereumjs/testrpc)
testrpc

truffle test
  

 
 
