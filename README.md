# Numerical phantom to evaluate tractography algorithms

This dataset is based on the ISBI2013 phantom; however, due to known issues with the original simulator's diffusion-weighted magnetic resonance imaging (DW-MRI) signal generation, the DW-MRI data cannot be used to quantify tractography. Hence, **this phantom is based only on the actual geometry of the bundles** of the original ISBI2013 phantom, and does not include any DW-MRI data. Here is a description of the files/folders:

- `streamlines.tck`: the streamlines in this tractogram were created directly from the geometrical description of the 27 bundles, i.e., not using any tracking algorithm.

- `density.nii.gz`: this is the (normalized) streamline density map of the ground-truth tractogram, which is assumed to be proportional to the actual axonal density of each bundle.

- `brain_mask.nii.gz`: the binary mask of the phantom.

- `wm.nii.gz` and `gm.nii.gz`: the binary mask of white matter and the label mask of the 53 gray-matter regions, respectively.

- `TOD`: folder containing the Track Orientation Density (TOD) images from the ground-truth streamlines at different angular resolutions, i.e. `lmax`, along with the corresponding peaks.

- `connectome_fbc.csv` and `connectome_nos.csv`: the ground-truth connection strength of each bundle expressed by the Fiber Bundle Capacity (FBC) and Number Of Streamlines (NOS) metrics.

