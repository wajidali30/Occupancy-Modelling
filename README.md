# Occupancy Modelling

Occupancy models estimate the probability that a species occupies a site, accounting for imperfect detection. Let $$N$$ be the total number of survey sites, and $$T$$ be the total number of distinct sampling occasions. 

## Key Parameters

The simplest occupancy model (MacKenzie _et al_. 2002) consists of two key parameters:

- $$\psi_i$$: Probability that site $$i$$ is occupied.
- $$p_{it}$$: Probability of detecting the species at site $$i$$ at visit/occasion/time $$t$$.

## Assumptions

- Sites are occupied by the species of interest for the duration of the survey period, with no new sites becoming occupied after surveying has begun, and no sites abandoned before the cessation of surveying (i.e., the sites are “closed” to changes in occupancy).
- At each sampling occasion, investigators use sampling methods designed to detect the species of interest.
- Species are never falsely detected at a site when absent, and a species may or may not be detected at a site when present.
- Non-detection of the species does not imply absence.
- Detection of the species at a site is assumed to be independent of detecting the species at all other sites.
- The resulting data for each site can be recorded as a vector of 1's and 0's denoting detection and non-detection, respectively, for the occasions on which the site was sampled.
- Detection history: $$y_i=1$$ (if detected), $$y_i=0$$ (if not detected) in a given visit.

## General Likelihoods

For $N$ sites, the likelihood of specie occupancy is modeled as:

$$
L(\psi, p_{it}|y_i)=\prod_{i=1}^NL(\psi_i, p_{it}|y_i)
$$
where $L(\psi_i, p_{it}|y_i)$ is the liklihood of site $i$ given by
$$
L(\psi_i, p_{it}|y_i)=\psi_i \prod_{j=1}^Tp_{it}^{y_{it}}(1-p_{it})^{1-y_{it}}+(1-\psi_i)\prod_{j=1}^T(1-p_{it})^{1-y_{it}}
$$



## Examples

### Example I

Let site $$i$$ have history $$y=01010$$ (i.e., $$y_{i1}=y_{i3}=y_{i5}=0$$ and $$y_{i2}=y_{i4}=1$$). The likelihood of detection would be:

$$
\psi_i\left[p_{i1}^0(1-p_{i1})^1p_{i2}^1(1-p_{i2})^0p_{i3}^0(1-p_{i3})^1p_{i4}^1(1-p_{i4})^0p_{i5}^0(1-p_{i5})^1\right]=\psi_i\left[(1-p_{i1})p_{i2}(1-p_{i3})p_{i4}(1-p_{i5})\right]
$$

### Example II

Let site $$k$$ have history $$y=00000$$ (i.e., $$y_{k1}=y_{k2}=y_{k3}=y_{k4}=y_{k5}=0$$). The likelihood of detection would be:

$$
\psi_k\left[(1-p_{k1})(1-p_{k2})(1-p_{k3})(1-p_{k4})(1-p_{k5})\right]=\psi_k\prod_{t=1}^5(1-p_{kt})
$$

### Example III (Missing Observations)

If sampling does not take place at site $$i$$ at time $$t$$, then that occasion contributes no information to the model likelihood for that site. For example, consider the history $$10\_11$$, where no sampling occurred at time 3. The likelihood for this site would be:

$$
\psi_i p_{i1}(1-p_{i2})p_{i4}p_{i5}
$$

### Example IV

If presence and detection probabilities are constant across monitoring sites, that is $$\psi_i=\psi$$ and $$p_{it}=p_t$$ for all $$i$$. Furthermore, let $$n_t$$ be the number of sites where the species was detected at time $$t$$, and $$n$$ be the total number of sites at which the species was detected at least once. The combined model likelihood can be written as:

$$
L(\psi,\textbf{p})=\left[\psi^n \prod_{t=1}^Tp_t^{n_t}(1-p_t)^{n-n_t}\right]\times\left[\psi \prod_{t=1}^T(1-p_t)+(1-\psi)\right]^{N-n}
$$

## Extensions to the Model (Covariates)

If occupancy probability is assumed to depend on site characteristics such as habitat type or patch size, and if detection probability varies with certain measurable variables such as weather conditions, the covariate information $$\mathbf{X}$$ can be easily introduced into the model using:

$$
\mathbf{\theta}=\frac{\exp(\mathbf{X}\mathbf{B})}{1+\exp(\mathbf{X}\mathbf{B})}
$$

where the parameter of interest for occupancy and/or detection probability is $$\theta$$ and $$\mathbf{B}$$ is the vector of model parameters.

- Because occupancy does not change over time during the sampling (the population is closed), appropriate covariates would be time-constant and site-specific, whereas covariates for detection probabilities could be time-varying and site-specific (e.g., air or water temperature).
- Time-varying, site-specific covariates can be collected and used regardless of whether the species is detected.
- However, it would not be possible to use covariates that change over time and cannot be measured independently of the detection process.

If $$\hat{\psi}_i$$ denotes the occupancy probability measured as a function of covariates at site $$i$$, then the average species presence probability is given by:

$$
\bar{\hat{\psi}}=\frac{\sum_{i=1}^N\hat{\psi}_i}{N}
$$

### References
- MacKenzie, D.I., Nichols, J.D., Lachman, G.B., Droege, S., Andrew Royle, J. and Langtimm, C.A., 2002. Estimating site occupancy rates when detection probabilities are less than one. Ecology, 83(8), pp.2248-2255.
