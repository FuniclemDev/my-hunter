# Architecture — Sky Sentinel (My Hunter)

## Organisation des fichiers

```
main.c            Point d'entrée : état initial, boucle principale, game over
include/my.h      Structure d'état unique (struct get_form_graph) et prototypes
LIB/create.c      Création des sprites, textures, sons ; aide (-h)
LIB/draw.c        Rendu de l'écran d'accueil et de la partie, défilement du décor
LIB/events.c      Événements (souris, clavier), collisions, score, difficulté
ressources/       Sprites (avion, soucoupe, explosion, viseur), fonds, police, musique
```

## Boucle de jeu

```
main
 ├─ formget_graph()          état initial (positions, vitesses, score, record)
 ├─ sfRenderWindow_create()  fenêtre 1366×768, 60 i/s
 ├─ check_error()            chargement des ressources
 └─ while (fenêtre ouverte)
     ├─ pollEvent → analyse_events()   clic, Espace, M, S, Entrée, Échap
     └─ get_loop()
         ├─ home == 1 → draw_home()    menu
         └─ run  == 1 → draw_game_run() partie
```

Tout l'état du jeu tient dans une seule structure passée par pointeur : c'est la façon la plus simple de partager
l'état entre fichiers sans variable globale (interdites par la norme Epitech).

## Mécaniques

- **Avion** : à chaque image, sa position verticale se rapproche du curseur de 3 px, avec une rotation de ±10° pendant la montée ou la descente.
- **Animation** : une `sfClock` déclenche un tick toutes les 0,15 s, qui décale le rectangle de texture (sprite sheet) de l'avion et de la soucoupe, et fait osciller l'avion.
- **Tir** : un clic bascule l'avion sur la frame de tir ; la soucoupe est touchée si elle est devant l'avion et à moins de 50 px de sa hauteur.
- **Difficulté** : quand le score dépasse le palier (6 au départ), le palier double et la vitesse des soucoupes augmente de 1.
- **Défilement** : deux copies du fond se suivent ; quand l'une sort à gauche (x ≤ -2300), elle est replacée derrière l'autre.
- **Esquive** : Espace lance une séquence de 4 frames (agrandissement puis retour) pendant laquelle les collisions sont ignorées.
