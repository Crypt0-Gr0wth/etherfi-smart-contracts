# Chapitre 8 — AuctionManager : l'enchere des operateurs de noeuds pour les slots de validateurs

`AuctionManager.sol` gere le marche par lequel les operateurs de noeuds (les entites qui font effectivement tourner l'infrastructure d'un validateur) obtiennent le droit d'etre associes a un nouveau validateur finance par le `LiquidityPool`. Deux seuils gouvernent ce marche : `whitelistBidAmount` (initialise a 0,001 ETH), un montant reduit reserve a des operateurs pre-approuves sur liste blanche, et `minBidAmount` (initialise a 0,01 ETH), le montant minimum pour un operateur non liste.

Ce systeme d'enchere inverse le modele economique habituel des pools de staking : plutot que le protocole choisisse et remunere ses operateurs, ce sont les operateurs qui misent pour le privilege d'etre selectionnes, le montant mise servant a la fois de signal de qualite (un operateur serieux mise plus pour un slot qu'il compte honorer) et de garantie financiere partiellement recuperable par le protocole en cas de mauvaise performance.

`updateSelectedBidInformation`, appelee depuis `StakingManager.createBeaconValidators` (chapitre 6) au moment ou une enchere est effectivement utilisee pour creer un validateur, marque l'enchere comme consommee et empeche sa reutilisation pour un autre validateur. `getBidOwner` permet de retrouver l'operateur associe a une enchere donnee, information reutilisee dans l'evenement `ValidatorRegistered` pour la compatibilite avec l'outillage existant.

[Chapitre suivant : EtherFiOracle, le comite de consensus hors-chaine](09-etherfioracle.md)
