# Exercice 1

## 1 - Utilisez openssl pour générer une paire de clés RSA de 1024 bits dans un fichier testCle.pem en la protégeant avec 3DES.

```bash
nderousseaux@david ~ % openssl genrsa -des3 -out mykey.pem 1024 -pass pass:password
```

## 2 - Décodez le fichier testCle.pem et affichez l’exposant et le modulus.

```bash
nderousseaux@david ex1 % openssl rsa -modulus -in testCle.pem -text
```

## 3 - Extraire la clé publique dans un fichier VOTRENOM_PUB.pem

```bash
nderousseaux@david ~ % openssl rsa -in mykey.pem -pubout -pass pass:password > DEROUSSEAUX_PUB.pem
```

## 4 - Envoyez à une autre personne de la salle un fichier court (20 caractères ) short_text chiffre en RSA de façon à ce qu’elle seule puisse le déchiffrer.

Je chiffre un fichier `secret.txt` avec la clé publique de mon contact :

```bash
nderousseaux@david ex1 % openssl rsautl -encrypt -inkey DEROUSSEAUX_PUB.pem -pubin -in secret.txt -out secret.enc
```

Il peut la déchiffrer ainsi :

```bash
nderousseaux@david ex1 % openssl rsautl -decrypt -inkey testCle.pem -in secret.enc -out secret.dec -pass pass:password
```

## 5 - Faites de même avec un fichier très volumineux (par exemple RFC8017.txt ) long_text. L’échange doit demeurer confidentiel comme précédemment. Que constatez- vous ?

On m'indique que la clé est trop courte pour le fichier.

## 6 - Mettez en œuvre une autre solution asymétrique avec *openssl* afin de transmettre ce fichier long_text de manière sécurisée

On peut générer aléatoirement une clé AES, l'utiliser pour chiffrer de manière symétrique le fichier. Ensuite utiliser RSA pour envoyer cette clé, cryptée de manière asymétrique.

On génère une clé symétrique

```bash
nderousseaux@david ex1 % openssl rand 64 > symmetric_keyfile.key
```

On crypte en AES le fichier long :

```bash
nderousseaux@david ex1 % openssl enc -aes-128-cbc -kfile symmetric_keyfile.key -in rfc8017.txt -out rfc8017.enc 
```

On crypte la clé symétrique en RSA

```bash
nderousseaux@david ex1 % openssl rsautl -encrypt -inkey DEROUSSEAUX_PUB.pem -pubin -in symmetric_keyfile.key -out symmetric_keyfile.enc
```

Le destinataire recevra le fichier chiffré et la clé aes chiffrée également.

Il déchiffre la clé AES avec sa clé privée :

```bash
nderousseaux@david ex1 % openssl rsautl -decrypt -inkey testCle.pem -in symmetric_keyfile.enc -out symmetric_keyfile.dec 
```

Et il déchiffre la fichier avec la clé AES :

```bash
nderousseaux@david ex1 % openssl aes-128-cbc -d -in rfc8017.enc -out rfc8017.dec -kfile symmetric_keyfile.dec
```

## 7 - Maintenant que vous pouvez chiffrer vos échanges, faites-en sorte de garantir l’intégrité en utilisant une signature.

On peut signer un fichier ainsi :

```bash
nderousseaux@david ex1 % openssl dgst -sha512 -sign testCle.pem -out secret.txt.sha1 secret.txt 
```

On peut vérifier la signature ainsi : 

```bash
nderousseaux@david ex1 % openssl dgst -sha512 -verify DEROUSSEAUX_PUB.pem -signature secret.txt.sha1 secret.txt 
```

## 8 - Votre solution garantit-elle également l’authentification des échanges ?

Non, car je ne peux pas être sur de l'intégrité de la clé publique envoyée au début. Par contre je suis sur de discuter de manière anonyme avec la même personne.
