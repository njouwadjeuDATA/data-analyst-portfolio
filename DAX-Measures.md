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
