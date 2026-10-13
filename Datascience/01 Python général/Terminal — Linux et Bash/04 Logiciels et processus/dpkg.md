# dpkg

## Pour faire quoi ?

Consulter ou gérer directement les paquets Debian installés, niveau plus bas que `apt`.

## Arguments

L’option choisit l’action ; elle peut être suivie d’un paquet ou d’un fichier `.deb`.

## Options ou éléments utiles

`-l` liste les paquets ; `-L paquet` liste les fichiers d’un paquet ; `-s paquet` affiche son état ; `-i fichier.deb` installe un fichier Debian local.

## Exemple du cours

```bash
dpkg -l
```

## Vigilances et repères

Le cours le cite comme gestionnaire de paquets. Pour les installations courantes depuis les dépôts, `apt` est généralement plus simple car il gère les dépendances.
