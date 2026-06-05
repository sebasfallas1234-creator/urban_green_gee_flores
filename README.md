[README.md](https://github.com/user-attachments/files/28625266/README.md)
# Urban Green Infrastructure Analysis — Canton of Flores, Heredia
### Google Earth Engine | Sentinel-2 | Dynamic World | JavaScript API

Geospatial analysis of urban green spaces in the Canton of Flores, Heredia, Costa Rica (6.75 km²), developed as part of a thesis research project on native bee diversity and melliferous plant interactions in urban environments at Universidad Nacional de Costa Rica.

This repository contains Google Earth Engine scripts for mapping and characterizing the full urban green infrastructure of the canton using Sentinel-2 SR imagery and Dynamic World land classification, providing the spatial framework for biodiversity sampling and future temporal monitoring of urban green space change.

---

## Study Area

**Location:** Canton of Flores, Heredia, Costa Rica  
**Extension:** 6.75 km²  
**Context:** Smallest canton in Costa Rica, located within the Greater Metropolitan Area (GAM)  
**Research purpose:** Spatial characterization of urban green spaces to support native bee (Apidae) biodiversity sampling and conservation planning

---

## Scripts

### 01 — NDVI Classification (`01_ndvi_classification.js`)

Processes Sentinel-2 SR imagery to calculate vegetation indices and classify land cover across the study area.

**What it does:**
- Filters Sentinel-2 SR collection by date and cloud cover (<10% threshold)
- Selects the least cloudy image for initial visualization
- Calculates **NDVI** (Normalized Difference Vegetation Index) and **NDBI** (Normalized Difference Built-up Index) across annual median composite
- Classifies NDVI into 3 land cover categories:
  - Class 1 (NDVI < 0.2): Bare soil / urban surfaces
  - Class 2 (NDVI 0.2–0.4): Sparse vegetation / transitional areas
  - Class 3 (NDVI ≥ 0.4): Dense vegetation / green spaces
- Extracts quantitative statistics (mean, standard deviation) via `reduceRegion`
- Exports classified raster as GeoTIFF to Google Drive

**Key outputs:**
- NDVI classification map (3 classes)
- Descriptive statistics (mean NDVI, standard deviation)
- GeoTIFF export for ArcGIS/QGIS integration

---

### 02 — Dynamic World Composite (`02_dynamic_world_composite.js`)

Integrates Google's Dynamic World V1 ML land classification with Sentinel-2 imagery to produce annual land cover composites.

**What it does:**
- Filters both Dynamic World and Sentinel-2 collections for a full annual period
- Links both collections using `linkCollection` for multi-source analysis
- Generates annual composites:
  - Sentinel-2 median composite (RGB visualization)
  - Dynamic World mode composite (most frequent land class per pixel)
- Exports both composites as GeoTIFF to Google Drive

**Dynamic World land classes:**
| Value | Class |
|-------|-------|
| 0 | Water |
| 1 | Trees |
| 2 | Grass |
| 3 | Flooded vegetation |
| 4 | Crops |
| 5 | Shrub & scrub |
| 6 | Built area |
| 7 | Bare ground |
| 8 | Snow & ice |

---

## Methods

```
Sentinel-2 SR Collection
        │
        ├─── Cloud filtering (< 10-20%)
        ├─── Annual median compositing
        ├─── NDVI / NDBI band math
        ├─── Threshold classification
        └─── reduceRegion statistics
        
Dynamic World V1
        │
        ├─── Annual temporal filtering
        ├─── linkCollection (Sentinel-2)
        ├─── Mode composite
        └─── GeoTIFF export → ArcGIS/QGIS
```

---

## Tools & Data Sources

| Tool | Purpose |
|------|---------|
| Google Earth Engine (JavaScript API) | Cloud-based geospatial processing |
| Sentinel-2 SR Harmonized (`COPERNICUS/S2_SR_HARMONIZED`) | Multispectral imagery (10m resolution) |
| Dynamic World V1 (`GOOGLE/DYNAMICWORLD/V1`) | ML-based land cover classification |
| ArcGIS Pro / QGIS | Post-processing and final cartography |
| R | Biodiversity statistical analysis (Shannon-Wiener, Kruskal-Wallis) |

---

## Research Context

This spatial analysis supports a broader thesis study on the interactions between native bees (*Apidae*) and melliferous plants in urban gardens of the Canton of Flores. The methodology is designed to:

1. Map the full urban green infrastructure as a sampling framework
2. Identify spatial distribution of green spaces for biodiversity sampling site selection
3. Enable future temporal monitoring of urban green space change — tracking increases or decreases in vegetation cover over time

**Thesis title:** *Abejas nativas y plantas melíferas en entornos urbanos: una evaluación en la ciudad de Heredia*  
**Institution:** Universidad Nacional de Costa Rica, Escuela de Ciencias Biológicas  
**Program:** Licenciatura en Manejo de Recursos Naturales y Conservación de la Biodiversidad

---

## How to Use

1. Open [Google Earth Engine Code Editor](https://code.earthengine.google.com/)
2. Copy and paste the desired script
3. Replace the `geometry` asset path with your own study area geometry:
   ```javascript
   var geometry = ee.FeatureCollection("projects/YOUR_PROJECT/assets/YOUR_ASSET");
   ```
4. Adjust date ranges and cloud cover thresholds as needed
5. Run and export results to Google Drive

---

## Author

**Sebastián Fallas Valencia**  
B.S. Geography with Emphasis in Territorial Planning  
Licenciatura (in progress), Natural Resource Management & Biodiversity Conservation  
Universidad Nacional de Costa Rica  
sebas.fallas1234@gmail.com

---

## License

This project is open for academic and research use. Please cite appropriately if used in publications.
