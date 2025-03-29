+++
date = '2025-03-18T15:55:16+01:00'
draft = false
title = 'Buspirate_I2C_LCD'
+++

# I2C
## Sniffing mode
Le but de cet article est d'utiliser le Bus Pirate pour
sniffer un bus I2C entre une Arduino Uno et un écran LCD
en I2C. Ensuite, nous injecterons des valeurs pour prendre
le contrôle de l'écran LCD.

Voilà une photo du montage:
![Description de l'image](/images/buspirate_I2C.jpg)

Démarche à suivre pour sniffer un bus I2C:
![Description de l'image](/images/pirate_bus_i2c_sniffer.png)

Dans cet exemple, on clear l'écran LCD et on affiche un nombre à la position du curseur (qui est déjà à (0,0)).

Le montage pour sniffer est le suivant:
```
Bus pirate <-> Ecran LCD
GND        <-> GND
MOSi       <-> SDA
CLK        <-> SCL
```

Ref pour les différents protocoles pour le bus pirate v3: [ici](http://dangerousprototypes.com/docs/Bus_Pirate_I/O_Pin_Descriptions)

## Code de l'arduino
Code de l'Arduino (code qui tourne pour une Arduino Uno):
```c
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2); // I2C address 0x27, 16 column and 2 rows

int c = 0;

void setup()
{
  lcd.init(); // initialize the lcd
  lcd.backlight();
}

void loop()
{
  lcd.clear();                 // clear display
  lcd.setCursor(0, 0);         // move cursor to   (0, 0)
  lcd.print("Hello: ");        // print message at (0, 0)
  lcd.setCursor(2, 1);         // move cursor to   (2, 1)
  lcd.print(c);
  delay(2000);                 // display the above for two seconds
  c++;
  if (c > 10) {
    c = 0;
  }
}

```

Le montage Arduino/LCD est le suivant:
```
Arduino UNO <-> Ecran LCD
GND        <-> GND
A4         <-> SDA
A5         <-> SCL
VCC        <-> VCC
```

Pour le reverse, j'utiliserai ce code ci-dessus.

## Reverse engineering

Pour les captures, j'ai isolé au maximun les lignes de code de l'Arduino pour avoir une correspondance entre la
la ligne de code et la communication I2C.
De plus, on est obligé de le faire ligne par ligne car le buffer de mon bus pirate est limité.


### Capture du clear

Capture pour la ligne `lcd.clear();`
```
[0x4E+0x08+][0x4E+0x0C+][0x4E+0x08+][0x4E+0x18+][0x4E+0x1C+][0x4E+0x18+]
```

### Capture du setCursor(x,y)
L'ecran LCD est une matrice 16x2, on note les coordonnées (colonne, ligne).


Note: il est possible d'avoir un contrôle plus fin sur un carreaux de la  matrice,
aller voir [ici](https://arduinogetstarted.com/tutorials/arduino-lcd-i2c) .

Capture pour la ligne `lcd.setCursor(0, 0);`
```
[0x4E+0x88+][0x4E+0x8C+][0x4E+0x88+][0x4E+0x08+][0x4E+0x0C+][0x4E+0x08+]
```

Capture pour la ligne `lcd.setCursor(1, 0);`
```
[0x4E+0x88+][0x4E+0x8C+][0x4E+0x88+][0x4E+0x18+][0x4E+0x1C+][0x4E+0x18+]
```

Capture pour la ligne `lcd.setCursor(0,1)`
```
[0x4E+0xC8+][0x4E+0xCC+][0x4E+0xC8+][0x4E+0x08+][0x4E+0x0C+][0x4E+0x08+]
```

#### Analyse
Après plusieurs injections, du pirate bus vers l'écran LCD, on en deduit:

*setCursor(x,y)*
Pour changer la valeur de `x`, on écrit dans la zone mémoire `[0x4E+0x88+][0x4E+0x8C+][0x4E+0x88+]`,
puis en envois la donnée `[0x4E+0xp8+][0x4E+0xpC+][0x4E+0xp8+]` avec `p` la valeur de `x`et `x` entre `0x1` et `0xF`

Pour changer la valeur de `y`, on écrit dans la zone mémoire `[0x4E+0xC8+][0x4E+0xCC+][0x4E+0xC8+]`,
puis en envois la donner `[0x4E+0xp8+][0x4E+0xpC+][0x4E+0xp8+]` avec `p` la valeur de `y`et `y` entre `0x1` et `0xF`

En résumé, on a:
setCursor(p,0): `[0x4E+0x88+][0x4E+0x8C+][0x4E+0x88+][0x4E+0xp8+][0x4E+0xpC+][0x4E+0xp8+]`

setCursor(0,p): `[0x4E+0xC8+][0x4E+0xCC+][0x4E+0xC8+][0x4E+0xp8+][0x4E+0xpC+][0x4E+0xp8+]`


### Capture d'un print
Pour la capture, on devait clear l'écran à chaque affichage.
Il y a donc un clear puis un print.
Capture pour la ligne `lcd.print(c);` avec un curseur (0,0):
```
//affichage d'un 1
[0x4E+0x08+][0x4E+0x0C+][0x4E+0x08+][0x4E+0x18+][0x4E+0x1C+][0x4E+0x18+][0x4E+0x39+][0x4E+0x3D+][0x4E+0x39+][0x4E+0x19+][0x4E+0x1D+][0x4E+0x19+]

//affichage d'un 6
[0x4E+0x08+][0x4E+0x0C+][0x4E+0x08+][0x4E+0x18+][0x4E+0x1C+][0x4E+0x18+][0x4E+0x39+][0x4E+0x3D+][0x4E+0x39+][0x4E+0x69+][0x4E+0x6D+][0x4E+0x69+]
```
On peut retirer le clear la cmd clear, on a donc:
```
//affichage d'un 1
[0x4E+0x39+][0x4E+0x3D+][0x4E+0x39+][0x4E+0x19+][0x4E+0x1D+][0x4E+0x19+]

//affichage d'un 6
[0x4E+0x39+][0x4E+0x3D+][0x4E+0x39+][0x4E+0x69+][0x4E+0x6D+][0x4E+0x69+]
```

#### Analyse
Tout comme le setCursor, on a une zone mémoire qui est écrite avant l'envoi de la donnée:
`[0x4E+0x39+][0x4E+0x3D+][0x4E+0x39+]` puis une donnée pour l'affichage `[0x4E+0xp9+][0x4E+0xpD+][0x4E+0xp9+]`
avec `p` la valeur à afficher (p étant un chiffre entre 0-9). A noter, que l'on analyse que les chiffres mais
on pourrait procéder de la même façon pour les lettres.


## Envoi de commande
On envois un 6 à l'écran LCD.

Etat initial:
![Description de l'image](/images/buspirate_i2c_injection_0.jpg)

Injection du chiffre 6:
![Description de l'image](/images/pirate_bus_i2c_inject_6.png)

Etat final:
![Description de l'image](/images/buspirate_i2c_injection_6.jpg)

Video youtube [ici](https://youtube.com/shorts/zQPMAxmdo_I)

# Ref

Librerie pour l'écran LCD en I2C: [ici](https://github.com/marcoschwartz/LiquidCrystal_I2C)
(Guide sur l'utilisation de l'écran [ici](https://arduinogetstarted.com/tutorials/arduino-lcd-i2c))
