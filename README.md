# setwd("C:/Users/gngia/OneDrive/Documents/new obis")

# Install robis package and run with 179=Norway and polygon excludes Svalbard and some outer marine records/areas (records from 23.02.2024)
library("dplyr")
library("ggplot2")
library("robis")
o <- occurrence(areaid = "179", mof = TRUE, geometry = "POLYGON ((34.4004 70.2535, 30.5332 71.2047, 26.5254 71.5416, 22.1660 71.2500,
           19.5645 70.7695, 16.5410 70.1344, 13.4473 69.0560, 11.1973 67.7361, 12.5332 67.3060, 
           11.4082 66.3939, 9.8613 65.4765, 8.4551 64.7666, 6.9785 63.9451, 5.0098 62.9072,
           3.5332 61.9968, 3.5332 58.3279, 7.2598 57.2790, 11.0567 58.4017, 12.4629 59.0948, 11.3379 60.6473,
           8.6660 60.5783, 8.3145 62.2924, 16.3301 65.6220, 17.2442 67.6293, 20.1973 68.7523,
           23.9238 69.6998, 26.5254 70.1344, 29.5488 69.5284, 32.2910 69.4051, 34.4004 70.2535))")
unique(o$kingdom)
View(o)

# Filter rows based on the specified conditions
filtered_data <- o %>%
  filter(kingdom %in% c("Animalia", "Plantae", "Chromista", NA))
unique(filtered_data$kingdom)
View(filtered_data)

# Plot map
library("maps")
library("ggplot2")
map_ggplot(filtered_data, color = "blue")

library("sf")
# Transform to an sf object and set crs
occ_sf <- st_as_sf(filtered_data, coords = c("decimalLongitude", "decimalLatitude"))
occ_sf <- st_set_crs(occ_sf, "EPSG:4326")
print(occ_sf1 <- st_crs(occ_sf))


# Read the shapefile into an sf object
library(sf)
shp <- st_read("C:/Users/gngia/OneDrive/Documents/new obis/GIS/Kommuner_Fylker_Intersect.shp", stringsAsFactors=FALSE)
#View(shp)
shp <- st_transform(shp, crs = "EPSG:4326")
print(shp1 <- st_crs(shp))

class(shp)
class(occ_sf)

# Plot the shapefile and occurrence data
library(sf)
library(ggplot2)
ggplot() +
  geom_sf(data = shp) +
  geom_sf(data = occ_sf, color = "green", size = 1) +
  theme_bw()

#unique(filtered_data$country)

# Perform a spatial join
map_occ_joined <- st_join(occ_sf, shp)

# Now you have the marine organism data associated with county information
# Count the occurrences of each species within each county:
map_occ_joined_county <- map_occ_joined %>%
  group_by(FYLKESNAVN) %>%
  summarize(species_count = n_distinct(originalScientificName))

View(map_occ_joined_county)

# Count the number of occurrences of each species within each county
map_occ_joined_species_with_county <- map_occ_joined %>%
  group_by(FYLKESNAVN, originalScientificName) %>%
  summarize(occurrences = n())
View(map_occ_joined_species_with_county)

class(map_occ_joined_species_with_county)

# Plot the join
library(sf)
library(ggplot2)
ggplot() +
  geom_sf(data = shp) +
  geom_sf(data = map_occ_joined_species_with_county, aes(color = occurrences), size = 0.25) +
  theme_bw()

# OBS! This apparently takes a WHILE to run I didn't have time to run it on 12.03.2024. Run in the morning and see if it works
library(sf)
library(ggplot2)
ggplot() +
  geom_sf(data = shp) +
  geom_sf(data = map_occ_joined_species_with_county, aes(color = map_occ_joined_species_with_county$originalScientificName), size = 0.25) +
  theme_bw()

--
#replace x and y from this example from Glenn's Map_script_1 with the FYLKENAVN or the originalSpeciesName or something?
ggplot() +
  geom_point() +
  geom_point(aes(x=8.75, y=58.416), colour="blue")+
  
--------
# If there are invalid geometries, attempt to repair them
invalid_geometries <- st_is_valid(shp)
if (any(!invalid_geometries)) {
  shp <- st_make_valid(shp)
}

invalid_geometries <- st_is_valid(occ_sf)
  if (any(!invalid_geometries)) {
    occ_sf <- st_make_valid(occ_sf)
  }

# Intersect layers and plot the intersected object
#https://github.com/rstudio/cheatsheets/blob/main/sf.pdf
library("sf")
library("ggplot2")
map_occ_intersect <- st_intersects(shp, occ_sf,sparse = TRUE)
ggplot() +
  geom_sf(data = map_occ_intersect, color = "pink", size = 1) +
    theme_bw()

---------
#find habitat/bathymetry data -> maybe on geonorge
#email being sent to mapping authoriy on 04.03.2024 to clarify on 12nm/territorial waters border
#consider doing more research on how far from the coast are territorial waters
#are all the records I have now marine/coastal? find a way to do this/go through table -> maybe through checklist obis

#https://manual.obis.org/common_formatissues.html#spatial
#https://r.geocompx.org/reproj-geo-data.html#reproj-geo-data

#https://jsta.github.io/glatos-spatial_workshop_materials/01-vector-open-shapefile-in-r/
#https://www.marineregions.org/downloads.php
#https://www.r-bloggers.com/2017/01/extracting-and-enriching-ocean-biogeographic-information-system-obis-data-with-r/
#https://manual.obis.org/common_qc.html#non-marine-species
#https://manual.obis.org/common_formatissues.html#spatial
#https://kartkatalog.geonorge.no/metadata/sjoekart-maritim-infrastruktur/a894ea02-d2dc-4550-ac3e-49230ceed42a
#https://kartkatalog.geonorge.no/metadata/norways-maritime-boundaries/e106adf4-c9d8-4fce-a9b5-7886a4126d23

#https://manual.obis.org/access
#https://github.com/iobis/robis/blob/master/vignettes/getting-started.Rmd

#wicket_0.4.0.tar.gz