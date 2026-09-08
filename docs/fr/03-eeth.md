# Chapitre 3 — EETH : le token rebasant a parts et son rate limiting sur mint/burn

`EETH.sol` implemente le meme motif que le stETH de Lido : au lieu de stocker un solde ERC-20 classique par utilisateur, le contrat stocke un nombre de `shares` par adresse et un `totalShares` global. Le solde ERC-20 apparent (`balanceOf`) est calcule a la volee en interrogeant `LiquidityPool.amountForShare` — c'est ce qui fait que le solde eETH d'un detenteur augmente automatiquement a chaque rebase du pool, sans qu'aucune transaction de transfert ne soit necessaire pour distribuer les recompenses.

Contrairement a un ERC-20 standard, les transferts d'eETH ne bougent donc pas un solde en tokens mais un nombre de shares equivalent au montant demande au taux courant, via `TransferShares` (evenement dedie en plus du `Transfer` ERC-20 standard). Le contrat implemente egalement `EIP-2612` (`permit`), permettant d'approuver une allocation par signature hors-chaine plutot que par une transaction `approve` separee.

Une protection specifique a ce depot est le rate limiting sur les operations qui changent l'offre totale : `EETH_MINT_LIMIT_ID` et `EETH_BURN_LIMIT_ID` definissent chacun un « bucket » (via `IEtherFiRateLimiter`) consomme a chaque mint ou burn, plafonnant la quantite d'eETH pouvant etre creee ou detruite dans une fenetre de temps donnee. Le commentaire du code precise explicitement que les transferts, eux, ne sont pas limites puisqu'ils ne changent pas l'offre totale — seule la creation ou la destruction de supply est consideree comme une surface a risque justifiant un frein automatique, par exemple contre un bug ou une compromission d'un chemin de mint.

[Chapitre suivant : WeETH, l enveloppe a solde fixe pour l integration DeFi](04-weeth.md)
