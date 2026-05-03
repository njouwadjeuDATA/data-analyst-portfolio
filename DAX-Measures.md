# 📐 DAX Measures Documentation

## 🛒 Project 2 : E-Commerce Dashboard

### Revenue Measures
```dax
CA_Total = SUM(fact_commandes[montant_total])
```
> Calculates the total revenue from all orders.

```dax
CA_2024 = CALCULATE([CA_Total], YEAR(fact_commandes[date_commande]) = 2024)
CA_2025 = CALCULATE([CA_Total], YEAR(fact_commandes[date_commande]) = 2025)
CA_2026 = CALCULATE([CA_Total], YEAR(fact_commandes[date_commande]) = 2026)
```
> Calculates revenue filtered by year.

```dax
Panier_Moyen = DIVIDE([CA_Total], [Nb_Clients])
```
> Average basket per customer.

```dax
Nb_Clients = DISTINCTCOUNT(fact_commandes[id_client])
```
> Total number of unique customers.

```dax
Articles_Total = SUM(fact_commandes[quantite])
```
> Total number of articles sold.

```dax
Fidelite_Moyenne = AVERAGE(fact_commandes[jours_entre_achats])
```
> Average number of days between purchases per subscription type.

```dax
Note_Moyenne = AVERAGE(fact_commandes[note_satisfaction])
```
> Average customer satisfaction score.

---

## 🌍 Project 1 : Africa Talent Intelligence Dashboard

```dax
Average Salary = AVERAGE(job_salaries[salary_in_usd])
```
> Calculates the average salary in USD across all records.

```dax
Total Positions = COUNT(job_salaries[poste])
```
> Total number of job positions in the dataset.

```dax
Unique Profiles = DISTINCTCOUNT(job_salaries[poste])
```
> Number of unique job profiles.

```dax
Number of Countries = DISTINCTCOUNT(job_salaries[country])
```
> Total number of distinct countries in the dataset.
