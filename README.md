# Using cGANs to find strong gravitational lenses in Euclid

# Diving deeper into Strong Gravitational Lensing with cGANs 

[![DOI](https://zenodo.org/badge/690453944.svg)](https://zenodo.org/badge/latestdoi/690453944)

Supporting materials for Euclid strong lensing working group meeting Bologna February 2024.

This repository contains notebooks and additional files for getting filter response curves for JWST NIRcam imaging, simulating strong gravitational lenses for JWST and Euclid observation configurations and the source code for the cGAN. Each notebook gives a walkthrough and detailed explanation of how to use the code provided. 

## Data Preparation for the cGAN
The strong gravitational lenses dataset was simulated using [lenstronomy](https://lenstronomy.readthedocs.io/en/latest/). Note that only NIRcam F200W and F356W filters are supported by lenstronomy in the observation configuration file for JWST. To simulate strong gravitational lenses as observed by JWST NIRcam imaging, you will have to import:

> JWST_Config.py

To simulate the strong gravitational lenses as observed by both JWST NIRcam and Euclid-VIS, Euclid-NISP, you can follow:

> Lens_Dataset.ipynb

This notebook closely follows that of [LensFindery-McLensFinderFace](https://github.com/JoshWilde/LensFindery-McLensFinderFace/tree/main).
This returns the simulated lenses for all 6 JWST NIRcam filters, Euclid-VIS and Euclid NISP-JYH filters with a *.fits* extension of size 640x640 for JWST and 64x64 for Euclid. We then upscale Euclid to the pixel resolution of JWST. This is done due to the difference in pixel scales of the two telescopes. More information is given in the notebook.

## The Network
<img width="560" alt="image" src="https://github.com/RubyPC/Anomaly_Detection_with_cGANs/assets/106536925/cf6becbd-7dd4-4ae7-87d6-39ab19fa8e7a">

To use the cGAN, follow:

> cGANs_JWST.ipynb

The data fed to the cGAN is loaded from each individual waveband file.

## Results after Training
The cGAN predicts the strong gravitational lenses observed by the long wavelength filters of JWST NIRcam to a high accuracy. We also test whether the cGAN can predict long wavelength JWST NIRcam data from Euclid-VIS and Euclid-NISP data. Below shows examples of results produced by the network.
<img width="460" alt="Output3" src="https://github.com/RubyPC/cGAN_Strong_Lensing/assets/106536925/fd883707-37d4-4a32-b3d0-d1c578a3d9d9">


Predicting long wavelength JWST NIRcam data from Euclid-VIS or Euclid-NISP data or a mixture of both would be a beneficial application of the cGAN. The pixel resolutions between the two Euclid instruments and JWST NIRcam are different, so we must expect a different output from JWST short wavelength to long wavelength. It could be the case that predicting strong gravitational lenses as observed by JWST from Euclid observations is a method of anomaly detection- observations made by Euclid that are potential gravitational lenses could be proved as non-lenses as observed by JWST. Although this does not solve the problem of finding strong gravitational lenses, it is useful for improving the purity and recall of our strong lens finding methods.

Further results showing each individual filter prediction and Euclid to JWST predictions are given in the cGAN jupyter notebook.

## Some Useful Links
* [JWST User Documentation](https://jwst-docs.stsci.edu/)
* [JWST NIRcam Imaging Information](https://jwst-docs.stsci.edu/jwst-near-infrared-camera)
* [Euclid Red Book](https://arxiv.org/abs/1110.3193)
* [Euclid VIS and NISP Insruments](https://www.euclid-ec.org/public/mission/vis/)

### References
* [Lenstronomy](https://arxiv.org/abs/1803.09746)
* [Detecting Strong Gravitational Lenses with CNNs](https://arxiv.org/abs/2202.127760)
* [Image-to-Image Translation with conditionl Adversarial Networks](https://doi.org/10.48550/arXiv.1611.07004)
* [Generative Adversarial Networks](https://doi.org/10.48550/arXiv.1406.2661)
* [Using cGANs for Anomaly Detection in JWST Imaging](https://arxiv.org/abs/2310.09073)
