# 🌐 Fee Router — On‑Chain Payment Splitter (ETH & ERC‑20)

**Fee Router** egy okosszerződés‑alapú bevétel‑router: az **ETH** és **ERC‑20 token** kifizetéseket fogadja,
automatikusan **fee‑t von le (bps)** és **továbbítja** a maradékot a címzetteknek. Átlátható, auditálható,
frontendhez kész `Pay()` hívással és Subgraph metrikákkal.

---

## 🚀 Fő funkciók
- ✅ **ETH + ERC20** támogatás
- 🔁 **Automatikus fee routing** (pl. 2% → admin wallet)
- 🤝 **Partner split** több címzettel
- 🧾 **Események**: `Paid`, `PaidToken` – Subgraph követéshez
- 🧰 **Frontendhez kész** `Pay()` gomb/függvény példák
- 📈 **Dashboard** (Subgraph / Dune)

---

## ⚙️ Hálózatok & Szerződések
- **Network:** Ethereum Mainnet / Sepolia (példa)
- **Contract name:** `FeeRouter`
- **Solidity:** ^0.8.20
- **Etherscan verify:** ajánlott (link helye: _TODO_)

Részletek és ABI: lásd [`CONTRACTS.md`](./CONTRACTS.md).

---

## 💻 Integráció (rövid)
```solidity
IFeeRouter(router).pay{value: msg.value}(token, receiver, amount);
```
Teljes útmutató: [`INTEGRATION_GUIDE.md`](./INTEGRATION_GUIDE.md)

---

## 📊 Napi bevétel képlet
```
napi_fee = tranzakciók_száma × átlagos_bruttó_összeg × (fee_bps / 10_000)
példa: 200 × 0.02 ETH × 2% = 0.08 ETH/nap
```

---

## 🌍 Globális láthatóság (listázás)
- DappRadar, DefiLlama, ETHGlobal, Gitcoin, ThirdWeb, CMC Community
- Lépések: [`GLOBAL_LISTING.md`](./GLOBAL_LISTING.md)

---

## 🔐 Biztonság
- Multisig admin (Safe) ajánlott
- Etherscan verifikáció
- Tenderly monitor / Etherscan Alerts
- Lásd [`SECURITY.md`](./SECURITY.md)

---

## 📞 Kapcsolat (példa)
- Email: contact@feerouter.xyz
- X/Twitter: @feerouter
- Web: https://feerouter.xyz

---

## 🪪 Licenc
MIT — lásd [`LICENSE`](./LICENSE)
