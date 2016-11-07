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
| klippe |
| fenster |
| impact structure |

## SQL Examples 
### To add unique gids to a table: 
````INSERT INTO sources.fault_lines (gid) SELECT (SELECT max(gid) + sources.fold_lines.gid FROM sources.fault_lines) from sources.fold_lines````

### To add type data into the new gid rows created: 
````UPDATE sources.fault_lines SET type=fold_lines.type FROM sources.fold_lines WHERE fault_lines.gid=(select max(gid) + 1 - fold_lines.gid from sources.fault_lines)````
