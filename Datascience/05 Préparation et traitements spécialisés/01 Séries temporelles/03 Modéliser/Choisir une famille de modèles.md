# Choisir une famille de modèles

| Situation | Point de départ possible |
|---|---|
| Référence simple | dernière valeur, moyenne ou saison précédente |
| Dépendance aux valeurs passées | AR, MA, ARMA |
| Série non stationnaire | ARIMA |
| Saisonnalité explicite | SARIMA |
| Variables explicatives futures disponibles | SARIMAX |
| Plusieurs séries liées | VAR |
| Tendance et saisonnalités avec changements souples | Prophet |

## Avant de choisir

1. définir l'horizon de prévision ;
2. identifier la fréquence et les saisonnalités plausibles ;
3. construire une référence simple ;
4. choisir une validation chronologique ;
5. comparer les modèles avec les mêmes fenêtres et métriques ;
6. contrôler les résidus et la cohérence métier.

Le modèle le plus complexe n'est utile que s'il améliore une référence pertinente sur des données futures.
