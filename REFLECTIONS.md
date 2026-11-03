# Reflections — CO2 time series

## 1. Stage 0 — Checking the "no real data available" assumption

Il aurait suffi de lire la doc de l'API Open-Meteo Air Quality ou de faire un seul appel test avec le paramètre `past_days=90` pour voir si l'historique revenait. Ça prend deux minutes, alors que construire tout un générateur de fausses données à partir d'une hypothèse non vérifiée fait perdre du temps et donne des résultats qui ne veulent rien dire. Une hypothèse sur les données disponibles, c'est quelque chose à tester avant de concevoir le reste.

## 2. Stage 1 — Prediction for Le Mans CO2 over the last 90 days

Ma prédiction : les 90 derniers jours vont d'août à début novembre. En été les plantes absorbent beaucoup de CO2, donc il est au plus bas vers fin septembre, puis il remonte à l'automne quand la végétation ralentit. Je m'attends donc à une courbe qui baisse un peu puis remonte, avec au global une tendance plutôt en hausse.

## 3. Stage 2 — Is the gap-detection code unnecessary?

Non. Zéro trou sur un run, ça veut juste dire que la source était propre cette fois-ci, pas qu'elle le sera toujours : l'API peut changer, renvoyer des valeurs nulles un jour de panne, ou on peut remplacer la source par un vrai capteur qui, lui, perd des mesures. Si on supprime la détection, le jour où un trou arrive, le lissage et la tendance seraient calculés dessus sans que personne ne s'en rende compte. Le code ne coûte presque rien et il protège contre ce cas.


## 4. Stage 3 — Rolling mean vs. exponential smoothing


## 5. Stage 4 — What happens if the API call fails every retry?

Après 4 échecs, `fetch_hourly_co2` lève une `RuntimeError` avant d'écrire `series.csv`, donc l'étape `fetch_series.py` échoue et GitHub Actions arrête le job : `analyze_series.py` et le commit ne sont jamais exécutés. Les deux fichiers dans le repo restent donc ceux de la semaine d'avant. Le dashboard ne casse pas, il affiche des données périmées, et le seul indice visible est la date « Data last analyzed » qui n'avance plus. C'est mieux qu'un dashboard cassé, mais c'est silencieux pour le visiteur.


## 6. Stage 5 — How far would I trust my own projection?


## 7. Stage 6 — Why the pipeline kept working after one change
