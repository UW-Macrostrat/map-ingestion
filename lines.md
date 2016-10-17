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

| Fields to include |
| :---------------- |
| name |
| type |
| direction |
| descrip |
| new_type |


| Line types     |
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
