il est a
### Séries additives

Comme vu dans la partie II. une série temporelle additive se définie de la façon suivante:

X𝑡=T𝑡+S𝑡+𝜖𝑡

où:

- X𝑡 est notre série temporelle
- T𝑡 est la tendance
- S𝑡 est la saisonnalité
- 𝜖𝑡 est le résidus
- toutes dépendant du temps 𝑡

Mathématiquement, le résidus se calcule donc très simplement:

𝜖𝑡=X𝑡−T𝑡−S𝑡

Autrement dit, il suffit simplement de soustraire à la série sa tendance et sa saisonnalité pour obtenir le résidus.

### Séries multiplicatives

Pour une série multiplicative le principe est équivalent. La définition mathématique d'une série temporelle est:

X𝑡=T𝑡∗S𝑡∗𝜖𝑡

De même, mathématiquement, le résidus se calcule donc très simplement:

𝜖𝑡=X𝑡T𝑡∗S𝑡

## Afficher le résidus de la serie

```python
decomp = seasonal_decompose(airpass, model="multiplicative")
residus = decomp.resid

plt.figure(figsize=(10, 4))
plt.plot(residus)
plt.title("Résidus de la série")
plt.xlabel("t")
plt.show();
```
![[Pasted image 20261017101655.png]]

### Que cherche-t-on sur le graphique des résidus ?
![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20sans-serif%22%20font-weight%3D%22430%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(26%2C%2028%2C%2031\)%22%20viewBox%3D%220%200%20170%2095%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cpath%20d%3D%22M16%208V80H164%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%221%22%20opacity%3D%22.4%22%2F%3E%3Cpath%20d%3D%22M16%2044H164%22%20stroke%3D%22currentColor%22%20stroke-dasharray%3D%224%203%22%20opacity%3D%22.35%22%2F%3E%3Cpolyline%20points%3D%2216.0%2C44.0%2019.3%2C41.4%2022.6%2C54.6%2025.9%2C31.9%2029.2%2C45.2%2032.5%2C52.1%2035.8%2C36.7%2039.1%2C50.2%2042.4%2C36.5%2045.7%2C44.7%2049.0%2C55.1%2052.3%2C31.8%2055.6%2C47.5%2058.9%2C45.2%2062.2%2C41.5%2065.5%2C52.8%2068.8%2C31.5%2072.1%2C47.3%2075.4%2C52.1%2078.7%2C35.3%2082.0%2C49.6%2085.3%2C37.7%2088.6%2C46.1%2091.9%2C53.5%2095.2%2C30.6%2098.5%2C48.9%20101.8%2C46.2%20105.1%2C40.9%20108.4%2C51.2%20111.7%2C32.0%20115.0%2C49.3%20118.3%2C51.3%20121.6%2C33.8%20124.9%2C49.8%20128.2%2C39.0%20131.5%2C46.8%20134.8%2C51.5%20138.1%2C30.0%20141.4%2C50.8%20144.7%2C46.5%20148.0%2C39.9%20151.3%2C50.1%20154.6%2C33.0%20157.9%2C51.0%20161.2%2C49.9%22%20fill%3D%22none%22%20stroke%3D%22%232583bd%22%20stroke-width%3D%221.7%22%20stroke-linejoin%3D%22round%22%2F%3E%3C%2Fsvg%3E)![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20sans-serif%22%20font-weight%3D%22430%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(26%2C%2028%2C%2031\)%22%20viewBox%3D%220%200%20170%2095%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cpath%20d%3D%22M16%208V80H164%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%221%22%20opacity%3D%22.4%22%2F%3E%3Cpath%20d%3D%22M16%2044H164%22%20stroke%3D%22currentColor%22%20stroke-dasharray%3D%224%203%22%20opacity%3D%22.35%22%2F%3E%3Cpolyline%20points%3D%2216.0%2C64.9%2019.3%2C59.1%2022.6%2C68.7%2025.9%2C60.2%2029.2%2C57.6%2032.5%2C66.1%2035.8%2C55.6%2039.1%2C56.4%2042.4%2C63.0%2045.7%2C51.5%2049.0%2C55.5%2052.3%2C59.3%2055.6%2C47.8%2058.9%2C54.5%2062.2%2C55.1%2065.5%2C44.7%2068.8%2C53.3%2072.1%2C50.5%2075.4%2C42.1%2078.7%2C51.8%2082.0%2C45.8%2085.3%2C40.2%2088.6%2C49.7%2091.9%2C41.1%2095.2%2C38.7%2098.5%2C47.1%20101.8%2C36.6%20105.1%2C37.5%20108.4%2C44.0%20111.7%2C32.4%20115.0%2C36.6%20118.3%2C40.2%20121.6%2C28.7%20124.9%2C35.6%20128.2%2C36.0%20131.5%2C25.7%20134.8%2C34.4%20138.1%2C31.4%20141.4%2C23.2%20144.7%2C32.8%20148.0%2C26.7%20151.3%2C21.2%20154.6%2C30.8%20157.9%2C22.0%20161.2%2C19.8%22%20fill%3D%22none%22%20stroke%3D%22%232583bd%22%20stroke-width%3D%221.7%22%20stroke-linejoin%3D%22round%22%2F%3E%3C%2Fsvg%3E)
![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20sans-serif%22%20font-weight%3D%22430%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(26%2C%2028%2C%2031\)%22%20viewBox%3D%220%200%20170%2095%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cpath%20d%3D%22M16%208V80H164%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%221%22%20opacity%3D%22.4%22%2F%3E%3Cpath%20d%3D%22M16%2044H164%22%20stroke%3D%22currentColor%22%20stroke-dasharray%3D%224%203%22%20opacity%3D%22.35%22%2F%3E%3Cpolyline%20points%3D%2216.0%2C44.0%2019.3%2C32.2%2022.6%2C30.3%2025.9%2C25.9%2029.2%2C23.8%2032.5%2C34.3%2035.8%2C42.9%2039.1%2C47.7%2042.4%2C59.2%2045.7%2C64.8%2049.0%2C59.5%2052.3%2C57.4%2055.6%2C52.0%2058.9%2C37.6%2062.2%2C30.7%2065.5%2C29.1%2068.8%2C23.5%2072.1%2C27.2%2075.4%2C39.1%2078.7%2C44.5%2082.0%2C52.1%2085.3%2C63.5%2088.6%2C62.9%2091.9%2C57.9%2095.2%2C56.5%2098.5%2C46.0%20101.8%2C33.1%20105.1%2C30.4%20108.4%2C26.7%20111.7%2C23.3%20115.0%2C32.6%20118.3%2C42.3%20121.6%2C46.7%20124.9%2C57.6%20128.2%2C65.0%20131.5%2C60.2%20134.8%2C57.5%20138.1%2C53.5%20141.4%2C39.5%20144.7%2C31.0%20148.0%2C29.6%20151.3%2C24.1%20154.6%2C25.9%20157.9%2C37.8%20161.2%2C44.1%22%20fill%3D%22none%22%20stroke%3D%22%232583bd%22%20stroke-width%3D%221.7%22%20stroke-linejoin%3D%22round%22%2F%3E%3C%2Fsvg%3E)![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20sans-serif%22%20font-weight%3D%22430%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(26%2C%2028%2C%2031\)%22%20viewBox%3D%220%200%20170%2095%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cpath%20d%3D%22M16%208V80H164%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%221%22%20opacity%3D%22.4%22%2F%3E%3Cpath%20d%3D%22M16%2044H164%22%20stroke%3D%22currentColor%22%20stroke-dasharray%3D%224%203%22%20opacity%3D%22.35%22%2F%3E%3Cpolyline%20points%3D%2216.0%2C44.0%2019.3%2C43.0%2022.6%2C45.8%2025.9%2C42.7%2029.2%2C43.4%2032.5%2C46.9%2035.8%2C40.4%2039.1%2C45.6%2042.4%2C46.1%2045.7%2C38.9%2049.0%2C48.9%2052.3%2C43.0%2055.6%2C39.8%2058.9%2C51.3%2062.2%2C38.5%2065.5%2C43.5%2068.8%2C50.9%2072.1%2C34.8%2075.4%2C49.2%2078.7%2C46.9%2082.0%2C34.3%2085.3%2C54.4%2088.6%2C40.1%2091.9%2C38.0%2095.2%2C56.5%2098.5%2C33.2%20101.8%2C45.5%20105.1%2C53.6%20108.4%2C29.2%20111.7%2C54.2%20115.0%2C45.8%20118.3%2C30.7%20121.6%2C60.4%20124.9%2C35.7%20128.2%2C38.1%20131.5%2C60.9%20134.8%2C27.1%20138.1%2C49.3%20141.4%2C54.6%20144.7%2C24.0%20148.0%2C60.2%20151.3%2C42.9%20154.6%2C28.5%20157.9%2C66.3%20161.2%2C29.9%22%20fill%3D%22none%22%20stroke%3D%22%232583bd%22%20stroke-width%3D%221.7%22%20stroke-linejoin%3D%22round%22%2F%3E%3C%2Fsvg%3E)

- A — Résidus sans structure visible : c'est ce que l'on recherche généralement. La décomposition a capté les grandes régularités.
    
- B — Tendance persistante : la tendance a probablement été mal estimée.
    
- C — Oscillations régulières : il reste vraisemblablement une saisonnalité.
    
- D — Dispersion croissante : l'amplitude des variations n'est pas correctement prise en compte. Une transformation logarithmique ou un modèle multiplicatif peut être pertinent.

**Le résidu est aussi un outil de diagnostic qui permet de savoir si l'on a correctement représenté le phénomène ou s'il reste quelque chose à expliquer.**
