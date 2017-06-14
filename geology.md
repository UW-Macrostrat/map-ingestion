# Step 1: Add columns for missing data.
Needed columns: name, strat_name, hierarchy, age, description



**Add a column:**
** 
```
ALTER TABLE table_name ADD COLUMN column_name TEXT;
```


# Step 2: Fill in missing data.
**Update example:**
```
UPDATE sources.table_name set name='St. Peter Formation' where unit='Os';
```

# Joining early_ids and late_ids to the geology table


**Add early_id and late_id columns to geo table:**
```
ALTER TABLE table_name ADD COLUMN early_id INTEGER;
ALTER TABLE table_name ADD COLUMN late_id INTEGER;
```

**Run first to assign late_ids and early_ids to single age strings**

```
UPDATE table_name 
SET late_id = m.id, early_id = m.id 
FROM macrostrat.intervals m
WHERE age ILIKE m.interval_name;
```

**Run second to see what ages still need to be matched**

```
SElECT distinct age FROM table_name WHERE early_id is NULL;
```

**Set late_id and early_id with a string replace** \
**Run if age data uses 'upper' and 'lower' instead of 'late' and 'early'**

```
UPDATE table_name 
SET early_id = m.id, late_id = m.id 
FROM macrostrat.intervals m 
WHERE m.interval_name ILIKE REPLACE(age,'Lower','Early') 
  AND early_id IS NULL;
```

**Set late_id to right side of age string**

```
UPDATE table_name 
SET late_id = l.id 
FROM macrostrat.intervals l 
WHERE age ILIKE concat('%-', l.interval_name) 
  AND late_id IS NULL;
```

**Set early_id to left side of age string**

```
UPDATE table_name 
SET early_id = e.id 
FROM macrostrat.intervals e 
WHERE age ILIKE concat(e.interval_name, '-%') 
  AND early_id IS NULL;
```

**Set late_id and early_id using an exact string match**

```
UPDATE table_name 
SET late_id = l.id, early_id=e.id 
FROM macrostrat.intervals l, macrostrat.intervals e 
WHERE l.interval_name = 'Cambrian' 
  AND e.interval_name = 'Neoproterozoic' 
  AND age ILIKE 'Late Proterozoic-Early Paleozoic';
```

