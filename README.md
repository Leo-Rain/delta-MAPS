# NOW IN PYTHON: LOOK AT THE FOLDER py-dMAPS

# delta-MAPS
δ-MAPS is a method that identifies the semi-autonomous components of a spatio-temporal system and studies their weighted and potentially lagged interactions. The semi-autonomous components of the system are modeled as spatially contiguous, possibly overlapping, functionally homogeneous domains. To the best of our knowledge, δ-MAPS is the first method that can identify spatially contiguous and possibly overlapping clusters (i.e., domains). At a second step, δ-MAPS infers a directed and weighted network between the identified domains. Edge direction captures the temporal ordering of events while the weight associated to each edge captures the magnitude of interaction between domains.

# How to run it?

- The spatiotemporal field to mine has to be a .mat file and be located in the folder "data". delta-MAPS accepts as input,   datasets defined on a regular two-dimensional grid with time series specified at each point. 

- Two files have to be locate in the main folder:

            (1) A .txt file containing a single line specifying the path to the folder data (where the dataset is stored).              
                In the deltaMAPS folder downloadable here it is called fileloc.txt              
            (2) A .txt containing the latitudes of your datasets. Example: in the case of a dataset with 1 degree resolution in
                latitudes, this file will contain a single line written as follow: 90, 89, 88, ..., -88, -89, -90
                In the deltaMAPS folder downloadable here it is called latFile.txt

- To run the code, you have to be in the folder src and type the following commands

      (a) javac -classpath "DistLib_0.9.1.jar:jamtio.jar" *.java
      (b) java -Xmx4g -cp ":DistLib_0.9.1.jar:jamtio.jar" DeltaMAPSclm ../fileloc.txt ../latFile.txt k alpha maxLag netSigLevel q 
  
  Step (a) is compile all java files (if you make a change in the files is necessary to recompile)
  Step (b) runs the code:
  
                        - java tells to the system that you're calling the java runtime environment
                        - -Xmx4g specifies the maximum memory that the system is allowed to use. So in this example is 4
                          gigabytes.
                        - -cp ":DistLib_0.9.1.jar:jamtio.jar" DeltaMAPSclm tells the java runtime environment to start running  
                           DeltaMapsClimate.jar which is our main program to infer areas and the network between areas. 
                        - The rest are inputs of deltaMAPS:
                                   - ../fileloc.txt file containing one line specifying the path where the data are stored
                                   - ../latFile.txt file containing one line with the values of latitudes
                                   - k is the neighborhood size for peak identification
                                   - alpha is the domain identification significance level which will determine \delta.                                     
                                   - maxLag is the maximum possible lag that you want to use 
                                   - netSigLevel is the network identification significance level 
                                   - q = TRUE/FALSE. 
                                                If TRUE, the False Discovery Rate will be used for the network inference
                                                If FALSE, a t-test will be used for the network inference.
                                 
                                   
                                   
  Given different fields (noisier or smoother), these parameters may change.
  As an example, in reference [1] we used the following parameter for mining the HadISST dataset for the period 1971-2015:
             
             - k = 16 (an empirical method on how to determine the "right" k is described in ref [4])
             - alpha = 0.02
             - maxLag = 12. Given the monthly resolution of the dataset (HadISST), this implies that the network inference 
               computed the lag-correlations between every couple of domains from a minimum of -12 months to 12 months.
             - netSigLevel = 0.001  
             - FALSE
  
  - IMPORTANT
  Masked values should be set to -1000000
  
  Notes:
  (a) delta-MAPS is currently implemented in Java. Codes for the network inference in Mathematica can also be found in https://github.com/FabriFalasca/network_Inference . Eventually though, we plan in implementing the code in Python. 
  (b) In this repository we also add the main code for preprocessing the data. Codes for preprocessing are: (i) preProcessing.m, (ii) keepGoodYears.m, (iii) regressConfSen.m . The main code being preProcessing.m
                                                                                                   
                 
Some references

- [1] Bracco, A., F. Falasca, A. Nenes, I. Fountalis, C. Dovrolis (2018)
Advancing climate science with Knowledge-Discovery through Data mining
NPJ Climate and Atmosph. Science,4, doi:10.1038/s41612-017-0006-4 (freely available at the web page https://www.nature.com/articles/s41612-017-0006-4.epdf?author_access_token=8yKsWYkfRLJ28rJHh9KOeNRgN0jAjWel9jnR3ZoTv0Oq67pf4dFJ9xRac7v8-Q584LfS0snu0-26JHkQ8CJsqj5BToQ-jjytnD0wU6-ir8tLRxw4bqDtcfr4OPezgAJAXuV7s5Wt69I53GTpJDEjLw%3D%3D )

- [2] A detailed description of the method can be found in Fountalis, I., Dovrolis, C., Bracco, A. et al. Appl Netw Sci (2018) 3: 21. https://doi.org/10.1007/s41109-018-0078-z

- [3] Falasca, F., A. Bracco, A. Nenes, I. Fountalis (2018)
Dimensionality reduction and network inference for climate data using delta-MAPS: application to the CESM Large Ensemble sea surface temperature. Journal of Advances in Modeling Earth Systems.
https://agupubs.onlinelibrary.wiley.com/doi/epdf/10.1029/2019MS001654

- [4] Falasca, F., Crétat, J., Braconnot, P. et al. Spatiotemporal complexity and time-dependent networks in sea surface temperature from mid- to late Holocene. Eur. Phys. J. Plus 135, 392 (2020). https://doi.org/10.1140/epjp/s13360-020-00403-x
