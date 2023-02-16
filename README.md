# Lusatia-BicycleRoutes-LST-LULC

## Load library
```
library(raster)
library(sf)
library(leaflet)
library(shiny)
library(tidyverse)
library(RColorBrewer)
library(rgdal)
library(leaflet.opacity)
```

## Set Working directory
```
setwd("")
```

## Read Bicycle Routes table
```
bicycle_routes = read.csv("Lusatia_bicycle_routes.csv")
```

## Lusatia Bicycle Routes data source:
### 1. Lusatia Tourism website
https://lausitz.de/de/tourismus/radwandern.html

### 2. Brandenburg Tourism website
https://www.reiseland-brandenburg.de/suche/?q=Seenland-Route&no_cache=1

## Read Bicycle Routes Shapefile and Waypoint
```

Niederlausitzer_Bergbautour = readOGR("1_Niederlausitzer Bergbautour/1_Niederlausitzer Bergbautour_tracks_geom.shp")
Niederlausitzer_Bergbautour_points = readOGR("1_Niederlausitzer Bergbautour/1_Niederlausitzer Bergbautour_waypoints.shp")

DahmeRadweg = readOGR("2_DahmeRadweg/2_DahmeRadweg_tracks.shp")
DahmeRadweg_points = readOGR("2_DahmeRadweg/2_DahmeRadweg_waypoints.shp")

Elsterradtour = readOGR("4_Elsterradtour/4_Elsterradtour_tracks_geom.shp")
Elsterradtour_points = readOGR("4_Elsterradtour/4_Elsterradtour_waypoints.shp")

KohleWindWasser = readOGR("5_Kohle-Wind und Wasser-Radtour/5_Kohle-Wind und Wasser-Radtour_tracks_geom.shp")
KohleWindWasser_points = readOGR("5_Kohle-Wind und Wasser-Radtour/5_Kohle-Wind und Wasser-Radtour_waypoints.shp")

FürstPücklerWeg = readOGR("6_Fürst-Pückler-Weg/6_Fürst-Pückler-Weg_tracks_geom.shp")
FürstPücklerWeg_points = readOGR("6_Fürst-Pückler-Weg/6_Fürst-Pückler-Weg_waypoints.shp")

SchwarzeElsterRadweg = readOGR("7_Schwarze-Elster-Radweg/7_Schwarze-Elster-Radweg_tracks_geom.shp")
SchwarzeElsterRadweg_points = readOGR("7_Schwarze-Elster-Radweg/7_Schwarze-Elster-Radweg_waypoints.shp")

Gurkenradweg = readOGR("8_Gurkenradweg/8_Gurkenradweg_tracks_geom.shp")
Gurkenradweg_points = readOGR("8_Gurkenradweg/8_Gurkenradweg_waypoints.shp")

Hofjagdweg = readOGR("9_Hofjagdweg/9_Hofjagdweg_tracks_geom.shp")
Hofjagdweg_points = readOGR("9_Hofjagdweg/9_Hofjagdweg_waypoints.shp")

OderNeißeRadweg = readOGR("10_Oder-Neiße-Radweg/10_Oder-Neiße-Radweg_tracks_geom.shp")
OderNeißeRadweg_points = readOGR("10_Oder-Neiße-Radweg/10_Oder-Neiße-Radweg_waypoints.shp")

Spreeradweg = readOGR("11_Spreeradweg/11_Spreeradweg_tracks_geom.shp")
Spreeradweg_points = readOGR("11_Spreeradweg/11_Spreeradweg_waypoints.shp")

TourBrandenburg = readOGR("12_Tour Brandenburg/12_Tour Brandenburg_tracks_geom.shp")
TourBrandenburg_points = readOGR("12_Tour Brandenburg/12_Tour Brandenburg_waypoints.shp")

SeenlandRoute = readOGR("13_Seenland-Route/13_Seenland-Route_tracks_geom.shp")
SeenlandRoute_points = readOGR("13_Seenland-Route/13_Seenland-Route_waypoints.shp")
```

## Data from Tobias
```
# Tobias

FuerstPuecklerWeg = readOGR("Tobias/fuerst-pueckler-weg_tracks_geom.shp")

Mühlenrundtour = readOGR("Tobias/Mühlenrundtour_tracks_geom.shp")
Mühlenrundtour_points = readOGR("Tobias/Mühlenrundtour_waypoints.shp")

NiederlausitzerBergbautour = readOGR("Tobias/Niederlausitzer Bergbautour_tracks_geom.shp")

SpreewälderFünfSchlösser = readOGR("Tobias/Spreewälder Fünf-Schlösser-Radtour_tracks_geom.shp")
SpreewälderFünfSchlösser_points = readOGR("Tobias/Spreewälder Fünf-Schlösser-Radtour_waypoints.shp")

Tour1 = readOGR("Tobias/Tour1_tracks_geom.shp")
```

## Read Lusatia Administrative Boundary Shapefile
```
Lusatia = readOGR("Lusatia_Administrative_Boundary_EPSG4326_Dissolved.shp")
```

## Read Land Surface Temperature (Calcaculated from Landsat 8)
```
LST = raster("LST_2022_07_19_Landsat8L1_Lusatia_WGS84.tif")
```

## Read Lusatia Districts Boundary
```
Bautzen = st_read("Bautzen.shp")
Cottbus = st_read("Cottbus.shp")
DahmeSpreewald = st_read("Dahme-Spreewald.shp")
ElbeElster = st_read("Elbe-Elster.shp")
Görlitz = st_read("Görlitz.shp")
OberspreewaldLausitz = st_read("Oberspreewald-Lausitz.shp")
SpreeNeiße = st_read("Spree-Neiße.shp")
```

## Mask LST by Districts
```
Bautzen_LST = raster::mask(LST, Bautzen)
Bautzen_LST = raster::crop(Bautzen_LST, Bautzen)

Cottbus_LST = raster::mask(LST, Cottbus)
Cottbus_LST = raster::crop(Cottbus_LST, Cottbus)

DahmeSpreewald_LST = raster::mask(LST, DahmeSpreewald)
DahmeSpreewald_LST = raster::crop(DahmeSpreewald_LST, DahmeSpreewald)

ElbeElster_LST = raster::mask(LST, ElbeElster)
ElbeElster_LST = raster::crop(ElbeElster_LST, ElbeElster)

Görlitz_LST = raster::mask(LST, Görlitz)
Görlitz_LST = raster::crop(Görlitz_LST, Görlitz)

OberspreewaldLausitz_LST = raster::mask(LST, OberspreewaldLausitz)
OberspreewaldLausitz_LST = raster::crop(OberspreewaldLausitz_LST, OberspreewaldLausitz)

SpreeNeiße_LST = raster::mask(LST, SpreeNeiße)
SpreeNeiße_LST = raster::crop(SpreeNeiße_LST, SpreeNeiße)
```

## Color Palette for LST
```
# Color Pallettes
pal_LST <- colorNumeric(
  palette = rev("RdYlBu") ,
  domain = values(LST), na.color = "transparent",  reverse = TRUE)
```

## Read ESRI LULC data
```
# LULC 
LULC = raster("ESRI_LULC_2021/Lusatia_ESRI_LULC_2021_Raw.tif")

pal_LULC <- colorNumeric(c("#419bdf", "#397d49", "#000000", "#7a87c6", "#e49635",
                      "#000000", "#c4281b", "#a59b8f", "#a8ebff", "#616161", "#e3e2c3"), values(LULC),
                    na.color = "transparent")


Cottbus_LULC = raster("Cottbus_LULC.tif")
Dahme_Spreewald_LULC = raster("Dahme_Spreewald_LULC.tif")
Elbe_Elster_LULC = raster("Elbe_Elster_LULC.tif")
Oberspreewald_Lausitz_LULC = raster("Oberspreewald_Lausitz_LULC.tif")
Spree_Neisse_LULC = raster("Spree_Neisse_LULC.tif")
Bautzen_LULC = raster("Bautzen_LULC.tif")
Goerlitz_LULC = raster("Goerlitz_LULC.tif")
```

## Plot Leaflet interactive map
```
LST_Lusatia = leaflet() %>%
  
  setView(lng = 14.332868, lat = 51.756310, zoom = 10) %>% 
  
  # Basemap (baseGroups)
  addTiles(group = "OSM (default)") %>%
  addProviderTiles(providers$OpenStreetMap, group = "Open Street Map") %>% 
  addProviderTiles(providers$Esri.WorldImagery, group = "ESRI World Imagery") %>% 
  addProviderTiles(providers$CartoDB.Positron, group = "CartoDB Positron") %>%
  
  
  # Bicycle Routes (OverlayGroups)
  addPolylines(data = Niederlausitzer_Bergbautour, color = "#990000", weight = 5, opacity = 0.8, group = "Niederlausitzer Bergbautour") %>%
  addPolylines(data = DahmeRadweg, color = "#999900", weight = 5, opacity = 0.8, group = "DahmeRadweg") %>%
  addPolylines(data = Elsterradtour, color = "#009900", weight = 5, opacity = 0.8, group = "Elsterradtour") %>%
  addPolylines(data = KohleWindWasser, color = "#009999", weight = 5, opacity = 0.8, group = "KohleWindWasser") %>%
  addPolylines(data = FürstPücklerWeg, color = "#000099", weight = 5, opacity = 0.8, group = "FürstPücklerWeg") %>%
  addPolylines(data = SchwarzeElsterRadweg, color = "#990099", weight = 5, opacity = 0.8, group = "SchwarzeElsterRadweg") %>%
  addPolylines(data = Gurkenradweg, color = "#404040", weight = 5, opacity = 0.8, group = "Gurkenradweg") %>%
  addPolylines(data = Hofjagdweg, color = "#663300", weight = 5, opacity = 0.8, group = "Hofjagdweg") %>%
  addPolylines(data = OderNeißeRadweg, color = "#336600", weight = 5, opacity = 0.8, group = "OderNeißeRadweg") %>%
  addPolylines(data = Spreeradweg, color = "#006633", weight = 5, opacity = 0.8, group = "Spreeradweg") %>%
  addPolylines(data = TourBrandenburg, color = "#003366", weight = 5, opacity = 0.8, group = "TourBrandenburg") %>%
  addPolylines(data = SeenlandRoute, color = "#000066", weight = 5, opacity = 0.8, group = "SeenlandRoute") %>%
  
  # Tobias
  addPolylines(data = FuerstPuecklerWeg, color = "#990000", weight = 5, opacity = 0.8, group = "Tobias: FuerstPuecklerWeg") %>%
  addPolylines(data = Mühlenrundtour, color = "#990099", weight = 5, opacity = 0.8, group = "Tobias: Mühlenrundtour") %>%
  addPolylines(data = NiederlausitzerBergbautour, color = "#336600", weight = 5, opacity = 0.8, group = "Tobias: NiederlausitzerBergbautour") %>%
  addPolylines(data = SpreewälderFünfSchlösser, color = "#003366", weight = 5, opacity = 0.8, group = "Tobias: SpreewälderFünfSchlösser") %>%
  addPolylines(data = Tour1, color = "#000066", weight = 5, opacity = 0.8, group = "Tobias: Tour1") %>%
  
  # Bicycle Waypoints
  addMarkers(data = Niederlausitzer_Bergbautour_points, lng = ~Niederlausitzer_Bergbautour_points@coords[,1], lat = ~Niederlausitzer_Bergbautour_points@coords[,2], group = "Niederlausitzer Bergbautour", 
             popup = paste0(Niederlausitzer_Bergbautour$name, "<br>Length: ", round(Niederlausitzer_Bergbautour$length/1000, 2), " km")) %>%
  addMarkers(data = DahmeRadweg_points, lng = ~DahmeRadweg_points@coords[,1], lat = ~DahmeRadweg_points@coords[,2], group = "DahmeRadweg", 
             popup = paste0(DahmeRadweg$name, "<br>Length: ", round(DahmeRadweg$length/1000, 2), " km")) %>%
  addMarkers(data = Elsterradtour_points, lng = ~Elsterradtour_points@coords[,1], lat = ~Elsterradtour_points@coords[,2], group = "Elsterradtour", 
             popup = paste0(Elsterradtour$name, "<br>Length: ", round(Elsterradtour$length/1000, 2), " km")) %>%
  addMarkers(data = KohleWindWasser_points, lng = ~KohleWindWasser_points@coords[,1], lat = ~KohleWindWasser_points@coords[,2], group = "KohleWindWasser", 
             popup = paste0(KohleWindWasser$name, "<br>Length: ", round(KohleWindWasser$length/1000, 2), " km")) %>%
  addMarkers(data = FürstPücklerWeg_points, lng = ~FürstPücklerWeg_points@coords[,1], lat = ~FürstPücklerWeg_points@coords[,2], group = "FürstPücklerWeg", 
             popup = paste0(FürstPücklerWeg$name, "<br>Length: ", round(FürstPücklerWeg$length/1000, 2), " km")) %>%
  addMarkers(data = SchwarzeElsterRadweg_points, lng = ~SchwarzeElsterRadweg_points@coords[,1], lat = ~SchwarzeElsterRadweg_points@coords[,2], group = "SchwarzeElsterRadweg", 
             popup = paste0(SchwarzeElsterRadweg$name, "<br>Length: ", round(SchwarzeElsterRadweg$length/1000, 2), " km")) %>%
  addMarkers(data = Gurkenradweg_points, lng = ~Gurkenradweg_points@coords[,1], lat = ~Gurkenradweg_points@coords[,2], group = "Gurkenradweg", 
             popup = paste0(Gurkenradweg$name, "<br>Length: ", round(Gurkenradweg$length/1000, 2), " km")) %>%
  addMarkers(data = Hofjagdweg_points, lng = ~Hofjagdweg_points@coords[,1], lat = ~Hofjagdweg_points@coords[,2], group = "Hofjagdweg", 
             popup = paste0(Hofjagdweg$name, "<br>Length: ", round(Hofjagdweg$length/1000, 2), " km")) %>%
  addMarkers(data = OderNeißeRadweg_points, lng = ~OderNeißeRadweg_points@coords[,1], lat = ~OderNeißeRadweg_points@coords[,2], group = "OderNeißeRadweg", 
             popup = paste0(OderNeißeRadweg$name, "<br>Length: ", round(OderNeißeRadweg$length/1000, 2), " km")) %>%
  addMarkers(data = Spreeradweg_points, lng = ~Spreeradweg_points@coords[,1], lat = ~Spreeradweg_points@coords[,2], group = "Spreeradweg", 
             popup = paste0(Spreeradweg$name, "<br>Length: ", round(Spreeradweg$length/1000, 2), " km")) %>%
  addMarkers(data = TourBrandenburg_points, lng = ~TourBrandenburg_points@coords[,1], lat = ~TourBrandenburg_points@coords[,2], group = "TourBrandenburg", 
             popup = paste0(TourBrandenburg$name, "<br>Length: ", round(TourBrandenburg$length/1000, 2), " km")) %>%
  addMarkers(data = SeenlandRoute_points, lng = ~SeenlandRoute_points@coords[,1], lat = ~SeenlandRoute_points@coords[,2], group = "SeenlandRoute", 
             popup = paste0(SeenlandRoute$name, "<br>Length: ", round(SeenlandRoute$length/1000, 2), " km")) %>%
  
  # Tobias
  addMarkers(data = Mühlenrundtour_points, lng = ~Mühlenrundtour_points@coords[,1], lat = ~Mühlenrundtour_points@coords[,2], group = "Tobias: Mühlenrundtour", 
             popup = paste0(Mühlenrundtour$name, "<br>Length: ", round(Mühlenrundtour$length/1000, 2), " km")) %>%
  addMarkers(data = SpreewälderFünfSchlösser_points, lng = ~SpreewälderFünfSchlösser_points@coords[,1], lat = ~SpreewälderFünfSchlösser_points@coords[,2], group = "Tobias: SpreewälderFünfSchlösser", 
             popup = paste0(SpreewälderFünfSchlösser$name, "<br>Length: ", round(SpreewälderFünfSchlösser$length/1000, 2), " km")) %>%
  
  # LST Raster
  addRasterImage(Bautzen_LST, colors = pal_LST, opacity = 0.7, group = "Land Surface Temperature") %>%
  addRasterImage(Cottbus_LST, colors = pal_LST, opacity = 0.7, group = "Land Surface Temperature", layerId = "raster") %>%
  addRasterImage(DahmeSpreewald_LST, colors = pal_LST, opacity = 0.7, group = "Land Surface Temperature") %>%
  addRasterImage(ElbeElster_LST, colors = pal_LST, opacity = 0.7, group = "Land Surface Temperature") %>%
  addRasterImage(Görlitz_LST, colors = pal_LST, opacity = 0.7, group = "Land Surface Temperature") %>%
  addRasterImage(OberspreewaldLausitz_LST, colors = pal_LST, opacity = 0.7, group = "Land Surface Temperature") %>%
  addRasterImage(SpreeNeiße_LST, colors = pal_LST, opacity = 0.7, group = "Land Surface Temperature") %>%
  
  # LULC Raster
  addRasterImage(Cottbus_LULC, colors = pal_LULC, opacity = 0.8,
                 group = "Land Use Land Cover 2021") %>%
  addRasterImage(Dahme_Spreewald_LULC, colors = pal_LULC, opacity = 0.8,
                 group = "Land Use Land Cover 2021") %>%
  addRasterImage(Elbe_Elster_LULC, colors = pal_LULC, opacity = 0.8,
                 group = "Land Use Land Cover 2021") %>%
  addRasterImage(Oberspreewald_Lausitz_LULC, colors = pal_LULC, opacity = 0.8,
                 group = "Land Use Land Cover 2021") %>%
  addRasterImage(Spree_Neisse_LULC, colors = pal_LULC, opacity = 0.8,
                 group = "Land Use Land Cover 2021") %>%
  addRasterImage(Bautzen_LULC, colors = pal_LULC, opacity = 0.8,
                 group = "Land Use Land Cover 2021") %>%
  addRasterImage(Goerlitz_LULC, colors = pal_LULC, opacity = 0.8,
                 group = "Land Use Land Cover 2021") %>%
  
  
  # Lusatia Boundary
  addPolylines(data = Lusatia, color = "red", weight = 5, opacity = 0.5, group = "Lusatia") %>%
  
  # Legend: LST
  addLegend(pal = pal_LST, 
            values = values(LST),
            title = "Land Surface Temperature (C) <br> 2022_07_19", opacity = 0.9, group = "Land Surface Temperature") %>%
  
  # Legend: LULC
  addLegend(
    values = values(Cottbus_LULC),
    title = "ESRI LULC 2021",
    colors = c("#419bdfff", "#397d49ff", "#000000ff", "#7a87c6ff", "#e49635ff", "#000000ff", "#c4281bff", "#a59b8fff", "#a8ebffff", "#616161ff", "#e3e2c3ff"),
    labels = c("Water", "Trees", "", "Grass", "Crops", "", "Built Area", "Flooded vegetation", "Scrub/shrub", "Bare ground", "Rangeland"),
    opacity = 1, group = "Land Use Land Cover 2021") %>%
  
  
  # Layer Control
  addLayersControl(baseGroups = c("OSM (default)", "ESRI World Imagery", "CartoDB Positron"),
                   overlayGroups = c("Land Surface Temperature",
                                     "Land Use Land Cover 2021",
                                     
                                     "Niederlausitzer Bergbautour", 
                                     "DahmeRadweg",
                                     "Elsterradtour",
                                     "KohleWindWasser",
                                     "FürstPücklerWeg",
                                     "SchwarzeElsterRadweg",
                                     "Gurkenradweg",
                                     "Hofjagdweg",
                                     "OderNeißeRadweg",
                                     "Spreeradweg",
                                     "TourBrandenburg",
                                     "SeenlandRoute",
                                     "Tobias: FuerstPuecklerWeg",
                                     "Tobias: Mühlenrundtour",
                                     "Tobias: NiederlausitzerBergbautour",
                                     "Tobias: SpreewälderFünfSchlösser",
                                     "Tobias: Tour1",
                                     
                                     "Lusatia"
                   ),
                   
                   options = layersControlOptions(collapsed = FALSE)) %>%
  
  
  # Hide Group: Bicycle Routes 
  hideGroup("Niederlausitzer Bergbautour") %>% 
  hideGroup("DahmeRadweg") %>% 
  hideGroup("Elsterradtour") %>% 
  hideGroup("KohleWindWasser") %>% 
  hideGroup("FürstPücklerWeg") %>% 
  hideGroup("SchwarzeElsterRadweg") %>% 
  hideGroup("Gurkenradweg") %>% 
  hideGroup("Hofjagdweg") %>% 
  hideGroup("OderNeißeRadweg") %>% 
  hideGroup("Spreeradweg") %>% 
  hideGroup("TourBrandenburg") %>% 
  hideGroup("SeenlandRoute") %>%
  
  hideGroup("Tobias: FuerstPuecklerWeg") %>% 
  hideGroup("Tobias: Mühlenrundtour") %>% 
  hideGroup("Tobias: NiederlausitzerBergbautour") %>% 
  hideGroup("Tobias: SpreewälderFünfSchlösser") %>% 
  hideGroup("Tobias: Tour1") %>%

  # Hide Group: LST
  hideGroup("Land Surface Temperature") %>% 
  
  # Hide Group: LULC 
  hideGroup("Land Use Land Cover 2021") %>% 

  # Opacity
  addOpacitySlider(layerId = "raster")
```

## Export map as HTML
```
LST_Lusatia
htmlwidgets::saveWidget(LST_Lusatia, "Lusatia_BicycleRoutes_LST_LULC.html", selfcontained = T)
```

