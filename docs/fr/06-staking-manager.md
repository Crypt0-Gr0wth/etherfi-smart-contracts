# Chapitre 6 — StakingManager et la creation des validateurs via EtherFiNode

`StakingManager.sol` orchestre la naissance d'un nouveau validateur en deux temps distincts. `instantiateEtherFiNode`, reservee au role `EXECUTOR_OPERATIONS_ROLE`, deploie un nouveau proxy `BeaconProxy` pointant vers `etherFiNodeBeacon` — un contrat `EtherFiNode` frais, optionnellement associe des sa creation a un `EigenPod` EigenLayer via `etherFiNodesManager.createEigenPod`.

La creation effective du validateur passe ensuite par `createBeaconValidators`, appelable uniquement par le `LiquidityPool`, qui envoie le depot initial de `INITIAL_DEPOSIT_AMOUNT` (1 ETH, et non les 32 ETH complets) au contrat de depot officiel de la beacon chain (`depositContractEth2.deposit`). Ce depot initial reduit utilise des identifiants de creation (`validatorCreationDataHash`, un hash de la cle publique, de la signature et du bid de l'operateur) verifies contre un statut de cycle de vie (`ValidatorCreationStatus` : REGISTERED, CONFIRMED, INVALIDATED) qui empeche la reutilisation ou la double-creation d'un meme validateur.

Les identifiants de retrait (`withdrawalCredentials`) du depot pointent vers l'`EigenPod` associe au `EtherFiNode`, obtenus via `addressToCompoundingWithdrawalCredentials` — c'est ce lien direct entre validateur et EigenPod qui permet le restaking natif du chapitre suivant, plutot qu'un simple depot vers une adresse de retrait passive.

[Chapitre suivant : EtherFiNode et le restaking natif via EigenPod](07-etherfinode-eigenpod.md)
