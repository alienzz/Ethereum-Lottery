
I built this as a way to learn Solidity. I've been quite obsessed with Ethereum and blockchain tech in general lately. I decided to make this simple contract for educational purposes. It is currently running on the Ropsten network and can be found [here](https://ropsten.etherscan.io/address/0x959aFc9B05c81afaAD7C930203296ECa43e802Ec).

I chose the Ropsten because I wanted a way to keep track of the outgoing transactions. The Rinkeby block explorer did not expose internal transactions and thats how the payout function works. I figured it'd be more transparent if there was a way to at least see who the payment was going to so Ropsten it is!

### How the game works
- A player sends a transaction of 0.1 to the address (currently on Ropsten 0x959aFc9B05c81afaAD7C930203296ECa43e802Ec). Any other amount will ```revert()```
- After 11 players have joined, the contract will 'randomly' select one of the eleven and send them 1 ETH.
- The remaining 0.1 ETH is currently sent to the contract owner. If this goes live, I'll probably set this to an external address such as a charity or what not.


### How the random function works

```js
  function random(uint upper) internal returns (uint) {
    seed = uint(keccak256(keccak256(playerPool[playerPool.length -1], seed), now));
    return seed % upper;
  }
```
This generates a number between 0 and the size of the player array (as of now, always set to 11). Due to the deterministic nature of the blockchain, it's very hard (or expensive) to get any kind of randomized outcome. The downside is people being able to determine the winning index before hand. To compensate for this, I used the 11th player's address (and current block time) as part of the hash since the payout function wins as soon as the 11th player buys in. On the front end, I decided not to display how many contestants are currently entered to prevent someone from gaming it as the 11th entry. Also, the prize is so low that miners would be not be interested in throwing away blocks to influence it in their favor.

### notes
This is just for education purposes. If you'd like to test, you can check out the current site [here](https://evening-everglades-76899.herokuapp.com/) however this is ONLY on the ROPSTEN NETWORK. Please do not send real ether here as it will be lost. This was only a fun little way for me to teach myself solidity. 
