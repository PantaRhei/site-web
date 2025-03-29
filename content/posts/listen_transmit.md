+++
title = 'Listen-transmit project 1/n'
date = 2024-10-17T12:50:24+02:00
draft = true
+++

# Introduction
L'objectif de ce projet est de créer une station de réception et de transmission
à partir d'un Raspberry Pi 5 4 Go. On utilisera un HackRF One pour la réception
et l'émission, ainsi qu'un RTL-SDR pour la réception.

Ainsi, le Raspberry Pi 5 pourrait remplacer le Portapack du HackRF One.
Il offrirait une puissance de calcul et une capacité d'adaptation à
d'autres dispositifs SDR plus importante que le Portapack.

De plus, le Raspberry Pi 5 est plus facilement dissimulable que le Portapack.
On peut très bien cacher le Raspberry Pi 5 dans un sac et le contrôler depuis
un PC ou un téléphone. Ainsi, lors d'une utilisation dans un magasin ou un restaurant,
le Raspberry Pi 5 serait caché avec les dispositifs SDR et les antennes dans un sac.

# OS utilisé.
Après plusieurs essais, le choix se porte sur [Dragon OS](https://sourceforge.net/projects/dragonos-focal/).
L'avantage est qu'il est déjà préconfiguré pour le HackRF One et le RTL-SDR.
Il propose un serveur VNC pour les applications graphiques et un serveur SSH.
De plus, la plupart des outils utilisés pour la SDR y sont présents.

L'installation se fait très facilement à l'aide du logiciel : `Raspberry Pi Imager`.

Voila un neofetch du système installé:
![Description de l'image](/images/neofetch_dragon_os_rb.png)

# Connection sur la Rasberry pi 5
## WIFI
Pour cela, nous avons plusieurs solutions :
- Utiliser un réseau Wi-Fi généré par un téléphone. Dans notre cas, nous
avons choisi de créer un réseau Wi-Fi avec un téléphone. Lors de l'allumage
du Raspberry Pi 5, il se connecte automatiquement au réseau Wi-Fi et, depuis
un PC ou un téléphone, on peut se connecter au Raspberry Pi 5 avec un client
SSH ou VNC viewer. Le problème de cette méthode est que le téléphone qui génère
le réseau Wi-Fi ne peut pas se connecter au Raspberry Pi 5 en même temps
(je pense que c'est faisable, mais cela doit nécessiter une configuration
spécifique du téléphone).
- La deuxième solution est de faire créer un hotspot par le Raspberry Pi 5,
mais cela comporte un risque de consommation d'énergie plus élevée.




## Ethernet
Cette solution peut être envisagée, mais pour le moment, elle n'a pas été testée.

# Alimentation
Étant donné la nature du projet, il faut une alimentation externe
transportable pour le Raspberry Pi. À ce stade, je n'ai fait aucun test sérieux.

J'ai tenté de brancher ma batterie externe de 20 000 mAh
(avec une sortie de 2 ampères) empruntée à une amie.
Malheureusement, cela ne marche pas ; le Raspberry Pi
s'éteint quand je commence à lui en demander plus.

# Exemple d'utilisation avec deux devices SDR
Dans cet exemple, on utilise deux dispositifs SDR :
Le HackRF One, qui utilise l'outil grgsm_scanner pour détecter les cellules GSM et
le RTL-SDR, qui utilise l'outil rtl_433 pour détecter les dispositifs à 433 MHz (beaucoup de voitures).

![RTL 433 + grgsm_scanner](/images/RTL_gsm_rb.png)

On peut voir que `grgsm_scanner` est assez gourmand en ressources.
![htop](/images/htop_rtl_gsm_rb.png)

Un problème constaté est qu'il est difficile de brancher les deux dispositifs SDR ;
le RTL-SDR a tendance à prendre beaucoup de place.
![Connection hackrf + rtl-sdr](/images/connectic_RB.jpg)

À ce stade, si vous voulez faire des tests, je vous conseille
vraiment d'utiliser VNC Viewer et de connecter le Raspberry Pi à votre réseau Wi-Fi.



# Problème
- La rasberry pi 5 chaud beaucoup quand on le met dans un sac (no shit)...
- Le RTL-SDR a aussi tendance à chauffer (mais normal).

Conclusion si vous cherchez à faire un four cherchait vers cette piste.
