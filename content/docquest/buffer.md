---
title: Buffer
description: Configurer un buffer sur Master System
date: 
---

# Configurer un buffer

## Définition
> DATAS --> __RAM__ --> VRAM  
> 
Le buffer est un emplacement en __RAM__, sur lequel on peut stocker des données pour les mettre à jour avant de les envoyer _au bon moment_ à la VRAM du VDP .

## VBLANK, le bon moment
> Frame 1 --> __VBLANK__ -> Frame 2 --> __VBLANK__ --> Frame 3 ...  

Le VDP s'occupe de dessiner l'écran de jeu 60 fois par seconde. A chaque fin de dessin d'un écran le rayon doit se recaller en haut à gauche de l'écran pour de nouveau dessiner l'écran suivant. C'est à ce moment là, lors du recalage du rayon, appelé VBLANK, qu'il faut envoyer les données stockées mises à jour en RAM vers la VRAM.

## Connaitre le moment du VBLANK
```z80
; Etat du Status Register du VDP :
_______________
bits | 76543210
_______|||_____
       100----- [7] VBLANK en cours (1 = oui)
       010----- [6] Sprite Overflow
       001----- [5] Sprite Collision
                [-] Bits non utilisés
``` 
L'état du VDP est stocké sur un registre dans le VDP, le __Status Register__, il ne fait pas parti des registres accessibles.  
Ce registre de 8 bits est mis à jour en permanance, on va alors le lire pour savoir quand le bit 7 passera à zéro.  
Ce bit 7 représente l'état du VBLANK. Tant que le VDP dessine l'écran, le bit 7 est à 1, puis il passe à zéro lorsque le rayon doit se recaler, c'est __LE moment du VBLANK__, c'est à ce moment là qu'on envoie les données de la RAM vers la VRAM.

## La durée d'un VBLANK
(à faire)

## VBLANK Handler
En __Mode 1__, quand une interruption comme le __VBLANK__ se produit, le processeur va aller __systématiquement et automatiquement__ lire à l'adresse `$0038` puis exécuter les instructions qui s'y trouvent.  

`Si on souhaite un comportement durant les interruptions :`
```Z80
    org $0038
    jp vblank_routine

    ; code ...

    org $c000
vblank_routine:
    ; code de la routine ...
    reti    
```

`Si on ne souhaite aucun comportement durant les interruptions :` 
```Z80    
    org $0038
    reti
```

## Quelles données stocker et mettre à jour ?
Comme le VDP s'occupe de la partie graphique, on peut donc utiliser le Buffer pour :  
* Charger une Tile
* Déplacer un Sprite
* Mettre à jour l'état d'une Tile pour l'animer
* Charger / Changer une palette de couleurs
* Mettre à jour le scrolling Horizontal / Vertical

## La RAM
> __Emplacement de la RAM :__ `$c000`  
>__Taille de la RAM :__ 8 Ko étendus de `$c000` à `$dfff`  
  
C'est à cet emplacement qu'il faut écrire ses données, donc stocker ses _variables_, pour les mettre à jour et attendre de les envoyer plus tard vers la VRAM.  

## Exemples 

Pur Z80 (sans commentaires)
```z80
wait_vblank:
    in a,($bf)
    bit 7,a
    jr z,wait_vblank

    ld a,(10h)
    ld (posx),a
    ld a,(50h)
    ld (posy),a

    org $c000
posx: ds 1
posy: ds 1
```

Pur Z80 (avec commentaires)
```z80
; On prépare 2 emplacements en RAM puis on leur donne une valeur par la suite.
 
    ld a,(10h)      ; La future valeur de la variable posx 
    ld (posx),a     ; La valeur 10h est envoyée à $c000
    ld a,(50h)      ; La future valeur de la variable posy 
    ld (posy),a     ; La valeur 50h est envoyée à $c001

    org $c000       ; Emplacement de la RAM
posx: ds 1          ; ds (Define Space) de 1 octet en $c000
posy: ds 1          ; ds (Define Space) de 1 octet en $c001
```

Avec WLA-DX
```Z80
.section "RAM" slot RAM
posx: ds 1
posy: ds 1
.ends

    ld a,(10h)
    ld (posx),a
    ld a,(50h)
    ld (posy),a
```

## Analyse et débugguage
(point d'arret avec Emulicious)
(Possible avec l'IDE Z80 ?)

## Récapitulatif

__CODAGE__ :
* VDP init > Reg $01 > Bit 5 = 1
* IRQ > IM1 > EI
* (BUFFER) : RAM > org $c000 > vblank_routine 
* Wait VBLANK > Status Registre > Bit 7 = 0
* Compilation > Création de la ROM 

__LIVE :__  
* Lecture de la ROM
* VDP initialisé
* Ecriture des futures données en RAM
* VDP > Dessine Frame 1 
* STATUS REGISTER > Wait VBlank
* IRQ > Vecteur $0038 > vblank_routine
* RAM > VRAM
* VDP > Frame 2 > Dessin de l'écran mis à jour avec les nouvelles données  