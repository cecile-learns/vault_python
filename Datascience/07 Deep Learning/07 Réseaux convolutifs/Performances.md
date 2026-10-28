

Le premier modèle (modèle A) a été construit en 3 blocs de Conv2D -> MaxPooling. Aucun dropout ou Batch Normalization n'a été implémenté. Ses performances ont été les suivantes :
![[Pasted image 20261026103324.png]]

Le deuxième modèle (modèle B) a été construit en 3 blocs de Conv2D -> BatchNormalization -> Conv2D -> BatchNormalization -> MaxPooling -> dropout et a pris 3x plus de temps à l'entraînement. Ses performances ont été les suivantes :

![[Pasted image 20261026103407.png]]


Il est clair que le **modèle A overfit** énormément : ses performances sur train sont bonnes, mais le test s'effondre. **Le modèle B réussit à réduire cet overfitting**, notamment grâce au Batch Normalization et dropout, bien que sans l'annuler complètement.

En comparaison, notre modèle complet **n'overfit pas et bénéficie d'un entraînement encore plus stable** que le modèle B. En plus de cela, l'Early Stopping permet d'économiser un temps de calcul significatif.