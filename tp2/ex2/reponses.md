# Exercice 2

## 1 - En utilisant GPG, chiffrez un message à l’aide d’une clé symétrique et déchiffrez-le à nouveau.

On peut chiffrer un fichier ainsi :

```bash
nderousseaux@david ex2 % gpg --symmetric --cipher-algo AES256 secret.txt
```

Et déchiffrer ainsi :

```bash
nderousseaux@david ex2 % gpg -o secret.dec -d secret.txt.gpg     
```

## 2 - Générez une paire de clés asymétriques correspondants à votre identité réelle

```bash
nderousseaux@david ex2 % gpg --gen-key
```

## 3 - Maintenant que nous avons nos clés, nous voulons partager la clé publique au monde entier, et nous n’allons pas l’envoyer à chaque correspondant. Nous allons la déposer sur un serveur d’échange de clés. Envoyez votre clé sur le serveur d’échanges keyserver.ubuntu.com

```bash
nderousseaux@david ex2 % gpg --keyserver keyserver.ubuntu.com --send-keys 4B034C6260B7B4E04419C6E28766834D0A65954B
```

## 4 - Récupérez la clé publique d'une personne de votre groupe via ce serveur d'échange.

On peut rechercher une clé à partir d'une adresse mail :

```bash
nderousseaux@david ex2 % gpg --keyserver keyserver.ubuntu.com --search-keys n.derousseaux@icloud.com
```

Ensuite, on peut récupérer une clé ainsi :

```bash
nderousseaux@david ex2 % gpg --keyserver keyserver.ubuntu.com --recv-keys 77e6401943ca0e10
```

## 5 - Peut-on être certain de l'identité du possesseur de la clé gpg ?

Non, on pourrait mettre une clé sur le serveur en mentant sur son identité.

## 6 - Chiffrez en asymétrique un fichier secret.gpg à destination d'une autre personne de votre groupe à l’aide de sa clé publique

```bash
nderousseaux@david ex2 % gpg --encrypt --recipient 8766834D0A65954B secret.txt
```

## 7 - À l’aide de votre clé privé, déchiffrez le fichier secret secret.gpg provenant d'une autre personne de votre groupe

```bash
nderousseaux@david ex2 % gpg --output secret.txt.dec --decrypt secret.txt.gpg 
```

## 8 - Chiffrez et signez un nouveau fichier secretSi*gne.gpg* à destination d'une autre personne de votre groupe et transférez-lui ce fichier.

```bash
nderousseaux@david ex2 % gpg --encrypt --sign --recipient 8766834D0A65954B secret.txt
```

## 9 - Vérifiez la signature du fichier réceptionné et déchiffrez celui-ci avec votre clé privée.

```bash
nderousseaux@david ex2 % gpg --output secret.txt.dec --decrypt secret.txt.gpg        
```

## 10 - Établissez une "Toile de confiance" en signant les clés des membres du groupe. Le concept est la "keysigning party" où par votre signature vous confirmez (avec un certain niveau de confiance) le lien entre l'identité réelle (Carte d'identité...) et la clé gpg proposée. Attention cependant à ne pas signer n’importe-quelle clé. La clé ainsi signée peut-être renvoyée sur un serveur de clé

Méthode -> A  prend la clé de B. Il ajoute sa signature. A envoie la clé de B signée à B. B met à jour sa clé avec la nouvelle signature. B met à jour sa clé sur le serveur
