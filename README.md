# Numerical phantom to evaluate tractography algorithms

This dataset is based on the ISBI2013 phantom; however, due to known issues with the diffusion-weighted magnetic resonance iamging (DW-MRI) signal generation of the original simulator, the DW-MRI data cannot be used to quantify tractography. Hence, **this phantom is based only on the actual geometry of the bundles** of the original ISBI2013 phantom, and does not include any DW-MRI data. Here is a description of the files.

- `ground_truth_streamlines.tck`: the streamlines in this tractogram were created directly from the geometrical description of the 27 bundles, i.e., not using any tracking algorithm.

- `ground_truth_density.nii.gz`: this is the (normnalized) streamline density map of the ground-truth tractgram, which is assumed to be proportional to the actual axonal density of each bundle. The file `ground_truth_density.scheme` is needed to fit any other tractogram to the this density map using COMMIT.

- `brain_mask.nii.gz`: the binary mask of the phantom.

- `wm.nii.gz` and `gm.nii.gz`: the binary mask of white matter and the label mask of the 53 gray-matter regions, respectively.

- `ground_truth_fODF.nii.gz`: the ground-truth fiber Orientation Distribution Functions (fODF).

- `ground_truth_connectome_fbc.csv` and `ground_truth_connectome_nos.csv`: the ground-truth connection strength of each bundle expressed by the Fiber Bundle Capacity (FBC) and Number Of Streamlines (NOS) metrics.

## Notes

To ground-truth fODF were estimated by first computing the Track Orientation Density Imaging (TODI) of the ground-truth streamlines with the command:
```bash
tckmap ground_truth_streamlines.tck ground_truth_fODF.nii.gz -template ground_truth_density.nii.gz -tod 8 -precise
```
and then normalizing them to obtain probability density functions, i.e., summing up to 1, using this code:
```python
import numpy
import nibabel

nii = nibabel.load( 'ground_truth_fODF.nii.gz' )
nii_img = nii.get_fdata()

nii0_img = nii_img[:,:,:,0] * numpy.sqrt(4.0*numpy.pi)
idx = nii0_img<1e-6
nii0_img[ idx ] = 1.0
nii0_img = 1.0 / nii0_img
nii0_img[ idx ] = 0.0

niinorm_img = nii_img.copy()
for i in range(nii_img.shape[3]):
    niinorm_img[:,:,:,i] *= nii0_img
nibabel.Nifti1Image( niinorm_img, nii.affine ).to_filename( 'ground_truth_fODF.nii.gz' )
```
