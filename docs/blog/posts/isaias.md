---
date:
  created: 2026-10-09
authors:
  - sbattula
  - ksarna
  - svyas
  - jmframe
slug: isaias-streamflow-forecast
---

# Forecasting Alabama streamflow ahead of Hurricane Isaias

As Hurricane Isaias approaches the northern Gulf Coast, we are issuing streamflow forecasts for 12 Alabama watersheds to test our kriging-based data assimilation method during a real-world event. **We are sharing these forecasts before the anticipated flood response, while the outcome is still unknown.** The question is whether updating a hydrologic model with information from the gauge network helps it predict what comes next.

Our earlier work examined Hurricane Helene retrospectively (Battula et al., 2026a, 2026c, 2026d). This time, we are recording predictions ahead of the event. Once the storm and river response have passed, we will compare these forecasts with USGS observations and report the results in a follow-up blog.

<!-- more -->

<p align="center">
  <img src="/blog/pics/asaias/isaias_track.png" alt="Hurricane Isaias forecast track and key messages in the supplied National Hurricane Center advisory" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*Hurricane Isaias track and key messages from the supplied [NOAA National Hurricane Center](https://www.nhc.noaa.gov/) graphic, labeled Advisory 13, October 9, 2026, 4:00 p.m. CDT. This is a snapshot of the advisory available for this post.*

<p align="center">
  <img src="/blog/pics/asaias/map.jpg" alt="Alabama map showing the 12 USGS gauges, their upstream drainage basins, HUC8 subbasin boundaries, and river networks" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*The 12 forecast locations and their upstream watersheds. Basin and river layers come from USGS hydrologic datasets; the Alabama outline comes from the U.S. Census Bureau. Numbers match the forecast panels below.*

## How the forecasts work

Kriging uses spatial relationships among measured streamflows to estimate runoff between gauges. We first divide flow by drainage area so that large and small watersheds can be compared. The result is an estimated runoff field, along with an estimate of interpolation uncertainty. These are **pseudo-observations**, rather than direct measurements at every catchment; our [earlier kriging blog](https://hub.ciroh.org/blog/continental-scale-streamflow-kriging-ngiab-nrds) explains the approach (Battula et al., 2026b).

The data assimilation workflow described in our Helene technical study uses these estimates to update water stored in the soil, groundwater, and runoff reservoirs of the Conceptual Functional Equivalent (CFE) rainfall-runoff model. An ensemble Kalman filter balances the model ensemble's uncertainty against the kriging uncertainty: less certain estimates receive less weight. T-Route then moves the modeled flow through the river network to each outlet. Weather forecasts drive the model forward from the updated starting conditions; kriging alone does not predict future flow.

We compare forecasts initialized with data assimilation (DA) against an **open-loop** baseline that runs without those updates. The supplied panels show routed 18-hour forecasts from successive hourly issue times. Their spread shows variation among ensemble members, not a guarantee that the eventual flow will fall within it. Sparse gauges can miss a localized flood peak, and uncertain rainfall can change both its size and timing.

## What we will evaluate after the event

After the river response has passed and observations are available, we will assess peak flow, peak timing, and accuracy at different forecast lead times, comparing DA with the open-loop baseline. Verification will use observations after each forecast's issue time; those future observations cannot inform the forecast being evaluated. Our follow-up post will report where assimilation helped, where it did not, and what the event teaches us about the method. No forecast skill for this event has been established yet.

## Related work

- **2026a:** Battula, S., Frame, J. M., Sarna, K., Vyas, S., & Powers, E. (2026, September). Spatially complete calibration and data assimilation for streamflow forecasting: A case study of Tropical Cyclone Helene. *Alabama Water Resources Conference*, Orange Beach, AL.
- **2026b:** Battula, S., Sarna, K., Vyas, S., Vemula, S., Patel, A., & Frame, J. M. (2026, May). Continental-Scale Streamflow Simulation Using Kriging in the NGIAB-NRDS NextGen Ecosystem. *CIROH Developers Conference*, Salt Lake City, UT.
- **2026c:** Battula, S., Frame, J. M., Sarna, K., Vyas, S., & Powers, E. (2026, May). Spatially complete autoregression and data assimilation for streamflow forecasting: A case study of Tropical Cyclone Helene. *CIROH CIWRO Flash Flood Forecasting Workshop*, Norman, OK.
- **2026d:** Battula, S., Powers, E., & Frame, J. M. (2026, March). Ensemble Streamflow Forecasting Using Deep Learning: A Case Study of Tropical Cyclone Helene. *37th Conference on Hurricanes and Tropical Meteorology*, San Diego, CA.

## Forecasts issued before the event

These panels preserve the forecasts available for this post. Colors identify DA forecast issue times; thin curves are ensemble members. The open-loop forecasts provide the comparison, and black dots show USGS observations available before the dotted cutoff. Dashed open-loop segments mark leads using National Water Model (NWM) fallback forcing, as indicated in each panel's legend. Times are in UTC and discharge is in cubic meters per second (m³/s).

### 1. Choctawhatchee River near Newton

<p align="center">
  <img src="/blog/pics/asaias/02361000_a.jpg" alt="Routed 18-hour DA and open-loop streamflow forecasts for USGS 02361000, Choctawhatchee River near Newton" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*[USGS 02361000](https://waterdata.usgs.gov/monitoring-location/USGS-02361000/) · Choctawhatchee River near Newton.*

### 2. Blackwater River near Bradley

<p align="center">
  <img src="/blog/pics/asaias/02369800_a.jpg" alt="Routed 18-hour DA and open-loop streamflow forecasts for USGS 02369800, Blackwater River near Bradley" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*[USGS 02369800](https://waterdata.usgs.gov/monitoring-location/USGS-02369800/) · Blackwater River near Bradley.*

### 3. Conecuh River at Brantley

<p align="center">
  <img src="/blog/pics/asaias/02371500_a.jpg" alt="Routed 18-hour DA and open-loop streamflow forecasts for USGS 02371500, Conecuh River at Brantley" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*[USGS 02371500](https://waterdata.usgs.gov/monitoring-location/USGS-02371500/) · Conecuh River at Brantley.*

### 4. Patsaliga Creek near Brantley

<p align="center">
  <img src="/blog/pics/asaias/02372250_a.jpg" alt="Routed 18-hour DA and open-loop streamflow forecasts for USGS 02372250, Patsaliga Creek near Brantley" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*[USGS 02372250](https://waterdata.usgs.gov/monitoring-location/USGS-02372250/) · Patsaliga Creek near Brantley.*

### 5. Murder Creek near Evergreen

<p align="center">
  <img src="/blog/pics/asaias/02374500_a.jpg" alt="Routed 18-hour DA and open-loop streamflow forecasts for USGS 02374500, Murder Creek near Evergreen" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*[USGS 02374500](https://waterdata.usgs.gov/monitoring-location/USGS-02374500/) · Murder Creek near Evergreen.*

### 6. Hatchet Creek below Rockford

<p align="center">
  <img src="/blog/pics/asaias/02408540_a.jpg" alt="Routed 18-hour DA and open-loop streamflow forecasts for USGS 02408540, Hatchet Creek below Rockford" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*[USGS 02408540](https://waterdata.usgs.gov/monitoring-location/USGS-02408540/) · Hatchet Creek below Rockford.*

### 7. Mulberry Creek at Jones

<p align="center">
  <img src="/blog/pics/asaias/02422500_a.jpg" alt="Routed 18-hour DA and open-loop streamflow forecasts for USGS 02422500, Mulberry Creek at Jones" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*[USGS 02422500](https://waterdata.usgs.gov/monitoring-location/USGS-02422500/) · Mulberry Creek at Jones.*

### 8. Sipsey Fork near Grayson

<p align="center">
  <img src="/blog/pics/asaias/02450250_a.jpg" alt="Routed 18-hour DA and open-loop streamflow forecasts for USGS 02450250, Sipsey Fork near Grayson" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*[USGS 02450250](https://waterdata.usgs.gov/monitoring-location/USGS-02450250/) · Sipsey Fork near Grayson.*

### 9. North River near Samantha

<p align="center">
  <img src="/blog/pics/asaias/02464000_a.jpg" alt="Routed 18-hour DA and open-loop streamflow forecasts for USGS 02464000, North River near Samantha" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*[USGS 02464000](https://waterdata.usgs.gov/monitoring-location/USGS-02464000/) · North River near Samantha.*

### 10. Elliotts Creek at Moundville

<p align="center">
  <img src="/blog/pics/asaias/02465493_a.jpg" alt="Routed 18-hour DA and open-loop streamflow forecasts for USGS 02465493, Elliotts Creek at Moundville" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*[USGS 02465493](https://waterdata.usgs.gov/monitoring-location/USGS-02465493/) · Elliotts Creek at Moundville.*

### 11. Satilpa Creek near Coffeeville

<p align="center">
  <img src="/blog/pics/asaias/02469800_a.jpg" alt="Routed 18-hour DA and open-loop streamflow forecasts for USGS 02469800, Satilpa Creek near Coffeeville" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*[USGS 02469800](https://waterdata.usgs.gov/monitoring-location/USGS-02469800/) · Satilpa Creek near Coffeeville.*

### 12. Paint Rock River near Woodville

<p align="center">
  <img src="/blog/pics/asaias/03574500_a.jpg" alt="Routed 18-hour DA and open-loop streamflow forecasts for USGS 03574500, Paint Rock River near Woodville" style="width: 100%; max-width: 1100px; height: auto;" />
</p>

*[USGS 03574500](https://waterdata.usgs.gov/monitoring-location/USGS-03574500/) · Paint Rock River near Woodville.*

