# Chapitre 5 -- Limites et perimetre de ce parcours

Ce parcours couvre le protocole de reception et d assemblage des messages
Flashblocks via WebSocket (fragments compresses ou non, sequence d index
determinant le debut ou la continuation d un bloc), le decodage des
transactions serialisees au format OP-Stack et le calcul local de leur
hash, ainsi que la couche d interface : bascule entre vue classique et vue
Flashblocks, connexion de portefeuille, envoi d une transaction de test et
suivi visuel de son inclusion en temps reel, et selection du reseau
(mainnet ou Sepolia).

Sont volontairement laisses hors champ : l implementation cote serveur du
flux Flashblocks lui-meme (le depot ne contient que le client qui le
consomme, pas l infrastructure de sequenceur qui le produit), le detail de
l algorithme de compression Brotli au-dela de son usage direct via la
librairie `brotli-dec-wasm`, et les composants d interface generiques
(`Header`, `NetworkSelector`, les primitives `ui/card`, `ui/switch`,
`ui/label`) qui relevent de choix de presentation sans logique propre a
Flashblocks. La page `/docs` de l application elle-meme se contente de
rediriger vers la documentation officielle
(`docs.base.org/chain/flashblocks`), signe que le concept de Flashblocks en
tant que tel (son role dans l architecture de sequencement de Base, ses
garanties de finalite) est documente par Base ailleurs et non reexplique
dans le code de cette demo.

L objectif de ce parcours est de comprendre precisement comment une
application cliente consomme et visualise ce flux de blocs partiels, pas de
redocumenter l architecture de sequencement de Base dans son ensemble.
