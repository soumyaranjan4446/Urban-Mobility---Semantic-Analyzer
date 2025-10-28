# 🏙️ Project: Semantic Mobility Analyzer for NYC

This project analyzes a large-scale NYC taxi trajectory dataset to discover "functional regions" in New York City. It moves beyond simple hotspot mapping by clustering taxi zones based on their complete 24/7 activity profiles, allowing for the inference of semantic meaning (e.g., "Work Zone," "Residential Zone," "Nightlife Hub").

The core methodology is a practical implementation of the spatio-temporal data mining framework described in the paper:

**Ghosh, S., Ghosh, S. K., & Buyya, R. (2020).**  
*MARIO: A spatio-temporal data mining framework on Google Cloud to explore mobility dynamics from taxi trajectories.*  
Journal of Network and Computer Applications, 164, 102692.

---

## 🗺️ Final Semantic Map

The final result is a map where ~260 taxi zones are clustered into 6 distinct groups based on their "activity DNA." Each color represents a unique, learned "zone personality."

An interactive version of this map is available here:  
### 🔗 Explore the Interactive Map
<img src="https://github.com/soumyaranjan4446/Urban-Mobility---Semantic-Analyzer/blob/main/images/map.png">

---

## 📌 Methodology

This project was executed in four distinct phases, following the logic of the MARIO framework.

---

### ✅ Phase 1: Data Pre-processing

The NYC Yellow Taxi dataset and the official Taxi Zone shapefile were loaded. The trip data was cleaned, and all trips were enriched with time-based features (`pickup_hour`, `dropoff_hour`, `day_type`).

An initial sanity check of the raw data confirmed the expected, city-wide mobility patterns:

• Weekdays (blue): A bimodal (two-peak) pattern for morning and evening commutes  
• Weekends (orange): A unimodal (single-peak) pattern for later, sustained leisure and nightlife activity

---

### ✅ Phase 2: Mobility Event & Feature Engineering

This was the most critical step, where we defined our "functional regions" as the official NYC Taxi Zones.

Following the paper, we defined "mobility events" (pick-ups and drop-offs) and aggregated their counts by region, hour, and day type. This created a **96-dimension feature vector** (24 hours × 2 day types × 2 event types) for each of the ~260 taxi zones.

The maps below show the total aggregated pick-ups ("Travel Demand") and drop-offs. This shows where activity is (volume), but not the pattern. Our project's goal was to cluster based on the 96-feature pattern.

---

### ✅ Phase 3: ML Clustering & Semantic Analysis

With the feature matrix ready, we performed unsupervised machine learning to find the patterns.

• Normalization: Used StandardScaler to cluster based on the shape of a zone's activity pattern  
• Clustering: Used KMeans, and k = 6 was selected via the Elbow Method  
• Analysis: Examined cluster profiles to infer semantic meaning (e.g., "Source" vs "Sink")

---

### ✅ Phase 4: Visualization

• Static Map: GeoPandas visualization of clusters on NYC Taxi Zones  
• Interactive Map: Folium-based map (semantic_map_interactive.html)  
• Cluster Profiles: Plotted to reveal functional region behaviors

---

## 🎯 Semantic Cluster Profiles: The 6 "Personalities" of NYC

Below are the interpreted functional characteristics of each learned cluster.

---

### 🏠 Cluster 0: "The Outer-Borough Residential (Source)"

Analysis:  
This profile (covering Queens, Brooklyn, Bronx...) is a classic "Source" region. It shows a sharp peak in Weekday Pick-ups (red line) between 7-8 AM as people leave for work, and a corresponding peak in Weekday Drop-offs (blue line) in the evening as they return home. Its overall volume is much lower than the Manhattan clusters.

---

### ✈️ Cluster 1: "The Transport Hub" (Airports)

Analysis:  
This profile is unmistakable. It shows massive, sustained 24/7 activity with very high volumes for both pick-ups and drop-offs. The pattern is nearly identical on weekdays and weekends, showing it is not tied to a 9-to-5 commute. This unique signature belongs to JFK and LaGuardia Airports.

---

### 🏢 Cluster 3: "The 9-to-5 Work Zone (Sink)"

Analysis:  
This Manhattan cluster is the quintessential "Work Zone" and a classic "Sink" region. On weekdays, there is a massive influx of Drop-offs (blue line) in the morning (peaking 8-10 AM) and a massive exodus of Pick-ups (red line) in the evening (peaking 5-7 PM). On weekends, the area is comparatively quiet.

---

### 🏙️ Cluster 4: "The High-Density Work/Daytime Hub"

Analysis:  
This profile is also a Manhattan work zone but with a different flavor. The weekday drop-offs start high (the 8 AM commute) but stay high all day, peaking again at 4 PM. This suggests a dense commercial area (like Cluster 3) but with more all-day activity (meetings, errands, lunch) beyond the strict 9-to-5.

---

### 🎭 Cluster 2: "The Entertainment & Nightlife Zone"

Analysis:  
This Manhattan cluster's activity builds throughout the day, with drop-offs and pick-ups both peaking in the evening. Crucially, the Weekend (right chart) is significantly busier than the weekday, with high activity lasting late into the night. This is the signature of areas known for dining, theater, and nightlife.

---

### 🛍️ Cluster 5: "The All-Day Leisure & Shopping Zone"

Analysis:  
Similar to Cluster 2, this is another high-activity Manhattan zone. However, its weekend profile shows a massive, broad peak in both pick-ups and drop-offs that lasts all afternoon and evening (12 PM - 10 PM). This suggests areas defined by all-day attractions like shopping, tourism, and restaurants (e.g., SoHo, Times Square).

