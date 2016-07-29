# Burwell processing

## Workflow

+ Find, download, evaluate, and process dataset
  + double check all fields for accuracy: **innaccurate and incomplete data are worse than no data**
  + if adding new fields do not name fields with spaces or non-standard characters
  + do not include duplicated information (e.g., if there is a already a field the unit name and you are adding a field for its descriptions by copying and pasting, do not include the strat_name in the description)
+ Create a new folder for the dataset, and save the geology geometry as `geology.shp` and the faults as `lines.shp`
+ Rename and save any `txt` or `pdf` metadata as `metadata.x`
+ Secure copy this new folder to `teststrata:/Users/collaborator/ready`
+ Create a new issue on this repository and fill out all fields

## Data of interest and importance 
+ In general, there is no reason to delete anything from the original data directories that are download. Original data should be preserved
+ Data of specific use (in order of priority):
  + *polygons* showing bedrock and surficial geology with age, lithology/description, and a name of some kind
  + *lines* identifing faults, folds, dikes, and beds and other geology-specific features. lines not pertaining to geological features (e.g., map boundaries, roads, transects, cross section lines, etc.) are not to be processed.
  + *points* identifying measurements of strike/dip, foliation, lineation, and other rock-specific features. points of general interest (e.g., mines, craters, stations) are not to be processed.
