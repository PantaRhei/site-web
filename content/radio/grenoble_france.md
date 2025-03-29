+++
title = 'Grenoble - France'
date = 2024-10-08T13:37:44+02:00
description = "Signal rencontré à Grenoble"
draft = false
+++
Note : Cet article est en cours de rédaction sur un temps potentiellement infini.
Les hypothèses et les fréquences ne sont pas nécessairement exactes ou à jour.

# Bibliothèque universitaire de Grenoble (Institut Fourier)
## Signal detect: 2024-10-09 13:06:52

Software use: rtl_433 (for capture), gqrx (for visualisation)

Receveir: RTL-SDR V.4 (for capture), HackRF One(for visualisation)

Frequency: 433.436MHz (approximatif)

Model detected: Sompfy RTS, utiliser pour les volets roulants;
Lien du commercant: [Lien](https://www.somfy.fr/assistance/notices/volet-roulant/radio-rts)

Position deuxième étages halles sud.

Periodicité: environ 1-2 minutes

Image de la capture:
![Description de l'image](/images/2024-10-09-433_436_000Hz_gqrx.png)
![Description de l'image](/images/2024-10-09-433_436_000Hz_RTL_capture.png)


## Signal detect: 2024-10-09 13:48:10

Software use: rtl_433 (for capture), gqrx (for visualisation)

Receveir: RTL-SDR V.4 (for capture), HackRF One(for visualisation)

Frequency: 433.920MHz (approximatif)

Model detected: Generic Remote. Utilisation inconnu.

Position deuxième étages halles sud.

Periodicité: environ 4-5 minutes, note que le signal n'a pas été redetecté. On
peut penser que ce signal provient d'une télécommande activé par un humain.

Image de la capture (zone entourer en noir correspond à la capture rtl-433):
![Description de l'image](/images/2024-10-09_433_920_000.png)


## Signal detect: 2024-10-09 14:51:00

Software use: gqrx (for visualisation)

Receveir: RTL-SDR V.4 (for capture)

Frequency: 445.000MHz-447.000MHz (approximatif)
- 445.338MHz
- 446.000MHz (à noter qu'il y un signal aussi à 446.020MHz.)

Mode: Narrow FM

Position deuxième étages halles sud. Signal correct dans le batiments mais avec pas mal de bruit.
On peut distinguer des conversations en Arabe et quelques mots en Français. Attention, on capte aussi
des convertions de radio ham.

Hypothèse sur le signal: Certainement du matériel [PMR446](https://fr.wikipedia.org/wiki/PMR446)
- Télécommunication des gardes de l'universiter? (Hypothèse à confirmer)

Image de la capture:
![Description de l'image](/images/2024-10-09_445_338_voice.png)
![Description de l'image](/images/2024-10-09_446_000_voice.png)


# Saint-Martin-d'Hères (au sud de la sortie de UGA)
- 445.337MHz (radio utiliser par une auto école de motos) 2024-10-09 17:00:00
En théorie, il est fortement possible que le signal puissent être capté à Grenoble,
de parts la mobilité des motos.
![Description de l'image](/images/2024-10-09_445_337_000_auto_ecole_motos.png)
- 446.182MHz (radio amateur fréquence) 2024-10-09 17:00:00
- 446.032MHz (radio magasin? confirmer mesure prise près du schmidt) 2024-10-09 17:27:00
Sur cette fréquence, on parle de rayon + étiquette. Hypothèse grande distribution ou
grand magasin. On parle maintenant de renfort caisse. Les employés parlent sur
une même frèquence quelqu'un vient de dire "je vais arriver".
Info sur elle est sur la meme fréquence que une fréquence de radio amateur.
Hypothèse: Le magazin a de plus grande chance d'être en zone 2.
![Description de l'image](/images/zoneAZ32ZAEDQZ.png)
note sur l'image: décaler la zone deux vers le sud de 100-300 métre.


# Saint-Martin-d'Hères (batiment IM2AG, amphi F018)
## Signal detect: 2024-10-11 11:06:00

Software use: gqrx (for visualisation)

Receveir: RTL-SDR V.4 (for visualisation)

Frequency: 130.000MHz (approximatif)

Mode: AM

Contexte: Fréquence aérien, au vu des discutions certainement la fréquence
en route du bassin grenoblois (à confirmer sur les cartes aéro).

![Description de l'image](/images/2024-10-11-130_000_000.png)

## Signal detect: 2024-10-11 11:48:00
Software use: gqrx (for visualisation)

Receveir: RTL-SDR V.4 (for visualisation)

Frequency: 867.502MHz (approximatif)

Mode: RAW

Contexte: Signal inconue, le signal est envoyer de façon périodique.
Il ressemble à ce signal en terme de son [Sigidwiki Snap Circuits SCROV-10 Remote Control](https://www.sigidwiki.com/wiki/Snap_Circuits_SCROV-10_Remote_Control).
Hors les fréquences d'émission ne sont pas les mêmes. Mais cela, reste troublant
car la périodicité du signal semble être les même.

Note: dans la zone on rencontre du LoRaWAN (sur l'image correspond au pic avec un plateau).
Rencontrer du LoRaWAN est cohérent car le batiment utilise des capteurs CO2 qui
commmunique en LoRAWAN. [Model du capteur](https://www.aura-co2.com/fr/).
On y apprend bien que:
> Aura-CO2 peut être connecté au réseau LoRaWAN, permettant un accès en ligne aux données en utilisant une interface web et une application mobile.

Il existe aussi d'autre capteur comme [celui-ci](https://www.dataprint.fr/support/elsys/ELSYS-ERS-CO2-datasheet.pdf)

Les signaux en LoRaWAN sont envoyé sporadiquement.

![Description de l'image](/images/2024-10-11_867_502_000.png)

## Signal detect: 2024-10-11 12:34:54

Software use: rtl_433

Receveir: RTL-SDR V.4 (for visualisation)

Frequency: 433.000MHz (approximatif)

Contexte: Des véhicules viennent de passer.
Ce n'est pas la première fois que je captes des capteurs de pressions
pour les pneus, j'ai remarqué que les emetteurs spamment un peu beaucoup :p.

Pour Schrader-EG53MA4:
[Lien vers des infos](https://github.com/merbanan/rtl_433_tests/blob/master/tests/Schrader_EG53MA4_TPMS/01/README.md)
Selon le github de RTL-433:
> Frames are Manchester encoded ASK data.

Abarth-124Spider:
[Lien vers l'issue GitHub](https://github.com/merbanan/rtl_433/issues/1271)

Hyundai-VDO:
[Site qui vends ce TPMS](https://www.tunershop.fr/tpms-capteur-de-pression-des-pneus-continental-vdo-oem-a2c1446770080-silver-pour-hyundai-accent-genesis-i20-i30-ix20-santa-fe.html)


![Description de l'image](/images/2024-10-11_rtl_433.png)
