# Exercice 1

## 1 - Effectuez un hachage du mot **Bonjour** en utilisant MD5 et SHA1

```bash
nderousseaux@david ~ % echo -n Bonjour | openssl md5
ebc58ab2cb4848d04ec23d83f7ddf985
```

```bash
nderousseaux@david ~ % echo -n Bonjour | openssl sha1
f30ecbf5b1cb85c631fdec0b39678550973cfcbc
```

## 2 - Effectuez un hachage de la phrase **Bonjour je suis un hash** en utilisant  MD5 et SHA1

```bash
nderousseaux@david ~ % echo -n Bonjour je suis un hash | openssl md5
10510039afcf5245156966ac195f59b3
```

```bash
nderousseaux@david ~ % echo -n Bonjour je suis un hash | openssl sha1
ce9735a2e09be60e88a82e870842886a21c7a3cd
```

## 3 - Comparez le résultat des sorties MD5 puis des sorties SHA1, que pouvez-vous en conclure ?

Les sorties md5 et sha1 donnent un résultat différent en fonction de la chaîne d'entrée, mais de même taille.  Les deux en hexadecimal. Le hash sera différent en fonction de la phrase entrée.

## 4 - Encodez le mot **Bonjour** en Base64

```bash
nderousseaux@david ~ % echo -n Bonjour | openssl base64 
Qm9uam91cg==
```

## 5 - Encodez la phrase **Bonjour je ne suis pas un hash** en Base64

```bash
nderousseaux@david ~ % echo -n Bonjour je ne suis pas un hash | openssl base64
Qm9uam91ciBqZSBuZSBzdWlzIHBhcyB1biBoYXNo
```

## 6 - Comparez le résultat des deux sorties, que pouvez-vous en conclure ?

On constate que le début est identitique et que la sortie est plus longue pour la chaine la plus longue.

## 7 - Pouvez-vous effectuer les opérations inverses afin de retrouver les phrases des questions 2 et 5 ?

Il est impossible de procèder à l'opération inverse avec les hashs de la question 2 sans brute-force

Pour les encodage de la question 5, il est possible de procéder à l'opération inverse :

```bash
nderousseaux@david ~ % openssl base64 -d <<< Qm9uam91ciBqZSBuZSBzdWlzIHBhcyB1biBoYXNo     
Bonjour je ne suis pas un hash% 
```

## 8 - Que pouvez conclure sur le hachage et l’encodage ?

Le hash permet de fournir une "emprinte" d'une données. Tandis que l'encodage change la manière de l'écrire sans pour autant  le rendre indéchiffrable.
