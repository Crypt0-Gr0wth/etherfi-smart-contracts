# Chapitre 9 — EtherFiOracle : le comite de consensus hors-chaine sur l'etat de la beacon chain

L'etat reel des validateurs (soldes, recompenses accumulees, slashings eventuels) vit sur la beacon chain, une donnee qu'un contrat Ethereum ne peut pas lire directement. `EtherFiOracle.sol` organise un comite de membres de confiance qui observent independamment cet etat et soumettent des rapports identiques jusqu'a atteindre un consensus, un motif similaire a celui employe par d'autres protocoles de liquid staking pour combler ce fosse entre la couche d'execution et la couche de consensus.

`submitReport` accepte un `OracleReport` structure de la part de n'importe quel membre du comite. Le contrat regroupe les rapports identiques et incremente un compteur de soutien (`consenState.support`) ; des que ce compteur atteint `quorumSize` (le nombre minimum de membres devant s'accorder), `consensusReached` devient vrai et le rapport devient executable par `EtherFiAdmin` (chapitre suivant). `quorumSize` est lui-meme ajustable par gouvernance via `setQuorumSize` et `manageCommitteeMember`, permettant de faire evoluer la taille du comite et le seuil de consensus requis dans le temps.

Ce decouplage entre soumission (n'importe quel membre peut soumettre) et consensus (il faut que plusieurs membres soumettent le meme rapport) protege contre un membre individuel compromis ou defaillant : un rapport frauduleux isole n'atteint jamais le quorum et n'est donc jamais execute.

[Chapitre suivant : EtherFiAdmin, l execution des rapports valides](10-etherfiadmin.md)
