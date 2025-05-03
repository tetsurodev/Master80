---
title: "Accueil"
---

![Console Sega Master System](img/index/sms.jpg)

# __ENJOY !!!__
### __Master80__ est un site dédié à la programmation de la console de jeu Sega Master System en langage Assembleur Z80 :)
---
<!-- ################################################ -->
<!-- #################    DEVLOG   ################## -->
<!-- ################################################ -->

# Mise à jour du site
_Samedi 03 mai à 02:48_

Ca fait plus d'une semaine que je n'ai pas mis à jour le site, alors j'ai rédigé les 3 derniers devlogs et j'envoie tout ça de suite :) 

---

# Simulation 
_Mercredi 30 avril à 09:50_  

J'ai testé le simulateur de circuits SimulIDE (Windows /Linux /Mac).  
Ce simulateur permet de créer et tester des circuits électroniques en plaçant virtuellement des puces, bus, etc...  
Evidemment j'ai commencé à concevoir un circuit avec un Z80, une RAM, une ROM, des BUS et un afficheur LED pour commencer à _m'approcher_ d'un fonctionnement d'une Master System très basique. Je suis encore loin d'un système opérationnel mais ça me permet déjà de mieux visualiser les comportements électroniques à leurs sources. Plus tard je pourrais injecter un code simple dans la ROM virtuelle pour qu'il soit interprété par le Z80.

---
# Z80 Simulator IDE
_Mardi 29 avril à 15:00_  

J'ai revu la prise en main de cet excellent simulateur du Z80, il permet de voir pas à pas le comportement de notre code, et mieux comprendre ce qu'il se passe en mémoire, sur les registres, le I/O ports, etc...

---
# VBLANK
_Lundi 28 avril à 14:00_  

J'ai revu le VBlank et son fonctionnement, tout en rédigeant un tuto sur la mise en place d'un buffer sur Master System. 

---
# Une journée pour le site
_Samedi 19 avril 2025 à 01:47_

Aujourd'hui je n'ai pas travaillé sur le Z80 car je me suis occupé du site. J'ai installé le thème, il me plait, j'espère que vous aussi.

```Z80
    ld hl, message
    /.../
message:
    db "Bonne nuit", 0
```

---
# Hello World 
_Vendredi 18 avril 2025 à 16:06_

Aujourd'hui je met en ligne un site dédié à la programmation de la console de jeu Sega Master System (SMS). 
Avec ce site je veux explorer, apprendre et partager mes expériences sur la programmation de la SMS en Assembleur Z80. Je vais tenir un devlog pour garder une trace de mon évolution et partager petits pas après petits pas mes avancées.
Je mettrais en ligne de la documentation, mes prototypes et qui sait, un jeu fini :)

---