# GALAH_DR2
In this repository, we describe the most basic routines to use GALAH Data Release 2. The GALAH collaboration released its second Data Release April 2018. 

You can download the FITS file of GALAH DR2 from:

https://datacentral.org.au/teamdata/GALAH/public/

An example X-match with Gaia DR1 could be:

```ruby
SELECT *
FROM gaiadr1.gaia_source AS gaia
INNER JOIN gaiadr1.tmass_best_neighbour AS tmass_xmatch
	ON gaia.source_id = tmass_xmatch.source_id
INNER JOIN gaiadr1.tmass_original_valid AS tmass
	ON tmass.tmass_oid = tmass_xmatch.tmass_oid
INNER JOIN user_sbuder.galah_dr2 AS galah_dr2
	ON tmass.designation = galah_dr2.star_id
```

If you have downloaded the file e.g. as 'GALAH_DR2.fits', do the following:

```python
from astropy import table
t = table.Table()
galah_dr2 = t.read('GALAH_DR2.fits')
```

The FITS files contains the keywords in the table below

| Field | Units | Data type | Description | 
| ------| ------|-----------|-------------|
| star_id | | char[16] | 2MASS ID number by default, UCAC4 ID number if 2MASS unavailable (begins with UCAC4-) |
| star_id | | char[16] | 2MASS ID number by default, UCAC4 ID number if 2MASS unavailable (begins with UCAC4-) | 
sobject_id 	| | int64 | Unique per-observation star ID  | 
gaia_id | | int64 | \Gaia\ DR1 identifier | 
ndfclass | | char[8] | Observation type (MFOBJECT (regular observation) or MFFLX (benchmark observation)) | 
field_id | | int64 | GALAH field identification number | 
raj2000 | deg | float64 | Right ascension from 2MASS, J2000 | 
dej2000 | deg | float64 | Declination from 2MASS, J2000 | 
jmag | mag | float64 | J magnitude from 2MASS | 
hmag | mag | float64 | H magnitude from 2MASS | 
kmag | mag | float64 | K magnitude from 2MASS | 
vmag_jk | mag | float64 | Synthetic V magnitude calculated from JHK, used for target selection | 
e_jmag | mag | float64 | Uncertainty in J magnitude, from 2MASS | 
e_hmag | mag | float64 | Uncertainty in H magnitude, from 2MASS | 
e_kmag | mag | float64 | Uncertainty in K magnitude, from 2MASS | 
snr_c1 | | float64 | Signal to noise per pixel in the HERMES blue channel | 
snr_c2 | | float64 | Signal to noise per pixel in the HERMES green channel | 
snr_c3 | | float64 | Signal to noise per pixel in the HERMES red channel | 
snr_c4 | | float64 | Signal to noise per pixel in the HERMES IR channel | 
rv_synt | km/s | float64 | Radial velocity from cross-correlation against synthetic spectra | 
rv_obst | km/s | float64 | Radial velocity from internal cross-correlation against data | 
rv_nogr_obst | km/s | float64 | Radial velocity from internal cross-correlation against data, uncorrected for gravitational redshift | 
e_rv_synt | km/s | float64 | Uncertainty in rv_synt | 
e_rv_obst | km/s | float64 | Uncertainty in rv_obst | 
e_rv_nogr_obst | km/s | float64 | Uncertainty in rv_nogr_obst | 
chi2_cannon | | float64 | Summed chi-squared over all spectral pixels | 
sp_label_distance | | float64 | Label distance similar to \citet{Ho2017 | 
flag_cannon | | int64 | Flags for spectrum information in a bitmask format | 
| | | | 0=No flag |
| | | | +1 The Cannon starts to extrapolate. For some stars the values could be incorrect. | 
| | | | +2 The chi2 of the best fitting model spectrum is significantly higher or lower | 
| | | | +4 Reduction flag raised | 
| | | | +8 Binary star | 
| | | | +16 Negative flux | 
| | | | +32 Oscillating continuum | 
| | | | +64 General reduction issues | 
| | | | +128 Emission lines | 
teff | K | float64 | Effective temperature | 
e_teff | K | float64 | Uncertainty of teff | 
logg | dex | float64 | Surface gravity | 
e_logg | dex | float64 | Uncertainty of logg | 
fe_h | dex | float64 | Iron abundance (not overall metallicity [M/H]) | 
e_fe_h | dex | float64 | Uncertainty in fe_h | 
vmic | km/s | float64 | Microturbulent velocity | 
e_vmic | km/s | float64 | Uncertainty in vmic | 
vsini | km/s | float64 | Line of sight rotational velocity | 
e_vsini | km/s | float64 | Uncertainty in vsini | 
alpha_fe | dex | float64 | alpha-enhancement, determined as an error-weighted combination of Mg, Si, Ca, Ti abundances | 
e_alpha_fe | dex | float64 | Uncertainty in alpha_fe | 
x_fe | dex | float64 | [X/Fe] abundance for element X.  | 
e_x_fe | dex | float64 | Uncertainty in x_fe | 
flag_x_fe | | int64 | Flags indicating difficulty in abundance determination in a bitmask format | 
| | | | 0=No flag | 
| | | | +1 Line strength below 2-sigma upper limit |
| | | | +2 The Cannon starts to extrapolate. For some stars the values could be incorrect. | 
| | | | +4 The chi2 of the best fitting model spectrum is significantly higher or lower. | 
| | | | +8 flag_cannon is not 0 |
