# Framework

As a jumping off point for collaboration, we build a compartmental model with some relaxed assumptions.

This model is meant to be one of the first steps in building a toolkit for more robust, general purpose epedimiological modeling. It should not be used for policy decisions in its current state.

The model is still one specific compartmental model, and it is likely that doing sensitivity analysis across several models at the same time (including individual level models and models relaxing different assumptions) would be beneficial. In the future, we would like to build tools for Bayesian inference on top of these models. 

As it is a preliminary model, it is not very well structured (and not very computationally efficient. In the future we would like to build an API interface that allows for feeding empirical data into the model, defaulting to sane priors for distributions and parameters where only summary statistics are available. When data is available, we would like to use tools like bayesian regression to estimate the impact of mitigation measures over empirical datasets more robustly.

Some caveats of the current model follow:

1. It is not exhaustive: tuberculosis and viruses like dengue fever may require additional state transitions. Different transmission vectors likely need to be modeled differently. Age based population boundaries may not be sufficient to model clustering well - there are likely sensitivities to health conditions more generally, and network effects and clustering can have counterintuitive effects (https://arxiv.org/abs/2003.10604). 
2. The model has a huge number of degrees of freedom, and parameters are not easy to properly tune. Sensitivity analysis based on loose estimates can at least give some understanding of dynamics and the level of stochasticity until the model is better specified.
3. In order to use this type of model properly to understand how a given popuation will respond to future pandemics, we need to start more substantial efforts into empirical medical anthropology and should invest heavily in mHealth and population level sensing. 


## Compartmental model

We expand the current state of the art formulation of compartmental models to have a much increased state space. This is due to the belief that behavior of individuals in different states cannot be fully captured by a reduced state space without the use of complex multimodal/compound distributions (also assuming the compartmental model is stochastic). In order to make this model more general purpose, we will need to add dynamics for different transmission vectors, population change and things like carrier states, temporary immunity. 

These states are also split by demographic population, and the opportunity to use the outputs of one model as the inputs of another leads to the possibility of hierarchical compartmental models.


S -> E -> P -> (V, Q) -> I -> R; I->P; V -> H -> (C, R); C -> (D,R)


S (susceptible): Individuals who are virus free

E (exposed): Individuals who will convert to an infectious state after a latency period

P (prodromal, infectious asymptomatic): Individuals who are shedding at a rate high enough to cause infections, but are not displaying symptoms

V (virulent, infectious symptomatic): Individuals who are displaying symptoms, and may cause a higher infection rate due to droplet spray (or potential aerosols)

Q (infectious subclinical): Individuals who will never display symptoms, but can continue to cause infections

I (isolated): Individuals who have changed behaviors because of suspicion of infection, but are not hospitalized

R (recovered): Individuals who have recovered and are no longer infectious.

H (hospitalized): Individuals with serious infections.

C (ICU): Individual in critical condition.

D (dead): Individuals that have been confirmed dead while infected with the disease, including from comorbidities.

P, V, Q, I, H all contribute transmissions to the S -> E transition. We assume imports can add cases to E, P, I, and V. 

Other things to explore is simultaneous modeling of an individual as being part of multiple populations:
  1. A state graph based on testing, where testing rates can influence the distribution of phase transitions to isolation, etc
  2. A state graph or multiple state graphs for 'belief' where various subpopulations express different beliefs, and react to mitigation strategies differently because of these.
  
Demographic breakdown of the states based on things other than age are likely worthwhile - current policies make it very evident that risk should be modeled differently for service workers and healthcare workers than the general population, and COVID's properties make it apparent that we should likely model the presence of existing conditions as part of the state. As we expand this state space, we start to approach individual level models, but mean field approximations may still be useful in terms of computational efficiency.



## Parameterization

The parameterization we rely on tries to avoid utilizing a r0 (the estimate of the basic reproduction number) by instead characterizing transmission based on:
- Contact rates. This is an approximation of the effective number of 'contact incidents' that a population has per unit of time. In the future, this should be decomposed further based on cliquishness (someone who contacts the same 10 individuals every day over the course of an infection should be modeled differently than someone who contacts 10 unique individuals each day). It should also incorporate different types of contact (passing in the subway is different than living in the same household). Given that current belief is that most COVID-19 transmission is hand-to-mouth, we have elected to maintain the heuristic that all types of contact are the same, and instead count living/working with someone as multiple units of contact.
- Infectious 'power'. A quantification of the rate of spread of the virus that is dependent on state. In actuality, infectious power is a more continuous function of time; SARS infectiousness is highest long after symptoms appear, while COVID-19 infectiousness may be high at the beginning of symptoms. These differences are handled by separating out the states, as above, and assigning different infectious powers.

Additionally, we parameterize by demographic subpopulations. For now, we only maintain age based populations, running separate systems of equations on each age class, as in [1]. Additionally, we build and parameterize a 'mixing matrix,' where each subpopulation has a probability of transmitting to other subpopulations [2].

We define a new transmission parameter set, characterized by a negative binomial distribution transmission function (which has biological backing; things can be characterized as failure of the virus or the human). 

The parameterization of transmission is defined based on the below parameters:

- κ: Heterogeneity. 
        - This is a fitted value that tries to approximate the heterogeneity of a given population. Subtantial work is needed to figure out how to fit this value, it is chosen arbitrarily here.
- τ: Infectious power:
      - A multiplier per unit of contact with population in each phase (it may actually react to different populations differently, but this is assumed to be captured through contact rate, below). Mutation of the virus is ignored for simplicity now. For a given value of κ, this can be chosen by approximating the number of people that would be infected in a closed population given varying levels of contact.
- ξ: Mixing matrix. 
      - A matrix for how different subpopulations interact. Could also be parameterized in time, or have stochastic properties (TODO)
- C: Contact rate.
      - 'Contact' is assumed to be a property of a population, dependent on both things such as connectedness, hygiene, and sensitivity to illness (how long do symptomatic people continue going to work, for example). Unit wise, we say this is the number of people someone has spent long enough with (per day) to touch a shared surface before half-life. Things like subway rides, grocery stores, contribute as well. This time dependent parameter is a quantization of the level of contact that a population has with other people. In the future this should be decomposed by the elements mentioned above and modeled by public health measurement. 
- M: Mitigations:
      - Mitigation measures change this quantity (in time) [3]. Mitigations can change all of the above (through partial immunity, decreasing high connectivity graph edges, changing population isolation, and changing number of contacts, respectively). These are assumed operate as heaviside functions, so that the difference equations remain simple.
    
These parameters all come together to form the NBD transmission function [4]. Infection rate per day:
    
    dS/dT = S * -1/κ * ln(1 + r*κ)
    r = τξCM * prevalence
    
r and κ are assumed gamma distributed (understanding how to chose the shape parameter of the gamma distributions and change it based on mitigations requires further research).

Additional endemic parameters include:

- μρ: Phase transition mean. 
    - Phase transitions sample from a gamma distribution based on this mean. Parameters are considered fixed, though mitigations may change the mean and shape parameter. Empirical data on variance, skewness, or more detailed statistics of phase transitions would allow us to choose better distributions/ parameters.
- ρ: Phase splitting.
    - What percentage of people exiting the previous phase end up in that statte. Modified by severity.


Other Demographically/state split or time varying parameters include: 

- Se: Severity prior.
    - A prior on the severity of the severity of the disease, relative to a healthy adult, normally distributed. While this is stage dependent (TODO), we assume that it applies the same in each stage, using the probability of progression from hospital to ICU as a proxy, centered on 40-49 year olds [5]. Static (non-stochastic) for now (TODO).
- Mi(t): Mitigation effectiveness.
    - How Effective are specific mitigations are modifying phase times or contact rates (i.e. getting people to isolate removes people from the prodromal pool faster and moves them to isolating, awareness gets virulent people to isolate faster).
- I(t): Import rate.
    - How many people are being imported per day (through travel) into each phase. These numbers need to be tuned differently for different types of subpopulations [6]. In the future this could be expanded to allow different networks to interact, or with separate policies for local and plane travel to explain network effects like in NJ/CT.
- α: Testing probability by phase. 
    - Parameterized in time, due to interventions like test availability, early testing.


Additional initial conditions include a date range, population, age distribution, and initial number of cases (and how they're distributed in the population). We aim to show that there are huge variations in the overall growth curve based on initial conditions under heterogenous mixing.

[1]: See https://github.com/neherlab/covid19_scenarios

[2]: For example, pre-school age children are likely to transmit to parents and grandparents, but less likely to transmit to people in their mid 20s in urban areas. The transmission probabilities for elderly people are dependent on the prevalence of extended family units.

[3]: For example, social distancing executed by the full population would have a proportional impact on all of the above, but social distancing that is not taken seriously is likely to have a larger impact on vC(t) than iC(t). These mitigation strategies themselves can likely be parameterized and characterized with sufficient data from mHealth, sensors, medical anthropology.

[4]: See https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4808916/pdf/ijerph-13-00253.pdf

[5]: http://weekly.chinacdc.cn/en/article/id/e53946e2-c6c4-41e9-9a9b-fea8db1a8f51

[6]: Interstate or intercity travel matters when performing analysis at that scale. With sufficient compute, these params could allow us to model network effects, having populations feeding directly into one another (though for now we maintain that export rate is 0).

