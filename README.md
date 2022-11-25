# The Southern-Hemisphere Spectroscopic Redshift Compilation
---
## Description
This page is reserved for releases of a compilation of spectrocopic redshifts for the Southern Hemisphere.

This compilation contains over 3000 catalogues of spectroscopic redshifts from services such as VizieR, HEASARC, and CasJobs. The columns avaliable are:
* RA: Right ascension (degrees),
* DEC: Declination (degrees),
* z: spectroscopic redshift,
* e_z: error in the spectroscopic redshift,
* f_z: flag for the spectroscopic redshift quality,
* class_spec: spectroscopic classification of the object,
* source: TAP service and catalogue from which the information was obtained.

Not all catalogues used contain information about the redshift error, quality flags, and classes. In these situations the value is left empty.

## How it was done
A script written in Python is used to download all catalogues from the VizieR and HEASARC TAP services using the following queries:

RA columns:
```
SELECT t.table_name, c.column_name AS ra_col, c.description as ra_desc
FROM tap_schema.tables AS t
JOIN tap_schema.columns AS c USING (table_name)
WHERE c.ucd = 'pos.eq.ra;meta.main'
```

DEC columns:
```
SELECT t.table_name, c.column_name AS dec_col, c.description as dec_desc
FROM tap_schema.tables AS t
JOIN tap_schema.columns AS c USING (table_name)
WHERE c.ucd = 'pos.eq.dec;meta.main'
```

Redshift tables:
```
SELECT t.table_name, t.description, c.column_name AS zcol, c.description as zcol_desc
FROM tap_schema.tables AS t
JOIN tap_schema.columns AS c USING (table_name)
WHERE c.ucd = 'src.redshift'
```

Redshift error columns:
```
SELECT t.table_name, c.column_name AS zerr_col, c.description as zerr_desc
FROM tap_schema.tables AS t
JOIN tap_schema.columns AS c USING (table_name)
WHERE c.ucd = 'stat.error;src.redshift'
```

Redshift flag columns:
```
SELECT t.table_name, t.description, c.column_name AS zflg_col, c.description as zflg_desc
FROM tap_schema.tables AS t
JOIN tap_schema.columns AS c USING (table_name)
WHERE c.ucd = 'meta.code.error;src.redshift' OR c.ucd = 'meta.code.qual;src.redshift' -> VizieR
WHERE c.ucd = 'meta.code;src.redshift' -> HEASARC
```

Morphological classification columns:
```
SELECT t.table_name, c.column_name AS class_col, c.description as class_desc
FROM tap_schema.tables AS t
JOIN tap_schema.columns AS c USING (table_name)
WHERE c.ucd = 'meta.code.class' -> VizieR
WHERE c.ucd = 'src.class' -> HEASARC
```
