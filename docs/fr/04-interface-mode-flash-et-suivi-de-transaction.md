# Chapitre 4 -- L interface : mode Flash, selecteur de reseau et suivi d une transaction en direct

Le composant `BlockExplorer` (`src/components/block-explorer.tsx`) orchestre
toute l experience utilisateur. Un interrupteur "Flash Mode" (`Switch`)
bascule l affichage entre une seule colonne de blocs classiques
(`NormalBlockList`) et une disposition a deux colonnes juxtaposant les blocs
classiques et les Flashblocks (`FlashBlockList`) -- cette derniere affichant
en plus, via `SubBlockCard`, chaque sous-bloc individuellement des qu il
arrive, avec une bordure distincte pour le sous-bloc "en attente"
(`isPending`) en cours de construction.

La connexion de portefeuille repose sur wagmi (`useAccount`, `useConnect`
avec le connecteur `injected()`, `useDisconnect`) et le `NetworkProvider`
(`src/contexts/network-context.tsx`) fournit un `WagmiProvider` configure
avec les chaines `base` et `baseSepolia` de viem. Le composant
`SendTransaction` verifie d abord que le portefeuille est sur le bon
reseau (comparant `chainId` a la chaine cible selon le reseau selectionne,
proposant un bouton "Switch Network" sinon), puis envoie une transaction de
test fixe (0.0001 ETH vers une adresse codee en dur) via
`useSendTransaction`. Des reception du hash de transaction, celui-ci est
ajoute a un dictionnaire `txns` (adresse hash -> booleen), qui sert de table
de surbrillance transmise a `FlashBlockList` et `NormalBlockList` : dans les
deux vues, la transaction de l utilisateur apparait alors en rouge et
remonte en tete de la liste des transactions affichees dans son sous-bloc
ou bloc, rendant visible le moment exact ou elle est incluse.

Le `NetworkSelector` permet de basculer entre `mainnet` et `sepolia` ; ce
changement reinitialise entierement l etat du hook `useFlashblocks` (via un
effet declenche sur le changement de `websocketUrl`) puisque chaque reseau
possede son propre flux WebSocket et donc sa propre sequence de blocs.
