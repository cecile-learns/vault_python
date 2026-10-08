print# Opérations courantes

## Syntaxe

```python
moyenne = df['col_valeur'].mean()
total = df['col_valeur'].sum()
minimum = df['col_valeur'].min()
maximum = df['col_valeur'].max()
mediane = df['col_valeur'].median()
modes = df['col_valeur'].mode()
```

## Fonction

- `mean()` : calcule la moyenne.
- `sum()` : calcule la somme.
- `min()` : retourne la valeur minimale.
- `max()` : retourne la valeur maximale.
- `median()` : calcule la médiane.
- `mode()` : retourne une Series contenant la ou les valeurs les plus fréquentes.

## Vigilances & tips

Ces méthodes ignorent les valeurs manquantes par défaut. `mode()` peut retourner plusieurs valeurs.
