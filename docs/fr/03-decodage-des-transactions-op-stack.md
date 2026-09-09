# Chapitre 3 -- Decoder les transactions brutes d un fragment OP-Stack

Chaque fragment `diff.transactions` contient des transactions serialisees
sous leur forme hexadecimale brute, au format specifique de la pile
OP-Stack (l infrastructure de rollup sur laquelle Base est construite).
La fonction `convertBlock` (dans `useFlashblocks.ts`) utilise
`parseTransaction` du module `viem/op-stack` pour decoder chacune de ces
chaines en un objet transaction exploitable (`from`, `to`, `value`), avec un
`try/catch` autour de chaque parsing individuel afin qu une transaction mal
formee ou d un type non reconnu n interrompe pas le traitement de tout le
fragment -- une resilience necessaire face a un flux temps reel dont le
contenu n est pas entierement sous le controle de l application.

Point notable : le hash de chaque transaction n est pas fourni par le flux
Flashblocks lui-meme a ce niveau, il est recalcule cote client via
`keccak256` applique directement sur la chaine hexadecimale brute de la
transaction serialisee. C est ce hash calcule localement qui sert ensuite a
faire correspondre une transaction affichee a une transaction que
l utilisateur vient d envoyer (voir chapitre 4), permettant de la
mettre en surbrillance des qu elle apparait dans le flux, sans attendre de
confirmation server-side.

Les fonctions utilitaires de `src/utils/block-utils.ts` completent ce
traitement cote presentation : `transactionsFor` aplati les sous-blocs d un
`Block` en une liste unique de transactions, `sortByHighlighted` remonte en
tete de liste les transactions marquees comme suivies par l utilisateur, et
`truncateHash`/`getRelativeTime` formatent respectivement les adresses
tronquees et les timestamps relatifs ("Xs ago") affiches dans l interface.
