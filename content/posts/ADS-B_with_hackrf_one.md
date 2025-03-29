+++
title = 'Lire les ADS-B avec un hackRF One'
date = 2024-10-03T18:31:59+02:00
draft = false
+++

# Introduction
Si vous avez un HackRF One et que vous voulez lire les signaux ADS-B,
il est possible que vous ayez des difficultés à trouver un logiciel.
Le logiciel le plus connu pour lire les signaux ADS-B est dump1090.

Sur le github officiel de dump1090, on se renconte très rapidement
que le HackRF One n'est pas pris en charge. Cependant, il existe
un fork sur ce [github](https://github.com/esuldin/dump1090).

Je vous conseille très fortement d'utiliser debian ou une distribution
qui est basé sur debian.

# Antenne


# Fonctionnement de dump1090
Dans notre cas nous allons utiliser le mode `--interactive` qui permet
d'afficher les informations des avions en temps réel.
Pour cela, il suffit de lancer la commande suivante:
```bash
./dump1090 --interactive
```
(Pour les restes je vous invite à vous référer à la documentation)


![Resultat de la commande](/images/dump1090.png)

Sur l'image ci-dessus, on peut voir les informations des transponteur
d'avions qui sont capturé par le HackRF One. On peut voir dans la colonne
`Flight`l'identifiant de l'avion. Dans les colonnes suivant on a
d'autres informations comme la vitesse, l'altitude, la position.

![Resultat de la commande](/images/fr1.png)
![Resultat de la commande](/images/fr2.png)

Bien entendu quand j'ai écrit cet article, un seul avion est
à la portée de mon antenne (comme par hazard).
