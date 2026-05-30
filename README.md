# Site Selection for a Vineyard - Spatial Analysis on a Digital Elevation Model (DEM)

## 🍇 Project Goal
This project involves conducting a spatial analysis within a GIS environment to identify suitable land for establishing a vineyard[cite: 8]. The analysis is based on evaluating favorable terrain morphology, land cover, and existing environmental protection zones[cite: 10].

## 💻 Software & Technologies
The project was conducted using desktop GIS software and integrated geoprocessing libraries:
* **QGIS:** The primary graphical environment used for visualization and spatial analysis[cite: 179, 447].
* **GRASS GIS:** Integrated library used for advanced raster neighborhood operations (`r.neighbors`) and physical area grouping (`r.clump`)[cite: 396, 502].

## 🗺️ Input Data
The following raster layers were used for the analyses:
* **DEM (Digital Elevation Model):** Terrain height data (`nmt_dol_pradn_10m`)[cite: 14, 17].
* **Corine Land Cover:** A land cover map (`CLC_dol_pradn_10m`)[cite: 15, 18].
* **Protected Areas:** Boundaries of the Ojców National Park and its buffer zone (`OPN_dol_pradn_10m`)[cite: 16, 18].

## 📋 Location Criteria
The following conditions were defined to designate potential vineyard areas:
* **Slope Aspect (Exposure):** To maximize sunlight, slopes with southern, southeastern, or southwestern exposure are required (an azimuth quadrant of **+/- 45°** from the **180°** azimuth)[cite: 146, 151].
* **Terrain Slope:** For the profitability of mechanical grape harvesting, areas with slopes ranging from **5°** to **20°** were selected[cite: 147, 151].
* **Land Cover:** "Green" areas—specifically undeveloped and unforested lands—are preferred (CLC categories: 12, 18, 20, 21, 29)[cite: 325, 340].
* **Environmental Protection:** The Ojców National Park area was strictly excluded from the analysis[cite: 322, 340].
* **Investment Area:** To support industrial-scale operations, the minimum contiguous area of the vineyard must be at least **10 hectares**[cite: 327].

## 🛠️ Conducted Analyses and Key Algorithms
The following stages were completed using the QGIS Processing Toolbox and Map Algebra:

1.  **Generating DEM Derivative Maps:** * *Slope*: Calculating the terrain slope map (in degrees)[cite: 192].
    * *Aspect*: Calculating the map to determine the direction of the maximum slope[cite: 251].
2.  **Terrain Selection:** Using **Raster Calculator** (Map Algebra) to extract areas that simultaneously meet the specific slope and aspect requirements[cite: 331].
3.  **Environmental Selection:** Using **Reclassify by table** on the land cover layer (CLC) and excluding legally protected areas (OPN)[cite: 341].
4.  **Filtering and Generalization of Results:**
    * Applying a **Modal filter** in a 7x7 window (`r.neighbors` algorithm) to reduce "information noise," remove isolated single pixels, and smooth the boundaries of the selected patches[cite: 373, 394].
    * Performing **Raster generalization** (Sieve) with 8-pixel neighborhood disabled to eliminate small clusters, retaining only contiguous areas of at least 10 ha[cite: 448, 449].
5.  **Grouping Areas (Clumping):** Using the **`r.clump`** tool (with diagonal cells disabled) to assign a new, unique attribute value to each physically discrete group of pixels[cite: 500, 533]. This allows for subsequent independent statistical analysis of each site[cite: 501].
6.  **Data Cleaning:** Utilizing table reclassification to assign the `-9999` (NoData) value to the image "background" to isolate the chosen clusters[cite: 568].

   ## 🗺️ Result
   <img width="1327" height="913" alt="Result" src="https://github.com/user-attachments/assets/61e0eb6d-d557-4975-b79c-00d6e65b7b6d" />
