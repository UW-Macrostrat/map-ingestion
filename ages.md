# Joining early_ids and late_ids to the geology table

**Run first to see what ages need to be matched**

```
SElECT distinct age FROM temp2 WHERE early_id is NULL;
```

**Set late_id and early_id to single age string**

```
UPDATE temp2 
SET late_id = m.id, early_id = m.id 
FROM macrostrat.intervals m
WHERE age ILIKE m.interval_name;
```

**Set late_id to right side of age string**

```
UPDATE temp2 
SET late_id = m.id 
FROM macrostrat.intervals m 
WHERE age ILIKE concat('%-', m.interval_name) 
  AND late_id IS NULL;
```

**Set early_id to left side of age string**

```
UPDATE temp2 
SET early_id = m.id 
FROM macrostrat.intervals m 
WHERE age ILIKE concat(m.interval_name, '-%') 
  AND early_id IS NULL;
```

**Set late_id and early_id using an exact string match**

```
UPDATE temp2 
SET late_id = m.id, early_id=e.id 
FROM macrostrat.intervals m, macrostrat.intervals e 
WHERE m.interval_name = 'Cambrian' 
  AND e.interval_name = 'Neoproterozoic' 
  AND age ILIKE 'Late Proterozoic-Early Paleozoic';
```

**Set late_id and early_id with a string replace**

```
UPDATE temp2 
SET early_id = m.id, late_id = m.id 
FROM macrostrat.intervals m 
WHERE m.interval_name ILIKE REPLACE(age,'Lower','Early') 
  AND early_id IS NULL;
```