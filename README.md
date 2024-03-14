# GCC_CTF2k24_web3


**First Challenge** 

2000 years from now, the earth has seen many wars, and the surface of the earth was forever damaged. Humans had to develop floating cities, the first one was Synthatsu, made by what was left of the Japanese history. Mixing futuristic building with a hint of traditional looks. This town was well known for its close combat weapons. One of the most prized katana maker works in that town. But just yesterday, some thugs named Beyond robbed his shop, and took some of his work!

**Goal:** Will you be able to get them for yourself?

Contracts 

Challenge.sol

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

import "./KatanaSale.sol";

contract Challenge{
    KatanaSale public katanaSale;
    address constant public PLAYER = 0xCaffE305b3Cc9A39028393D3F338f2a70966Cb85;

    constructor () payable {
        katanaSale = new KatanaSale(10 ether, 100);
    }

    function isSolved() public view returns(bool){
        if(katanaSale.balanceOf(PLAYER) >= 60){
            return true;
        }
        return false;
    }

}
```

KatanaSale.sol
```

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract KatanaSale {
    
    address public beyond;
    uint256 public katanaPrice;
    uint256 public katanaSold;
    string public name = "one-katana";
    string public symbol = "KTN";
    uint256 public totalSupply;
    mapping(address => uint256) public balanceOf;

    event Sold(address buyer, uint256 amount);

    constructor(uint256 _katanaPrice, uint256 _totalSupply) {
        beyond = msg.sender;
        katanaPrice = _katanaPrice;
        totalSupply = _totalSupply;
    }

    function transfer(address to, uint256 value) external {
        require(balanceOf[msg.sender] >= value, "Insufficient balance");
        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;
    }

    function becomeBeyond(string memory passPhrase) external {
        require (keccak256(abi.encode(passPhrase)) == keccak256(abi.encode("I will check out @BeyondBZH and @gcc_ensibs on X")));
        beyond = msg.sender;
    }

    function buyKatana(uint256 _numberOfKatana) external payable {
        require(msg.value == _numberOfKatana * katanaPrice, "Incorrect Ether value");
        katanaSold += _numberOfKatana;
        balanceOf[msg.sender] += _numberOfKatana;
        emit Sold(msg.sender, _numberOfKatana);
    }

    function endSale() external {
        require(msg.sender == beyond, "Only a true Beyond can end the sale");
        balanceOf[msg.sender] += totalSupply - katanaSold;
    }
}
```
**Solution:**

To slove this challenge we need to make the balance of the player greater than or equal to 60, for that follow the steps

1.When we Call the becomeBeyond function with the arguement "I will check out @BeyondBZH and @gcc_ensibs on X", then we became the beyond.

2.Call the endSale function and  we got the totalSupply since we don't call the buyKatana function.


**Note:** Used remix to solve this challenge

Solution Contract


```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract KatanaSale {
    
    address public beyond;
    uint256 public katanaPrice;
    uint256 public katanaSold;
    string public name = "one-katana";
    string public symbol = "KTN";
    uint256 public totalSupply;
    mapping(address => uint256) public balanceOf;

    event Sold(address buyer, uint256 amount);

    constructor(uint256 _katanaPrice, uint256 _totalSupply) {
        beyond = msg.sender;
        katanaPrice = _katanaPrice;
        totalSupply = _totalSupply;
    }

    function transfer(address to, uint256 value) external {
        require(balanceOf[msg.sender] >= value, "Insufficient balance");
        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;
    }

    function becomeBeyond(string memory passPhrase) external {
        require (keccak256(abi.encode(passPhrase)) == keccak256(abi.encode("I will check out @BeyondBZH and @gcc_ensibs on X")));
        beyond = msg.sender;
    }

    function buyKatana(uint256 _numberOfKatana) external payable {
        require(msg.value == _numberOfKatana * katanaPrice, "Incorrect Ether value");
        katanaSold += _numberOfKatana;
        balanceOf[msg.sender] += _numberOfKatana;
        emit Sold(msg.sender, _numberOfKatana);
    }

    function endSale() external {
        require(msg.sender == beyond, "Only a true Beyond can end the sale");
        balanceOf[msg.sender] += totalSupply - katanaSold;
    }
}

contract Challenge{
    KatanaSale public katanaSale;
    address constant public PLAYER = 0xCaffE305b3Cc9A39028393D3F338f2a70966Cb85;

    constructor () payable {
        katanaSale = new KatanaSale(10 ether, 100);
    }

    function isSolved() public view returns(bool){
        if(katanaSale.balanceOf(PLAYER) >= 60){
            return true;
        }
        return false;
    }

}
```

