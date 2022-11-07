# Exercice 4

## 1- Illustrez une des faiblesses du mode ECB pour du chiffrement symétrique par bloc.

Utilisez l’algorithme de chiffrement BlowFish (bf-ecb), pour générer un résultat chiffré d’un texte contenant 16 caractères « a » avec les contraintes suivantes : aucun « padding » et aucun « salage »

```bash
nderousseaux@david ex4 % openssl bf-ecb -in texte.txt -out texte.enc -nosalt -pass pass:password 
```

## 2 - Affichez le résultat avec la commande *xxd*.

```bash
nderousseaux@david ex4 % xxd texte.enc texte.enc.bin
```

## 3 - Montrez que le mode ECB n’utilise pas de vecteur d’initialisation.

Le mode ECB n'utilise pas de vecteur d'initialisation, car, dans un même chiffrement, le même bloc de 8 octets donnera le même résultat.

## 4 - Montrez que le mode CBC corrige la faille précédente en utilisant le même mot de passe et le même texte.

```bash
nderousseaux@david ex4 % openssl bf-cbc -in texte.txt -out texte-cbc.enc -nosalt -pass pass:password
nderousseaux@david ex4 % xxd texte-cbc.enc texte-cbc.enc.bin
```

Dans le fichier `.bin`, on peut voir qu'en mode CBC, le même groupe de 8 octets est chiffré différement.

## 5 - Montrez que le mode CBC utilise un vecteur d'initialisation IV.

```bash
nderousseaux@david ex4 % openssl bf-cbc -in texte.txt -out texte-cbc.enc -nosalt -debug -pass pass:password -P
```

Cette commande nous retourne le vecteur `IV`. 

## 6 - Montrez qu’avec le même algorithme et même clef, un texte clair donnera toujours le même texte chiffré.

```bash
nderousseaux@david ex4 % openssl bf-cbc -in texte.txt -out texte-cbc2.enc -nosalt -pass pass:password   
nderousseaux@david ex4 % xxd texte-cbc2.enc texte-cbc2.enc.bin
nderousseaux@david ex4 % diff texte-cbc2.enc.bin texte-cbc.enc.bin 
nderousseaux@david ex4 % echo $?
0
```

## 7 - Trouvez une solution qui permet d’éviter le phénomène précédent.

Il suffit d'utiliser un sel, choisi aléatoirement

```bash
nderousseaux@david ex4 % openssl bf-cbc -in texte.txt -out texte-cbc3.enc -pass pass:password
nderousseaux@david ex4 % openssl bf-cbc -in texte.txt -out texte-cbc4.enc -pass pass:password
nderousseaux@david ex4 % xxd text-cbc3.enc text-cbc3.enc.bin
nderousseaux@david ex4 % xxd text-cbc4.enc text-cbc4.enc.bin
nderousseaux@david ex4 % diff text-cbc3.enc.bin text-cbc4.enc.bin
nderousseaux@david ex4 % $?
```

## 8 - Quel algorithme est le plus rapide entre des-cbc, aes-128-cbc et bf-cbc ?

Time sur `bd-cbc` : 0.13 user, 0.15 system.

Time sur `aes-128-cbc` : 0.4user 0.15 system.

Time sur `des-cbc` : 0.19user 0.16 system.