# Burwell processing

## Workflow

**1. Find, download, and evaluate dataset**\
  Data Requirements: 
+ vector geometry
+ age
+ lithology / description
  
**2. Convert geospatial data to shapefile format if necessary**\
  e.g. if e00 see [conversion instructions](http://support.esri.com/technical-article/000004705)
  
**3. Import shapefile data table(s) of interest into PostgreSQL**

Data of Interest (in order of priority):
  + *polygons* showing bedrock and surficial geology with age, lithology/description, and a name of some kind
  + *lines* identifing faults, folds, dikes, and beds and other geology-specific features. lines not pertaining to geological features (e.g., map boundaries, roads, transects, cross section lines, etc.) are not to be processed.
  + *points* identifying measurements of strike/dip, foliation, lineation, and other rock-specific features. points of general interest (e.g., mines, craters, stations) are not to be processed.

Run the following commands in terminal to import shapefile attribute tables:\
**NOTE**: If doing this for the first time, download the [Burwell Repository](https://github.com/UW-Macrostrat/burwell), and save the folder as `burwell` on your machine.*

Change directory to burwell repo folder


`cd [path to repo folder]`


Import to Postico

`./import table_name [path to shapefile] false LATIN1`
   

**4. Process dataset**

Move the issue to the "In progress" column in [Processing](https://github.com/UW-Macrostrat/burwell-processing/projects/1).

Update or fill in tables using source materials (if necessary) in Postico:\
Double check all fields for accuracy: **innaccurate and incomplete data are worse than no data.** \
If adding new fields, do not name fields with spaces or non-standard characters.\
Do not include duplicated information (e.g., if there is a already a field the unit name and you are adding a field for its descriptions by copying and pasting, do not include the strat_name in the description.)

1. Update the geology table (see [geology instructions](https://github.com/UW-Macrostrat/burwell-processing/blob/master/geology.md))
2. Homogenize data for lines into a single table  (see [line instructions](https://github.com/UW-Macrostrat/burwell-processing/blob/master/lines.md))
3. Homogenize data for points into a single table (see [point instructions](https://github.com/UW-Macrostrat/burwell-processing/blob/master/points.md))
  
**5. Zip completed data table(s) (in terminal)** 
  + Ex. `pg_dump -x -O -t sources.table_name burwell|gzip>table_name.sql.gz`
  
**6. Create a new folder for the dataset, and include:** 
   + original data should be preserved. In general, there is no reason to delete anything from the original data directories that are download
   + shapefiles (save geology geometry as `geology.shp`, lines (faults, folds, etc.) as `lines.shp`, and points (strike/dip) as `points.shp`)
   + zipped SQL dump(s) of updated attribute tables
   + any `txt` or `pdf` metadata renamed and saved as `metadata.x`
   + any other relevant source materials (maps, pamphlets, map descriptions, readme, etc.)
   
**7. Secure copy this new folder to teststrata:**
  
Push data to teststrata in terminal:   
  
`scp -P 2200 -r [path] collaborator@teststrata.geology.wisc.edu:/users/collaborator`

**8. Create a new issue on this repository and fill out all fields**

**9. Move the issue to the "Ready to import" column in [Processing](https://github.com/UW-Macrostrat/burwell-processing/projects/1)** and assign Cambro to the issue
