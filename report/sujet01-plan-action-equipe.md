## Contexte et objectif

Nous sommes le **30 septembre 2030**. En tant qu'Alice, alumni STL-INSTA (promotion 2025–2026) avec cinq années d'expérience dans les SI parisiens, je reçois de X. un courriel m'annonçant officiellement la constitution de l'équipe projet pour le **Ticket #1664** (POC d'un moteur de recherche d'images).

L'objectif de ce document est de fournir, en pièce jointe de ma réponse à X., un **plan d'action structuré** ainsi qu'un **calendrier prévisionnel** pour l'équipe projet.

## 1. Courriel à X.

> **Objet** : [#1664] Plan d'action prévisionnel pour le POC moteur de recherche
>
> Bonjour X.,
>
> Merci pour ton mail, je suis très heureuse de faire partie de ce nouveau projet !
>
> Vous trouverez ci-joint une proposition de plan d'action pour le POC du moteur de recherche. Il contient un calendrier prévisionnel et la répartition des tâches.
>
> En partant du principe que nous débutons la semaine prochaine (soit début le **6 octobre 2030**), et en suivant ce plan d'action, nous estimons une **livraison prévue pour le 12 novembre 2030**.
>
> N'hésite pas à revenir vers nous si tu souhaites des ajustements.
>
> Bien cordialement,
>
> Alice
> WONDROUS-IT

## 2. Équipe projet et rôles

- **Alice** (moi-même) : cheffe de projet technique, coordination globale, conception fonctionnelle, architecture, pilotage des risques.
- **Bob** : lead tech, arbitrages techniques critiques, conception des composants cœur du moteur, revue de code sur les parties sensibles. Très fort, inhumain, une vraie machine ; guru de la tech toujours tiraillé entre mille et une missions ; très peu disponible ; 10 ans dans les SI les plus connus du Silicon Sentier.
- **Cindy** : UX / UI, conception des wireframes et maquettes, cohérence de l'expérience utilisateur. Designer UX, libre comme une freelance depuis 2 ans ; avait commencé comme infographiste avant d'évoluer en designer technique.
- **David** : QA, stratégie de test, écriture de plans de test, pilotage de la non-régression. Associé à l'équipe depuis trois mois ; QA de la boite depuis 1 an ; 3 ans d'expérience dans les SI.
- **Eddy** : développeur apprenti, implémentation incrémentale des fonctionnalités, automatisation simple, support tests. Apprenti en alternance SU UPMC.

## 3. Phases du projet

### 3.1 Cadrage fonctionnel et technique (Semaine 1)
- Relecture du ticket général (#1664) et clarification des contraintes métier.
- Ateliers rapides avec les métiers pour figer les quatre fonctionnalités clés du moteur.
- Définition des scénarios d'usage principaux (recherche par mot-clef, recherche RegEx, classement, recherche multiple).
- Définition des métriques de performance et des scénarios de montée en charge.
- Lancement de la collecte d'une base d'URLs d'images suffisamment volumineuse pour le POC.

### 3.2 Conception UX / UI et architecture (Semaines 2–3)
- Ateliers rapides avec Cindy pour dessiner les écrans clefs et les parcours utilisateurs.
- Spécification de l'API (endpoints, formats des requêtes / réponses, codes d'erreur).
- Conception du modèle de données (table d'index, métadonnées, structure de stockage des URLs).
- Choix de l'architecture WebAPI et des technologies principales.
- Mise en place de l'usine logicielle minimale (CI, repo, tests automatiques).

### 3.3 Développement itératif du moteur (Semaines 3–6)
- Implémentation incrémentale des endpoints :
  - `/myImageFromKeyword/<str:name>/`
  - `/myImageFromRegEx/<str:name>/`
  - `/myImagesFromManyKeywords/<str:name>/`
- Mise en place du mécanisme de classement (critère de pertinence déterministe).
- Récolte et alimentation d'une première **banque d'images** significative.
- Intégration avec la base d'images et gestion des performances (indexation, caches).

### 3.4 Stratégie de test et validation (Semaines 6–7)
- Définition des cas de test unitaires et d'intégration.
- David et Eddy construisent une batterie de tests fonctionnels et de charges.
- Mise en place de jeux de données pour la montée en charge.
- Exécution de campagnes de tests et corrections.

### 3.5 Tests de montée en charge (Semaine 7)
- Définition des scénarios de charge (volume de requêtes, volume de données).
- Exécution de scénarios de montée en charge et collecte de métriques (latence, débit, erreurs).
- Mesure des performances (latence, débit, consommation de ressources).
- Ajustement (indexation, mise en cache simple, paramétrage).

### 3.6 Finalisation du POC et restitution (Semaine 8)
- Stabilisation du code et des données.
- Gel de la version POC.
- Rédaction du rapport de tests de montée en charge.
- Rédaction d'un rapport de retours d'expérience, incluant les résultats des tests de montée en charge et les pistes d'industrialisation.
- Préparation de la démonstration et des supports (slides, résumé exécutif).

## 4. Calendrier prévisionnel et volume de travail

### 4.1 Répartition globale par phase

| Phase                                   | Alice | Bob | Cindy | David | Eddy | Total JH |
|----------------------------------------|:-----:|:---:|:-----:|:-----:|:----:|:--------:|
| 1. Cadrage fonctionnel et technique    |  2    | 1   |  0    |  0    |  1   |    4     |
| 2. Conception UX / UI et architecture  |  3    | 2   |  5    |  0    |  2   |   12     |
| 3. Développement itératif du moteur    |  6    | 3   |  0    |  0    | 12   |   21     |
| 4. Stratégie de test et validation     |  2    | 1   |  0    |  4    |  3   |   10     |
| 5. Tests de montée en charge           |  2    | 1   |  0    |  3    |  2   |    8     |
| 6. Finalisation du POC et restitution  |  3    | 1   |  1    |  1    |  2   |    8     |
| **Total**                              | **18**|**9**|**6**  |**8**  |**22**| **63 JH**|

### 4.2 Vue par personne

| Membre | Rôle principal             | JH prévus | Commentaire                         |
|--------|----------------------------|:---------:|-------------------------------------|
| Alice  | Pilotage + conception      |    18     | Coordination globale + développements |
| Bob    | Lead tech                  |     9     | Ateliers d'architecture et revue   |
| Cindy  | UX / UI                    |     6     | Conception et itération des écrans |
| David  | QA                         |     8     | Stratégie de test + exécution      |
| Eddy   | Développement / support QA |    22     | Implémentation et automatisation   |

Ce volume reste cohérent avec un POC ambitieux mais réaliste sur environ huit semaines calendaires, en tenant compte des autres engagements de chacun (notamment Bob).

## 5. Diagramme de Gantt

```{.mermaid}
gantt
    dateFormat  YYYY-MM-DD
    title       Ticket #1664 – Plan prévisionnel (version initiale)

    section Cadrage
    Cadrage fonctionnel et technique :crit, 2030-10-06, 5d

    section Conception
    UX / Wireframes & architecture   :active, 2030-10-13, 10d

    section Développement
    Dev fonctionnalités principales  : 2030-10-25, 15d

    section Tests
    Stratégie de test & validation   : 2030-11-09, 7d
    Montée en charge                 : 2030-11-16, 3d

    section Finalisation
    Finalisation POC & restitution   : 2030-11-19, 3d
```

\newpage