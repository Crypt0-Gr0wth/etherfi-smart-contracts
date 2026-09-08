# Chapitre 10 — EtherFiAdmin : l'execution des rapports valides et le declenchement du rebase

`EtherFiAdmin.sol` est le contrat qui fait le pont entre un rapport d'oracle ayant atteint le consensus (chapitre 9) et les effets concrets sur le `LiquidityPool`. `executeTasks` recoit le `OracleReport` valide et en derive plusieurs actions : la plus visible est l'appel a `liquidityPool.rebase(_report.accruedRewards, _report.protocolFees)`, qui declenche exactement le mecanisme de distribution de recompenses plafonnee decrit au chapitre 5.

En centralisant l'execution des rapports dans `EtherFiAdmin` plutot que de laisser `EtherFiOracle` appeler directement le `LiquidityPool`, le protocole separe cleanement la responsabilite de constater un consensus (EtherFiOracle) de celle de decider quelles actions concretes en decoulent et sous quelles conditions supplementaires (EtherFiAdmin) — une couche ou peuvent se loger des verifications additionnelles specifiques a chaque type de tache (rebase, validation de validateurs, gestion des retraits) sans complexifier le contrat d'oracle lui-meme.

C'est egalement `EtherFiAdmin`, en tant que seul appelant autorise de `rebase`, qui porte la responsabilite du plafond negatif sur les rebases mentionne au chapitre 5 : le code de `LiquidityPool.rebase` indique explicitement que le cas negatif est verifie « plus strictement » du cote oracle, c'est-a-dire dans la logique de validation des rapports d'`EtherFiAdmin` plutot que d'etre reverifie dans `LiquidityPool` lui-meme.

[Chapitre suivant : WithdrawRequestNFT, la file d attente de retrait sous forme de NFT](11-withdrawrequestnft.md)
