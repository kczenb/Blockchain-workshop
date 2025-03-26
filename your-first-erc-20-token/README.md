# Ethereum Smart Contracts Lab: Build Your First ERC-20 Token

##  Objective
By the end of this 1-hour lab, you'll understand:
- What Ethereum smart contracts are
- What the ERC-20 standard is
- How to write, deploy, and interact with your own ERC-20 token using Remix IDE
- The concepts of `approve()`, `transferFrom()`, and `allowance`
- Real-world use cases like subscription services

---

## Tools Required
- A web browser
- [Remix IDE](https://remix.ethereum.org)
- (Optional) MetaMask browser extension for extended activities

---


## Why?
**If you could create your own token, what would it be called, and what would it be used for?**  

---

##  Write Your Own ERC-20 Token

###  Open Remix
Visit [https://remix.ethereum.org](https://remix.ethereum.org)

###  Create a New File
File name: `UNMToken.sol`

### 3️⃣ Paste the Code Below

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract UNMToken {
    string public name = "UNMCoin";
    string public symbol = "UNMT";
    uint8 public decimals = 18;
    uint256 public totalSupply;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        totalSupply = 1000 * 10 ** uint256(decimals);
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value, "Insufficient balance.");
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[_from] >= _value, "Insufficient balance.");
        require(allowance[_from][msg.sender] >= _value, "Not allowed.");

        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;

        emit Transfer(_from, _to, _value);
        return true;
    }
}

