# Using cGANs to find Strong Gravitational Lenses in Euclid

[![DOI](https://zenodo.org/badge/690453944.svg)](https://zenodo.org/badge/latestdoi/690453944)

This repository aims at addressing the problem of strong gravitational lens finding in the era of large wide-field surveys such as Euclid. cGANs (or *conditional Generative Adversarial Networks*) have proven successful in image to image translation tasks[[1]](https://doi.org/10.48550/arXiv.1611.07004) but also at anomaly detection[[2]](https://arxiv.org/abs/2310.09073). In this repository, it is shown that a cGAN- when trained on Euclid data of non-lenses- can find lensing systems in Euclid via anomaly detection, giving an alternative method of lens finding.

This repository contains notebooks and additional files for data preparation and the source code for the cGAN. This repository uses data from the Euclid Early Release Observations *(ERO)* of the Perseus Cluster which is now publicly available. What is covered in this repository is given via detailed Python notebooks as follows:
1. A reprojection of Euclid VIS-band data onto a NISP-band world coordinate system.
2. A tutorial of how to simulate strong gravitational lenses and create a dataset.
3. A tutorial of how to load the data from the Perseus cluster and detect and extract sources in each filter. Additionally, how to take the individual sources and paint a simulated gravitational lens in.
4. The source code for the cGAN and a detailed tutorial of how to use it.

Also included in this repository, are files that are essential for using the above notebooks. More detail can be found below.

## Data Preparation for the cGAN
The VIS-band image of the Perseus Cluster is much greater than that for the NISP-bands due to its different angular resolution. To extract sources from the data with coordinates that correspond to the same point in all 4 filters, we reproject the VIS-band data onto the same image plane as the NISP-band data. This is made simple using Astropy's [reproject](https://reproject.readthedocs.io/en/stable/) package. Installation information is given on the webpage. To do this reprojection, you will have to follow this short example:

> VIS_to_NISP_reproject.ipynb

After reprojection, the filters have the same dimensions and share a local world coordinate system. However, the files are very large and are too big to do source extraction with. So, each filter is likewise split into "crops" which each represent 1/100th of the entire Perseus data. This makes it possible to perform source detection and extraction from which the centre positions of each detected object is saved to a *.csv* file and a $100\times100$ pixel cutout is made of each object and saved. This corresponds to $20\times20$ arcseconds per pixel for each detected object. This is given in more detail in the following:

> Lens_Dataset.ipynb

Though, this includes the extraction for one crop only. For the entire training and test data set for the cGAN, this notebook extracted crops of the entire Perseus data. 

In the remainder of the notebook, simulated strong gravitational lenses are painted into a subsection of the extracted sources to form part of the test set for the cGAN. It is in only 10% of the test set that the lenses are present. To simulate the strong gravitational lenses for Euclid-VIS, Euclid-NISP, you can follow:

> Lens_Simulation.ipynb

This notebook closely follows that of [LensFindery-McLensFinderFace](https://github.com/JoshWilde/LensFindery-McLensFinderFace/tree/main).
This returns the simulated lenses for Euclid-VIS and Euclid NISP filters with a *.fits* extension of size 64x64 for Euclid. More information is given in the notebook.

## The Network
<img width="560" alt="image" src="https://github.com/RubyPC/Anomaly_Detection_with_cGANs/assets/106536925/cf6becbd-7dd4-4ae7-87d6-39ab19fa8e7a">

To use the cGAN, follow:

> cGANs_Euclid_ERO.ipynb

The data fed to the cGAN is loaded from each individual waveband file. The cGAN takes as input 3 Euclid filters (VIS, NISP-Y and NISP-J) for each extracted object from the Perseus cluster. The goal is for the cGAN to predict the NISP-H data for each object. 

The entire dataset of cutouts from the Perseus cluster includes 50,000 objects which is split into 40,000 for training, 5,000 for validation and 5,000 for testing. The training set do *not* contain the simulated gravitational lenses such that the network learns the morphologies present in cutouts from the Perseus cluster and rare systems and configurations such as lenses remain unknown to the network. The test set is composed of 10% simulated gravitational lenses (500 lenses) and the goal is for the cGAN to find these lenses by poorly predicting their NISP-H data --> by detecting them as anomalies.

## Results after Training
The cGAN predicts the NISP-H band for the cutouts from the Perseus cluster containing non-lenses very well, with very minimal residuals between the ground truth NISP-H band data and the predictions by the cGAN. Contrastingly, the predictions on the test set containing the simulated lenses are predicted poorly with greater residuals between the true data and the prediction with the prediction images being unclear and noisy and missing a lot of luminosity of the arcs and rings. Predictions on non-lenses (of which there are 4500 in the test set) during test time are predicted very well. See below for example results (rows 1-4 showing lenses, row 5 showing non-lens in the test set).
<img width="625" alt="image" src="https://github.com/RubyPC/cGAN_Lens_Finding/assets/106536925/b69c9f5a-28c7-441e-a941-178b0673f788">

Interestingly, there are few lenses in the test set (~15) which are well predicted. These were inspected and were all multiple image systems that would have been difficult to detect by eye without prominent arcs and rings. 

For more results, see the notebook above.

## Some Useful Links
* [Euclid Red Book](https://arxiv.org/abs/1110.3193)
* [Euclid VIS and NISP Insruments](https://www.euclid-ec.org/public/mission/vis/)

### References
* [Lenstronomy](https://arxiv.org/abs/1803.09746)
* [Detecting Strong Gravitational Lenses with CNNs](https://arxiv.org/abs/2202.127760)
* [Image-to-Image Translation with conditionl Adversarial Networks](https://doi.org/10.48550/arXiv.1611.07004)
* [Generative Adversarial Networks](https://doi.org/10.48550/arXiv.1406.2661)
* [Using cGANs for Anomaly Detection in JWST Imaging](https://arxiv.org/abs/2310.09073)
