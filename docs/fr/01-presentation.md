# Chapitre 1 -- Presentation de base-flashblocks-demo

Ce depot est une application Next.js de demonstration officielle de Base qui
visualise en temps reel les "Flashblocks" : une fonctionnalite de production
de blocs a faible latence propre a Base, qui decoupe un bloc classique
(environ 2 secondes) en plusieurs sous-blocs partiels (environ 200
millisecondes chacun) diffuses progressivement avant que le bloc complet ne
soit finalise. L objectif de la demo est purement pedagogique et visuel :
montrer, cote a cote, la difference entre l experience "bloc classique"
(l utilisateur attend le bloc complet) et l experience "Flashblocks"
(l utilisateur voit les transactions apparaitre presque instantanement, sous-
bloc par sous-bloc).

L application se connecte a un flux WebSocket expose par l infrastructure
Base (l URL exacte est fournie via des variables d environnement,
`NEXT_PUBLIC_MAINNET_WEBSOCKET_URL` et `NEXT_PUBLIC_SEPOLIA_WEBSOCKET_URL`),
qui diffuse en continu des messages "Flashblock" -- des fragments de bloc en
cours de construction. La demo assemble ces fragments en temps reel dans le
navigateur pour reconstituer visuellement l etat d un bloc au fur et a
mesure de sa construction, puis l affiche a cote de la vue "bloc classique"
equivalente (qui n apparait qu une fois le bloc entierement finalise).

L interface permet egalement de connecter un wallet (via wagmi) et d envoyer
une petite transaction de test, afin d observer concretement en combien de
temps cette transaction apparait dans un sous-bloc Flashblock compare a un
bloc classique -- rendant tangible le gain de latence percue. Un selecteur
de reseau permet de basculer entre Base mainnet et Base Sepolia (testnet).
