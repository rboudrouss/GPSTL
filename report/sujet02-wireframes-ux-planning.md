## Contexte et objectif

Nous sommes le **7 octobre 2030**. Cindy, notre designer UX, quitte brutalement le projet pour des raisons indépendantes du Ticket #1664 (obtention inespérée de sa carte verte des US).

L'objectif de ce document est de :

- proposer à X. une contre-proposition constructive où Alice reprend le rôle UX de Cindy
- présenter une première version des wireframes du moteur
- fournir un Gantt mis à jour, intégrant une semaine de retard

## 1. Courriel de contre-proposition à X.

> **Objet** : [#1664] Contre-proposition, prise en charge du rôle UX/UI
>
> Bonjour X.,
>
> Merci pour ta réactivité et pour l'information concernant Cindy. Je comprends la situation (félicitations à elle pour sa carte verte !), cependant, pour éviter un nouveau retard et préserver l'élan de l'équipe, je te propose de reprendre immédiatement le rôle UX/UI que tenait Cindy pour le Ticket #1664.
>
> Je pense qu'il serait plus efficace de garder le rôle en interne plutôt que de chercher un prestataire externe.
>
> D'une part, cela nous permettrait de préserver la continuité du projet : j'ai déjà coordonné l'analyse et rédigé un premier plan d'action (ci-joint), et le remplacement de Cindy par une ressource extérieure impliquerait forcément une phase d'onboarding (prise en main du ticket, réunions de transfert, mise en contexte...) qui risquerait de retarder encore le démarrage du développement. Sans parler des éventuels risques fonctionnels, notamment les écarts de vision produit et les allers-retours lents entre les équipes.
>
> Ensuite, l'équipe actuelle est déjà prête et parfaitement alignée. Bob couvre tout l'aspect technique (performance et intégration), Eddy est disponible, formé sur les technologies du projet et extrêmement motivé, tandis que David gère le QA. De mon côté, je peux reprendre le lead sur la partie UX/UI ainsi que la coordination fonctionnelle.
>
> Je joins les wireframes et le Gantt mis à jour à ce message. Je reste à ta disposition pour toutes questions supplémentaires.
>
> Merci pour ta confiance.
>
> Bien cordialement,
>
> Alice
> WONDROUS-IT

## 2. Principes UX retenus

Les wireframes suivent quelques principes simples :

- Simplicité : un champ de recherche central, des options avancées repliées par défaut.
- Progressivité : l'utilisateur découvre d'abord la recherche simple, puis la recherche avancée et multiple.
- Lisibilité : résultats présentés avec l'URL, les mots-clefs associés et un indicateur de pertinence.
- Traçabilité : rappel visuel des mots-clefs ou de l'expression régulière utilisée pour la requête.

## 3. Gantt mis à jour après le départ de Cindy

Le départ de Cindy impose un décalage d'une semaine du lancement effectif du développement, le temps pour Alice de produire les wireframes essentiels. Le décalage est absorbé en reprenant le rôle d'UX tout en maintenant la dynamique de développement.

### 3.1 Répartition des JH mise à jour

| Étape                              | Équipe                | JH  |
|------------------------------------|-----------------------|:---:|
| Analyse et Architecture            | Alice, Bob            | 4   |
| Collecte et préparation BDD        | Eddy                  | 4   |
| Dév API : Recherche simple et RegEx| Bob (Eddy support)    | 6   |
| Dév API : Classement               | Bob                   | 3   |
| Dév API : Recherche multiple       | Alice, Eddy           | 4   |
| UI/UX                              | **Alice** (ex-Cindy)  | 3   |
| QA                                 | David                 | 4   |
| Montée en charge et optimisation   | Bob, David            | 4   |
| Documentation et packaging         | Eddy, Alice           | 3   |

**Total JH par personne** :

- Alice : **11 JH** (au lieu de 9 JH initialement, car elle reprend l'UX)
- Bob : **17 JH** (inchangé)
- ~~Cindy : 3 JH~~ (partie)
- David : **8 JH** (inchangé)
- Eddy : **11 JH** (inchangé)

**Total : 47 JH** (au lieu de 63 JH avec Cindy, car Alice mutualise coordination + UX)

**Livraison prévisionnelle** : **~18 novembre 2030** (au lieu du 12 novembre, soit 1 semaine de glissement).

### 3.2 Principes du nouveau planning

- La conception UX est assurée par Alice sur les semaines 2 et 3, en parallèle partiel avec l'architecture.
- Eddy commence dès la semaine 2 la mise en place de l'infrastructure technique (CI, squelette de l'API).
- Le développement des fonctionnalités de recherche démarre une semaine plus tard, mais sur une base mieux cadrée.

```{.mermaid}
gantt
    dateFormat  YYYY-MM-DD
    title       Ticket #1664 – Planning mis à jour (après départ de Cindy)

    section Cadrage
    Cadrage fonctionnel et technique :crit, 2030-10-06, 5d

    section UX & Architecture
    Wireframes par Alice                :active, 2030-10-13, 7d
    Architecture et outillage           : 2030-10-13, 7d

    section Développement
    Endpoints de recherche              : 2030-10-20, 15d
    Recherche multiple et classement    : 2030-11-04, 7d

    section Tests & montée en charge
    Plan de tests et exécution          : 2030-11-11, 7d

    section Finalisation
    Finalisation POC & restitution      : 2030-11-18, 3d
```

L'effort UX est recentré sur Alice, ce qui évite une nouvelle phase d'onboarding pour un prestataire externe et limite le risque de décalage supplémentaire. La charge de développement reste concentrée sur Bob et Eddy, avec une fenêtre de tests toujours suffisante pour sécuriser le POC.


\newpage