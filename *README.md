# Gap analysis in the effectiveness of the current Marine Protected Area network design framework with respect to biodiversity conservation in coastal Norway
This repository is used for my master's thesis (May 2024).

The purpose of this repo is to record the R script used to analyse species occurrence data with regards to coastal Norway and administrative borders. It was used in a broader way to inform on MPA network design and MPA criteria.

ArcGIS Pro 3.2 (3.2.49743) and R version 4.4.0 "Puppy Cup" are used.
List of files in this repository:

Full_list_of_species is the list of species found in the coastal territorial waters of Norway, and generally excluding Bacteria, Protozoa, Fungi, and Biota incertae sedis.

R Scrit gives the full R script built and used for this project.
R packages and citations gives the full list of R packages and their citations.

OBIS Mapper map with coords and filter gives the OBIS Mapper map with the filter to keep occurrences within Norway (areaid = "179"), the drawn polygon, and WKT string to delimt the polygon.
 
- Helpful links:
  - https://obis.org/ (OBIS)
  - https://api.obis.org/
  - https://manual.obis.org/
  - https://mapper.obis.org/
  - https://www.geonorge.no/
  - https://www.rdocumentation.org/
  - https://livingatlas.arcgis.com/en/home/
  - https://www.marinespecies.org/ (WoRMS)
  - https://r.geocompx.org/reproj-geo-data.html#reproj-geo-data
  - https://jsta.github.io/glatos-spatial_workshop_materials/01-vector-open-shapefile-in-r/
  - https://www.marineregions.org/downloads.php
  - https://www.r-bloggers.com/2017/01/extracting-and-enriching-ocean-biogeographic-information-system-obis-data-with-r/
  - https://manual.obis.org/common_qc.html#non-marine-species
  - https://manual.obis.org/common_formatissues.html#spatial
  - https://kartkatalog.geonorge.no/metadata/sjoekart-maritim-infrastruktur/a894ea02-d2dc-4550-ac3e-49230ceed42a
  - https://kartkatalog.geonorge.no/metadata/norways-maritime-boundaries/e106adf4-c9d8-4fce-a9b5-7886a4126d23
  - https://github.com/iobis/robis/blob/master/vignettes/getting-started.Rmd (@pieterprovoost)
  - https://github.com/IUCNN/IUCNN.git 
  - https://iucnn.github.io/IUCNN/
   
Author: G. Giannone - MSc Biology, Norwegian University of Science and Technology - Trondheim, Norway

License: MIT License, Copyright (c) 2024 gn-gia
