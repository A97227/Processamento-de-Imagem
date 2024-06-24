# Automatic Segmentation of Retinal Vessels in Fundus Images

## Retinianos em Imagens de Fundo de Olho

## Students: Luís Gonçalves PG54011; Sandra Ferreira PG54221
## Supervisor: Carlos Silva

## Abstract
This project developed an adapted method for the segmentation of retinal vessels using only the fundus image to be segmented. 
Based on the method described by S. Chaudhuri et al. in [1], the edge treatment was adapted for each image.

## I. Introduction

Autonomous segmentation of retinal vessels allows for the analysis and detection of variations, aiding in diagnosing retinal pathologies. 
The project uses Python and libraries like numpy, matplotlib, and scipy. The method was tested on the DRIVE dataset.

## Methods
An algorithm was developed to segment and detect retinal vessels, enhancing image quality and aiding in diagnosing conditions like diabetic retinopathy and glaucoma. The method relies on matched filters for bidirectional filtering to handle vessel orientations.

## Algorithm Implementation
####  Gaussian Function: Implements the matched filter in 2D.
#### Filter Application: Applies the filter and selects the highest value for each pixel.
#### Border Removal: Created masks to eliminate image borders.
#### Binarization: Applied Otsu's method to highlight blood vessels.
#### Evaluation: Calculated Accuracy, Sensitivity, Specificity, and PPV to assess performance.
#### Results
#### Initial metrics:

Accuracy: 62.1% ± 1.30%
Sensitivity: 32.3% ± 15.3%
Specificity: 98.6% ± 0.4%
PPV: 71.8% ± 18.1%


#### Improved metrics after adaptive border creation:

Accuracy: 62.2% ± 0.9%
Sensitivity: 47.5% ± 7.8%
Specificity: 98.8% ± 1.1%
PPV: 86.0% ± 7.6%
Conclusion
The project successfully developed an autonomous system for segmenting retinal vessels from images. Future work includes removing undesired areas and enhancing thin vessel detection.

## VI. Bibliografia
[1] S. Chaudhuri, S. Chatterjee, N. Katz, M. Nelson and M. Goldbaum, "Detection of 
blood vessels in retinal images using two-dimensional matched filters," in IEEE 
Transactions on Medical Imaging, vol. 8, no. 3, pp. 263-269, Sep 1989
[2] Carlos Silva, Guia Do Projeto – “Segmentação Automática de Vasos Retinianos em 
Imagens de Fundo de Olho”
[3] Microsoft PowerPoint - chp 3 lecture notes – Acessado em : 
https://ee.eng.usm.my/eeacad/mandeep/EEE436/chp%203.pdf 01/02/2024
