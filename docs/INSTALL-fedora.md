# Installer et lancer Sky Sentinel sur Fedora

Testé sur Fedora 39 (CSFML 2.5.2, GCC 13).

```bash
sudo dnf install gcc make CSFML-devel
git clone git@github.com:FuniclemDev/my-hunter-source.git
cd my-hunter-source
make
./my_hunter        # ./my_hunter -h pour l'aide
```

Le jeu charge ses ressources par chemin relatif (`ressources/…`) : lancez-le depuis la racine du dépôt.
`make clean` supprime le binaire.
