# Description
These tools ([jupyter notebooks](https://jupyter.org/) ) ingest data from the NOAA Hydrometeorological Design Studies Center ([HDSC](https://www.nws.noaa.gov/oh/hdsc/index.html)) and return unique, weighted excess rainfall events suitable for use in 2D hydraulic *rain-on-grid* models. This approach relies on:

  1. Meteorological data
  2. NRCS and NOAA sampling
  3. Hydrologic CN loss methord


---

## Contents

- [__PrecipTable__](PrecipTable.ipynb): Retrieve NOAA Atlas 14 precipitation statisics for an Area of Interest (AOI) and prepare NRCS nested hyetograph shapes to be applied to the`EventsTable_Stratified`notebook.

- [__EventsTable_Stratified__](EventsTable_Stratified.ipynb): Calculates a stratified sample of runoff events given rainfall and maximum potential retention distributions. For each each event and corresponding return interval, the event weight, runoff value, maximum potential retention value, and rainfall value are calculated.

- [__EventsTable_Traditional__](EventsTable_Stratified.ipynb): Calculates a stratified sample of runoff events given rainfall and maximum potential retention distributions. For each each event and corresponding return interval, the event weight, runoff value, maximum potential retention value, and rainfall value are calculated. Outputs are exported as JSON and DSS files

- [__reEventsTable__](reEventsTable.ipynb): Calculates the reduced excess rainfall given a user-specified stormwater removal rate and capacity. Given user-specified contributing areas (stormsheds), the lateral inflow hydrograhs are also calculated for each event.

- `ProjectArea_HUC_Number.xlsx `: Excel Workbook used to store the CN, stormwater removal rate and capacity, and information on lateral inflow domains for each pluvial domain within a pluvial model. This Workbook is called by `EventsTable`, `PM-EventsTable`, `distalEventsTable`, and `reEventsTable`.

*The ([CN Method](https://www.nrcs.usda.gov/Internet/FSE_DOCUMENTS/stelprdb1044171.pdf)) is currently the only transform method in use for this project. Other transforms are available and can be adopted into the tool with minor modifications.

---

## Workflow

1. Run [PrecipTable](PrecipTable.ipynb) in order to calculate the area-averaged precipitation frequency table for the specified durations as well as to determine the NOAA Atlas 14 volume and region.
    ```
      Inputs:
        1. HUC code (can be HUC8, HUC10, or HUC12)
        2. Optional/as needed: 
            - Precipitation event durations; the standard  durations used is 24 hour.

      Outputs:
        1. A spreadsheet with the area-averaged precipitation frequency table for each duration, along with the NOAA Atlas 14 volume and region numbers.
        2. A spreadsheet with the NRCS nested hyetograph shapes.
        3. A spreadsheet with the NOAA 50% decile hyetograph shapes for each quartile (& weight of each quartile)
    ```
    
    
2. Run [EventsTable_Traditional](EventsTable_Traditional.ipynb)  in order to calculate excess rainfall events and [reEventsTable](reEventsTable.ipynb) to perform the stormwater reduction (optional).

    ```
      Inputs:
        1. PrecipTable.xlsx from step 1, which contains precipitation frequency tables and the NOAA Atlas 14 volume and region number. Note that the volume and region number may also be entered manually.
        2. Pluvial_Parameters.xlsx metadata file which contains the curve number and information on the stormwater infrastructure.
        4. Storm durations
        5. Filenames and paths for outputs
        6. EventsTable.ipynb

      Outputs:
        1. Precipitation statistics for each duration
        2. HTML copy of notebook
        3. Excess Runoff hyetographs in JSON and DSS format
    ```
