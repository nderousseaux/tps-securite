# TP n°3 sécurité - Section notée

## Question 39. Installez un serveur FTP si ce n’est pas encore fait et chargez le module netfilter de gestion du protocole FTP

On installe `proftpd` :

```sh
nderousseaux@david ~ % apt install profpd
```

On active le module ftp :

```sh
nderousseaux@david ~ % modprobe nf-conntrack-ftp
```

## **Question 40.** Après avoir effacé toutes les règles précédentes, commencez par interdire tout paquet reçu.

On vérifie qu'il n'y a aucune règle :

```sh
nderousseaux@david ~ % iptables -nvL
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

```

On interdit tout les paquets entrants :

```sh
nderousseaux@david ~ % iptables -A INPUT -j DROP
```

## **Question 41.** Testez les connexions FTP vers la machine de votre voisin. Qu’observez-vous ?

Il est impossible de se connecter à un server ftp. En effet, il est probable que `netfilter` bloque les réponse du serveur en les considérant comme des paquets entrant.

## **Question 42.** Autorisez sur votre machine les paquets provenant du serveur et qui correspondent aux connexions (que le client connaît car c’est lui qui les a établis).

```bash
nderousseaux@david ~ % iptables -A INPUT -p tcp -m tcp --sport 21 -m conntrack --ctstate ESTABLISHED,NEW -j ACCEPT
```

## Question 43. Essayez ensuite (une fois connecté) un ls. Qu’observez-vous ? Pourquoi ?

```bash
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||40004|)
```

Cela ne fonctionne, pas, la connection passe en mode passif, et le serveur choisi 40004 comme port de connection. Cela ne marche pas, car le port 40004 est évidemment bloqué à cause de la règle établie dans la question 40.

## Question 44.  Autorisez les transferts de données en FTP depuis le serveur vers votre machine.

L'ajout de la règle suivante permet de permet d'accepter les connections sur le port 20 de la part d'un client auparravant connecté (en l'occurence sur le port 21). 

```bash
nderousseaux@david ~ % iptables -A INPUT  -p tcp -m tcp --sport 20 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
```

Pour des raisons que j'ignore, mon serveur ftp ne voulait pas marcher en mode actif (port déterminé par le client) cependant, cette règle permet d'accepter les échanges de mon client avec le mode passif :

```bash
nderousseaux@david ~ % iptables -A INPUT -p tcp -m tcp --sport 1024: --dport 1024: -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
```

