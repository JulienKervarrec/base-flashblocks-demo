# Chapitre 2 -- Le protocole Flashblocks : reception et assemblage des sous-blocs

Le coeur technique du depot est le hook `useFlashblocks` (`src/hooks/
useFlashblocks.ts`), qui ouvre une connexion WebSocket brute vers l URL du
flux Flashblocks et interprete chaque message recu. Un message peut arriver
sous deux formes : soit du JSON texte brut (detecte en verifiant que le
contenu commence par `{`), soit des donnees binaires compressees en Brotli
(decompressees via le paquet `brotli-dec-wasm`, une implementation
WebAssembly de Brotli, avant d etre parsees comme JSON) -- une optimisation
de bande passante pour un flux a tres haute frequence.

Chaque message desserialise represente un objet `Flashblock` avec trois
sections : `base` (les en-tetes du bloc en cours de construction : numero de
bloc, timestamp, limite de gas, fee recipient, etc., presents uniquement
dans le tout premier fragment), `diff` (l etat incremental : racine d etat,
racine des recus, et surtout la liste des transactions ajoutees par ce
fragment specifique), et `metadata.receipts` (les recus des transactions
deja incluses). Le champ `index` indique la position du fragment dans la
sequence de construction du bloc : `index === 0` signale le debut d un
nouveau bloc, tandis que les index suivants representent des mises a jour
incrementales du meme bloc.

La machine a etats du hook (`State { blocks, pendingBlock }`) gere cette
sequence : a la reception d un fragment d index 0, le `pendingBlock`
precedent (s il existe) est archive dans la liste `blocks` (plafonnee a 25
elements via `clamp`) et un nouveau `pendingBlock` est cree ; a la reception
d un fragment d index superieur, `updateBlock` ajoute un nouveau `subBlock`
au `pendingBlock` existant sans le remplacer. Ce mecanisme permet a
l interface d afficher un bloc "en cours de construction" qui s enrichit
visuellement de sous-bloc en sous-bloc, plutot que d attendre passivement
l evenement unique de finalisation.
