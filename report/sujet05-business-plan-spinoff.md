## Contexte et hypothèses

Nous sommes le **6 novembre 2030** à Londres. Avec X., nous préparons un spin-off autour du moteur de recherche d'images issu du Ticket #1664, d'abord en B2B, avec une possible extension vers le B2C.

L'intrapreneuriat, c'est le terme qu'Aline a conseillé de me documenter pendant la semaine. À vrai dire, X. semblait satisfait, voire enthousiaste, lorsque j'ai fini par lui proposer le plan. « Parfait. Faisons comme ce que tu proposes. On va avoir un spin-off. Pour ce projet, je mets la moitié sur la table, à toi de trouver l'autre. » J'ai proposé 40% des parts à X., pour 50% des investissements dans la nouvelle structure, plus mon salaire actuel.

**Objectif** : présenter aux VC de la City un business plan chiffré et une courbe dépenses vs. recettes mettant en évidence le point d'inflexion (moment où les recettes cumulées dépassent les dépenses cumulées).

### Hypothèses principales

- Monnaie : k€ (milliers d'euros).
- Horizon : 5 années (Année 0 = lancement).
- Modèle B2B initial (licences + services), ouverture progressive à des offres B2C.
- Structure du capital (ordre d'idée) : X. 40 %, Alice 25 %, Harry 25 %, VC de Londres 10 %.

## 1. Tableau prévisionnel dépenses / recettes

| Année | Dépenses (k€) | Recettes (k€) | Dépenses cumulées | Recettes cumulées | Solde cumulé |
|-------|---------------|---------------|-------------------|-------------------|--------------|
| 0     | 300           | 0             | 300               | 0                 | -300         |
| 1     | 400           | 150           | 700               | 150               | -550         |
| 2     | 450           | 450           | 1 150             | 600               | -550         |
| 3     | 500           | 900           | 1 650             | 1 500             | -150         |
| 4     | 550           | 1 400         | 2 200             | 2 900             | +700         |
| 5     | 600           | 2 000         | 2 800             | 4 900             | +2 100       |

Le **point d'inflexion** se situe entre les années **3 et 4** :

- fin Année 3 : recettes cumulées (1 500 k€) < dépenses cumulées (1 650 k€) → solde cumulé : -150 k€
- fin Année 4 : recettes cumulées (2 900 k€) > dépenses cumulées (2 200 k€) → solde cumulé : +700 k€

### Détail des dépenses par année

- **Année 0** : Développement initial du POC, infrastructure, premiers recrutements (300 k€)
- **Année 1** : Équipe élargie, marketing B2B, infrastructure cloud (400 k€)
- **Année 2** : Consolidation de l'équipe, amélioration du produit, premiers clients (450 k€)
- **Année 3** : Expansion commerciale, R&D pour B2C, montée en charge infrastructure (500 k€)
- **Année 4** : Internationalisation, équipe commerciale renforcée (550 k€)
- **Année 5** : Optimisation, scalabilité, préparation levée série A (600 k€)

### Détail des recettes par année

- **Année 0** : Aucune recette (phase de développement)
- **Année 1** : Premiers contrats B2B pilotes (150 k€)
- **Année 2** : Montée en puissance B2B, abonnements récurrents (450 k€)
- **Année 3** : Base installée B2B solide, premiers revenus B2C (900 k€)
- **Année 4** : Expansion internationale, offre freemium B2C (1 400 k€)
- **Année 5** : Maturité du produit, revenus récurrents stables (2 000 k€)

## 2. Courbe dépenses vs. recettes

```{.matplotlib}
import matplotlib.pyplot as plt

annees = [0, 1, 2, 3, 4, 5]
depenses = [300, 400, 450, 500, 550, 600]
recettes = [0, 150, 450, 900, 1400, 2000]

# Calcul des cumulés
cum_dep = []
cum_rec = []

s_dep = 0
s_rec = 0
for d, r in zip(depenses, recettes):
    s_dep += d
    s_rec += r
    cum_dep.append(s_dep)
    cum_rec.append(s_rec)

plt.figure(figsize=(8, 5))
plt.plot(annees, cum_dep, label="Dépenses cumulées", color="red", marker="o", linewidth=2)
plt.plot(annees, cum_rec, label="Recettes cumulées", color="green", marker="o", linewidth=2)

# Approximation du point d'inflexion (entre année 3 et 4)
plt.axvline(x=3.5, color="gray", linestyle="--", linewidth=1.5)
plt.text(3.6, (cum_dep[3] + cum_rec[3]) / 2,
         "Point d'inflexion\n(~Année 3-4)",
         ha="left", va="center", fontsize=10,
         bbox=dict(boxstyle="round,pad=0.3", facecolor="yellow", alpha=0.5))

plt.xlabel("Année", fontsize=12)
plt.ylabel("Montant cumulé (k€)", fontsize=12)
plt.title("Évolution dépenses vs. recettes cumulées - Spin-off Ticket #1664", fontsize=14, fontweight="bold")
plt.legend(fontsize=11)
plt.grid(True, linestyle=":", linewidth=0.5, alpha=0.7)
plt.tight_layout()
```

## 3. Commentaire pour les VC de la City

### Risques et opportunités

- Le **risque principal** est concentré sur les années 0 à 2 :
  - construction du produit,
  - obtention des premiers contrats B2B,
  - ajustement du modèle de pricing.
- À partir de l'Année 3, la base installée permet :
  - des **revenus récurrents** (abonnements, maintenance),
  - une mutualisation des coûts d'infrastructure.
- L'Année 5 illustre le potentiel de **scalabilité** du moteur, en particulier si l'on ouvre une offre B2C freemium adossée au moteur.

### Avantages compétitifs

- **Technologie éprouvée** : le POC du Ticket #1664 a déjà démontré la faisabilité technique et les performances en montée en charge.
- **Équipe expérimentée** : Alice (lead tech), X. (sponsor stratégique), Harry (business development).
- **Marché porteur** : besoin croissant de solutions de recherche d'images pour les entreprises (e-commerce, médias, archives).

### Stratégie de sortie

- Introduction en bourse. To the moon !

\newpage