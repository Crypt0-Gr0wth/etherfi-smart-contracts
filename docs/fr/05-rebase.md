# Chapitre 5 — rebase : la distribution des recompenses plafonnee a 25 points de base

`rebase` (`contracts/core/LiquidityPool.sol`), appelable uniquement par `etherFiAdminContract`, est le point d'entree unique par lequel les recompenses de staking et de restaking observees hors-chaine sont integrees a la comptabilite du pool. Il recoit `_accruedRewards` (positif ou negatif) et `_protocolFees`, ajuste `totalValueOutOfLp` en consequence, puis mint les frais protocolaires au `feeRecipient` via `depositToRecipient`.

Le code impose un plafond strict sur toute augmentation positive : `MAX_POSITIVE_REBASE_BPS`, fixe a 25 points de base (0,25 %) de la valeur totale du pool par rebase. Le commentaire du code justifie ce choix par un ordre de grandeur concret — 25 points de base correspondent approximativement a un mois d'accumulation de recompenses a 3 % de rendement annuel — et precise que cette constante n'est deliberement pas configurable par la gouvernance : c'est un invariant fixe destine a limiter les degats d'un rebase buggue ou d'un appelant compromis au niveau meme du calcul du taux de change des shares, independamment des controles effectues cote oracle.

Le code prend soin de ne verifier ce plafond que dans la branche positive (`_accruedRewards > 0`) : reinterpreter un `int128` negatif comme `uint128` inverserait le bit de signe et produirait une valeur enorme qui declencherait le plafond a tort. Le plafond negatif (perte de valeur, par exemple en cas de slashing) est intentionnellement gere plus strictement du cote de l'oracle (`EtherFiAdmin`, chapitre 10) plutot que redondant ici.

[Chapitre suivant : StakingManager et la creation des validateurs via EtherFiNode](06-staking-manager.md)
