# Solar Tools for BGT Simulation in AEDT Icepak

This repository contains a Jupyter notebook that:

- Computes solar position (zenith, altitude, azimuth) from latitude, longitude, and timezone-aware timestamps.
- Converts pyranometer **Global Horizontal Irradiance (GHI)** reading into **Direct Normal Irradiance (DNI)** and **Diffuse Horizontal Irradiance (DHI)** using the **Erbs et al. (1982)** correlation.
- Generates **solar direction unit vectors** in a local East–North–Up (ENU) coordinate system, suitable for AEDT Icepak simulations for Black Globe Thermometers (BGT).

---

## Brief Methodology

### Solar geometry

For each timestamp, the notebook:

- Converts the **timezone-aware local time** to UTC.
- Computes **day of year**, **solar declination**, and **equation of time (EOT)** using standard approximations.
- Derives **local solar time** from UTC, longitude, and the EOT.
- Computes **hour angle**, then **solar zenith angle**, **solar altitude**, and **solar azimuth** using standard formulas.

These steps follow the treatments in Duffie & Beckman (2013) and Cooper (1969), with the overall approach consistent with Reda & Andreas’ (2004) solar position algorithm.

### Irradiance decomposition (GHI to DNI + DHI)

Given:

- Measured **GHI** from a horizontal pyranometer
- Computed **solar zenith angle** and **extraterrestrial irradiance**

The notebook then

- Computes a **clearness index** that compares measured GHI to theoretical extraterrestrial irradiance on a horizontal plane.
- Uses the **Erbs et al. (1982)** correlation to estimate the **diffuse fraction** of GHI.
- Recovers **DHI** and **DNI** from the standard relationship between GHI, solar beam, and diffuse components on a horizontal plane.

For times when the sun is at or below the horizon, the code:

- Sets **DNI = 0** (no direct beam).
- Treats **GHI as purely diffuse** i.e., DHI = GHI.
- Marks quantities like clearness index and diffuse fraction as `None` to indicate that the empirical decomposition model is only applied under daytime conditions.

### Solar direction vectors (ENU)

The notebook defines a local **East–North–Up (ENU)** coordinate system:

- +X = East
- +Y = North
- +Z = Up

Using the computed **solar altitude** and **azimuth**, it constructs a unit vector **from the site to the sun** (`sun_vec_enu`).

At night (sun below the horizon), the code returns a zero vector to signal that no meaningful solar direction exists for direct radiation.

---

## Key functions

- `day_of_year(d: datetime)` – day-of-year from a Python datetime.
- `solar_declination_deg(n)` – solar declination from day-of-year.
- `equation_of_time_min(n)` – equation of time in minutes.
- `solar_zenith_from_utc(lat_deg, lon_deg, when_utc)` – zenith angle from UTC, latitude, and longitude.
- `decompose_ghi_to_dni_dhi(ghi, theta_z_deg, n)` – Erbs-based decomposition of GHI into DNI and DHI.
- `solar_angles_and_vectors(lat_deg, lon_deg, when_local)` – zenith, altitude, azimuth, and ENU direction vectors for the sun and incoming rays.

---

## References

- Duffie, J. A., & Beckman, W. A. (2013). Solar Engineering of Thermal Processes (4th ed.). Wiley.

- Erbs, D. G., Klein, S. A., & Duffie, J. A. (1982). Estimation of the diffuse radiation fraction for hourly, daily and monthly-average global radiation. Solar Energy, 28(4), 293–302.

- Cooper, P. I. (1969). The absorption of radiation in solar stills. Solar Energy, 12(3), 333–346.

- Spencer, J. W. (1971). Fourier series representation of the position of the sun. Search, 2(5), 172–175.

- Reda, I., & Andreas, A. (2004). Solar position algorithm for solar radiation applications. Solar Energy, 76(5), 577–589.

---
