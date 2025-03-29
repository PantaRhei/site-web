+++
title = 'Resume'
date = 2024-10-02T21:58:16+02:00
draft = false
+++

## RTL 433
Logiciel qui permet de scanner les signaux 433MHz.
[Github](https://github.com/merbanan/rtl_433)

### Installing
Installer sur Debian:
```bash
sudo apt-get install rtl-433
```
Installer sur Arch:
```bash
sudo pacman -S rtl-433
```


### Utilisation
Scanner sur le 433MHz avec le HackRF One:
```bash
rtl_433 -d "HackRF One"
```

Scanner sur le 433MHz et sauvegarder tout les signaux (inconnue ou connue):
```bash
rtl_433 -d "HackRF One" -S all
```

Lire un fichier capturer avec rtl_433:
```bash
rtl_433 -r file_with_signal
```

## gsm (gr-gsm)
Logiciel qui permet de scanner les signaux GSM.
[Github](https://github.com/ptrkrysik/gr-gsm)
[Usage](https://github.com/ptrkrysik/gr-gsm/wiki/Usage)

### Utilisation
Scanne les cellules gsm:
```bash
grgsm_scanner -v
```

## ADS-B
Logiciel qui permet de scanner les signaux ADS-B.

Frequence:
- 1090MHz => Mode-S Extended Squitter,
- 1030MHz => Interrogator,
- 978MHz => UAT.

Article sur [Sigidwiki](https://www.sigidwiki.com/wiki/ADS-B)


[Github compatible avec le hackrf](https://github.com/esuldin/dump1090)

[Github original](https://github.com/MalcolmRobb/dump1090)

### Utilisation
Scanner les signaux ADS-B:
```bash
dump1090 --interactive
```
