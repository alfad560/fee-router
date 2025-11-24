# CONTRACTS.md — FeeRouter szerződés

## Áttekintés
A **FeeRouter** ETH‑ot és ERC‑20 tokent fogad, **fee‑t számít bps-ben**, a fee‑t az admin tárcára küldi,
a maradékot pedig a címzett(ek)nek. Minden tranzakció eseményt lő (`Paid`, `PaidToken`).

## Interfész (példa)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

interface IERC20 {
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function transfer(address to, uint256 value) external returns (bool);
}

contract FeeRouter {
    uint256 public feeBps;          // pl. 200 = 2%
    address public feeReceiver;     // admin/multisig

    event Paid(address indexed from, address indexed to, uint256 amount, uint256 fee);
    event PaidToken(address indexed token, address indexed from, address indexed to, uint256 amount, uint256 fee);
    event FeeUpdated(uint256 oldFee, uint256 newFee);
    event FeeReceiverUpdated(address oldRecv, address newRecv);

    constructor(uint256 _feeBps, address _feeReceiver) {
        feeBps = _feeBps;
        feeReceiver = _feeReceiver;
    }

    receive() external payable {}

    function setFee(uint256 newFeeBps) external {
        require(msg.sender == feeReceiver, "only admin");
        emit FeeUpdated(feeBps, newFeeBps);
        feeBps = newFeeBps;
    }

    function setFeeReceiver(address newReceiver) external {
        require(msg.sender == feeReceiver, "only admin");
        emit FeeReceiverUpdated(feeReceiver, newReceiver);
        feeReceiver = newReceiver;
    }

    function pay(address payable to) external payable {
        require(msg.value > 0, "no value");
        uint256 fee = (msg.value * feeBps) / 10_000;
        (bool s1,) = payable(feeReceiver).call{value: fee}("");
        require(s1, "fee xfer failed");
        (bool s2,) = to.call{value: msg.value - fee}("");
        require(s2, "pay xfer failed");
        emit Paid(msg.sender, to, msg.value, fee);
    }

    function payToken(address token, address to, uint256 amount) external {
        require(amount > 0, "no amount");
        uint256 fee = (amount * feeBps) / 10_000;
        require(IERC20(token).transferFrom(msg.sender, feeReceiver, fee), "fee tf fail");
        require(IERC20(token).transferFrom(msg.sender, to, amount - fee), "pay tf fail");
        emit PaidToken(token, msg.sender, to, amount, fee);
    }
}
```

## Események
- `Paid(from, to, amount, fee)` — ETH
- `PaidToken(token, from, to, amount, fee)` — ERC‑20

## Admin ajánlások
- **Gnosis Safe** mint `feeReceiver`
- **Etherscan verify** + **Tenderly** monitoring
- **Eseményekre** épülő Subgraph a metrikákhoz
