# GD-1 Stellar Stream Selection — Midterm Report

## Objective
Recreate Figure 3 from [Malhan et al. 2024 (arXiv:2405.19410)](https://arxiv.org/pdf/2405.19410), which isolates the GD-1 stellar stream from Gaia field-star data using a combination of photometric (isochrone-based) and kinematic (proper motion) selection cuts.

## Steps

1. **Query the isochrone.** Used the CMD tool with photometric system set to Gaia DR2, fixed age (12 Gyr) and metallicity, default settings otherwise. Got output of a table of stars at varying masses with `Gmag`, `G_BPmag`, `G_RPmag`.

2. **Applied the distance modulus.** Assumed d = 8500 pc:
    ```python
       d = 8500  # parsecs
       distance_modulus = 5 * math.log10(d) - 5
       iso['G_app'] = iso['Gmag'] + distance_modulus
    ```
   
3. **Compute colors.** `color = BP_mag − RP_mag` for both the isochrone and the raw Gaia data.

4. **Plot the raw color-magnitude diagram (CMD).** 2D histogram of the raw data in color-magnitude space, overlaid with the shifted isochrone track, to visually confirm where GD-1 members should fall relative to the model.

5. **Photometric selection.** Defined a polygon around the isochrone in CMD space and used `matplotlib.path.Path.contains_points` to select stars near the expected stellar track.

6. **Kinematic (proper motion) selection.** Defined a second polygon in `pmra`–`pmdec` space targeting GD-1's known proper-motion signature, applying the same point-in-polygon masking.

7. **Combined visualization.** Built a 3×3 grid comparing all data, photometrically-selected data, and photometrically + kinematically selected data — each in CMD, proper-motion, and sky-position (RA/Dec) space — showing how each cut progressively isolates the GD-1 stream from the background.
