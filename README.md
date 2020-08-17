# lottery-system
Build a lottery system using Solidity &amp; deploy a smart contract

# data types in Solidity

Boolean: A boolean is declared with the keyword bool that has two possible values – true and false.

          bool a = true;
          bool b = !a;

          a == b -> false
          a != b -> true
          a || b -> true
          a && b -> false
Integers: An integer is declared with the keyword int or uint (unsigned integer).

          int a = 2;
          int b = 4;

          a + b -> 6
          a == b -> false
          a != b -> true
          a > b -> false
Addresses: An address is declared with the keyword address. An address type defines where you can find a contract or an account on the blockchain. When you interact with a contract, you can know who interacts with the contracts by accessing “msg.sender”.

          address owner = msg.sender;
          address contractAddress = this;

          owner == 0x760485F58A01AsE94e306A57260OPEA2fFDa5054 -> false if you are not 0x760485F58A01AsE94e306A57260OPEA2fFDa5054
Mapping: A mapping binds a key to a value and is declared by specifying type of the key and value. This is how money belonging to an address is stored:

          mapping(address => uint) usersBet;
          usersBet[msg.sender] = 10;
Enums: An enum is declared with the keyword enum. It’s used to create a user-defined type in Solidity. They are explicitly convertible to and from all integer types but implicit conversion is not allowed. The explicit conversions check the value ranges during runtime and a failure causes an exception.

          ActionChoices choice;

          ActionChoices constant defaultChoice = ActionChoices.GoStraight;

          function setGoStraight() 
          {
          choice = ActionChoices.GoStraight;
          }

          function getChoice() returns (ActionChoices) 
          {
          return choice;
          }

          function getDefaultChoice() returns (uint) 
          {
          return uint(defaultChoice);
          }

# Decentralized Lottery System
This lottery system shall be fabricated in such a way that multiple users are able to send money and participate in a lottery draw. The more money the user sends, higher the chance of him winning all the funds. When the owner of the lottery decides to close the lottery, a winner will be chosen and the total bets will be transferred to the winner.

We start by defining a contract “Lottery” where we store 3 variables and the address of the owner:

          mapping(address => uint) usersBet;
          mapping(uint => address) users;
          uint nbUsers = 0;
          uint totalBets = 0;

address owner;
Next, we define a constructor(same name as that of the contract) to initialize the address of owner to store address of the person who initially deployed the contract. This “owner of the contract” is the owner of our lottery system.

          function Lottery() {
          owner = msg.sender;
          }
To store each players bet, we use a mapping. It is not possible to iterate through the values of a mapping if the indices are not linear. So, to iterate through all the bets, we need to separately store the number of people who placed bets(nbUsers) and the list of the address of the players in another mapping(users).

We then make the “Bet” function. Just like normal accounts, smart contracts can contain Ethereum. The amount of Ether sent is stored in “msg.value”. So, when “bet” is called, we first check if some Ethereum has already been sent(msg.value > 0) . Then, we store the amount sent in the “usersBet ” mapping. If the bet from this user is equal to 0, we increment “nbUsers ” and store the address of the player. This helps us iterate through all the players when the lottery ends.

          function Bet() public payable  {
             if (msg.value > 0) {
                if (usersBet[msg.sender] == 0) { // Is it a new player
                   users[nbUsers] = msg.sender;
                   nbUsers += 1;
                }
              usersBet[msg.sender] += msg.value;
              totalBets += msg.value;
             }
          }
Now, we have to pick the winner.  This is done by the “EndLottery()” function which is accessible only to the owner of the lottery. We pick a random number between 0 and the total amount of Ethereum played, iterate through the players and check who won. When a player is found, the contract is destroyed by passing the winner’s address as an argument to “selfdestruct()”. This winner receives all the Ethereum that is stored in the contract.

          function EndLottery() public {
             if (msg.sender == owner) {
                 uint sum = 0;
                 uint winningNumber = uint(block.blockhash(block.number-1)) % totalBets;
                 for (uint i=0; i < nbUsers; i++) {
                    sum += usersBet[users[i]];
                    if (sum >= winningNumber) {
                        selfdestruct(users[i]);
                        return;
                    }
                 }
             }
