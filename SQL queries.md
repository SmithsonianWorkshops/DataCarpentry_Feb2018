# Queries used in SQL workshop

## `SELECT`

```
SELECT record_id, month, day, year
FROM surveys;
```

```
SELECT *
FROM surveys;
```

```
SELECT month, day, year, weight
FROM surveys;
```

## Unique values: `DISTINCT`

```
SELECT DISTINCT species_id
FROM surveys;
```

```
SELECT DISTINCT year, species_id
FROM surveys;
```

## Calculated values

```
SELECT year, month, day, weight/1000
FROM surveys;
```

```
SELECT year, month, day, ROUND(weight/1000,2)
FROM surveys;
```

## Filtering: `WHERE`

```
SELECT *
FROM surveys
WHERE species_id='DM';
```

### Number of rows returned: `COUNT()`
SELECT COUNT(*)
FROM surveys
WHERE species_id='DM';

```
SELECT *
FROM surveys
WHERE species_id='DM' OR species_id='DS';
```

```
SELECT *
FROM surveys
WHERE species_id <> 'DM';
```

```
SELECT *
FROM surveys
WHERE weight >= 0;
```

### `WHERE` and `NULL` values

This will *not* return rows were sex is `NULL`:
```
SELECT *
FROM surveys
WHERE sex != 'M' AND sex != 'F';
```

Use `IS NULL` or `IS NOT NULL` to return those:
SELECT *
FROM surveys
WHERE sex IS NULL;

## Summary results: `AVG()`, `SUM()`, `MIN()`,` MAX()`

```
SELECT AVG(weight), SUM(weight)/COUNT(*)
FROM surveys
WHERE species_id = "DM";
```

```
SELECT AVG(weight), SUM(weight)/COUNT(weight)
FROM surveys
WHERE species_id = "DM";
```

```
SELECT AVG(weight), SUM(weight)/COUNT(*)
FROM surveys
WHERE species_id = "DM" AND weight IS NOT NULL;
```

## Sorting: `ORDER BY`

```
SELECT *
FROM surveys
ORDER BY weight ASC;
```

```
SELECT *
FROM surveys
WHERE species_id = 'DM'
ORDER BY weight DESC;
```

## Grouping summary results: `GROUP BY`

```
SELECT species_id, COUNT(*)
FROM surveys
GROUP BY species_id;
```

```
SELECT year, species_id, COUNT(*)
FROM surveys
GROUP BY year, species_id;
```

## Filtering by summary results: `HAVING`

WRONG :(

```
SELECT year, species_id, COUNT(*)
FROM surveys
GROUP BY year, species_id
WHERE COUNT(*) > 10;
```

Works :)
```
SELECT year, species_id, COUNT(*)
FROM surveys
GROUP BY year, species_id
HAVING COUNT(*) > 10;
```

```
SELECT year, species_id, COUNT(*)
FROM surveys
WHERE year < 1980
GROUP BY year, species_id
HAVING COUNT(*) > 10;
```

## Combining tables: `JOIN`

```
SELECT *
FROM surveys
JOIN species
ON surveys.species_id = species.species_id;
```
### `JOIN` vs `LEFT JOIN`

Only includes records from surveys when `species_id` is set and present in `species`:

```
SELECT *
FROM surveys
JOIN species
USING (species_id);
```

Using `LEFT JOIN` includes all from surveys *even if `species_id` is NULL or not present in `species` table*:

```
SELECT *
FROM surveys
LEFT JOIN species
USING (species_id);
```

When adding columns, you need to specify which table they are from:

```
SELECT surveys.month, surveys.year, species.genus, species.species
FROM surveys
LEFT JOIN species
USING (species_id);
```

### Aliases for table or column names: `AS`

```
SELECT sur.month, sur.year, sp.genus, sp.species
FROM surveys AS sur
LEFT JOIN species AS sp
USING (species_id);
```

```
SELECT sp.genus, sp.species, COUNT(*)
FROM surveys AS sur
LEFT JOIN species AS sp
USING (species_id)
WHERE sp.genus = "Dipodomys"
GROUP BY sp.species;
```

You can use `AS` to refer to a column, this also changes how it's displayed in results

```
SELECT species.genus, species. species, ROUND(AVG(weight),2) AS "Average weight (g)"
FROM surveys
JOIN species
ON surveys.species_id = species.species_id
WHERE species.taxa = "Rodent"
GROUP BY species.species_id;
```
