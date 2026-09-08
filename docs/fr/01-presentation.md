# Chapitre 1 — Presentation d'ether.fi et de son architecture eETH/LiquidityPool

ether.fi est un protocole de liquid restaking : les utilisateurs deposent de l'ETH dans un `LiquidityPool` central en echange d'`eETH`, un token « rebasant » qui represente leur part du pool et croit automatiquement avec les recompenses de staking. Contrairement a un simple liquid staking, l'ETH depose sert egalement a faire tourner des validateurs dont les cles de retrait pointent vers des `EigenPod` EigenLayer, permettant un « restaking natif » directement au niveau du validateur plutot que via un token intermediaire.

L'architecture s'organise autour de quelques contrats cles : `LiquidityPool.sol` centralise la comptabilite ETH-en-attente et ETH-deja-deploye et emet les ordres de creation de validateurs ; `EETH.sol` est le token rebasant a parts (« shares ») emis en echange des depots ; `WeETH.sol` est sa version enveloppee a solde fixe, plus pratique pour l'integration DeFi ; `StakingManager.sol` deploie les contrats `EtherFiNode` (un par validateur) qui possedent chacun leur propre `EigenPod` ; `AuctionManager.sol` gere l'enchere par laquelle les operateurs de noeuds obtiennent le droit de faire tourner un validateur ; `EtherFiOracle.sol` et `EtherFiAdmin.sol` forment le pipeline de reporting qui fait remonter l'etat de la chaine beacon vers le `LiquidityPool`.

Ce parcours s'appuie sur le depot clone a la date d'ecriture, branche `master`. Fichiers centraux : `src/core/LiquidityPool.sol`, `src/core/EETH.sol`, `src/core/WeETH.sol`, `src/staking/StakingManager.sol`, `src/staking/EtherFiNode.sol`, `src/staking/AuctionManager.sol`, `src/oracle/EtherFiOracle.sol`, `src/oracle/EtherFiAdmin.sol`, `src/withdrawals/WithdrawRequestNFT.sol`, `src/governance/RoleRegistry.sol`.

Rien n'a ete installe, compile ni execute pour ecrire ces chapitres.

[Chapitre suivant : deposit et le calcul des shares eETH dans le LiquidityPool](02-liquiditypool-deposit.md)
