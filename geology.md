# Step 1: Add Columns for Missing Data.
Needed text columns: `name`, `strat_name`, `hierarchy`, `age`, `description` and/or `lithology` \
Optional text columns: `comments`

**ADD COLUMN example:**
```
ALTER TABLE sources.table_name ADD COLUMN column_name TEXT;
```

# Step 2: Fill in Missing Data.


Use source materials (maps, pamphlets, etc.) to update the geology table.
+ `name`: formal or informal unit name (often already in original attribute tables or in map legends or descriptions) 
+ `strat_name`: formal stratigraphic name (e.g. St. Peter Formation) of the lowest ranked unit in the name; leave column blank for units with informal names (e.g. alluvium). **NOTE:** Separate multiple names with semicolons

Example

name | strat_name
---------- | ----------
Minnekahta Limestone and Opeche Shale, undivided| Minnekahta Limestone; Opeche Shale


+ `hierarchy`: if denoted, formal name(s) of unit ranked above the `strat_name` unit. **Note:** If more than one rank above strat_name is denoted, separate the hierarchy names with "of the" 

Example

strat_name | hierarchy
---------- | ----------
Galeros Formation| Chuar Group of the Grand Canyon Supergroup

+ `age`: age interval(s) of unit 
+ `description`: description of map unit
+ `lithology`: lithology of map unit
+ `comments`: any relevant/additional comments

**UPDATE example:**
```
UPDATE sources.table_name set name='St. Peter Formation' where unit='Os';
```
**SELECT DISTINCT example:**

(May be useful to select distinct rows to see where data is missing)
```
SELECT DISTINCT unit_code, name, strat_name, hierarchy, age, description from sources.table_name;
```

# Step 3: Join early_id and late_id to the Geology Table.

**Add early_id and late_id columns to geo table:**
```
ALTER TABLE sources.table_name ADD COLUMN early_id INTEGER;
ALTER TABLE sources.table_name ADD COLUMN late_id INTEGER;
```

**Run first to assign late_ids and early_ids to single age strings**

```
UPDATE sources.table_name 
SET late_id = m.id, early_id = m.id 
FROM macrostrat.intervals m
WHERE age ILIKE m.interval_name;
```

**Run second to see what ages still need to be matched**

```
SElECT distinct age FROM table_name WHERE early_id is NULL;
```

**Set late_id and early_id with a string replace** \
**Run if age data uses 'upper' and 'lower' instead of 'late' and 'early'** \
**Run if age data uses 'Early Proterozoic', 'Middle Proterozoic', and 'Late Proterozoic' instead of 'Paleoproterozoic', 'Mesoproterozoic', and 'Neoproterozoic'**

```
UPDATE sources.table_name 
SET early_id = m.id, late_id = m.id 
FROM macrostrat.intervals m 
WHERE m.interval_name ILIKE REPLACE(age,'Lower','Early') 
  AND early_id IS NULL;
```

**Set late_id to right side of age string**

```
UPDATE sources.table_name 
SET late_id = l.id 
FROM macrostrat.intervals l 
WHERE age ILIKE concat('%-', l.interval_name) 
  AND late_id IS NULL;
```

**Set early_id to left side of age string**

```
UPDATE sources.table_name 
SET early_id = e.id 
FROM macrostrat.intervals e 
WHERE age ILIKE concat(e.interval_name, '-%') 
  AND early_id IS NULL;
```

**Set late_id and early_id using an exact string match**

```
UPDATE sources.table_name 
SET late_id = l.id, early_id=e.id 
FROM macrostrat.intervals l, macrostrat.intervals e 
WHERE l.interval_name = 'Cambrian' 
  AND e.interval_name = 'Neoproterozoic' 
  AND age ILIKE 'Late Proterozoic-Early Paleozoic';
```

# Sample End Product:

unit | name | strat_name | hierarchy| age | description | lithology | geom | early_id | late_id
---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ----------
Qp | Pediment deposits |   |   | Quaternary | Sand and gravel veneering pediment surfaces formed during several cycles of erosion. | unconsolidated; alluvium; mixed | 0106000020E610000001... | 421 | 421
Kh | Hunter Canyon Formation of the Mesaverde Group | Hunter Canyon Formation  |  Mesaverde Group | Cretaceous | Buff and gray medium- to coarse-grained massive cliff-forming sandstone and gray to greenish-gray shale. Thickness 375 to 1,400 ft. | clastic; sandstone; shale | 0106000020E610000001... | 33 | 33
Kbc | Burro Canyon Formation | Burro Canyon Formation  |   | Cretaceous | White, gray, and buff fine- to coarse-grained sandstone interbedded with green and purplish siltstone, shale, and mudstone, and thin beds of impure limestone. Sandstone is locally conglomeratic, particularly at base. Thickness a few feet to 125 feet. | sedimentary; clastic; sandstone | 0106000020E610000001... | 33 | 33
Kmu | Upper shale member of the Mancos Shale | Mancos Shale  |   | Cretaceous | Dark-gray to black soft shale with thin sandstone beds at various horizons. | clastic; shale | 0106000020E610000001... | 33 | 33
Kmvl | Lower part of the Mesaverde Group | Mesaverde Group  |   | Cretaceous | Light-gray and brown massive sandstone, gray shale, and some coal. Upper boundary is approximately same horizon as top of Trout Creek Sandstone Member of the Iles Formation. | clastic; sandstone; shale; coal (minor) | 0106000020E610000001... | 33 | 33
Jse | Summerville Formation and Entrada Sandstone | Summerville Formation; Entrada Sandstone  |   | Jurassic | Summerville: alternating thin beds of gray, green, and red siltstone, gray, greenish-gray, and reddish-brown fine-grained sandstone, and red and greenish-gray shale. Thickness 40-60 ft. Entrada Sandstone: buff, white, and pink medium-grained massive cross-bedded sandstone. Thickness 100-300 ft. | clastic; sandstone; crossbedded | 0106000020E610000001... | 48 | 48
pCr | Complex of gneisses and schists |   |   | Mesoproterozoic to Paleoproterozoic | Gneisses and schists; intruded by silicic to intermediate igneous rocks. | metamorphic; intermediate; gneiss; schist | 0106000020E610000001... | 259 | 258
