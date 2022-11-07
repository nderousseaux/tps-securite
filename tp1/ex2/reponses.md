# Exercice 2

## 1 - Chiffrez la phrase **Chiffrement en BlowFish** dans un fichier *result1-bf-cbc* en utilisant bf-cbc.

```bash
nderousseaux@david ~ % echo "Chiffrement en BlowFish" |openssl bf-cbc -out result1-bf-cbc -pass pass:password
```

## 2 - Chiffrez à nouveau la même phrase avec le même mot de passe dans un fichier *result2-bf-cbc.*

```bash
nderousseaux@david ~ % echo "Chiffrement en BlowFish" |openssl bf-cbc -out result2-bf-cbc -pass pass:password
```

## 3 - Comparez les fichiers result 1-bf-cbc et result2-bf-cbc à l’aide de la commande xxd, que constatez-vous ?

```bash
nderousseaux@david tp1 % xxd result1-bf-cbc result1-bf-cbc.bin
nderousseaux@david tp1 % xxd result2-bf-cbc result2-bf-cbc.bin
nderousseaux@david tp1 % diff result1-bf-cbc.bin result2-bf-cbc.bin 
```

On constate que les deux fichiers sont absolument pas identitique.

## 4 - Vérifiez que le déchiffrement des 2 fichiers précédents donne bien le même résultat.

```bash
nderousseaux@david tp1 % openssl bf-cbc -d -in result1-bf-cbc -pass pass:password
Chiffrement en BlowFish
nderousseaux@david tp1 % openssl bf-cbc -d -in result2-bf-cbc -pass pass:password
Chiffrement en BlowFish
```

## 5 - Refaites les questions 1 à 4 en rajoutant l’option -nosalt lors du chiffrement. Que pouvez-vous conclure?

On peut en conclure que le sel, généré aléatoirement au moment du chiffrement intervient dans le processus de chiffrement, rendant le chiffrement de deux messages identitiques différents. Ainsi, on évite que quelqu'un puisse "deviner" le message, et on rend beaucoup plus difficile le brute force.