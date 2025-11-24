# INTEGRATION_GUIDE.md — Integrációs útmutató

## 1) On‑chain integráció (Solidity)
```solidity
interface IFeeRouter {
    function pay(address token, address receiver, uint256 amount) external payable;
    function pay(address payable to) external payable; // ETH rövid hívás
    function payToken(address token, address to, uint256 amount) external;
}
```

Példa ETH fizetésre:
```solidity
IFeeRouter(router).pay{value: msg.value}(address(0), receiver, msg.value);
```

Példa token fizetésre:
```solidity
IERC20(token).approve(router, amount);
IFeeRouter(router).payToken(token, receiver, amount);
```

## 2) Frontend (ethers.js)
```js
const router = new ethers.Contract(routerAddress, abi, signer);
const tx = await router.pay(receiver, { value: amount });
await tx.wait();
```

## 3) „Pay” gomb HTML-ben
```html
<button id="pay">Pay with FeeRouter</button>
<script>
document.getElementById('pay').onclick = async () => {
  const tx = await router.pay(receiver, { value });
  await tx.wait();
  alert('Payment successful');
};
</script>
```

## 4) Subgraph lekérdezés (példa)
```graphql
{
  paids(first: 20, orderBy: timestamp, orderDirection: desc) {
    from
    to
    amount
    fee
  }
}
```
