## Contexte

Nous sommes le **14 octobre 2030** dans le TGV de retour vers Paris. Bob a proposé d'orienter le développement du Ticket #1664 en **TDD (Test Driven Development)**, avec l'arrivée de **Freddie**, apprenti QA sous la responsabilité de David.

L'objectif de ce document est de :

- proposer à Bob un plan d'action TDD intégrant Freddie dans l'équipe ;
- présenter une batterie structurée de cas de test (16 au total) pour le moteur de recherche d'images ;
- identifier un test volontairement rouge (qui échoue avec l'état actuel du code) afin de piloter la prochaine incrémentation.

## 1. Courriel à Bob

> **Objet** : Ticket #1664 – Plan d'action TDD et intégration de Freddie
>
> Bonjour Bob,
>
> Suite à ta proposition de piloter le Ticket #1664 en TDD et à l'arrivée de Freddie dans l'équipe, je te partage ci-joint une première version de notre stratégie de tests, ainsi qu'un ensemble de cas de test structurés pour le moteur de recherche d'images.
>
> L'idée est la suivante :
>
> - partir des quatre fonctionnalités clés (recherche simple, recherche RegEx, classement, recherche multiple),
> - définir pour chacune une batterie de tests unitaires et fonctionnels,
> - utiliser les tests comme langage commun entre Dev et QA (toi, David, Freddie, et moi),
> - faire écrire à Freddie une partie significative des tests (sous ta supervision et celle de David),
> - intégrer Freddie sur la formalisation des cas de test et l'automatisation (tests API et tests de charge),
>
> Je détaille ci-après l'état actuel du développement et une première liste de cas de tests. Si tu valides l'approche, je propose que Freddie et David la déclinent ensuite en scripts exécutables (par exemple via pytest + outillage de charge), avec revue par toi et moi.
>
> Je t'ai également joint un Gantt mis à jour intégrant le temps nécessaire à la conception et à l'automatisation de ces tests.
>
> Dis-moi si cette approche te convient ou si tu souhaites ajuster la granularité des tests ou leur priorisation.
>
> Merci pour ton soutien,
>
> Alice

## 2. État actuel du développement

- Un premier endpoint `myImageFromKeyword/<name>/` est disponible, avec recherche exacte sur la table d'index.
- La recherche RegEx est partiellement implémentée mais sans optimisation ni gestion poussée des erreurs d'expression régulière.
- Le classement des résultats est encore naïf (ordre de la base ou tri aléatoire).
- La recherche multiple par mots-clés (`myImagesFromManyKeywords/<name>/`) n'est qu'esquissée : séparation des mots-clés reconnue, mais sélection d'URLs distinctes non garantie.

## 3. Stratégie TDD globale

- **Niveau 1 – Tests unitaires** :
  - Comportement des fonctions de recherche (mot-clef exact, RegEx, recherche multiple).
  - Gestion des cas limites (aucun résultat, doublons d'URL, mots-clefs inconnus).
- **Niveau 2 – Tests d'intégration** :
  - Appels HTTP aux endpoints WebAPI.
  - Format des réponses (schéma JSON, codes d'erreur).
- **Niveau 3 – Tests de non-régression** :
  - Jeux de données de référence pour vérifier que les modifications n'introduisent pas de régressions sur les fonctionnalités clés.

## 4. Batterie de cas de test (16 tests)

Chaque cas décrit l'objectif, une instance de test et les principaux `assert_true` associés (sur les réponses JSON/HTTP ou sur des fonctions internes du moteur).

| ID   | Objectif principal                                      | Instances de test (exemples)                         | Assert(s) clé(s)                                  |
|------|---------------------------------------------------------|------------------------------------------------------|---------------------------------------------------|
| T01  | Recherche simple – mot-clef exact, 1 résultat           | name = "oyster"                                     | 1 URL retournée, contient mot-clef "oyster"     |
| T02  | Recherche simple – mot-clef inexistant                  | name = "unicorn"                                    | 0 résultat, code HTTP 200, liste vide            |
| T03  | Recherche simple – casse insensible (si spécifié)       | name = "Sea_Bream" / "sea_bream"                  | Résultats identiques                              |
| T04  | Recherche simple – plusieurs résultats triés            | name = "crab"                                      | Liste triée par pertinence non aléatoire         |
| T05  | Recherche RegEx – motif préfixe                         | name = "^sea"                                      | URLs dont un mot-clef commence par "sea"       |
| T06  | Recherche RegEx – motif suffixe                         | name = "crab$"                                     | URLs dont un mot-clef finit par "crab"         |
| T07  | Recherche RegEx – motif invalide                        | name = "[abc"                                      | Erreur contrôlée, code HTTP 400                  |
| T08  | Recherche multiple – 2 mots-clefs distincts             | name = "sea%20bream%1664oyster"                    | 2 URLs distincts, bons mots-clefs                |
| T09  | Recherche multiple – 4 mots-clefs (exemple du sujet)    | name = "sea%20bream%1664oyster%1664king%20crab%1664caviar" | 4 URLs distincts, 1 mot-clef par URL    |
| T10  | Recherche multiple – mot-clef inexistant au milieu      | name = "sea%20bream%1664xxx%1664king%20crab"       | Erreur claire ou URL manquante documentée        |
| T11  | Classement – critère de pertinence stable               | même requête exécutée 2 fois                        | Même ordre de résultats entre exécutions         |
| T12  | Performance – temps de réponse borne sur petit dataset  | 100 requêtes séquentielles                          | Latence < seuil défini                            |
| T13  | Performance – temps de réponse sous charge modérée      | 1000 requêtes réparties                             | Taux d'erreur acceptable, latence raisonnable    |
| T14  | Robustesse – entrée vide                                | name = ""                                          | Code HTTP 400, message explicite                 |
| T15  | Robustesse – encodage spécial                           | name avec espaces (%20) et séparateurs (%1664)      | Décodage correct, pas d'erreur interne           |
| T16* | TDD – cas rouge : recherche multiple + contrainte forte | 4 mots-clefs, banque partielle                      | Échec actuel attendu (voir ci-dessous)           |

`T16*` est le test rouge TDD : il spécifie un comportement cible que le code actuel ne satisfait pas encore.

## 5. Cas de test rouge (T16*) – TDD

**Objectif** : garantir que la recherche multiple retourne, pour chaque mot-clef demandé, un **URL distinct** parmi un ensemble limité d'URLs, même lorsque la banque d'images est partiellement couverte.

- **Instance** :
  - Banque d'images réduite à quelques dizaines d'URLs.
  - Requête : `.../myImagesFromManyKeywords/sea%20bream%1664oyster%1664king%20crab%1664caviar/`.
- **Comportement attendu** :
  - 4 URLs distincts.
  - Chaque URL contient exactement un des mots-clefs demandés dans ses métadonnées.
  - Pas de doublon d'URL même si un même URL pourrait contenir plusieurs mots-clefs.

**Pourquoi ce test échoue aujourd'hui ?**

L'implémentation actuelle de la recherche multiple (version naïve) :

- effectue des recherches successives par mot-clef ;
- concatène les listes de résultats sans contrainte forte d'unicité par URL ;
- ne garantit pas qu'un URL ne soit pas réutilisé pour plusieurs mots-clefs.

Le test `T16*` formalise précisément cette contrainte d'unicité et nous obligera à :

- introduire une logique de sélection d'URLs garantissant des résultats distincts ;
- ajouter une contrainte de sélection d'URLs distinctes dans la logique de recherche multiple ;
- documenter le comportement en cas d'impossibilité (par exemple, si la banque ne contient pas assez d'URLs distincts).

## 6. Localisation des asserts et schéma général

- Les **tests unitaires** seront écrits en priorité en Python (par exemple avec `pytest`) et exécutés en CI :
  - assert sur les valeurs retournées (listes d'URLs, codes HTTP, messages d'erreur) ;
  - assert sur l'ordre des résultats pour le classement.
- Les **tests d'intégration** s'appuieront sur des appels HTTP simulés :
  - asserts sur le schéma de réponse (présence des champs attendus) ;
  - asserts sur la tolérance aux entrées invalides.

Schéma TDD :

1. Écriture du test (par Freddie ou David, revue par Bob / Alice).
2. Exécution du test – il échoue (cas de T16* notamment).
3. Implémentation / ajustement du code.
4. Re-exécution du test jusqu'au passage au vert.
5. Intégration du test dans la suite de non-régression.

## 7. Gantt mis à jour avec Freddie (QA)

Freddie est intégré principalement sur la formalisation et l'automatisation des tests, en binôme avec David.

```{.mermaid}
gantt
    dateFormat  YYYY-MM-DD
    title       Ticket #1664 – Planning avec TDD et Freddie

    section Développement
    Implémentation endpoints (itérations) : 2030-10-14, 10d

    section Tests unitaires
    Spécification cas de test (David, Freddie) : 2030-10-16, 5d

    section TDD autour de la recherche multiple
    Implémentation test T16 (Freddie)          :active, 2030-10-21, 3d
    Correction code et refactor (Alice, Eddy) : 2030-10-24, 5d

    section Tests de charge
    Scénarios de montée en charge             : 2030-10-21, 7d

    section Finalisation
    Campagne de non-régression + ajustements : 2030-10-28, 6d
```

Ce Gantt montre que l'écriture des tests ne vient pas « après coup » : elle est intégrée au cœur du développement, avec une responsabilité explicite confiée à Freddie sous la supervision de David et de Bob.

Ce plan TDD donne à Freddie un rôle central dans la qualité du moteur, tout en gardant un cadre clair pour l'équipe de développement.


\newpage