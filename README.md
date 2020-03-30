# Understand Covid

[![gitter](https://img.shields.io/gitter/room/understand-covid/community?style=flat-square)](https://gitter.im/understand-covid/community)
[![jogl](https://img.shields.io/static/v1?label=join%20us%20on&message=JOGL&color=red&link&style=flat-square)](https://app.jogl.io/project/169)
[![license](https://img.shields.io/github/license/understand-covid/proposal?style=flat-square)](https://github.com/understand-covid/proposal/blob/master/LICENSE)

We would like to build a toolkit that makes building 'responsible' epidemiological forecasting models easy and computationally efficient. It should make sane assumptions about variable distributions, include ways to integrate many types of empirical data in a straightforward way, allow for sensitivity analysis and understanding of uncertainty, capture possible dynamics of different types of suppression, mitigation and control tactics. 

With sufficient resources, we would also try to design systems that maintain up to date, easily ingestible, empirical data on population level statistics ranging from demography and network connectedness on a granular level to granular public health data.

## A proposal for deep collaboration in epidemiological modeling.

Over the last three months, we've seen an alarming failure of the publically available epidemiological models to forecast the spread of COVID-19 through the world. While many epidemiologists warned early on that there was the potential for millions of deaths globally, most of those estimations were based on a much slower spread of the disease than has transpired in Iran, Lombardy, Spain, New York, and a growing number of other locations. Ultimately, the CFR and IFR that have been predicted early on may be accurate, but the threat that this pandemic could infect the entire globe could result in death tolls in the tens or hundreds of millions.

As such, governments across the world have taken drastic actions to limit spread. While these measures have been effective in some places, they have enormous ramifications to the global economy and the livelihoods of inidividuals. Consensus is missing on the effectivenes of these different measures, as well as an understanding of how relaxing some of the mitigation measures could lead the disease to accelerate once again.

## The challenges

nCov19 is a novel virus that was poorly understood as it started to propagate through the world. While epidemiologists and virologists got to work as soon as they could on understanding the mechanics by which the disease propagates through the body and through society, it continues to befuddle, reacting differently in different populations. The virus seems to spread almost entirely through droplets, but people are much more infectious early on than those with SARS (the disease's closest cousin) were. It has a long latency period and many people are largely asymptomatic, making it hard to track growth and hard to assess the impact of various mitigation strategies. Testing rates (and strategies) vary enormously by country, leading it to be the case that there are is huge variance in approximations of CFR and R0. Some have resorted to measuring spread through death rates, but given that it seems especially deadly in older demographics and those with existing conditions, these stats can also be misleading. Further, given the heterogenous and cliquish nature of populations, it propagates very differently in each population - early on in New York, hospitalizations were largely in young and healthy people. 

Media coverage of the virus has also posed challenges in terms of risk assessment and public health. A deluge of information and misinformation has led the media coverage to fluctuate between people needing masks and masks being ineffective, a propagation of the belief that the virus is somehow connected to cultural of ethnic identity, and many people believing that they are much safer than they actually are. Many media outlets leaned on comparisons with different viruses, stating that the r0 and CFR were lower or more controllable than some of the other epidemics we have seen recently.


## Problem statement

The team believes that COVID-19 illustrates many of the issues that exist in epidemiological modeling and public health risk assessment, and wishes to use the momentum that exists right now to garner more investment into modeling, tooling, and data systems so that the ongoing mitigation efforts and handling of future epidemics can be done in a more measured way.

It is our belief that the current state of epidemiological modeling factors pretty directly into the poor containment of COVID-19. Epidemiological models that seem to have been used for risk assessment:

  1. Broadly rely on estimated R0 values. R0 is a notoriously fickle quantity [[1]], and does not provide sufficient information to assess risk in any scenario. While the quantity is useful in the right hands, it is easily misconstrued and abused.
  2. Often do not include robust estimates of uncertainty. Some analysis is often done via sensitivity analysis, but too many variables are left non-random and the state space is not sufficiently explored. 
  3. Rely on some set of assumptions of homogeneity. This varies from simulations that are completely deterministic based on initial conditions to others that relax a few assumptions of homogeneity and inject stochasticity in a handful of points in a relatively simple model. Even sophisticated individual/agent based models often do not do a good job of modeling heterogeneity in behavior or oversimplify the mechanics of transmission. 
  4. Broadly do not take into account the differing properties of different populations. While some models include spatial heterogeneity or model individuals, these often make unrealistic assumptions about social graphs or are wrongfully cross applied to another population with different cultural norms, population density, and public health statuses.
  5. Do not do a good job of allowing decision makers to see the impact of potential mitigation measures. The impact of mitigation measures on model dynamics is often left to speculation, adding some manually estimated damping term on growth. Without deep understanding of the model in question, it is hard to understand the temporal sensitivity of measures or understand the difference in effectiveness of different types of mitigation strategies. Further, given that uncertainty estimates are rare, any well designed inclusion of mitigation strategies into models can still lead to poor decision making.
  6. Are often built post-hoc, after the data from an outbreak is well understood. While this is understandable, many models suffer from overfitting and confirmation bias.
    
As such, we believe that it is time to build a more robust framework for modeling epidemiological risk, starting from first principles as often as possible. We believe that a general purpose toolkit should exist that allows the modeling of any new virus in a way that includes new information seamlessly as it is available and gives tools for assessing mitigation strategies. This toolkit should allow for the use of multiple styles of models (including looking at how much different models diverge) and include uncertainty estimates by default. This is a huge undertaking, but an incredibly valuable one.

## Components

In order to build this general purpose toolkit, we need many different components to line up.

1. A more sophisticated set of compartmental and individual/agent based models. 

    Compartmental models by definition make mean-field approximations and often ignore important dynamics. Agent based models often make simplifying assumptions because of seeming computational intractability. As the number of compartments goes up, so too does computational complexity, approaching agent based models. We do not believe that this is an acceptable tradeoff - instead of making simplifying assumptions in both cases, we should invest in hybrid approaches, improved access of computing resources for epidemiological modelers, and improved efficiency of algorithmic implementations. If physicists / material scientists and atmospheric scientists can build models with millions of finite elements in a computationally tractable way, so too can epidemiologists. We should be open to more flexible models and new approaches to modeling.
  
    Further, we should be building tools that allow for better uncertainy estimates in these models. Bayesian approaches going beyond simple MCMC and Bayesian regression over a single parameter are necessary. We should be utilizing deep learning and bayesian neural nets. We should look into things like adversarial learning. We should investigate techniques to build more computationally performant approximations of the extremely complex models we build (similar to shallowing of deep neural nets), so as to allow for better explorations of the parameter space. We should explore state spaces of different types of models to see if there are unexpected emergent phenomena.

2. Better incorporation of public health and population level data.

    Models should take into account the extremely varied natures of different populations by default. Demographic information about ages/genders is not enough. Population density and network connectivity (in terms of degree and all of its moments) should be included. Estimations of hygiene (on both an individual and population level) and the level of education on preventative measures should be included. Quantizations of the public's receptiveness to various mitigation measures should be developed. Understanding of prevalence of different types of existing conditions is necessary. The relationship between the public and different channels of media dissemination should be modeled, measured and incorporated. Water/air quality, city cleanliness... the list goes on. Public health surveillance data should be available in as digestible and granular manner as possible, and where data is missing, we need to develop methods of making estimates of reasonable priors. We need to dive into understanding how different mitigation strategies are likely to impact our models and model them appropriately. 
    
3. Improved utilization of empirical data from medical studies.

    We need to develop guidelines for including empirical distributions in models. Different types of clinical data should be categorized, and the procedures for leveraging this data to plug into a given model should be well understood. We need to study things like the relationship between transmission and shedding rates and do a better job of estimating distributions over various transmission vectors and progress through the disease. This should be done in a way that we can see the kind of impact that changing our assumptions about transmission probability and viral mechanics have on risk, as opposed to approximating them separately and then feeding in static parameters.

4. Improved data availability for realtime diagnostics.

    The spread of disease should be have a trail that is highly available and reliable. Much like data exhaust in the corporate context, proper modeling and data engineering is necessary. It is unacceptable that the most commonly used data format for tracking COVID-19 growth changed data formats without notice in the middle of the epidemic [[2]]. It is unacceptable that there is a preponderance of missing data, and that datasets lack sufficient information to determine what is incomplete or inaccurate. It is unacceptable that even counties that do keep up to date records often do not follow the same format and do not make historical data available [[3]]. It should be the case that every population's response to mitigation measures can be measured in a consistent manner. While we can do things like measure turnstile data [[4]], a tool that makes this data available in a less ad-hoc way should exist.
  
5. Improved interpretability and interoperability.
  
    There is a risk that models that we build that incorporate all of this information become less interpretable, rather than more. We must design systems that enable people without deep understanding of the model to make responsible decisions based on outputs. This means development of a new set of metrics and a new set of tools for risk assessment. We believe that the types of mitigation strategies that a community can employ and existential risks they face should all be encoded into the model. For example, social distancing should have a different impact on the model than travel restrictions. Awareness campaigns of the virus at large and preventative measures should be modeled differently. These strategies should be encoded simply and in a way that the inputs can be measured directly, and should be reactive to the population. Additionally, the outputs and inputs should be able to be connected to other systems directly without the need for user intervention.
    
Several other projects have started to undertake some of these challenges, including Epidemicforecasting.org and Kaggle [[5]], [[6]]. Both have started to cultivate useful datasets that could help quantify the effects above, but are likely to need to be manually maintained and are of varying quality and ingestibility.

The former leans on GLEAMviz, which is likely the closest model to what we aim to build [[7]]. However, it is closed source, does not have demographic breaks (just geographical ones), doesn't have good support for mitigation strategies or distributional assumptions, does not have good support for presets, and requires building things through a GUI - it is not interoperable with existing data stacks.
  
## Approaches

  We believe that tools accomplishing any of the goals above should be a public good and available to public health officials throughout the world. As such, we suggest an open science approach. We are actively looking for collaborators from all disciplines who believe they have something to add. 
  
  As a first contribution, the primary author has developed an [initial compartmental model for COVID-19](https://github.com/understand-covid/proposal/tree/master/model) that adds additional states and more stochastic variables, as well as using biologically motivated distributions for parameters. It allows for modeling of a few different mitigation strategies. This model is not meant to be used as a tool for making predictions as of now; it is merely a proof of concept. Documentation of this model and the source code for it are available in this repository.
  
  Moving forward, we plan to start to scope out work on an initial set of software based tools to put together epidemiological models - we aim to be influenced by and build upon great projects like scikit-learn, Pyro.ai, Spark, and Prefect.io.
  
  This proposal is open to feedback and criticism - the authors are not authorities on the state of epidemiological modeling by any means, and corrections and exposure to relevant prior art are appreciated. In the coming days, we plan to update this proposal with more actionable next steps.
  
## Contact / Collaboration
  
  If you are interested in collaborating or helping fund this project:

[![email](https://img.shields.io/static/v1?label=email&message=johnurbanik@gmail.com&color=red&&style-flat-square)](mailto:johnurbanik@gmail.com)
[![twitter](https://img.shields.io/twitter/follow/johnurbanik?label=%40johnurbanik&style=flat-square)](https://twitter.com/johnurbanik)
[![gitter](https://img.shields.io/gitter/room/understand-covid/community?style=flat-square)](https://gitter.im/understand-covid/community)
[![jogl](https://img.shields.io/static/v1?label=join%20us%20on&message=JOGL&color=red&link&style=flat-square)](https://app.jogl.io/project/169)
  
  
    
    
[1]: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3157160/
[2]: https://github.com/CSSEGISandData/COVID-19/issues
[3]: https://www1.nyc.gov/site/doh/covid/covid-19-main.page
[4]: https://github.com/toddwschneider/nyc-subway-turnstile-data
[5]: http://epidemicforecasting.org/
[6]: https://www.kaggle.com/covid-19-contributions
[7]: http://www.gleamviz.org/