# Cartographie optimale — optimisation de tournées sous contrainte de batterie

Projet de **recherche opérationnelle** : un robot autonome doit parcourir un maximum de rues
distinctes d'une agglomération pour y collecter des mesures (qualité de l'air, bruit, état de la
chaussée). Sa batterie est limitée : il doit revenir se recharger à sa borne, qu'il faut également
**placer** sur le réseau. L'objectif est de maximiser le nombre de rues différentes parcourues dans
le nombre de jours imparti.

## Modélisation

Chaque ville est fournie sous forme de **graphe** au format JSON : intersections (sommets), routes
(arêtes) avec longueur et sens unique éventuel, capacité de batterie et nombre de jours.

- Construction du graphe orienté avec **NetworkX**, en respectant les sens uniques ;
- calcul des distances de chaque intersection à la borne par **plus court chemin** (Dijkstra) ;
- choix de la borne de recharge, puis construction d'un itinéraire jour par jour.

## Heuristique

`evaluate_node.py` note chaque voisin candidat :

- **pénalité forte** si la route a déjà été parcourue ;
- **bonus** si le voisin ouvre sur des routes inexplorées ;
- **pénalité** proportionnelle à la longueur de la route ;
- **rappel vers la borne** dès que la batterie descend sous 50 %, d'autant plus fort que la réserve
  est faible.

Plusieurs tirages sont effectués (dont la position de la borne) et la **meilleure solution valide**
est conservée ; son score est inscrit dans le nom du fichier de sauvegarde.

## Contenu

| Fichier | Rôle |
|---|---|
| `starter.py` | boucle de résolution, évaluation, sauvegarde de la meilleure solution |
| `evaluate_node.py` | fonction de score d'un déplacement |
| `test_solution.py` | vérification de la validité d'un itinéraire et calcul du score |
| `display.py` | animation de l'itinéraire (turtle) |
| `datasets/` | jeux de données routiers (Pac-Man, Manhattan, Paris, Londres, …) |
| `solutions/` | itinéraires produits, horodatés et notés |
| `énoncé.md` | sujet du projet |

## Exécution

```bash
pip install networkx
python starter.py      # choix du jeu de données, puis sauvegarde interactive de la solution
```

## Note

Projet réalisé en binôme dans le cadre du module de recherche opérationnelle.
