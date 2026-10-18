# Exploratory results
Source: saved outputs in the supplied notebook; collection timestamp and exact OSM boundary IDs are unknown.

## Map inventory
| Campus | Boundary area (ha) | Buildings | Named buildings (%) | POIs | Named POIs (%) | Indoor-tagged objects |
|---|---:|---:|---:|---:|---:|---:|
| Dankook-Jukjeon | 47.1 | 45 | 93.3 | 79 | 62.0 | 30 |
| Dankook-Cheonan | 27.8 | 22 | 86.4 | 10 | 70.0 | 0 |
| KAIST | 113.8 | 202 | 55.0 | 338 | 18.9 | 16 |

Counts include the acquisition buffer; areas do not. These are not validated whole-campus inventories. Indoor counts alone do not establish the presence or absence of navigable room/corridor topology.

## Landmark proximity (R = 50m)
| Campus | Nodes with zero named representatives (%) | Mean representative count |
|---|---:|---:|
| Dankook-Jukjeon | 14.8 | 2.03 |
| Dankook-Cheonan | 67.2 | 0.36 |
| KAIST | 31.8 | 1.41 |

Names in OSM do not imply visible signs. Representative points are not facade/entrance locations. Nodes are not uniformly distributed along paths.

## Descriptor uniqueness (angle step = 30 degrees)
| Campus | Evaluated intersections | Distinct descriptors | Shape only (%) | Shape + name set (%) |
|---|---:|---:|---:|---:|
| Dankook-Jukjeon | 104 | 28 | 14.4 | 76.0 |
| Dankook-Cheonan | 42 | 20 | 28.6 | 50.0 |
| KAIST | 1187 | 93 | 3.1 | 43.7 |

Uniqueness is relative to this candidate set and descriptor. It is not localization accuracy. Landmark combination assumes complete, exact name observations. Campus size, mapping detail and candidate count affect comparisons.

## Removed unsupported conclusions
- Exact radius thresholds of 55/73/84m: no corresponding numerical computation in the supplied notebook.
- All indoor objects being indoor=yes: no tag-value summary shown.
- Necessity of building floor plans as the sole indoor localization resource.
- Actual localization success inferred only from map proximity.

## Verification status
Code and saved output were inspected. No new OSM acquisition, boundary verification, field experiment or sensitivity experiment was performed for this publication.
