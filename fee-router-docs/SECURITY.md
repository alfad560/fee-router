# SECURITY.md — Biztonsági irányelvek

- Admin/feeReceiver legyen **multisig** (Safe)
- **Etherscan verify** minden deploy után
- **Tenderly** / **Etherscan Alerts** a kritikus eseményekre
- Privát kulcsok **SOHA** ne kerüljenek repo-ba
- CI/CD secretek: `ETHERSCAN_API_KEY`, `ALCHEMY_API_KEY`, `TENDERLY_ACCESS_KEY`, `THEGRAPH_TOKEN`
- Fejlesztés: `pragma solidity ^0.8.20;` (beépített overflow védelmek)
