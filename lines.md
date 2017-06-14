# Lines

+ Often as separate files (faults, dikes, etc...)

## Processing
1. Import shapefile(s) into PostGIS
1.5 In the event there are multiple shapefiles, harmonize into a single table
  - Pick one (largest one is usually a good starting point)
  - Keep type-specific fields to a miniumum - map into existing fields as much as possible
  - Add max(gid) to rows that you are inserting into the homogenozied table
2. Add `new_type` and `new_direction` fields
3. Get unique types from dataset and map them into known values

**NOTE:** assign the new_type 'thrust fault' to reverse fault lines

| Fields we end up with |
| :---------------- |
| name |
| type |
| direction |
| descrip |
| new_type |


| Used new_types     |
| :------------- |
| anticline |
| bed |
| dike |
| fault |
| flow |
| fold |
| fracture |
| growth fault |
| lineament |
| marker bed |
| monocline |
| normal fault |
| shear zone |
| slide |
| strike-slip fault |
| suture |
| syncline |
| thrust fault |
| vein |
| zone |
| spreading center |
| extinct spreading center fracture |
| klippe |
| fenster |
| impact structure |

## SQL Examples 
### To add unique gids to a table: 
````INSERT INTO sources.faults (gid) SELECT (SELECT max(gid) + sources.folds.gid FROM sources.faults) from sources.folds````

### To add type data into the new gid rows created: 
````UPDATE sources.faults SET type=folds.type FROM sources.folds WHERE faults.gid=(select max(gid) + 1 - folds.gid from sources.faults)````
