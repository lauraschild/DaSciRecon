# Improving pollen-based tree cover reconstructions  
![graphic_abstract](/DaSciRecon/images/schema.png)


## Thanks for your interest!

Thanks for scanning the QR-code and wanting to find out more about my current project!   
Here you can find: 
- [Selected Methods](#methods)
- [Correction Coefficients](#correction-coefficients)
- [Data sources](#data-sources)
- [References](#references)
- a pdf of my [poster](#poster-download)
- more [about me](#about-me)

I'll keep this page up for the duration of the PalMod GA, but feel free to [contact me](#about-me) afterwards if you'd like to know more.  
  
## Methods

### Extracting remotely sensed tree cover
The tree cover was extracted for the pollen source area (PSA) of each sampling site. The PSA was calculated using REVEALS and was defined as the area in which 80% of the deposited pollen originated from. REVEALS uses wind speed and pollen dispersal to asses this.  
Mean tree cover from the remote sensing data set was extracted for these areas.

### Reconstructing tree cover
Tree cover was reconstructed from the original pollen records, the REVEALS model output, and the optimized simplified model. Records younger than 450 years before present were considered for this. The large temporal range is due to an assumed age uncertainty in records as well as noise in the signal.  
Tree cover was calculated by using the fraction of tree taxa abundance over total taxa abundance. However, this assumes that the entire modern pollen source area to be vegetated, which is rarely the case. Therefore, a correction for the openness of the pollen source area was applied. This openness correction was derived from the CCI land cover product by using the fraction of open cells in the pollen source area as a measure for its openness.  
![openness_correction](/DaSciRecon/images/open.png)

### Optimizing the Reconstruction
The reconstruction was optimized using a simplified model for converting pollen abundances into vegetation abundances and by optimizing the taxon-specific correction coefficients. The model is based on an R-value model. It assumes that the pollen flux density produced by a specific taxon can be derived with a taxon-specific pollen parameter (_&alpha;_). When these parameters are known for all taxa, historic vegeation (*v*) can be reconstructed from historic pollen records.  
![model-equations](/DaSciRecon/images/r-value.png)  
The optimizations aim was, therefore, to estimate these correction coefficients by trying to fit the reconstructed tree cover to the remotely sensed tree cover. Bounds for these coefficients were imposed on the optimization to avoid overfitting and to supply ecological information. For that purpose the REVEALS model outputs were used as a starting point.  
Even though REVEALS takes into account both the taxon-specific pollen productivity and fall speed, its relationship to the original pollen record can be broken down to a simple coefficient as well.  

![reveals_equat](https://latex.codecogs.com/svg.image?REVEALS&space;estimate_i&space;=&space;x_i&space;&space;*&space;Pollen&space;count_i)  

However, this coefficient, x, will not be the same between different samples, as it also depends on the abundance of the remaining taxa in the assemblage. This essentially means that the coefficient has to be normalized before it is comparable and can be used in the optimization.  
The easiest way of normalizing these coeffcients is by dividing them by the coeffcient of one taxa in each sample. This way the values all become relative to one taxon. An obvious taxon to use for this is Poaceae (grasses) as they are ubiquitous in records and already used to normalized pollen productivity estimates. The normalization was done using the following equation.  

![reveals_equation2](https://latex.codecogs.com/svg.image?x_i&space;=\frac{REVEALSestimate_i&space;*&space;Pollencount_{Poaceae}}{Pollencount_i*&space;REVEALSestimate_{Poaceae}})..


The normalized coefficients are very similar between samples (as they should). To now allow room for optimization, a buffer was added to the range of the coefficient values. This gives an upper and a lower bound for the coefficient of each taxon to be optimized. The correction coefficients for the remaining taxa were set to the median normalized coefficient extracted from REVEALS. For the results presented on the poster the coefficients for the ten most common taxa were optimized for each continent.  
The R-packages `optimParallel` was used to to run a parallelized optimization using the L-BFGS (limited-memory Broyde-Fletcher-Goldfarb-Shanno) algorithm. 

### Validating the new model
The new model was validated using spatial leave-one-out (SLOO) cross-validation. In a traditional LOO cross-validation one site is left out of the model calibration and used for testing subsequently. Ideally, this is repeated with the each site and the mean error can be calculated. This allows the use of the entire data set for model calibration, while still having a realistic error estimate.  
As ecological data is often spatially autocorrelated, the method has to be adjusted to account for that. This is why SLOO was used. Instead of only excluding one site, the sites in a certain proximity are also excluded from the model calibration. The testing is still only done on the original pixel.  
The buffer distance to be used can be estimated using variograms, which show the variance between sites as a function of distance. The range of fitted variograms was used to determine an appropriate buffer for each continental optimization.
![SLOO](/DaSciRecon/images/sloo.png)

## Correction coefficients
### Europe
| Taxon      | alpha  |
| ---------- | ------ |
| Pinus      | 10.628 |
| Betula     | 5.28   |
| Poaceae    | 1      |
| Alnus      | 9.039  |
| Corylus    | 1.13   |
| Quercus    | 2.916  |
| Cyperaceae | 0.556  |
| Fagus      | 1.691  |
| Picea      | 1.121  |
| Ericales   | 0.46   |
### Asia
| Taxon         | alpha  |
| ------------- | ------ |
| Artemisia     | 12.133 |
| Pinus         | 15.031 |
| Betula        | 12     |
| Cyperaceae    | 3.198  |
| Poaceae       | 1      |
| Amaranthaceae | 17.299 |
| Alnus         | 7.295  |
| Quercus       | 2.276  |
| Picea         | 8.526  |
| Asteraceae    | 3.077  |
### North America
| Taxon      | alpha |
| ---------- | ----- |
| Pinus      | 13.04 |
| Betula     | 4.096 |
| Quercus    | 1.802 |
| Picea      | 1.652 |
| Alnus      | 2.73  |
| Cyperaceae | 0.937 |
| Poaceae    | 1     |
| Artemisia  | 1.275 |
| Asteraceae | 0.563 |
| Tsuga      | 0.913 |
 
## Data Sources

| Data        | Dataset Name    | Source  |
| ------------- |-------------| -----|
| **Pollen**    | LegacyPollen 1.0 | Herzschuh et al. 2022|
| **REVEALS parameters**| Compilation of relative pollen productivity|Wiezcorek & Herzschuh 2020 |
| **Remote sensing tree cover** | Global 30m Landsat Tree Canopy Version 4       |    Sexton et al. 2013 |  
| **Remote sensing land cover**|CCI Land Cover (300m) | ESA 2017 |  

Peter Ewald applied the REVEALS model using the pollen dataset and the parameter compilation.
 
## References

- ESA, 2017. Land Cover CCI Product User Guide Version 2.  
- Fagerlind, F., 1952. The real signification of pollen diagrams. Bot. Not. 40.  
- Herzschuh, U., Li, C., B??hmer, T., Postl, A.K., Heim, B., Andreev, A.A., Cao, X., Wieczorek, M., Ni, J., 2022. LegacyPollen 1.0: A taxonomically harmonized global Late Quaternary pollen dataset of 2831 records with standardized chronologies. Earth Syst. Sci. Data Discuss. 1???25. https://doi.org/10.5194/essd-2022-37  
- Parsons, R.W., Prentice, I.C., 1981. Statistical approaches to R-values and the pollen??? vegetation relationship. Rev. Palaeobot. Palynol. 32, 127???152. https://doi.org/10.1016/0034-6667(81)90001-4  
- Prentice, I.C., Webb III, T., 1986. Pollen percentages, tree abundances and the Fagerlind effect. J. Quat. Sci. 1, 35???43. https://doi.org/10.1002/jqs.3390010105  
- Roberts, D.R., Bahn, V., Ciuti, S., Boyce, M.S., Elith, J., Guillera-Arroita, G., Hauenstein, S., Lahoz-Monfort, J.J., Schr??der, B., Thuiller, W., Warton, D.I., Wintle, B.A., Hartig, F., Dormann, C.F., 2017. Cross-validation strategies for data with temporal, spatial, hierarchical, or phylogenetic structure. Ecography 40, 913???929. https://doi.org/10.1111/ecog.02881  
- Sexton, J.O., Song, X.-P., Feng, M., Noojipady, P., Anand, A., Huang, C., Kim, D.-H., Collins, K.M., Channan, S., DiMiceli, C., Townshend, J.R., 2013. Global, 30-m resolution continuous fields of tree cover: Landsat-based rescaling of MODIS vegetation continuous fields with lidar-based estimates of error. Int. J. Digit. Earth 6, 427???448. https://doi.org/10.1080/17538947.2013.786146
- Sugita, S., 2007. Theory of quantitative reconstruction of vegetation I: pollen from large sites REVEALS regional vegetation composition. The Holocene 17, 229???241. https://doi.org/10.1177/0959683607075837  
- Telford, R.J., Birks, H.J.B., 2011. QSR Correspondence ???Is spatial autocorrelation introducing biases in the apparent accuracy of palaeoclimatic reconstructions???? Quat. Sci. Rev. 30, 3210???3213. https://doi.org/10.1016/j.quascirev.2011.07.019  
- Tuanmu, M.-N., Jetz, W., 2014. A global 1-km consensus land-cover product for biodiversity and ecosystem modelling. Glob. Ecol. Biogeogr. 23, 1031???1045. https://doi.org/10.1111/geb.12182  
- Wieczorek, M., Herzschuh, U., 2020. Compilation of relative pollen productivity (RPP) estimates and taxonomically harmonised RPP datasets for single continents and Northern Hemisphere extratropics. Earth Syst. Sci. Data 12, 3515???3528. https://doi.org/10.5194/essd-12-3515-2020  




## Poster download
![poster](/DaSciRecon/poster/PalMod_Laura_Schild-1.png)  
[download](/DaSciRecon/poster/PalMod_Laura_Schild.pdf)  

 
## About me
![authorportrait](/DaSciRecon/images/portrait2.jpg)  
I'm **Laura Schild**, an environmental scientist (B.Sc.) and conservation biologist (M.Sc.) with a focus on and passion for data science. 
I'm associated with WG 3 of PalMod Phase 2 and the vegetation task force. I'm doing my PhD in the working groups for *High-Latitude Vegetation Change* and *Earth System Diagnostics* in the section for Polar Terrestrial Environmental Systems at the AWI in Potsdam. My PhD project focuses on the variability of vegetation and climate in the Arctic and how we can use data science to investigate them. More broadly, I am interested in climate-vegetation interactions, climate change biology, biodiversity, and socio-ecological systems.  
- Find all my contact details on [my personal page](https://www.awi.de/ueber-uns/organisation/mitarbeiter/detailseite/laura-schild.html).

