# Chapitre 7 — EtherFiNode et le restaking natif via EigenPod

Chaque validateur ether.fi possede son propre contrat `EtherFiNode` (`src/staking/EtherFiNode.sol`), qui detient a son tour un `EigenPod` EigenLayer dedie — l'architecture native d'EigenLayer associant un pod par ensemble de validateurs partageant les memes identifiants de retrait. C'est cette relation directe entre le contrat de noeud ether.fi et l'EigenPod qui constitue le « restaking natif » : les recompenses de consensus de la beacon chain et les recompenses de restaking transitent par le meme chemin, sans passer par un token de restaking liquide intermediaire.

`EtherFiNode` importe les interfaces `IDelegationManager`, `IEigenPodManager`, `IEigenPod` et `IStrategy` d'EigenLayer directement (`src/interfaces/eigenlayer-interfaces/`), et definit la constante `EIGENLAYER_WITHDRAWAL_DELAY_BLOCKS` (100800 blocs, soit environ deux semaines) correspondant au delai de retrait impose par EigenLayer lui-meme apres une desinscription (« undelegation ») d'un operateur de restaking.

Le contrat marque explicitement son ancien etat de stockage lie au modele T-NFT/B-NFT (`LegacyNodeState`) comme deprecie — ce modele historique, ou chaque validateur etait represente par une paire de NFT (`TNFT`/`BNFT`, aujourd'hui dans `src/archive/`) donnant respectivement droit au capital et aux recompenses d'un validateur specifique, a ete remplace par la comptabilite globale a shares du `LiquidityPool` decrite aux chapitres 2 et 3, simplifiant le modele economique au prix de perdre l'attribution individuelle par validateur.

[Chapitre suivant : AuctionManager, l enchere des operateurs de noeuds](08-auction-manager.md)
