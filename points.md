# Points

## Processing
1. Import shapefile(s) into PostGIS
1.5 In the event there are multiple shapefiles, harmonize into a single table
  - Pick one (largest one is usually a good starting point)
  - Keep type-specific fields to a miniumum - map into existing fields as much as possible
  - Add max(gid) to rows that you are inserting into the homogenozied table
2. Add `point_type` field
3. Get unique types from dataset and map them into known values

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
