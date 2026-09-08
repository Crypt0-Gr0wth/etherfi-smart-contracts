# Chapitre 11 — WithdrawRequestNFT : la file d'attente de retrait sous forme de NFT

Un retrait d'eETH n'est pas instantane des lors que l'ETH correspondant est deploye dans des validateurs actifs plutot que disponible en liquidite immediate dans le pool. `WithdrawRequestNFT.sol` materialise chaque demande de retrait en cours sous la forme d'un NFT transferable : `requestWithdraw`, appelable uniquement par le `LiquidityPool`, enregistre le montant d'eETH et le nombre de shares au moment de la demande, et mint un NFT au demandeur representant sa creance.

Le fait que la demande soit un NFT plutot qu'une simple entree de mapping presente un interet pratique : la creance de retrait devient elle-meme transferable et negociable sur un marche secondaire avant meme sa finalisation, un utilisateur presse pouvant vendre son NFT de retrait a decote plutot que d'attendre la finalisation complete.

`finalizeRequests` marque une ou plusieurs demandes comme finalisees des que l'ETH necessaire a leur reglement a ete rendu disponible (via `LiquidityPool.addEthAmountLockedForWithdrawal`, chapitre 2), verifiable ensuite via `isFinalized`. Une fois finalisee, `claimWithdraw` permet au detenteur du NFT de bruler celui-ci contre l'ETH du, le contrat s'appuyant sur le taux fige au moment de la finalisation (et non le taux courant) pour garantir que la valeur promise ne varie plus une fois la demande traitee.

[Chapitre suivant : RoleRegistry, la gouvernance par roles granulaires](12-roleregistry.md)
