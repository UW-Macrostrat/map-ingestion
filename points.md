# Points

## Processing
1. Import shapefile(s) into PostGIS
1.5 In the event there are multiple shapefiles, harmonize into a single table
  - Pick one (largest one is usually a good starting point)
  - Keep type-specific fields to a miniumum - map into existing fields as much as possible
  - Add max(gid) to rows that you are inserting into the homogenozied table
2. Add `point_type` field
3. Get unique types from dataset and map them into known values
4. Add column for dip direction (if missing) if map uses right hand rule: 

| Fields we end up with |
| :---------------- |
| point_type |
| certainty |
| comments |
| strike |
| dip |
| dip_dir |

| Used point_types     |
| :------------- |
| bedding |
| foliation |
| fault surface |
| fold axis |
| joint |


## SQL Example for Right Hand Rule
### To add unique dip_dir to a table: 
````UPDATE sources.points set dip_dir=strike+90````

### Remember to account for strike values >270: 
````UPDATE sources.points set dip_dir=dip_dir-360 where dip_dir>360````
