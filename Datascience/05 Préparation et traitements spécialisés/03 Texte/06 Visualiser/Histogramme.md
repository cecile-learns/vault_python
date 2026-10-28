
Une autre façon de visualiser les mots les plus fréquemment utilisés par les internautes consiste à créer un histogramme indiquant le nombre de fois où le mot a été utilisé. Intuitivement, la construction de ce diagramme nécessite d'abord de compter le nombre d'occurrences des mots.

La classe `Counter` du module des collections transforme une liste en un dictionnaire où un élément de la liste (clé) est associé à son nombre d'occurrences.

La méthode `.most_common(int)` pour un dictionnaire retourne une liste des premiers mots int les plus utilisés avec leur nombre d'occurrences.

La fonction barplot(x,y) du module **seaborn** permet de construire un histogramme où la paire (x,y) représente respectivement les abscisses et les hauteurs de bâton de l'histogramme.

```Python
from collections import Counter
import seaborn as sns 
chaine = ' '.join(i.lower() for i in df_neg.Text)
dico = Counter(chaine.split())
mots = [m[0] for m in dico.most_common(15)]
freq = [m[1] for m in dico.most_common(15)]

plt.figure(figsize= (10,6))
sns.barplot(x=mots, y=freq)
plt.title('15 mots les plus fréquemment employés par les internautes laissant des mauvais commentaires')
```
![[Pasted image 20261014111724.png]]