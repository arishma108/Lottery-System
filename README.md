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
