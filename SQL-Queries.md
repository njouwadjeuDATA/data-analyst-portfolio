# 🗄️ SQL Queries Documentation

---

## 🌍 Project 1 : Africa Talent Intelligence Dashboard

**Database :**  Africa_jobs
**Tables :** `job_salaries`, `dim_pays`, `dim_poste`, `dim_secteur`

---

### 1. What is the average talent cost per African country?

```sql
SELECT 
    dp.pays,
    dp.region_afrique,
    ROUND(AVG(js.salaire_usd), 2) AS salaire_moyen_usd
FROM job_salaries js
JOIN dim_pays dp ON js.pays = dp.pays
GROUP BY dp.pays, dp.region_afrique
ORDER BY salaire_moyen_usd DESC;
```

---

### 2. Which African region offers the most competitive salaries?

```sql
SELECT 
    dp.region_afrique,
    ROUND(AVG(js.salaire_usd), 2) AS salaire_moyen_usd
FROM job_salaries js
JOIN dim_pays dp ON js.pays = dp.pays
GROUP BY dp.region_afrique
ORDER BY salaire_moyen_usd DESC;
```

---

### 3. Which sectors pay their employees the most?

```sql
SELECT 
    ds.secteur,
    ds.categorie_secteur,
    ROUND(AVG(js.salaire_usd), 2) AS salaire_moyen_usd
FROM job_salaries js
JOIN dim_secteur ds ON js.secteur = ds.secteur
GROUP BY ds.secteur, ds.categorie_secteur
ORDER BY salaire_moyen_usd DESC;
```

---

### 4. What is the salary benchmark by position?

```sql
SELECT 
    dpo.poste,
    dpo.niveau,
    ROUND(AVG(js.salaire_usd), 2) AS salaire_moyen_usd,
    ROUND(MIN(js.salaire_usd), 2) AS salaire_min,
    ROUND(MAX(js.salaire_usd), 2) AS salaire_max
FROM job_salaries js
JOIN dim_poste dpo ON js.poste = dpo.poste
GROUP BY dpo.poste, dpo.niveau
ORDER BY salaire_moyen_usd DESC;
```

---

### 5. Which country/sector/position trio offers the lowest recruitment cost?

```sql
SELECT 
    dp.pays,
    ds.secteur,
    dpo.poste,
    ROUND(AVG(js.salaire_usd), 2) AS salaire_moyen_usd
FROM job_salaries js
JOIN dim_pays dp ON js.pays = dp.pays
JOIN dim_secteur ds ON js.secteur = ds.secteur
JOIN dim_poste dpo ON js.poste = dpo.poste
GROUP BY dp.pays, ds.secteur, dpo.poste
ORDER BY salaire_moyen_usd ASC
LIMIT 10;
```

---

### 6. Which sectors are underpaid compared to the global average?

```sql
SELECT 
    ds.secteur,
    ROUND(AVG(js.salaire_usd), 2) AS salaire_moyen_secteur,
    ROUND((SELECT AVG(salaire_usd) FROM job_salaries), 2) AS moyenne_globale,
    ROUND(AVG(js.salaire_usd) - (SELECT AVG(salaire_usd) FROM job_salaries), 2) AS ecart
FROM job_salaries js
JOIN dim_secteur ds ON js.secteur = ds.secteur
GROUP BY ds.secteur
HAVING AVG(js.salaire_usd) < (SELECT AVG(salaire_usd) FROM job_salaries)
ORDER BY ecart ASC;
```

---

## 🛒 Project 2 : E-Commerce Dashboard

**Database :** E_commerce  
**Tables :** `fact_commandes`, `client`, `ville`, `abonnement`, `satisfaction`

---

### Performance & Revenue

### 1. What is the total revenue generated?

```sql
SELECT 
    ROUND(SUM(depenses_totales), 2) AS revenu_total
FROM fact_commandes;
```

---

### 2. Which city generates the most revenue?

```sql
SELECT 
    v.nom_ville,
    ROUND(SUM(fc.depenses_totales), 2) AS revenu_total
FROM fact_commandes fc
JOIN ville v ON fc.id_ville = v.id_ville
GROUP BY v.nom_ville
ORDER BY revenu_total DESC;
```

---

### 3. Which subscription type is the most profitable?

```sql
SELECT 
    a.type_abonnement,
    ROUND(SUM(fc.depenses_totales), 2) AS revenu_total,
    ROUND(AVG(fc.depenses_totales), 2) AS panier_moyen
FROM fact_commandes fc
JOIN abonnement a ON fc.id_abonnement = a.id_abonnement
GROUP BY a.type_abonnement
ORDER BY revenu_total DESC;
```

---

### 4. How do sales evolve month by month?

```sql
SELECT 
    annee,
    mois,
    ROUND(SUM(depenses_totales), 2) AS revenu_mensuel,
    COUNT(id_fait) AS nb_commandes
FROM fact_commandes
GROUP BY annee, mois
ORDER BY annee, mois;
```

---

### 5. Which year was the best performing?

```sql
SELECT 
    annee,
    ROUND(SUM(depenses_totales), 2) AS revenu_annuel,
    COUNT(id_fait) AS nb_commandes
FROM fact_commandes
GROUP BY annee
ORDER BY revenu_annuel DESC;
```

---

### Customer Segmentation

### 6. What is the customer breakdown by gender?

```sql
SELECT 
    c.genre,
    COUNT(DISTINCT fc.id_client) AS nb_clients,
    ROUND(SUM(fc.depenses_totales), 2) AS revenu_total
FROM fact_commandes fc
JOIN client c ON fc.id_client = c.id_client
GROUP BY c.genre;
```

---

### 7. Which age group spends the most?

```sql
SELECT 
    CASE 
        WHEN c.age < 25 THEN '18-24'
        WHEN c.age BETWEEN 25 AND 34 THEN '25-34'
        WHEN c.age BETWEEN 35 AND 44 THEN '35-44'
        WHEN c.age BETWEEN 45 AND 54 THEN '45-54'
        ELSE '55+'
    END AS tranche_age,
    ROUND(AVG(fc.depenses_totales), 2) AS depense_moyenne,
    ROUND(SUM(fc.depenses_totales), 2) AS depense_totale
FROM fact_commandes fc
JOIN client c ON fc.id_client = c.id_client
GROUP BY tranche_age
ORDER BY depense_totale DESC;
```

---

### 8. Which customer profile generates the most revenue?

```sql
SELECT 
    c.genre,
    CASE 
        WHEN c.age < 25 THEN '18-24'
        WHEN c.age BETWEEN 25 AND 34 THEN '25-34'
        WHEN c.age BETWEEN 35 AND 44 THEN '35-44'
        WHEN c.age BETWEEN 45 AND 54 THEN '45-54'
        ELSE '55+'
    END AS tranche_age,
    a.type_abonnement,
    ROUND(SUM(fc.depenses_totales), 2) AS revenu_total
FROM fact_commandes fc
JOIN client c ON fc.id_client = c.id_client
JOIN abonnement a ON fc.id_abonnement = a.id_abonnement
GROUP BY c.genre, tranche_age, a.type_abonnement
ORDER BY revenu_total DESC
LIMIT 10;
```

---

### 9. Do discounts impact sales?

```sql
SELECT 
    CASE 
        WHEN remise_appliquee = 0 THEN 'Sans remise'
        ELSE 'Avec remise'
    END AS remise,
    COUNT(id_fait) AS nb_commandes,
    ROUND(AVG(depenses_totales), 2) AS panier_moyen,
    ROUND(AVG(articles_achetes), 2) AS articles_moyens
FROM fact_commandes
GROUP BY remise;
```

---

### 10. Which city has the highest average basket?

```sql
SELECT 
    v.nom_ville,
    ROUND(AVG(fc.depenses_totales), 2) AS panier_moyen
FROM fact_commandes fc
JOIN ville v ON fc.id_ville = v.id_ville
GROUP BY v.nom_ville
ORDER BY panier_moyen DESC;
```

---

### 11. Which subscription buys the most articles?

```sql
SELECT 
    a.type_abonnement,
    ROUND(AVG(fc.articles_achetes), 2) AS articles_moyens,
    SUM(fc.articles_achetes) AS articles_total
FROM fact_commandes fc
JOIN abonnement a ON fc.id_abonnement = a.id_abonnement
GROUP BY a.type_abonnement
ORDER BY articles_moyens DESC;
```

---

### Loyalty & Satisfaction

### 12. Which subscription type retains customers best?

```sql
SELECT 
    a.type_abonnement,
    ROUND(AVG(fc.jours_depuis_dernier_achat), 2) AS jours_moyens_entre_achats
FROM fact_commandes fc
JOIN abonnement a ON fc.id_abonnement = a.id_abonnement
GROUP BY a.type_abonnement
ORDER BY jours_moyens_entre_achats ASC;
```

---

### 13. Do satisfied customers spend more?

```sql
SELECT 
    s.niveau_satisfaction,
    ROUND(AVG(fc.depenses_totales), 2) AS depense_moyenne,
    COUNT(fc.id_fait) AS nb_commandes
FROM fact_commandes fc
JOIN satisfaction s ON fc.id_satisfaction = s.id_satisfaction
GROUP BY s.niveau_satisfaction
ORDER BY depense_moyenne DESC;
```

---

### 14. Is there a satisfaction gap between Male and Female customers?

```sql
SELECT 
    c.genre,
    ROUND(AVG(fc.note_moyenne), 2) AS note_moyenne
FROM fact_commandes fc
JOIN client c ON fc.id_client = c.id_client
GROUP BY c.genre;
```

---

### 15. Which city has the highest average rating?

```sql
SELECT 
    v.nom_ville,
    ROUND(AVG(fc.note_moyenne), 2) AS note_moyenne
FROM fact_commandes fc
JOIN ville v ON fc.id_ville = v.id_ville
GROUP BY v.nom_ville
ORDER BY note_moyenne DESC;
```

---

### 16. Do discounts improve customer satisfaction?

```sql
SELECT 
    CASE 
        WHEN remise_appliquee = 0 THEN 'Sans remise'
        ELSE 'Avec remise'
    END AS remise,
    ROUND(AVG(note_moyenne), 2) AS note_moyenne
FROM fact_commandes
GROUP BY remise;
```

---

### 17. Is customer satisfaction improving over time?

```sql
SELECT 
    annee,
    mois,
    ROUND(AVG(note_moyenne), 2) AS note_moyenne_mensuelle
FROM fact_commandes
GROUP BY annee, mois
ORDER BY annee, mois;
```
