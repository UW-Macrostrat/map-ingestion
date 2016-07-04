# Burwell processing

## Workflow

+ Find, download, evaluate, and process dataset
  + double check all fields for accuracy: innaccurate and incomplete data is worse than no data
  + if adding new fields do not name fields with spaces or non-standard characters
  + do not include duplicated information (e.g., if there is a already a field the unit name and you are adding a field for its descriptions by copying and pasting, do not include the strat_name in the description)
+ Create a new folder for the dataset, and save the geology geometry as `geology.shp` and the faults as `lines.shp`
+ Rename and save any `txt` or `pdf` metadata as `metadata.x`
+ Secure copy this new folder to `teststrata:/Users/collaborator/ready`
+ Create a new issue on this repository and fill out all fields
