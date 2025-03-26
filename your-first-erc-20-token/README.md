# Ethereum Smart Contracts Lab: Build Your First ERC-20 Token

##  Objective
By the end of this lab, you will understand:
- What Ethereum smart contracts are
- What the ERC-20 standard is
- How to write, deploy, and interact with your own ERC-20 token using Remix IDE
- The concepts of `approve()`, `transferFrom()`, and `allowance`

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
File name: `SCPDToken.sol`

### 3️⃣ Paste the Code Below

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SCPDToken {
    string public name = "SCPDCoin";
    string public symbol = "SCPD";
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
```

##  Deployment Instructions (in Remix)


1. On the left panel, click the **Solidity Compiler** tab (icon: `</>`)
   - Make sure the compiler version is `0.8.8`
   - Click the **Compile UNMToken.sol** button
5. Then click the **Deploy & Run Transactions** tab (Ethereum icon)
   - Under *Environment*, select: `Remix VM (Cancun)`
   - Under *Contract*, make sure `SCPDToken` is selected
   - Click the **Deploy** button
3. Scroll down to the **Deployed Contracts** section
   - Click the dropdown to expand your token contract
   - You’ll now see the list of functions you can call

### Examples to try in Remix:

- Use `balanceOf(address)` to check how many tokens each address has
- Use `transfer(address to, uint256 amount)` to send tokens to another account
- Use `approve(address spender, uint256 amount)` to give permission to spend tokens
- Use `allowance(address owner, address spender)` to see how many tokens a spender can use
- Use `transferFrom(address from, address to, uint256 amount)` to move tokens after approval

### Tip:

- You can switch accounts using the top-right dropdown in Remix (JavaScript VM has 10 test accounts preloaded)
- Try simulating a real-world use case:
  1. Account A approves Account B to spend 50 UNMT
  2. Switch to Account B and call `transferFrom(A, C, 20)`
  3. Check balances of A and C, and the updated allowance

### Deploy On Testnet (Ethereum Sepolia)

## Deploying to Ethereum Sepolia Testnet (Optional)

Want to try deploying your token on a real testnet instead of Remix’s simulator? Here's how to use **MetaMask + Remix + Sepolia testnet**.

---

### Prerequisites

1. Install [MetaMask](https://metamask.io/) browser extension (if not already installed)
2. Set up your MetaMask wallet (create or import an account)
3. Connect MetaMask to the **Sepolia testnet**

---

### Add Sepolia Network to MetaMask

If it's not already listed:

1. Open MetaMask
2. Click your **account icon → Settings → Networks**
3. Add a new network manually:

| Field              | Value                                |
|--------------------|----------------------------------------|
| Network Name       | Sepolia Testnet                        |
| RPC URL            | `https://rpc.sepolia.org`              |
| Chain ID           | `11155111`                             |
| Currency Symbol    | `ETH`                                  |
| Block Explorer URL | `https://sepolia.etherscan.io`         |


---

### Get Free Sepolia ETH Using Google Web3 Faucet

- Go to: **[https://cloud.google.com/application/web3/faucet/ethereum/sepolia](https://cloud.google.com/application/web3/faucet/ethereum/sepolia)**

