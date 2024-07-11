# The Southern-Hemisphere Spectroscopic Redshift Compilation
<div align="center">
 
[![DOI - 10.5281/zenodo.11641314](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.11641314-0677b8?logo=doi&logoColor=white)](https://zenodo.org/doi/10.5281/zenodo.11641314)

[![GitHub tag](https://img.shields.io/github/tag/ErikVini/SpecZCompilation?include_prereleases=&sort=semver&color=blue)](https://github.com/ErikVini/SpecZCompilation/releases/)
[![License](https://img.shields.io/badge/License-MIT-blue)](#license)

[![Google Drive - Download](https://img.shields.io/badge/Google_Drive-Download-4688F4?logo=googledrive&logoColor=FFFFFF)](https://drive.google.com/file/d/1h4DmddmsLVkBqRMJCAOkLjk2ayu9OjWk/view)

</div>

## Description
This page is reserved for releases of a compilation of spectrocopic redshifts for the Southern Hemisphere (declination below +5 degrees) focused on galaxies. 

This compilation contains 3800+ catalogues of spectroscopic redshifts from services such as [VizieR](http://vizier.cds.unistra.fr/), [HEASARC](https://heasarc.gsfc.nasa.gov/), [SDSS](http://skyserver.sdss.org/CasJobs/), and others. After removing duplicates, the number of catalogues in the final compination is 1765 and the total number of objects is 5624559, including galaxies, stars, QSOs, and other object types. The catalogue name in the TAP services, work titles, number of objects, and authors are present in the files [`Catalogues_All.csv`](https://github.com/ErikVini/SpecZCompilation/blob/4aea730686da8e0df6a39b4e235a9aed6abdfb09/Catalogues_All.csv) and [`Catalogues_Used.csv`](https://github.com/ErikVini/SpecZCompilation/blob/4aea730686da8e0df6a39b4e235a9aed6abdfb09/Catalogues_Used.csv) for all downloaded tables and the ones used in the final compilation (after removing duplicates) respectively.

The catalogue can be downloaded via Google Drive in the "[Releases](https://github.com/ErikVini/SpecZCompilation/releases/latest)" section.

Compilation numbers:
* `STAR`: 4812282
* `GALAXY`: 2548065
* `QSO`: 507894
* `AGN`: 35192
* `GLOBCLUSTER`: 2245
* `SUPERNOVAE`: 1948
* `UNCLEAR` (described below): 529602

The columns available are:
* `RA`: right ascension (degrees),
* `DEC`: declination (degrees),
* `z`: spectroscopic redshift,
* `e_z`: error in the spectroscopic redshift,
* `f_z`: flag for the spectroscopic redshift quality,
* `class_spec`: spectroscopic classification of the object,
* `original_class_spec`: original spectroscopic classification of the object (before grouping),
* `source`: TAP service and catalogue from which the information was obtained.

Not all catalogues used contain information about the redshift error, quality flags, and/or classes. In these situations the value is left empty.

## How it was done
A script written in Python is used to download all catalogues from the VizieR and HEASARC table access protocol (TAP) services using a series of queries to obtain the table names, respective column names for coordinates, redshift, redshift error, and class, and their descriptions. This information is used to generate another table which contains all information needed to query for the objects (`RA`, `DEC`, `z`, `e_z`, `f_z`, `class_spec`).

Before downloading all possible tables, and since there are duplicates, a pre-processing is done to remove tables that:
* The `z` column does not correspond to spectroscopic redshifts (such as metallicities or distance above the plane of the galaxy, for example)
* The `e_z`, `f_z`, or `class` columns does not correspond to the information needed
Example: some tables have flags in magnitudes (e.g. in the z-band), but they share the same unified content descriptor (UCD) with spectroscopic redshift UCD, so the table may be downloaded with the wrong information.

This process is done for VizieR and HEASARC. In both cases any tables with zero objects are removed.

For VizieR, a correction for J1950 coordinates is applied. Also, any tables that represent distances as `cz` also have the redshift and error values corrected.

## Classes

For tables that have this information, a manual procedure was applied to group classes into `STAR`, `GALAXY`, `QSO`, `AGN`, `GLOBCLUSTER`, or `UNCLEAR`. When avaliable, sub-classes are included. Some examples are:

* `GALAXY`
  * `GALAXY(SF)`: Star-forming galaxies
  * `GALAXY(GROUP)`: Galaxy group
  * `GALAXY(RADIO)`: Radio galaxy
  * `GALAXY(CLUSTER)`: Cluster of galaxies
  * `GALAXY(PAIR)`: Pair of galaxies
  * `GALAXY(COMP)`: Composite galaxy
  * `GALAXY(UNCLEAR)`: Unclear galaxy (It's a extended object)
  * `GALAXY(TRIPLET)`: Galaxy triplet
  * `GALAXY(BCG)`: Brighest Cluster Galaxy
  * `GALAXY(LINER)`: LINER galaxy
  * `GALAXY(STRIPPING)`: Stripping galaxy
  * `GALAXY(LSB)`: Low surface brightness galaxy
  * `GALAXY(JELLYFISH)`: Jellyfish galaxy
* `STAR`
  * `STAR(NEB)`: Nebulae
  * `STAR(WD)`: White-dwarf star
  * `STAR(HII)`: HII region
* `SUPERNOVAE`
* `QSO`
  * `QSO(BLLAC)`: BL-Lac object
  * `QSO(BLAZAR)`: Blazar
* `AGN`
  * `AGN(Sy2)`: Seyfert 2 AGN
  * `AGN(Sy1)`: Seyfert 1 AGN
  * `AGN(Sy)`: AGN with unspecified Seyfert type
* `GLOBCLUSTER`
* `UNCLEAR`: This class represents classification that were unclear, according to the catalogue authors, or may not be correct
  * `UNCLEAR(XRAY)`
  * `UNCLEAR(IR)`
  * `UNCLEAR(POINTLIKE)`
  * `UNCLEAR(HI)`
  * `UNCLEAR(EXTENDED)`
  * `UNCLEAR(RADIO)`
  * `UNCLEAR(BROADLINE)`
  * `UNCLEAR(ASTEROID)`
  * `UNCLEAR(HII)`
  * `UNCLEAR(EmLS)`
  * `UNCLEAR(Sy2)`

The `UNCLEAR` class is reserved for objects where the classification was not clear enough to be included in the other six groups.

Be aware that this classification may change, and an update to class names to align them with the [SIMBAD object types](https://simbad.cds.unistra.fr/Pages/guide/otypes.htx) is underway.

## Flags

For tables with this information, a manual verification was made in order to classify flags as `KEEP`, or `REMOVE`. The `KEEP` flag indicates that the spectroscopic redshift is reliable according to the authors of the catalogue and the `REMOVE` indicates that the given measurement is not reliable. The original flag is maintained whitin parenthesis after the `KEEP` or `REMOVE` words.

## Merging catalogues

After all VizieR and HEASARC catalogues were downloaded and processed, they were concatenated with the [SDSS DR18](http://skyserver.sdss.org/CasJobs/), [PRIMUS](https://primus.ucsd.edu/version1.html), [NED](https://ned.ipac.caltech.edu/), [HECATE](https://hecate.ia.forth.gr/) [2dFLenS](https://2dflens.swin.edu.au/), [GLADE+](https://glade.elte.hu/),  [DESI](https://datalab.noirlab.edu/desi/access.php), and the Maddox et al. spectroscopic redshifts for the Fornax Cluster (Maddox, N. et al. 2019) catalogues.

## Duplicate removal procedure

After concatenating all catalogues, the resulting catalogue will inevitably have duplicate objects. To try and remove those objects, the `Python` package [`Scipy`](https://scipy.org/) was used.

<!-- To try and remove those objects the [STILTS](http://www.star.bris.ac.uk/~mbt/stilts/sun256/index.html) software was used. -->

Before removing duplicates, the table was sorted in order to keep the objects with most information at the top. The catalogue is then composed of blocks:
* Objects with `e_z`, `f_z`, and `class_spec`,
* objects with `e_z` and `class_spec`,
* objects with `e_z` and `f_z`,
* objects with `f_z` and `class_spec`,
* objects with `e_z`,
* objects with `class_spec`,
* objects with `f_z`, and
* objects without `e_z`, `f_z` or `class_spec`.

Inside each block, a priority is given to the object types in the order `GALAXY`, `AGN`, `SUPERNOVAE`, `QSO`, `STAR`, `GLOBCLUSTER`, `UNCLEAR`, and the remaining objects, and to the spectroscopic redshift flag (`KEEP`, `REMOVE`). This way the duplicate removal procedure tries to keep the most galaxies and gives priority to objects with a quality flag information.

An internal match is done using the sky coordinates with a 1 arcsecond maximum separation, keeping only the first ocurrence (thus the importance of the sorting procedure above).

<!-- An internal match is done using the `Sky+X` match parameter with `RA`, `DEC` and `z` with a 2 arcsecond maximum separation in coordinates and 0.002 in redshift, keeping only the first ocurrence (thus the importance of the sorting procedure above):
```
java -jar stilts.jar tmatch1 matcher=sky+1d values='RA DEC z' params='2 0.002' action=keep1 in=InputTable.csv out=OutputTable.csv
``` -->

## Known issues

Some redshifts are duplicated even after the previous duplicate removal procedure. This is more common for extended objects (such as big galaxies in nearby clusters). This happens because, although the measurements lie inside a 2 arcsecond radius, they differ by more than 0.002 in z, thus they are not detected as duplicated and are not removed.

The HEASARC table descriptions are incomplete. There seems to be a limit to how many characters are returned, so the text is cut.

## The final catalogue

![Distribution of objects in the sky. Image also available in "Images" folder.](Images/all_sky_specz_map_2024-06-20.png?raw=true "Distribution of objects in the sky.")

![Number of objects per class. Image also available in "Images" folder.](Images/class_distribution_2024-06-20.png?raw=true "Number of objects per class.")

![Number of objects per flag. Image also available in "Images" folder.](Images/flags_distribution_2024-06-20.png?raw=true "Number of objects per flag.")

![Distribution of redshifts per class. Image also available in "Images" folder.](Images/specz_distribution_2024-06-20.png?raw=true "Distribution of redshifts per class.")
