> # Data augmentation for SSVEPs using Source Aliasing Matrix Estimation

## Source Aliasing Matrix Estimation (SAME) based on paper [1]_.

We propose a data augmentation method named Source Aliasing Matrix Estimation (SAME) [1] to enhance the performance of state-of-the-art spatial filtering methods (i.e., eTRCA, TDCA) for SSVEP-BCIs. Based on the superposition model of SSVEPs, the task-related components are reconstructed by estimating the source aliasing matrixes. After adding noise, multiple artificial signals are generated and then added to the calibrated data in an appropriate proportion. 

This demo shows an example of using SAME for SSVEP-BCIs. The state-of-the-art algorithms for SSVEP-BCIs, i.e. ensemble task-related component analysis (eTRCA) [2] and task-discriminant component analysis (TDCA) [3] are used to compare the performance with and without SAME (w/SAME and w/oSAME, respectively).

> [1] Luo R., et al. "Data augmentation of SSVEPs using source aliasing matrix estimation for brain-computer interfaces". *IEEE Trans. Biomed. Eng.*, 2022. DOI: 10.1109/TBME.2022.3227036
>
> [2] Nakanishi M., et al. "Enhancing detection of SSVEPs for a high-speed brain speller using task-related component analysis". *IEEE Trans. Biomed. Eng*., 2018, 65(1), 104-112.
>
> [3] Liu B., et al. "Improving the Performance of Individually Calibrated SSVEP-BCI by Task-Discriminant Component Analysis". *IEEE Trans. Neural Syst. Rehabil. Eng*, 2021, 29, 1998-2007.

##  The main steps of SAME

1. SSVEP template averaged across trials is initially obtained.
   ![](https://latex.codecogs.com/svg.image?&space;&space;&space;\overline{\boldsymbol{X}}_{n}=\frac{1}{N_t}&space;\sum_{j=1}^{N_t}&space;\boldsymbol{X}_{n}^{(j)})

2. The estimated source signal is reconstructed by estimating the aliasing matrix of sine-cosine signal.
   
   ![](https://latex.codecogs.com/svg.image?\begin{aligned}&space;&space;&space;&&space;\widehat{\boldsymbol{\Phi}}={\operatorname&space;{&space;a&space;r&space;g&space;}&space;\operatorname&space;{&space;m&space;i&space;n&space;}}\left\|\overline{\boldsymbol{X}}_{n}-\boldsymbol{\Phi}&space;Y_{n}\right\|_F^2&space;&space;&space;\end{aligned})
   ![](https://latex.codecogs.com/svg.image?\begin{aligned}&space;&space;&space;&&space;\widehat{\boldsymbol{S}}_{n,&space;m}=\widehat{\boldsymbol{\Phi}}&space;Y_{n,&space;m}&space;&space;&space;\end{aligned})
   
3. Random noise is added to obtain multiple artificial generated signals.
   ![](https://latex.codecogs.com/svg.image?\boldsymbol{Z}_{n}^{(k)}=\hat{\boldsymbol{S}}_{n}&plus;\alpha&space;\boldsymbol{W}_{n&space;o&space;i&space;s&space;e}^{(k)})

​        α is used to control the intensity of the noise, here it is set to 0.05 in this study. Random noise is added just to increase the number of generated signals, with no substantial information. 

## Dataset

#### BETA dataset [4]_ from Tsinghua university.

> [4] Liu B., et al. "BETA: A Large Benchmark Database Toward SSVEP-BCI Application. *Frontiers in Neuroscience*, 2020, 14.

After downloading the dataset, we renamed S1,...S9 to S01,...S09 according to our personal habits. If you don't want to do that, you can change the variables *subject_id* in the functions *beta_TDCA_Aug()* and *beta_eTRCA_Aug()*.

## Results  

- #### demo-SAME-BETA-main.ipynb 

This file is used to calculate the classification accuracy. We have run it and obtained the results for all subjects when the time window is 0.5s, stored in *demo_beta_eTRCA_withoutSAME.mat*, *demo_beta_eTRCA_withSAME.mat*, *demo_beta_TDCA_withoutSAME.mat*, and *demo_beta_TDCA_withSAME.mat*.

The average accuracy across all subjects with different training trials (Nt) is listed as below:

|                | Nt=1  | Nt=2  | Nt=3  |
| -------------- | ----- | ----- | ----- |
| eTRCA(w/oSAME) | 0.119 | 0.521 | 0.609 |
| eTRCA(w/SAME)  | 0.522 | 0.626 | 0.678 |
| TDCA(w/oSAME)  | 0.184 | 0.608 | 0.683 |
| TDCA(w/SAME)   | 0.530 | 0.634 | 0.696 |

The original accuracy of eTRCA and TDCA obtained by our code is similar to the results in the paper [3].

We used a device with 40 vCPUs to compute the results for different subjects in parallel. If you do not have such a configuration, running this code may be a bit slower.

## Acknowledgement

Thanks to Liu B, the author of paper [3] and [4], for his patience in responding to my questions about TDCA.

## email

email: ruixin_luo@tju.edu.cn

This is my first paper. It may not be outstanding or perfect. But it encouraged me, an unconfident girl. I hope I can become more brave and confident in the future.

