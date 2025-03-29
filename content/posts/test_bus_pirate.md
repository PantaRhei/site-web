+++
title = 'Test_bus_pirate_V3'
date = 2024-11-17T16:31:27+01:00
draft = false
+++



![Resultat de la commande](/images/buspyrate.jpg)

Mon bus pirate (il existe la version 5)
```
Bus Pirate v3b
Firmware v5.10 (r559)  Bootloader v4.4
*************************************
http://dangerousprototypes.com
*****************
.....
*----------*
```

# La connexion
Dans mon cas, j'utilise minicom mais il y a plein d'autres solutions.
Si vous êtez sur linux pensez à ajouter votre utilisateur au groupe dialout (ou autre groupe selon votre distribution).
Perso, sur arch linux, j'ai ajouté  l'utilisateur au groupe uucp.
(cmd: `sudo usermod -a -G [uucp/dialout] $USER`).
IMPORTANT: pensez à reboot.
```bash
minicom -b 115200 -D /dev/tty.usbserial-A10LYGZA
```

Petit script pour trouver le bus pirate (il peut largement être amélioré):
```bash
#!/bin/bash
echo "Find my bus pirate"
echo "Deconnect pirate bus, and press any key to continue... "
read -n 1 -s -r -p ""
ls /dev/tty* > tmp1.txt
echo "Connect pirate bus, and press any key to continue..."
read -n 1 -s -r -p ""
ls /dev/tty* > tmp2.txt
diff tmp1.txt tmp2.txt
rm tmp1.txt tmp2.txt
#Output exemple
#./find_pirate_bus.sh
#Find my bus pirate
#Deconnect pirate bus, and press any key to continue...
#Connect pirate bus, and press any key to continue...
#3a4
#> /dev/tty.usbserial-A10LYGZA
```

# Self test
Pour lancer un self test on doit mettre des jumpers
entre les pin 3.3V et ADC, et entre les pin 5.0V et VPU.

Taper `~` pour lancer le self test.
Output:
```
Disconnect any devices
Connect (Vpu to +5V) and (ADC to +3.3V)
Space to continue
Ctrl
AUX OK
MODE LED OK
PULLUP H OK
PULLUP L OK
VREG OK
ADC and supply
5V(4.95) OK
VPU(4.95) OK
3.3V(3.29) OK
ADC(3.29) OK
Bus high
MOSI OK
CLK OK
MISO OK
CS OK
Bus Hi-Z 0
MOSI OK
CLK OK
MISO OK
CS OK
Bus Hi-Z 1
MOSI OK
CLK OK
MISO OK
CS OK
MODE and VREG LEDs should be on!
Any key to exit
```

# Alumer une led depuis la pin AUX
Schèmas éléctrique:
```
pin AUX du bus pirate -> resistance de 220 Ohm -> led -> GND du bus pirate
```

Lancer une connection sur le bus pirate, tapez `m` puis `9`.
Output:
```
HiZ>m
1. HiZ
2. 1-WIRE
3. UART
4. I2C
5. SPI
6. 2WIRE
7. 3WIRE
8. LCD
9. DIO
x. exit(without change)

(1)>9
Ready
```
Avec `v`, on peut afficher l'état des différents pins (voir exemple).
Avec `A` et `a`, on peut allumer ou éteindre une led.
Output:
```
DIO>A
AUX HIGH
DIO>v
Pinstates:
1.(BR)  2.(RD)  3.(OR)  4.(YW)  5.(GN)  6.(BL)  7.(PU)  8.(GR)  9.(WT)  0.(Blk)
GND     3.3V    5.0V    ADC     VPU     AUX     CLK     MOSI    CS      MISO
P       P       P       I       I       O       I       I       I       I
GND     0.00V   0.00V   0.00V   0.00V   H       L       L       L       L
DIO>a
AUX LOW
DIO>v
Pinstates:
1.(BR)  2.(RD)  3.(OR)  4.(YW)  5.(GN)  6.(BL)  7.(PU)  8.(GR)  9.(WT)  0.(Blk)
GND     3.3V    5.0V    ADC     VPU     AUX     CLK     MOSI    CS      MISO
P       P       P       I       I       O       I       I       I       I
GND     0.00V   0.00V   0.00V   0.00V   L       L       L       L       L
DIO>
```
On voit bien que la colonne 6 passe H (high) à L (low)


# Premier programme
Pour cela, on revient au mode de depart avec `#`.
On peut passer en mode script avec `s`

Pour ecrire un nouveau script, on entre `new`.
La commande `run` permet de lancer le script.
Et pour lister les lignes de code, on utilise `list`.
La doc pour le basic est dispo [ici](http://dangerousprototypes.com/docs/Bus_Pirate_BASIC_script_reference)

Le programme ci-dessous allume et éteint la led de la pin AUX toutes les secondes une dizaine de fois.

```basic
10 PRINT "start prog"
20 AUX 0
30 FOR I=1 TO 10
40 LET A=AUX
50 PRINT "LED= ";A
60 IF A=0 THEN AUX 1 ELSE AUX 0
70 DELAY 1000
80 NEXT I
```
Ça me rappelle le TI Basic (la bonne vieille époque).
Maintenant, il paraît que les lycéens ont Python sur leurs calculatrices.

# UART
Ressource sur UART: [ici](https://book.hacktricks.xyz/todo/hardware-hacking/uart)
On utilisera dans cette exemple une arduino (mkr wifi 1010).
Pour une Uno vous avez [cette lib](https://docs.arduino.cc/learn/built-in-libraries/software-serial/)


Connection:
```
Bus pirate <-> Arduino
GND        <-> GND
MOSI(TX)   <-> RX
MISO(RX)   <-> TX
```
## Lecture sur une arduino (mkr wifi 1010)
```
HiZ>m
1. HiZ
2. 1-WIRE
3. UART
4. I2C
5. SPI
6. 2WIRE
7. 3WIRE
8. LCD
9. DIO
x. exit(without change)

(1)>3
Set serial port speed: (bps)
 1. 300
 2. 1200
 3. 2400
 4. 4800
 5. 9600
 6. 19200
 7. 38400
 8. 57600
 9. 115200
10. BRG raw value

(1)>5
Data bits and parity:
 1. 8, NONE *default
 2. 8, EVEN
 3. 8, ODD
 4. 9, NONE
(1)>
Stop bits:
 1. 1 *default
 2. 2
(1)>
Receive polarity:
 1. Idle 1 *default
 2. Idle 0
(1)>
Select output type:
 1. Open drain (H=Hi-Z, L=GND)
 2. Normal (H=3.3V, L=GND)
## ICI choisir 2 //TODO: remettre au propre
(1)>
Ready
UART>(2)
Raw UART input
Any key to exit
azerty
```

Programme de l'Arduino (sur une Uno, les pins RX/TX peuvent être différents) :
```cpp
//Pin TX/RX
//TX 14
//RX 13

void setup() {
  Serial1.begin(9600); //Serial1 => communication UART
  Serial.begin(9600);  //Serial => débogage

  while (!Serial) {
    ;
  }
  Serial.println("Démarrage de la communication série...");
}

void loop() {
  //Verification d'une entrée utilisateur
  if (Serial.available()) {
    char sendChar = Serial.read();
    Serial1.print(sendChar);
    Serial.print("Envoyé : ");
    Serial.println(sendChar);
  }
}
```


# Ressource

- [PIC24FJ64GA004 FAMILY](https://dlnmh9ip6v2uc.cloudfront.net/datasheets/Components/General%20IC/32103_SPCN.pdf)
- [Manuel bus pirate](http://dangerousprototypes.com/docs/Bus_Pirate)
