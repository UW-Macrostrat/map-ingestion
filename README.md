# Burwell processing

## Workflow

**1. Find, download, and evaluate dataset**
  + Data requirements: vector geometries, ages, lithologies.
  
**2. Convert geospatial data to shapefile format if necessary**
  + e.g. if e00 see [conversion instructions](http://support.esri.com/technical-article/000004705)
  
**3. Import shapefile dataset(s) into PostgreSQL**

*NOTE: If doing this for the first time, first download the [Burwell Repository](https://github.com/UW-Macrostrat/burwell), and save the folder as `burwell` into documents on your machine.*

Run the following commands in terminal:

Change directory to burwell repo folder


`cd ~/Documents/burwell-master`

`./import table_name [path] false LATIN1`
   

**4. Process dataset**
  + Update or fill in tables using source materials (if necessary) in Postico:
      + Double check all fields for accuracy: **innaccurate and incomplete data are worse than no data**
      + If adding new fields, do not name fields with spaces or non-standard characters
      + Do not include duplicated information (e.g., if there is a already a field the unit name and you are adding a field for its descriptions by copying and pasting, do not include the strat_name in the description)
  + Assign early_ids and late_ids to units in geology table (see [ages instructions](https://github.com/UW-Macrostrat/burwell-processing/blob/master/ages.md))
  + Homogenize data for lines into a single table  (see [lines instructions](https://github.com/UW-Macrostrat/burwell-processing/blob/master/lines.md))
  + Homogenize data for points into a single table (see [points instructions](https://github.com/UW-Macrostrat/burwell-processing/blob/master/points.md))
  
**5. Zip completed data table(s)** 
  + Ex. `pg_dump -x -O -t sources.germany_geo burwell|gzip>germany_geo.sql.gz`
  
**6. Create a new folder for the dataset, and include:** 
   + shapefiles (save geology geometry as `geology.shp`, lines (faults, folds, etc.) as `lines.shp`, and points (strike/dip) as `points.shp`)
   + zipped SQL dump(s)
   + any `txt` or `pdf` metadata renamed and saved as `metadata.x`
   + any other relevant source materials (maps, pamphlets, map descriptions, readme, etc.)
   
**7. Secure copy this new folder to teststrata:**
  
Example command (in terminal):   
  
`scp -P 2200 -r ~/Documents/Macrostrat/source_folder collaborator@teststrata.geology.wisc.edu:/users/collaborator`

**8. Create a new issue on this repository and fill out all fields**

**9. Move the issue to the "Ready to import" column in [Processing](https://github.com/UW-Macrostrat/burwell-processing/projects/1)**

## Data of interest and importance 
+ In general, there is no reason to delete anything from the original data directories that are download. Original data should be preserved
+ Data of specific use (in order of priority):
  + *polygons* showing bedrock and surficial geology with age, lithology/description, and a name of some kind
  + *lines* identifing faults, folds, dikes, and beds and other geology-specific features. lines not pertaining to geological features (e.g., map boundaries, roads, transects, cross section lines, etc.) are not to be processed.
  + *points* identifying measurements of strike/dip, foliation, lineation, and other rock-specific features. points of general interest (e.g., mines, craters, stations) are not to be processed.
