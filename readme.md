# Cary Homebuying Neighborhoods

## Summary

This web map explores a sample of homebuying neighborhoods in Cary, North Carolina. It is designed for prospective buyers who want to understand where different parts of town fall in terms of housing type, typical price, and build era, rather than focusing on individual listings.

The project was inspired by the article *“8 Things to Consider Before Moving to Cary, NC”* by Hodge & Kittrell Sotheby’s International Realty, which highlights schools, amenities, and neighborhoods as key reasons people relocate to Cary. This map adds a spatial perspective to that discussion.

Neighborhood boundaries were downloaded from OpenStreetMap using Overpass Turbo and then filtered in QGIS from roughly 150 neighborhoods down to about 40 larger or more prominent areas by population. For each selected neighborhood, I added attributes describing dominant housing type (single-family, townhome, condo), price category (low, mid, high, luxury), typical build era (pre-2000 or 2000s), bedroom counts, notes, and data sources based on real estate websites.

The final application is built with Leaflet. Each neighborhood is represented by a colored house icon, where color indicates price category. Clicking a house updates a details panel that summarizes the neighborhood’s housing characteristics in a clear, map-based format.

## Project overview

This project is an interactive web map that highlights a sample of home‐buying neighborhoods in Cary, North Carolina. It is designed for prospective homebuyers who are trying to understand **where** different types of neighborhoods are located and how they differ in terms of **price**, **housing type**, **build era**, and **typical bedrooms**.

The map accompanies the article  
*“8 Things to Consider Before Moving to Cary, NC”* by Hodge & Kittrell Sotheby’s International Realty, which describes what makes Cary attractive to buyers (schools, amenities, lifestyle, and housing options). This web map translates those ideas into a geographic view of real neighborhoods and their typical housing characteristics.

Users can:

- See which parts of Cary tend to be more **affordable**, **mid-range**, **high-end**, or **luxury**.
- Compare **single-family**, **townhome**, and **condo** neighborhoods.
- Read short **notes** describing each neighborhood’s character, housing stock, and amenities.
- Click on a house icon and view all the attributes in a neighborhood details panel.

---

## Data sources

**Neighborhood geometries**

- Neighborhood boundaries for Cary, NC were downloaded from **OpenStreetMap** using **Overpass Turbo**.
- Data were exported in GeoJSON format and contain approximately **150 neighborhood polygons** within or near the Town of Cary.

**Neighborhood attributes**

For each selected neighborhood, additional attributes were compiled manually from public web sources such as:

- Major real estate platforms (e.g., Zillow, Realtor.com, Redfin).
- Neighborhood information sites (e.g., Neighborhoods.com, realty team pages).
- Hodge & Kittrell article and related local real estate writeups.

Each neighborhood’s attribute table includes a `source` field listing the main websites consulted for that neighborhood.

**Software and libraries**

- **QGIS** for data cleaning, spatial processing, and attribute editing.
- **Overpass Turbo** for querying OpenStreetMap neighborhood data.
- **Leaflet** for web mapping.
- **Font Awesome** (for house icons).
- **HTML/CSS/JavaScript** for the layout and interaction.

---

## Data preparation and cleaning

### 1. Extracting neighborhoods from OpenStreetMap

1. Used **Overpass Turbo** to query OpenStreetMap for neighborhood polygons in and around Cary, NC (e.g., `place=neighbourhood` and administrative areas intersecting the Town of Cary).
2. Exported the result as **GeoJSON**.
3. Loaded the GeoJSON into **QGIS** and verified / set the layer’s coordinate reference system to **EPSG:4326 (WGS 84)**.

### 2. Filtering and selecting the top neighborhoods

1. The initial Overpass Turbo dataset contained around **150 neighborhoods**.
2. The goal of the final project was not to map every small subdivision, but to focus on **major homebuying neighborhoods**.
3. In QGIS, I used the available **population or size–related attributes** and basic inspection to identify the **largest and most relevant neighborhoods by population**.
4. From this full set, I selected a final subset of approximately **40 neighborhoods**, representing some of the most significant homebuying areas in Cary (e.g., Amberly, Preston, Lochmere, Highcroft, etc.).

### 3. Creating point locations for mapping

1. The original features from OSM are polygons (neighborhood areas).
2. To map them as icons in Leaflet, I used QGIS to:
   - Generate **representative points** (centroids) inside each neighborhood polygon, or  
   - Place a labeled point in the most appropriate central location.
3. The resulting point layer is what is exported to the web as:
   - `assets/cary_homebuying_neighborhoods.geojson`

### 4. Adding neighborhood attributes

In QGIS, I created and populated several fields for each selected neighborhood:

- `type` – dominant housing type in the neighborhood
  - `single_family`
  - `townhome`
  - `condo`
- `price_cat` – price category based on approximate home values
  - `low` – typically **under ~$400k**
  - `mid` – typically **~$400k–$600k**
  - `high` – typically **~$600k–$800k**
  - `luxury` – typically **over ~$800k**
- `year_cat` – general build era
  - `pre2000` – majority of homes built before 2000
  - `2000s` – majority of homes built after 2000 (including 2010s)
- `beds` – typical number of bedrooms for a “standard” home in that neighborhood (usually 3–5)
- `notes` – a short descriptive sentence summarizing the neighborhood’s housing stock, character, and amenities
- `source` – a text list of the main websites used to determine the above attributes

To populate these:

1. For each neighborhood, I looked up several current and recent **for-sale** listings.
2. I reviewed **median listing prices**, **recent sale prices**, and typical **bed/bath counts**.
3. I scanned neighborhood descriptions (e.g., “luxury custom homes,” “midsize 3–4 bed single-family,” “townhome community,” etc.).
4. Based on this information, I assigned:
   - A **price category** (low, mid, high, luxury).
   - A **build era category** (`pre2000` vs `2000s`).
   - A **typical bedroom count**.
   - A short **note** summarizing the main takeaways
5. I added all attributes directly into QGIS and saved the edited layer.

Finally, I exported the cleaned and attributed point layer as:

- `assets/cary_homebuying_neighborhoods.geojson`

This file is what the Leaflet web map loads.

---

## Web map design and implementation

### Layout

- A simple page layout with:
  - A **title and description** at the top, explaining the purpose of the map and referencing the Hodge & Kittrell article.
  - The **map on the left**.
  - A **neighborhood details panel on the right**, which updates when the user clicks a marker.

### Symbology

- Each neighborhood is shown as a **Font Awesome house icon** (`fa-home`).
- **Color encodes price category**:
  - `low` → light blue (`#9ecae1`)
  - `mid` → medium blue (`#6baed6`)
  - `high` → dark blue (`#3182bd`)
  - `luxury` → deep purple (`#54278f`)
- A **scale bar** is included at the bottom left.
- A **legend** in the bottom right explains both the price categories and approximate price ranges (e.g., “Mid (~$400k–$600k)”).

### Interactivity

- There are **no popups on the map**; instead:
  - Clicking a house icon updates the **right-hand details panel**.
- The details panel shows:
  - Neighborhood **name**
  - **Housing type**
  - **Price category**
  - **Year category** (pre-2000 vs 2000s)
  - **Typical bedrooms**
  - **Notes** (summary description)
  - **Source** (main websites used)


This design keeps the map itself clean, while still providing detailed information in a readable, accessible panel.
