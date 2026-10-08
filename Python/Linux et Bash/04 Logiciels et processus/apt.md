# apt

## Pour faire quoi ?

Rechercher, installer, mettre à jour ou retirer des paquets sur une distribution Debian ou Ubuntu.

## Arguments

La première position indique l’action (`update`, `install`, etc.), puis viennent les noms de paquets quand nécessaire.

## Options ou éléments utiles

`update` actualise l’index ; 
`install paquet` installe ; 
`upgrade` met à jour les paquets ; 
`remove paquet` retire ; 
`search mot` cherche ;
`list --installed` liste ; 
`show paquet` décrit.

## Exemple du cours

```bash
sudo apt update
sudo apt install nom-du-paquet
sudo apt upgrade
apt list --upgradable
 
```

## Vigilances et repères

`apt update` ne met pas les logiciels à jour à lui seul : il actualise la liste disponible. Les opérations système demandent généralement `sudo`.
