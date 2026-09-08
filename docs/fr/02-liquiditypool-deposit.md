# Chapitre 2 — deposit et le calcul des shares eETH dans le LiquidityPool

`LiquidityPool.sol` distingue deux compteurs de comptabilite globale : `totalValueInLp` (l'ETH physiquement present dans le contrat, disponible pour des retraits immediats) et `totalValueOutOfLp` (l'ETH deploye ailleurs — dans des validateurs beacon-chain ou verrouille pour des retraits en cours de finalisation). Leur somme, retournee par `getTotalPooledEther()`, represente la valeur totale du pool a un instant donne.

`deposit` existe en plusieurs variantes selon l'appelant : la version publique `deposit(address _referral)` sert au depot direct d'un utilisateur, `depositToRecipient` est reservee au contrat `Liquifier` ou a `etherFiAdminContract` (par exemple pour reinjecter des frais protocolaires), et `deposit(address _user, address _referral)` est reservee au `membershipManager` du flux « ether.fan ». Chacune appelle en interne `_deposit`, qui frappe des shares eETH proportionnelles au montant depose relativement au `getTotalPooledEther()` courant.

`sharesForAmount` calcule ce nombre de shares par la formule `amount * totalShares / totalPooledEther`, arrondie vers le bas (`Math.Rounding.Down`) pour les depots — un arrondi qui favorise legerement le pool plutot que le deposant. `amountForShare` effectue l'operation inverse pour convertir des shares en montant ETH courant. Une variante `sharesForWithdrawalAmount`, utilisee lors des demandes de retrait, arrondit au contraire vers le haut (`Math.Rounding.Up`), garantissant que les erreurs d'arrondi favorisent toujours le protocole et non l'utilisateur qui retire.

[Chapitre suivant : EETH, le token rebasant a parts et son rate limiting](03-eeth.md)
