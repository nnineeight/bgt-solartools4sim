# BGT Solar Tools

This repository contains Python/Jupyter code to:

- Compute solar position (zenith, altitude, azimuth).
- Convert pyranometer global horizontal irradiance (GHI) into direct normal irradiance (DNI) and diffuse horizontal irradiance (DHI) using the Erbs correlation.
- Generate solar direction vectors (ENU) suitable for CFD/ANSYS Icepak simulations of black globe thermometers and related setups.

## Contents

- `notebooks/` (planned): Jupyter notebooks with the full calculation workflow.
- `solar_sun_vectors.ipynb` (initial): main notebook for GHI → DNI/DHI + sun vector.

## Requirements

- Python 3.9+
- `zoneinfo` (standard library in Python 3.9+)
- Jupyter or VS Code for running the notebook.

## Author

- Nibir Kanti Roy
