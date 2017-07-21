# Burwell processing

## Workflow
**Note:** The [brackets] in the example script indicate portions that must be replaced with specific directory paths or names specific to unique map sources. For example, a `path` might look like `~/Desktop/Folder`; a good `table_name` might be `Sheridan_geo`.

### 1. Find, download, and evaluate dataset

**Data Requirements:** 
+ vector geometry
+ age
+ lithology / description
  
### 2. Process geospatial data
Data comes in many different formats. If there are numerous download options available, choose GeoDatabase (.gdb), shapefile (.shp), and ArcInfo Coverage/Interchange (.e00), in that order of priority.

If the data is only available as an ArcInfo coverage, you must first convert it to a shapefile. You can either use ArcGIS ([instructions](http://support.esri.com/technical-article/000004705)), or `ogr2ogr` as so:

````
ogr2ogr -f "ESRI Shapefile" [output] [input.e00]
````

**Combine data types**
The data we are looking for is often split across numerous files/tables. For example, it is common to see faults and syn/anticlines in different files. Start by merging the datasets into their associated geometry types (polygons, lines, and points). 

  
 **Identify location of attributes**
 The attributes (name, age, description) of polygons and lines are found in many places. Before committing to copy/pasting them frmo a PDF, make sure you have checked all tables of the geodatabase, all metadata, and all text files. Often times it is possible to extract them more easily from a plain text file than a PDF.
 
 
### 3. Import shapefile data table(s) of interest into PostgreSQL

**Data of Interest (in order of priority):**
  + *polygons* showing bedrock and surficial geology with age, lithology/description, and a name of some kind
  + *lines* identifing faults, folds, dikes, and beds and other geology-specific features. lines not pertaining to geological features (e.g., map boundaries, roads, transects, cross section lines, etc.) are not to be processed.
  + *points* identifying measurements of strike/dip, foliation, lineation, and other rock-specific features. points of general interest (e.g., mines, craters, stations) are not to be processed.

**Run the following commands in terminal to import shapefile attribute tables:**\
**NOTE**: If doing this for the first time, download the [Burwell Repository](https://github.com/UW-Macrostrat/burwell), and save the folder as `burwell` on your machine.*

Change directory to burwell repo folder


`cd [path to repo folder]`


Import to Postico

`./import [table_name] [path to shapefile] false LATIN1`
   

### 4. Process dataset

**Move the issue to the "In progress" column in [Processing](https://github.com/UW-Macrostrat/burwell-processing/projects/1).**

**Update or fill in tables using source materials (if necessary) in Postico:**\
Double check all fields for accuracy: innaccurate and incomplete data are worse than no data. \
If adding new fields, do not name fields with spaces or non-standard characters.\
Do not include duplicated information (e.g., if there is a already a field the unit name and you are adding a field for its descriptions by copying and pasting, do not include the strat_name in the description).

1. Update the geology table (see [geology instructions](https://github.com/UW-Macrostrat/burwell-processing/blob/master/geology.md))
2. Homogenize data for lines into a single table  (see [line instructions](https://github.com/UW-Macrostrat/burwell-processing/blob/master/lines.md))
3. Homogenize data for points into a single table (see [point instructions](https://github.com/UW-Macrostrat/burwell-processing/blob/master/points.md))
  
### 5. Zip completed data table(s) (in terminal)**
For each table (polygons, lines, points), dump the table and zip it. **DO NOT COMBINE ALL TABLES INTO ONE DUMP**
`pg_dump -x -O -t sources.[table_name] burwell|gzip>[table_name].sql.gz`
  
### 6. Create a new folder for the dataset, and include:
   + original data should be preserved. In general, there is no reason to delete anything from the original data directories that are download
   + shapefiles (save geology geometry as `geology.shp`, lines (faults, folds, etc.) as `lines.shp`, and points (strike/dip) as `points.shp`)
   + zipped SQL dump(s) of updated attribute tables
   + any `txt` or `pdf` metadata renamed and saved as `metadata.x`
   + any other relevant source materials (maps, pamphlets, map descriptions, readme, etc.)
   
### 7. Secure copy this new folder to teststrata:
  
Push folder to teststrata in terminal:   
  
`scp -P 2200 -r [path] collaborator@teststrata.geology.wisc.edu:/users/collaborator`

### 8. Fill out all fields for source issue

### 9. Move the issue to the "Ready to import" column in [Processing](https://github.com/UW-Macrostrat/burwell-processing/projects/1)  and assign Cambro to the issue
