# Parameter Estimation

This serves as documentation of our current best guess about various parameters.

For parameters stratified by demographic, we link to a csv table.

## Estimates

| Parameter                                   	| Unit 	| Estimate Type   	| Estimate      	| Source                                                                                             	| Confidence                                                                                     	|
|---------------------------------------------	|------	|-----------------	|---------------	|----------------------------------------------------------------------------------------------------	|------------------------------------------------------------------------------------------------	|
| Incubation time                             	| days 	| mean            	| 5             	| [Li et al.](https://www.nejm.org/doi/full/10.1056/NEJMoa2001316)                                   	| medium; not sure of relationship with asymptomatic cases                                       	|
| Proportion of mild infections               	| %    	| mean            	| 80            	| [Z. Wu and McGoogan et al.](https://jamanetwork.com/journals/jama/fullarticle/2762130              	| low; very dependent on demographic and sensitive to asymptomatic fraction                      	|
| Duration of mild infectiousness             	| days 	| mean            	| 6             	| [Woelfel et al](https://www.medrxiv.org/content/10.1101/2020.03.05.20030502v1)                     	| low; could be much longer in asymptomatic cases                                                	|
| Proportion of severe infections             	| %    	| mean            	| 15            	| [Z. Wu and McGoogan et. al.](https://jamanetwork.com/journals/jama/fullarticle/2762130)            	| low; higher in older demographics, need age based and underlying condition based distributions 	|
| Time from symptoms to ICU admission         	| days 	| median, IQR     	| 12 (8-15)     	| [Zhou et al.](https://www.thelancet.com/journals/lancet/article/PIIS0140-6736(20)30566-3/fulltext) 	| low; study seems to be right censored                                                          	|
| Duration of severe infection (no isolation) 	| days 	| derived mean    	| 6             	| =[Time from symptoms to ICU admit] - [Duration of mild infections]                                 	| medium; while it is derived, it has more empirical basis                                       	|
| Proportion of Critical Infections           	| %    	| mean            	| 5             	| [Z. Wu and McGoogan et al.](https://jamanetwork.com/journals/jama/fullarticle/2762130              	| low, demographic breakdown                                                                     	|
| Time from hospital admission to death       	| days 	| median, derived 	| 6.5           	| [Zhou et al.](https://www.thelancet.com/journals/lancet/article/PIIS0140-6736(20)30566-3/fulltext) 	| low; study suspect                                                                             	|
| Time from hospital admission to recovery    	| days 	| derived dist    	| Negative Binomial(4.8, .24) 	| [Hospital stay analysis](length%20of%20stay/hospital_stay_analysis.ipynb)                                             	| medium; based on empirical death data in Wuhan                                                 	|
| Case Fatality Rate                          	| %    	| mean            	| 2%            	| [Z. Wu and McGoogan et. al.](https://jamanetwork.com/journals/jama/fullarticle/2762130)            	| low; seems to be at least double this in some demographics                                     	|



## Relationships

#### Infection rate

We believe infection rate is a product of contact rate with each infectious class, where contact rate is parameterized by the number of people that a person comes into contact with per day, the amount of time spent in public spaces in contact with fomites (and the turnover of those public spaces), hand hygiene, face touching rate, isolation speed, family unit size, and a large number of other parameters.

We do not believe R0 is a useful quantity as such, and believe R(t) needs to be a stochastic parameter split by demographic and with a fully defined distribution.


#### Severity by demographic

We would like to back an expected severity out by age and presence of underlying conditions.

We believe that there is a chance that severity is also related to viral load (leading to more severity in healthcare workers, for example). If we can show this discrepancy, we would like to start to parameterize things in terms of immune response / viral replication rates.


#### Hospital length of stay

We believe this is based on admission criteria and illness awareness and other cultural factors. We think that the baseline should be computed assuming that people hospitalize as soon as their symptoms are severe, but introduce a delay parameter. We think that this should be parameterized separately for people who (will) need ICU level treatment.