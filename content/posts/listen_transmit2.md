+++
title = 'Listen-transmit project 2/n'
date = 2024-10-29T16:53:59+01:00
draft = true
+++


Etant donnée que je n'ai pas une grande riche, on va
oublier l'idée de la batterie externe et l'on va trouver une prise.

# Mise en place d'un point wifi pour la RBpi5

Si ce n'est pas installé déjà:
```bash
sudo apt update
sudo apt install network-manager hostapd
```

On créait notre point wifi:
```bash
sudo nmcli dev wifi hotspot ifname wlan0 ssid "MonHotSpotSexy" password "SDR_sexy11"
```
(La maturité est un concept surévalué)


On a une connection, à priori, tout marche. (Quelque problème avec i3wm, mais c'est un autre sujet)
![La sainte connection](/images/prouprout.png)


Si vous avait des bug de clavier avec VNC Viewer sachait que faire une deconnection et une reconnection,
semble marcher. (Future job chez windows, je ne vois pas l'erreur. Pas d'erreur, pas de problème)
