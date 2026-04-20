# Numerical phantom to evaluate tractography algorithms

This dataset is based on the ISBI2013 phantom; however, due to known issues with the original simulator's diffusion-weighted magnetic resonance imaging (DW-MRI) signal generation, the DW-MRI data cannot be used to quantify tractography. Hence, **this phantom is based only on the actual geometry of the bundles** of the original ISBI2013 phantom. Here is a description of the files/folders:

- `geometry.json`: the geometry of the phantom. Due to issues with the CSF spheres included in the old phantom, this version only simulates white and gray matter, without any CSF regions.

- `streamlines.tck`: the streamlines in this tractogram were created directly from the geometrical description of the 27 bundles, i.e., not using any tracking algorithm.

- `density.nii.gz`: this is the (normalized) streamline density map of the ground-truth tractogram, which is assumed to be proportional to the actual axonal density of each bundle.

- `brain_mask.nii.gz`: the binary mask of the phantom.

- `wm.nii.gz` and `gm.nii.gz`: the binary mask of white matter and the label mask of the 53 gray-matter regions, respectively.

- `connectome_fbc.csv` and `connectome_nos.csv`: the ground-truth connection strength of each bundle expressed by the Fiber Bundle Capacity (FBC) and Number Of Streamlines (NOS) metrics.

- `fodf`: folder containing the fiber Orientation Distribution Functions (fODF), estimated from the ground-truth streamlines with Track Orientation Density imaging at different angular resolutions, i.e. `lmax`, along with the corresponding peaks.

- `dwi`: folder containing the DW-MRI data simulated with [Phantomas](https://github.com/ecaruyer/phantomas); this data is mainly intended for performing local reconstruction, e.g. CSD, and tracking. The signal was simulated at four different SNR levels, i.e. {10, 20, 30, 100), as follows:

```bash
phantomas_dwis -b dwi/bvals.txt -r dwi/bvecs.txt geometry.json --res 1.0 --fov 50.0 --snr 10.0 --output dwi/snr=10.nii.gz
phantomas_dwis -b dwi/bvals.txt -r dwi/bvecs.txt geometry.json --res 1.0 --fov 50.0 --snr 20.0 --output dwi/snr=20.nii.gz
phantomas_dwis -b dwi/bvals.txt -r dwi/bvecs.txt geometry.json --res 1.0 --fov 50.0 --snr 30.0 --output dwi/snr=30.nii.gz
phantomas_dwis -b dwi/bvals.txt -r dwi/bvecs.txt geometry.json --res 1.0 --fov 50.0 --snr 100.0 --output dwi/snr=100.nii.gz

mrtransform dwi/snr=10.nii.gz -identity dwi/snr=10.nii.gz -force
mrtransform dwi/snr=20.nii.gz -identity dwi/snr=20.nii.gz -force
mrtransform dwi/snr=30.nii.gz -identity dwi/snr=30.nii.gz -force
mrtransform dwi/snr=100.nii.gz -identity dwi/snr=100.nii.gz -force
```
