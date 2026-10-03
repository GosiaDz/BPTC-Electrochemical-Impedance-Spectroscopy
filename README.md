# BPT–CuO measurement dataset

Ba0.95Pb0.05TiO3 (BPT) ceramics with nominal CuO additions of 0, 0.05, 0.1, 1 and 3 wt.% relative to the BPT host mass. This package supplies numeric measurement exports and original microscopy files. There are no embedded figures, model fits or processed conductivity curves in the Excel workbooks.

## Sample identities

| Sample ID | CuO added per 100 g BPT (g) | SEM/EDS group | Selected point EDS source |
|---|---:|---:|---|
| BPT | 0 | 1 | 1 008asr |
| BPT-Cu005 | 0.05 | 3 | 3 002asr |
| BPT-Cu010 | 0.1 | 2 | 2 002asr |
| BPT-Cu100 | 1 | 5 | 5 010asr |
| BPT-Cu300 | 3 | 4 | 4 002asr |

The number after Cu codes the nominal CuO addition, not measured elemental Cu concentration. sample_key.csv preserves the legacy composition codes. measurement_index.csv maps every exported acquisition or image to its original filename and repository location. The five EDS spectra are one selected local point per composition; they are not statistical mean compositions.

## Folder structure

- EDS/BPT_series_EDS_spectra.xlsx: five sheets, one sample each; channel_index (zero-based), energy_keV and counts. All 4020 declared channels are retained.
- EDS/SPC/: five original, byte-preserved EDAX binary spectra, renamed by sample ID. SPC_metadata.json records acquisition information and decoding checks.
- EDS/BPT_series_EDS_quantification.xlsx: the original instrument's element-level quantification for the five selected points. wt_percent and at_percent retain the original element-normalized exports, including oxygen and any Al/Si. They are not renormalized to heavy elements. No absent Cu value is filled with zero. Original CSV files are retained in original_quantification/. intensity_error_percent is the exported relative signal-intensity error, not concentration standard uncertainty. One CSV additionally preserves these selected rows in a machine-readable flat table.
- SEM/: five sample folders, 42 original TIFF images. Only filenames change; image bytes, scale bars and laboratory annotations are preserved. The original names are in measurement_index.csv. These images belong to the 28 April 2022 session.
- EIS/: five sample folders; one workbook for each sample/run/gas stage. Each available temperature is a separate sheet. Total: 26 workbooks and 166 acquisitions.
- XRD/: five numerical profile workbooks. The values are copied without normalization or offsets from the existing recovered numeric export XRD_RECOVERED_PROFILES.csv. These are the available numerical profiles used in the manuscript.

## EIS conventions

Columns are freq_Hz, |Z|_ohm, phi_deg, Z'_ohm and Z''_ohm, in that order. Frequency, real impedance and imaginary impedance retain the supplied measurements. The modulus and phase are derived columns calculated by Excel formulas: |Z| = sqrt((Z')^2 + (Z'')^2); phi = atan2(Z'', Z') × 180/pi, in degrees. The phase formula uses ATAN(Z''/Z') with explicit quadrant correction, equivalent to atan2(Z'',Z'); this also handles zero real components. A zero complex impedance has undefined phase. Phase is the signed argument of Z = Z' + iZ'', without phase unwrapping; it is not its negative. Calculated values are cached for numerical import and update when the components are edited. The recorded sign of the imaginary component is retained. Nyquist plotting may use −Z''_ohm; the stored observations are not sign-reversed. The full stored frequency extent, generally 0.1 Hz–10 MHz, is retained. The manuscript analysis uses 0.1 Hz–1 MHz. No channels or interference points have been deleted from these exported measurement tables.

S1: initial air; S2: 3000 ppm NH3 in Ar; S3: air after NH3; S4: 10% H2 in Ar; S5: air after H2. Stages S1, S3 and S5 remain distinct. Pt is the electrode for this data set.

A and B denote successive ascending-temperature runs, with cooling back to the initial setpoint between runs. B is available for seven S1 air acquisitions of BPT-Cu010; no C run is supplied. Rounded kelvin temperatures name sheets (e.g. 473K). The index retains original Celsius setpoints and exact kelvin conversion (473.15 K for 200 °C). Missing nominal condition: BPT-Cu010, A, 723K, S3. No interpolated substitute is included.

## EDAX SPC decoding

Format version 0.70 is decoded as little-endian. Spectrum data begin at the header's dataStart byte offset, and numPts supplies the declared number of channels. Recorded counts are 32-bit integers. Energy follows startEnergy + channel_index × evPerChan/1000 (keV). The current records have 10 eV/channel, zero offset, 4020 declared points, 18 kV and 20 s live time. The last declared channel is 40.19 keV; endEnergy=40.0 in the acquisition header is retained separately and is not used to stretch or resample the energy axis. The remaining reserved part of the fixed-length 4096-channel buffer is not appended as additional measured channels.

Decoding was checked independently against the recorded total spectral count, maximum count and maximum-peak channel. No smoothing, background subtraction, normalization or peak fitting was introduced. All spectrum smoothing flags in the selected source headers are zero. Reference peak identifications from the original header are retained as metadata and do not imply that each line is reliably quantified in each sample.

Format source: EDAX, *Spectrum File Format Vers 0.70*, 10 February 2015, hosted with the RosettaSciIO format documentation:
https://hyperspy.org/rosettasciio/_downloads/9e2f0ccf5287bb2d17f1b7550e1d626f/SPECTRUM-V70.pdf

## Reuse and provenance

Excel source columns retain the supplied observations; EIS modulus and phase are explicitly derived from the source components; display formatting does not alter stored precision. EDS quantitative outputs are local semi-quantitative estimates. Differences from nominal composition may reflect local compositional heterogeneity and measurement effects; a representative statistical composition requires spatially distributed point measurements and/or mapping.

The SEM mapping identifies compositions; differences between old and new sessions do not imply that images are duplicates. Historical instrument filenames are retained in the index, including any inconsistent original internal acquisition labels. File checksums permit verification of the archived bytes.
