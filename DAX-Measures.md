# 📐 DAX Measures Documentation

---

## 🌍 Project 1 : Africa Talent Intelligence Dashboard

### Measures

```dax
Average Salary = AVERAGE(job_salaries[salary_in_usd])
```
> Salaire moyen en USD sur tous les enregistrements.

```dax
Moyenne Africaine = 
CALCULATE(
    AVERAGE(job_salaries[salaire_usd]),
    ALL(dim_secteur)
)
```
> Salaire moyen tous secteurs confondus (filtre dim_secteur ignoré).

```dax
Total Positions = COUNT(job_salaries[poste])
```
> Nombre total de postes dans le dataset.

```dax
Unique Profiles = DISTINCTCOUNT(job_salaries[poste])
```
> Nombre de profils uniques.

```dax
Number of Countries = DISTINCTCOUNT(job_salaries[country])
```
> Nombre de pays distincts dans le dataset.

---

## 🛒 Project 2 : E-Commerce Dashboard

### Chiffre d'affaires

```dax
CA_Total = SUM(fact_commandes[montant_total])
```
> Calcule le revenu total de toutes les commandes.

```dax
CA_2024 = CALCULATE([CA_Total], YEAR(fact_commandes[date_commande]) = 2024)
CA_2025 = CALCULATE([CA_Total], YEAR(fact_commandes[date_commande]) = 2025)
CA_2026 = CALCULATE([CA_Total], YEAR(fact_commandes[date_commande]) = 2026)
```
> CA filtré par année.

```dax
CA_vs_Année_Précédente = [CA_Total] - CALCULATE([CA_Total], SAMEPERIODLASTYEAR(dim_date[date]))
```
> Écart du CA par rapport à l'année précédente.

```dax
CA_YTD = TOTALYTD([CA_Total], dim_date[date])
```
> Cumul du CA depuis le début de l'année en cours (Year-To-Date).

### Clients & Produits

```dax
Nb_Clients = DISTINCTCOUNT(fact_commandes[id_client])
```
> Nombre total de clients uniques.

```dax
Panier_Moyen = DIVIDE([CA_Total], [Nb_Clients])
```
> CA moyen par client.

```dax
Articles_Total = SUM(fact_commandes[quantite])
```
> Total des articles vendus.

```dax
Fidelite_Moyenne = AVERAGE(fact_commandes[jours_entre_achats])
```
> Moyenne de jours entre achats par abonnement.

```dax
Note_Moyenne = AVERAGE(fact_commandes[note_satisfaction])
```
> Score moyen de satisfaction client.STINCTCOUNT(job_salaries[country])
```
> Nombre de pays distincts dans le dataset.
