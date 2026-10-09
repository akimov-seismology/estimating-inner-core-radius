
# Estimating Earth's Inner-Core Radius

Research notebooks for estimating Earth's inner-core radius from PKIKP and PKiKP travel times.

This repository accompanies the article **"Trying to measure the inner core radius"** by Andrei Akimov. It explores how arrival-time selection, earthquake origin time, and assumptions about seismic velocities affect the estimated radius.

## Research question

PKIKP travels through the inner core, while PKiKP reflects from its boundary. Their travel times respond differently to changes in inner-core radius.

Can absolute arrival times and same-station phase-time differences constrain the radius consistently? How much radius information remains after removing earthquake origin time?

## Fiji earthquake case study

The notebook examines the November 12, 2022 Fiji M7.0 earthquake using ISC catalog observations.

| Parameter | Value |
| --- | --- |
| ISC event ID | 625210364 |
| Latitude | -20.1146° |
| Longitude | -178.2086° |
| Depth | 601.2 km |
| Origin time | 07:09:14.55 UTC |
| Trial inner-core radii | 1080-1300 km |
| Radius increment | 2 km |
| Additional reference radius | 1217.5 km |

The earthquake location, depth, and origin time remain fixed throughout the radius search.

## Method

The notebook:

1. Selects catalog reports with an absolute ISC residual below 1 second.
2. Selects one report per station and phase, excluding station coordinate conflicts.
3. Builds modified ak135 models using continuous remapping of core depths.
4. Calculates travel times with TauP and applies ellipticity corrections.
5. Finds the radius with the lowest root mean square (RMS) residual for:
   - PKIKP absolute travel times.
   - PKiKP absolute travel times.
   - Both phases combined.
   - Same-station PKiKP-PKIKP time differences.
6. Calculates travel-time derivatives with respect to radius and plots their variation among stations.

Each fit uses a fixed set of observations with predictions available at every trial radius.

## Notebook

**`akimov_Fiji_continuous_radius.ipynb`**

The notebook contains the catalog observations, calculations, research notes, and plotting code. No separate input-data files are required.

## Installation

Install the required packages in your Python environment:

```bash
python -m pip install numpy matplotlib obspy ellipticipy pyproj jupyterlab
```

Launch JupyterLab:

```bash
jupyter lab
```

Open the notebook and run its cells from top to bottom.

## Running the analysis

The notebook can run from an otherwise empty, writable folder. It reads the reference ak135 model from the installed ObsPy package and creates the modified models automatically.

Results are written beneath the kernel's current working directory in:

```text
Fiji_2022_continuous_core_radius_numpy_v1/
```

The first run can take substantial time. Predictions are saved after each completed radius. Rerunning with the same inputs and settings reuses cached predictions.

## Outputs

The notebook produces:

- Best-fitting radii and minimum RMS values in `radius_summary.csv`.
- RMS profiles across the radius grid.
- Station-level predictions and residuals.
- Same-station differential residuals.
- Travel-time derivatives in seconds per kilometer.
- Plots of RMS curves, reference residuals, derivative curves, and sensitivity heatmaps.
- Audit files documenting filtering and unsupported observations.

## Results reported in the article

The completed October 6, 2026 run of the earlier continuous-model notebook produced:

| Fit | Observations | Best radius (km) | Minimum RMS (s) |
| --- | ---: | ---: | ---: |
| PKIKP absolute | 392 | 1214 | 0.525 |
| PKiKP absolute | 148 | 1214 | 0.563 |
| Combined absolute | 540 | 1214 | 0.535 |
| Same-station phase differences | 64 pairs | 1226 | 0.560 |

These values provide a comparison for reproducing the analysis. They are not hardcoded results. The current rewrite has been checked against the earlier selection and numerical calculations, but a complete TauP rerun has not yet been performed.

## Interpretation and limitations

These are **model-dependent radius estimates**, not a precise, independent measurement of Earth's inner-core radius.

- Errors in earthquake origin time can resemble a radius-dependent shift in absolute travel times.
- Same-station phase differences cancel origin time but retain sensitivity to location, depth, phase identification, and velocity-model errors.
- Filtering by catalog residual selects observations that already agree with the catalog calculation. It does not independently establish pick quality.
- Continuous core-depth remapping changes velocity gradients as well as the boundary radius.
- Similar wave paths can provide similar information, even when many stations are available.
- A 2 km search increment does not imply 2 km accuracy.
- This single-event analysis does not establish temporal changes in the inner core.

## Data and references

**Data:** ISC Bulletin, event 625210364, downloaded October 5, 2026. See [ISC citation guidance](https://www.isc.ac.uk/citations/).

- Engdahl, Flinn, and Massé (1974). *Differential PKiKP Travel Times and the Radius of the Inner Core*. [DOI](https://doi.org/10.1111/j.1365-246X.1974.tb05467.x)
- Ivan, Wang, and Hofstetter (2018). *Non quasi-Hemispherical Seismological Pattern of the Earth's Uppermost Inner Core*. [DOI](https://doi.org/10.1038/s41598-018-20657-x)
- Kennett, Engdahl, and Buland (1995). *Constraints on seismic velocities in the Earth from traveltimes*. [DOI](https://doi.org/10.1111/j.1365-246X.1995.tb03540.x)
- Crotwell, Owens, and Ritsema (1999). *The TauP Toolkit: Flexible Seismic Travel-Time and Raypath Utilities*. [DOI](https://doi.org/10.1785/gssrl.70.2.154)
- Russell, Rudge, Irving, and Cottaar (2022). *A re-examination of ellipticity corrections for seismic phases*. [DOI](https://doi.org/10.1093/gji/ggac315)

## Author

**Andrei Akimov**  
Independent researcher in earthquake seismology
