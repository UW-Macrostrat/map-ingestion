# Burwell processing

## Workflow

+ Find, download, and evaluate dataset
  + Data requirements: vector geometries, ages, lithologies.
+ Process dataset
  + Import shapefile attribute table(s) into Postgres
  + Update or fill in tables using source materials (if necessary)
  + Assign early_ids and late_ids to units in geology table (see [ages instructions](https://github.com/UW-Macrostrat/burwell-processing/blob/master/ages.md))
  + Homogenize data for lines into a single table  (see [lines instructions](https://github.com/UW-Macrostrat/burwell-processing/blob/master/lines.md))
  + Homogenize data for points into a single table (see [points instructions](https://github.com/UW-Macrostrat/burwell-processing/blob/master/points.md))
  + Double check all fields for accuracy: **innaccurate and incomplete data are worse than no data**
  + If adding new fields, do not name fields with spaces or non-standard characters
  + Do not include duplicated information (e.g., if there is a already a field the unit name and you are adding a field for its descriptions by copying and pasting, do not include the strat_name in the description)
+ Zip completed data table(s) 
  + Ex. `pg_dump -x -O -t sources.germany_geo burwell|gzip>germany_geo.sql.gz`
+  Save the geology geometry as `geology.shp`, lines (faults, folds, etc.) as `lines.shp`, and points (strike/dip) as `points.shp`
+ Create a new folder for the dataset, and include: 
   + shapefiles
   + zipped SQL dumps
   + any `txt` or `pdf` metadata renamed and saved as `metadata.x`
   + any other relevant source materials (maps, pamphlets, map descriptions, etc.)
+ Secure copy this new folder to `teststrata:/Users/collaborator/ready`
+ Create a new issue on this repository and fill out all fields
+ Move the issue to the "Ready to import" column in [Processing](https://github.com/UW-Macrostrat/burwell-processing/projects/1)

## Data of interest and importance 
+ In general, there is no reason to delete anything from the original data directories that are download. Original data should be preserved
+ Data of specific use (in order of priority):
  + *polygons* showing bedrock and surficial geology with age, lithology/description, and a name of some kind
  + *lines* identifing faults, folds, dikes, and beds and other geology-specific features. lines not pertaining to geological features (e.g., map boundaries, roads, transects, cross section lines, etc.) are not to be processed.
  + *points* identifying measurements of strike/dip, foliation, lineation, and other rock-specific features. points of general interest (e.g., mines, craters, stations) are not to be processed.
