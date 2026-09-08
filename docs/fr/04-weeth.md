# Chapitre 4 — WeETH : l'enveloppe a solde fixe pour l'integration DeFi

De nombreux protocoles DeFi (AMM, marches de pret) supposent qu'un solde de token reste constant tant qu'aucun transfert n'a lieu, une hypothese incompatible avec le solde rebasant d'eETH qui augmente silencieusement a chaque rebase. `WeETH.sol` resout ce probleme en proposant une enveloppe a solde fixe, exactement comme le wstETH de Lido enveloppe le stETH.

`wrap` convertit un montant d'eETH en weETH en calculant l'equivalent en shares au taux courant (`getWeETHByeETH`) et en frappant ce nombre exact de weETH ; `unwrap` effectue l'operation inverse. Le solde weETH d'un utilisateur ne change donc jamais tout seul : c'est le taux de conversion `getEETHByWeETH` qui augmente avec le temps, refletant les recompenses accumulees, tandis que le nombre de weETH detenus reste stable jusqu'au prochain wrap ou unwrap explicite — exactement le comportement attendu par la plupart des integrations DeFi.

Le contrat expose egalement une verification de solvabilite interne, `WeETHUnderbacked`, qui compare l'offre totale de weETH aux shares eETH detenues par le contrat lui-meme : si ces deux valeurs divergent au point que le weETH ne serait plus entierement adosse a des shares eETH reelles, certaines operations sont bloquees plutot que de laisser le systeme continuer a emettre un weETH sous-collateralise.

[Chapitre suivant : rebase, la distribution des recompenses plafonnee](05-rebase.md)
