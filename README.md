# 5G-NR3500-PUT-Campus-WinProp

This project simulates 5G NR3500 TDD radio coverage and wave propagation for two shared Orange/T-Mobile base station sites in the area of the Poznan University of Technology campus and its nearby urban surroundings. The workflow includes preparing the urban model in WallMan, setting up the network in ProMan, and analyzing the simulation results.

| Base station | Location | Infrastructure | Technology |
|---|---|---|---|
| Site 1 | ul. Św. Rocha 9, Poznań | Orange/T-Mobile shared NetWorks | NR3500 TDD |
| Site 2 | ul. Jana Pawła II 14, Poznań | Orange/T-Mobile shared NetWorks | NR3500 TDD |


### 1. Input data sources
* **OpenStreetMap** — map and urban area data used to prepare the simulation environment.
* **BTSearch** — map used to locate base station positions.
* **SI2PEM** — public database providing base station locations, technologies, frequency bands, and measurement reports (including antenna parameters such as azimuth, height, and downtilt).
* **Altair WinProp examples**
  * `5G TDD n79 100 MHz.wst` — used as the closest air interface template, with the frequency set to 3500 MHz.
  * `WiMax_3500MHz_60deg.apb` — used as an approximate 3500 MHz sector antenna pattern.


### 2. Urban Database Preparation and Preprocessing in WallMan

The urban model was created in **WallMan** from OpenStreetMap vector data (`kampus_PP.osm`) and converted into a WinProp urban database (`kampus_PP.odb`) containing buildings, vegetation and other physical obstacles affecting radio wave propagation. To improve the simulation workflow in ProMan, the environment was preprocessed using **Intelligent Ray Tracing (IRT)**.


### 3. ProMan Network Planning Project

A new network planning project was created in **ProMan** using the preprocessed urban database (`kampus_PP.oib`, not included due to GitHub file size limits).

Key simulation parameters:
* **Technology:** 5G NR
* **Band:** NR3500
* **Duplex:** TDD
* **Carrier frequency:** 3500 MHz
* **Channel bandwidth:** 100 MHz
* **MIMO:** 8 streams
* **Prediction height:** 1.5 m
* **Prediction resolution:** 10 m


### 4. Base Station Configuration

The base stations were configured in **ProMan** as MIMO sites with directional sector antennas. The antenna configuration was based on SI2PEM data and an approximate 3500 MHz sector antenna pattern.

General antenna setup:
* **Antenna pattern:** `WiMax_3500MHz_60deg.apb`
* **Polarization:** X polarization ±45°
* **Antenna gain:** 17 dBi
* **Max. Tx Power:** 50 dBm EIRP (theoretical value).

#### Poznań, ul. Św. Rocha 9

| Sector | Azimuth | Downtilt | Antenna height |
|---|---:|---:|---:|
| Sector 1 | 18° | 7° | 34.5 m |
| Sector 2 | 135° | 7° | 37.5 m |
| Sector 3 | 265° | 7° | 37.2 m |

#### Poznań, ul. Jana Pawła II 14

| Sector | Azimuth | Downtilt | Antenna height |
|---|---:|---:|---:|
| Sector 1 | 90° | 7° | 19.5 m |
| Sector 2 | 209° | 7° | 19.5 m |
| Sector 3 | 330° | 7° | 19.5 m |


### 5. Simulation Results

#### RSRP Map
<p align="center">
  <img src="screenshots/rsrp_map.png" alt="RSRP map">
</p>

#### Downlink Throughput Map
<p align="center">
  <img src="screenshots/downlink_throughput_map.png" alt="DL throughput map">
</p>

#### Cell Area Map
<p align="center">
  <img src="screenshots/cell_area_map.png" alt="Cell area map">
</p>


### 6. Observations

**RSRP Map:** The strongest signal levels are seen directly in front of the configured antenna sectors, especially in areas with LOS conditions. For example, around the base station at **ul. Jana Pawła II 14** in the northern part of the map, there are fewer buildings exceeding the base station antenna height compared to the area near **ul. Św. Rocha 9**. As a result, signal propagation is more effective in this area, with reduced shadowing and stronger coverage around the base station. In NLOS areas, the RSRP values drop. Overall, the use of two base stations improves the coverage of the analysed campus area.

**Downlink Throughput Map:** The throughput levels follow the RSRP pattern: the highest values, up to about **1.9 Gbit/s**, appear in areas with stronger coverage and better LOS conditions. Medium values, around **300–700 Mbit/s**, are visible in the central part of the analysed area, while the lowest values, below about **150 Mbit/s**, occur in shadowed NLOS areas and farther from the antenna sectors.

**Cell Area Assignment Map:** This map shows how the campus is divided among serving sectors, with each colour representing a different cell. The cell boundaries are strongly affected by antenna direction, buildings, and LOS/NLOS conditions. As a result, some sectors dominate large areas, while others are limited to the area close to their base station.
