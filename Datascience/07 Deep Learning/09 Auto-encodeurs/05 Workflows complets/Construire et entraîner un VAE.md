# Construire et entraîner un VAE

## Pour faire quoi ?

Apprendre un espace latent continu permettant de reconstruire puis générer de nouvelles observations.

## Pipeline mental

```text
entrée
→ z_mean et z_log_var
→ échantillonnage z
→ decoder
→ reconstruction
→ reconstruction loss + KL loss
```

## À adapter

- forme d'entrée ;
- dimension latente ;
- architecture de l'encoder et du decoder ;
- pondération éventuelle de la divergence KL ;
- loss de reconstruction.

## Paramètres et hyperparamètres

| Paramètre | Effet |
|---|---|
| dimension latente | richesse et facilité de visualisation |
| unités cachées | capacité du modèle |
| learning rate | stabilité de l'apprentissage |
| poids de la KL loss | régularité de l'espace latent contre fidélité des reconstructions |
| batch size | stabilité de l'estimation et mémoire |

## Vigilances & tips

Inspecter séparément la reconstruction loss et la KL loss : une loss totale seule ne montre pas quel terme domine.
