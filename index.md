# Improving pollen-based tree cover reconstructions  
![graphic_abstract](/DaSciRecon/images/schema.png)


## Thanks for your interest!

Thanks for scanning the QR-code and wanting to find out more about my current project!   
On here I have added some more details about 
- [Selected Methods](#methods)
- [Data sources](#data-sources)
- [References](#references)  
I also included a pdf of my [poster](#poster-download) as well as a link to the template I used.
I'll keep this page up for the duration of the PhD Days, but feel free to [contact me](#about-the-presenter) if you'd like to know more.  
  
## Methods

### Extracting remote sensing tree cover
The tree cover was extracted for the pollen source area (PSA) of each sampling site. The PSA was calculated using REVEALS and was defined as the area in which 80% of the deposited pollen originated from. REVEALS uses wind speed and pollen dispersal to asses this.  
Mean tree cover from the remote sensing data set was extracted for these areas.

### Reconstructing tree cover
Tree cover was reconstructed from the original pollen records, the REVEALS model output, and the optimized simplified model. Records younger than 450 years before present were considered for this. The large range is due to an assumed age uncertainty in records as well as noise in the signal.  
Tree cover was calculated by using the fraction of tree taxa abundance over total taxa abundance. However, this assumes that the entire modern pollen source area to be vegetated, which is rarely the case. Therefore, a correction for the openness of the pollen source area was applied. This openness correction was derived from the CCI land cover product by using the fraction of open cells in the pollen source area as a measure of its openness.  
![openness_correction](/DaSciRecon/images/open.png)

### Optimizing the Reconstruction
The reconstruction was optimized using a simplified model for converting pollen abundances into vegetation abundances and by optimizing the taxon-specific correction coefficients. The model is based on an R-value model. It assumes that the pollen flux density produced by a specific taxon can be derived with a taxon-specific pollen parameter (_&alpha;_). When these parameters are known for all taxa, historic vegeation (*v*) can be reconstructed from historic pollen records.  
![model-equations](/DaSciRecon/images/r-value.png)


### Validating the new model
The new model was validated using spatial leave-one-out (SLOO) cross-validation. In a traditional LOO cross-validation one site is left out of the model calibration and used for testing subsequently. Ideally, this is repeated with the each site and the mean error can be calculated. This allows the use of the entire data set for model calibration, while still having a realistic error estimate.  
As ecological data is often spatially autocorrelated, the method has to be adjusted to account for that. This is why SLOO was used. Instead of only excluding one site, the sites in a certain proximity are also excluded from the model calibration. The testing is still only done on the original pixel.  
The buffer distance to be used can be estimated using variograms, which show the variance between sites as a function of distance. The range of fitted variograms was used to determine an appropriate buffer for each continental optimization.
![SLOO](/DaSciRecon/images/sloo.png)
 
## Data Sources

| Data        | Dataset Name    | Source  |
| ------------- |-------------| -----|
| Pollen     | LegacyPollen 1.0 | Herzschuh et al. 2022|
| REVEALS parameters| Compilation of relative pollen productivity|Wiezcorek & Herzschuh 2020 |
| Remote sensing tree cover | Global 1-km Consensus Land Cover      |    Tuan & Jetz 2014 |  
| Remote sensing land cover|CCI Land Cover (300m) | ESA 2017 |  

Peter Ewald applied the REVEALS model using the pollen dataset and the parameter compilation.
 
## References

- ESA, 2017. Land Cover CCI Product User Guide Version 2.  
- Herzschuh, U., Li, C., Böhmer, T., Postl, A.K., Heim, B., Andreev, A.A., Cao, X., Wieczorek, M., Ni, J., 2022. LegacyPollen 1.0: A taxonomically harmonized global - Late Quaternary pollen dataset of 2831 records with standardized chronologies. Earth Syst. Sci. Data Discuss. 1–25. https://doi.org/10.5194/essd-2022-37  
- Sugita, S., 2007. Theory of quantitative reconstruction of vegetation I: pollen from large sites REVEALS regional vegetation composition. The Holocene 17, 229–241. https://doi.org/10.1177/0959683607075837  
- Tuanmu, M.-N., Jetz, W., 2014. A global 1-km consensus land-cover product for biodiversity and ecosystem modelling. Glob. Ecol. Biogeogr. 23, 1031–1045. https://doi.org/10.1111/geb.12182  
- Wieczorek, M., Herzschuh, U., 2020. Compilation of relative pollen productivity (RPP) estimates and taxonomically harmonised RPP datasets for single continents and Northern Hemisphere extratropics. Earth Syst. Sci. Data 12, 3515–3528. https://doi.org/10.5194/essd-12-3515-2020


## Poster download
![poster](/DaSciRecon/poster/Poster_LS.png)  
[download](/DaSciRecon/poster/Poster_LS.pdf)  
I used the better poster approach developed by Mike Morrison. You can read more about it [here](https://astrobites.org/2020/02/28/fixing-academic-posters-the-betterposter-approach/).
 
 
## About the presenter
![authorportrait](/DaSciRecon/images/portrait2.jpg)  
I'm **Laura Schild**, an environmental scientist (B.Sc.) and conservation biologist (M.Sc.) with a focus on and passion for data science.  
I'm doing my PhD in the working groups for *High-Latitude Vegetation Change* and *Earth System Diagnostics* in the section for Polar Terrestrial Environmental Systems at the AWI branch in Potsdam. My PhD project focuses on the variability of vegetation and climate in the Arctic and how we can use data science to investigate them. More broadly, I am interested in climate-vegetation interactions, climate change biology, biodiversity, and socio-ecological systems.  
- Find all my contact details on [my AWI page](https://www.awi.de/ueber-uns/organisation/mitarbeiter/detailseite/laura-schild.html).

