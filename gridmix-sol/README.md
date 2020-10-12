Get a Senario file?
me: market is open, I am registered
me: need to ask for energy me need to offer energy etc


Instructions for Deployment
1. Compile & Deploy the token contract KWH on a testnet (not Remix's JSVM)
    * params:  eg: [],"0x05854cA140caB11e2f5AAb284c6AB5415f96E26B"

2. Deploy masterSLEC
    * params: (addr of governor, addr of token contract from step 1)
    eg: 0x05854cA140caB11e2f5AAb284c6AB5415f96E26B,0x61941babd6b0642cc7820ce81d90bbcd94614ac9


3. Open up the deployed instance.

4. Participants register as prosumers & consumers with the function **register**
        params:
        bool canBuy 
        bool canSell
        uint8 buyerSpecs
        uint8 sellerSpecs
        eg: true,true,"2","2"

4. The Governor opens a market with the function **openMarket**
    param: maxPrice - this is the the price of energy on the commercial market that this market cannot exceed. So input a number.

5. Initial postings to develop trading cohorts - although the cohort is really the list appropriate sellers (see below).
   * prosumers use the function **offerEnergy** 
        - currently the price param is not being used
        - energy - is to be the amount of kWh energy

   * consumers use the function **askForEnergy** with the example params:
        - currently the price param is not being used
        - energy - is to be the amount of kWh energy

5. Consumers then will see who is an appropriate buyer with the **view** function **findAppropriateSellers**
    * the input param for this function is the index of the array of energyStreams (of Asks) that this consumer has registered
        - eg: 0   (for the 1st Ask of a buyer)
   - The function **findAppropriateSellers** currenly matching on 
   - returns an array like: 
   0:
tuple(address,uint256)[]: matches 0x05854cA140caB11e2f5AAb284c6AB5415f96E26B,0

   - consumer will choose an offer in this array to bid on
   - this array contains a nested array that contains the seller's address and the index number of this offer in the array of their energyStreams for sale. [address of seller, their offer id]

6. A consumer then bids on a specific offer with the function **bidToOffer** with example params:
    - "0","0x05854cA140caB11e2f5AAb284c6AB5415f96E26B","0","12"
    - the seller's info is in the array that got returned above
    - the buyer's params are the id of the Ask and the price that you want to pay

 - 
8. Funds are sent from the **send** function in the token's contract ( or from metamask?) and are sent to the SLEC contract - the amount sent by the consumer - must match the amount of the bid -if this is the 1st bid to this offer or if it is a subsquent bid then it should be the amount that the bid was increased over this consumer's previous bid. The userdata is the hex encoded amound of the seller's address, and the consumer's bid ID and the seller's offer's ID.

7. The current highest bid is visible to other buyers in the cohort  -** which function?**  or erase!!! View offers - mapping autogenerated function

8. Market is closed by the grid

9. The highest bid wins with the function **acceptHighestBid** -> hit by prosumer

10. Losing Consumers get their funds back with the function **withdraw** -> hit by consumer

11. The prosumer at the given time turns on their energyStream and the consumer opens their energy input.

12. After the consumer had consumed, they tell the contract that their order has been fulfilled with **transactionCompleted**

13. Utility (DSO) can buy the remain energyOffers with the funciton **buyFromRemainingSellers**

14. Utility (DSO) can sell to the remain energyOffers with the funciton **sellToRemainingBuyers**