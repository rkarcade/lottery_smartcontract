# lottery_smartcontract
A smart contract that chooses a winner based on whoever participates, and the winning pool goes to randomized winner. 

1. The lottery starts by accepting ETH transactions. Anyone having an Ethereum wallet can
send a fixed amount of 0.1 ETH to the contract’s address.
2. The players send ETH directly to the contract address and their Ethereum address is
registered. A user can send more transactions having more chances to win.
3. There is a manager, the account that deploys and controls the contract.
4. At some point, if there are at least 3 playesrs, he can pick a random winner from the
players list. Only the manager is allowed to see the contract balance and to randomly
select the winner.
5. The contract will transfer the entire balance to the winner’s address and the lottery is
reset and ready for the next round

Design structure:

1) SET THE STATE VARIABLE AND CONSTRUCTOR 

declaring a dynamic array of type address and the elements of the array will be the player's addresses
address payable[] public players; The array is public so it could be easily accessed from
external applications. Anyone will be able to see the contents of the array. one of the addresses in the array will be the winner.

2 types of addresses: payable and non-payable.
The idea behind this distinction is that only a payable address is an address you can send Ether to,
while a plain address cannot have Ether sent to.

lottery will also have a manager, which is the externally owned account that deploys the contract
starts the lottery, picks the winner, and resets it for the next round.

contract's constructor that will be called only once at contract creation and will set the
manager to the address of the account that deploys the contract.

2) ENTER THE LOTTERY

to enter the lottery, the user will send 0.1 eth to enter the lottery. the user's address will be added to the dynamic array and the sent Eth will be addeded to the contract balance.
enter a recriver function to be able to receive eth.in Solidity 0.6. 
The function is run automatically when the contract receives ETH from EOAs.


3) VALIDATION 

everyone should send the same amount which is 0.1 eth. but if user wants more chances, he can send the amount to the lottery contract.
this would added in the receive functionality


CONDITION 1 : REQUIRE THAT AMOUNT IS AT 0.1 ETHER
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
If the value sent to the contract is not equal to 0.1 Ether an exception is thrown and the transaction will be reverted to the initial state. Note that the transaction will consume all the gas till the require function.


CONDTION 2: only manager CAN SEE THE balance of the CONTRACT:
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>


so if another account, not the manager's calls the getBalance function, they cant read the balance.

Testing out the contract:
deploy the contract, then use the first address.

manager ddress: 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4

Now , submit a 3333 wei to the contract, an error would appear. but if you submit 100000000000000000, then it goes through which is equivalent to 0.1 eth.

so now to test the second condition, 0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2 tries to view the balance, and but they couldnt, results in an error.

4) HOW TO SELECT WINNER
calculate the random index of the dynamic array and the winner be selected

create our a function called random that will return a very big pseudorandom integer.

keccak256 hash algorithm. - This function takes a single argument of type bytes and since I want my random number to be computed
based on more random values, I’ll call another function called abi.encodePacked()

So abi.encodePacked() ; And the function will perform packed encoding of the given arguments and return a variable of type bytes.
and the function's arguments are: blocked.difficulty, block.timestamp, players

Block.difficulty is the proof of work difficulty of the current block; block.timestamp is the Unix timestamp of the current block and players.length is the number of players in the array.
The blockchain is constantly growing, so all of these values will be random for each call and we can assume that this will result in a random number just good for our lottery.

5) DEPLOYMENT 
using my metamask account 1 , which is consider the manager account.
select injected web3

deploy it and the metamask pop up, click confirm and then go straight into etherscan.io and you should see the contract trnx number.

0xbc02e09483a5ca511ecd84d547bfc3cdf4448d96

Open MEW and send 0.1eth to the contract as a participant and as well the two account i have on metmask to deploy 0.1 eth to the same contract account \
0xbc02e09483a5ca511ecd84d547bfc3cdf4448d96

once that's done, go back as the manager account which is acc1 on metamask to check balance and pick winner. if you try to use a non manager account, it will fail.
go on to etherscan, adn click on contract address, you should be able to see the list of trnx and including the lastest function that was used pickwinner

go to the winner's address, and click internal trx see tht the eth was transfered.
if you encountere error trying to enter the lottery , just increase the gas limit.



