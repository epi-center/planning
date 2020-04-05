# Assumptions

This simulation changes the assumptions of [Neher Lab](https://github.com/neherlab/covid19_scenarios), but is indebted to their initial modeling. 

## COVID-19 facts
- COVID-19 has a very long latency/incubation time, and a lot of variation [1]
- COVID-19 seems to have a large number of asymptomatic patients, with estimates ranging from 10%-80% of people being asymptomatic or subclinical. 
- COVID-19 sheds a huge amount of the virus in sputum as compared to SARS, even in asymptomatic patients [2].
- Many of the parameters of COVID-19 do not follow poisson distributions, but instead may be the mix of multiple distributions (for asymptomatic spreaders and symptomatic spreaders, for example). Biological intepretations suggest negative binomial distributions may be accurate.
- COVID-19 is quite stable on surfaces, so transmission via fomite is possible even in asymptomatic cases (via touching mucus membranes followed by surface, spitting, etc).
- COVID-19 shedding is at high enough levels for infection starting in prodromal phase
- Some suspicions that people who are 'recovered' may continue shedding for several days [3].
- Mitigation factors likely interact differently with asymptomatic and symptomatic people, as well as differently among demographic subgroups.

## COVID-19 known (estimated) parameters
- Incubation time between 2-14 days, mean 5.5 [4].
- Symptomatic time between 0-20 days, usually not longer than 10 [5].
- Infectious period from end of incubation to ~10 days later (or maybe 10 days after onset of symptoms?), with similar viral load in symptomatic vs asymptomatic ([6], [7]). Symptoms may just serve to increase infectiousness.
- Infectious period may continue after symptoms end, but the load likely lower and may not be sufficient for infection [8].
- CFR between 0.2-10%, likely dependent on prevalence in age groups and level of care
- CFR up to 50% in 70+ year olds, CFR near 0 in infants, high in elderly. 
- Chance of asymtomatic infection estimated to be as large as 30%.

## Changed assumptions
- R0 is 'broken' [9].
- Different populations have different contact rates, transmission rates, clustering, isolation boundaries, pre-existing health conditions, age distributions, resistance, etc. Distributions likely have different skewness and variance in addition to different means.
- A virus with properties like COVID-19 is especially sensitive to these cultural properties; these properties are magnified as compared to viruses that follow distributions where they are nearly always symptomatic, shed at an infectious rate after symptoms start, and have short delays. We assume that these need to be modeled in order to get an accurate picture of the growth.
- We assume that almost every variable needs to be modeled as a random variable, as the emergent properties are very different - super spreaders and other dangerous phenomena only arise when this is done.
- Like Neher, we assume that mixing between subpopulations is not homogenous, but model this as a mixing matrix.


1: https://www.cdc.gov/training/QuickLearns/exposure/2.html
2: https://www.medrxiv.org/content/10.1101/2020.03.05.20030502v1
3: https://jamanetwork.com/journals/jama/fullarticle/2762452
4: https://www.jwatch.org/na51083/2020/03/13/covid-19-incubation-period-update
5: https://www.uptodate.com/contents/coronavirus-disease-2019-covid-19
6: https://www.nejm.org/doi/10.1056/NEJMc2001737
7: https://www.medrxiv.org/content/10.1101/2020.03.05.20030502v1
8: https://jamanetwork.com/journals/jama/fullarticle/2762452
9: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3157160/