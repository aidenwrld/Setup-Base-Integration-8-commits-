# Setup-Base-Integration-8-commits-
Setup &amp; Base Integration (8 commits)
Hardhat Config (Base setup)
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.20",
  networks: {
    baseSepolia: {
      url: process.env.BASE_RPC,
      accounts: [process.env.PRIVATE_KEY],
    },
  },
};

ERC20 Test Token
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract BaseToken is ERC20 {
    constructor() ERC20("BaseToken", "BTK") {
        _mint(msg.sender, 1000000 * 10 ** decimals());
    }
}

🏦 Staking Contract
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Staking {
    mapping(address => uint256) public balance;
    mapping(address => uint256) public reward;

    function stake() external payable {
        balance[msg.sender] += msg.value;
        reward[msg.sender] += msg.value / 10;
    }

    function withdraw() external {
        uint256 amount = balance[msg.sender] + reward[msg.sender];
        balance[msg.sender] = 0;
        reward[msg.sender] = 0;
        payable(msg.sender).transfer(amount);
    }
}

Deploy Script
async function main() {
  const Contract = await ethers.getContractFactory("Staking");
  const contract = await Contract.deploy();
  await contract.deployed();

  console.log("Deployed to:", contract.address);
}

main();

wagmi Config (Base chain)
import { createConfig } from "wagmi";
import { baseSepolia } from "wagmi/chains";

export const config = createConfig({
  chains: [baseSepolia],
});

Wallet Connect Button
import { useAccount, useConnect } from "wagmi";

export default function Connect() {
  const { address } = useAccount();
  const { connect, connectors } = useConnect();

  return (
    <div>
      {address ? (
        <p>{address}</p>
      ) : (
        connectors.map((c) => (
          <button key={c.id} onClick={() => connect({ connector: c })}>
            Connect
          </button>
        ))
      )}
    </div>
  );
}

Fetch Token Price (CoinGecko)
export async function getPrice() {
  const res = await fetch(
    "https://api.coingecko.com/api/v3/simple/price?ids=ethereum&vs_currencies=usd"
  );
  return res.json();
}

Suggested Repo Structure
base-dapp/
│── contracts/
│   ├── Staking.sol
│   └── Token.sol
│
│── scripts/
│   └── deploy.js
│
│── test/
│   └── staking.test.js
│
│── frontend/
│   ├── pages/
│   ├── components/
│   └── wagmi.js
│
│── .env
│── hardhat.config.js
│── README.md

