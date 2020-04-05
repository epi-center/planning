# Data Organization, Sourcing, Collection, Modeling, Validation, and Access

In order to better capture dynamics in disease spread, we need better empirical datasets and derived data. With properly modeled data, we believe that it should be possible to more accurately capture the uncertainty inherent in epidemic growth and make for more consistent public facing communication and reliable recommendations in health economic policy.

## Vision

The below serves as a 1000 mile high vision for what this could look like in its full fledged state. [A separate document](potential_mvp) gives guidelines for what a minimum viable product looks like. 

#### Data Organization

Specify a set of critical disease statistics that we believe should be available publicly, and incorporate tools that allow us to change these dynamically.

#### Data Sourcing

Build tools that allows people to submit new datasets / data sources for incorporation into a data exchange / data warehouse. These datasets should be categorized into an ontology that allows structured sourcing of model params.

Additionally, build a tool that allows people to request the creation of new specific datasets, including specifying their needs/preferences for granularity.


#### Data Collection

Build pipelines and methodologies that pull data from studies and datasets and incorporate them into our ontology, including giving attribution. These pipelines should be specified based on a DAG of dependencies or a schedule for re-running. All data collection should be version controlled (i.e. the history of different parameters is linked to the pipelines / processed that computed them).


#### Data Modeling

We should build knowledge around the potential relationships between different parameters and demographics. With this knowledge, we should build pipelines that compute derived parameters and more complete distributions and demographic / population based splits.

For each parameter, we should maintain a 'best guess' at a complete empirical distribution, but also allow access to alternative computations and sources.


#### Data Validation (Automated / Manual)

Within our data exchange, we should have characterization of the coherence, completeness, and uncertainty of our various data parameters.

On a manual basis, data sources and pipelines should be able to be flagged for review (including adding a discussion thread). This should change the confidence of derived parameters dynamically.

On an automated basis, data pipelines should be built that compare different datasets and flag them for review if new inconsistencies are created.

#### Data Access

Tools should be built that allow querying the data exchange efficiently. Instead of just accessing the studies directly like in [GHDx](http://ghdx.healthdata.org/) or other similar exchanges, users should be able to query for parameter estimates and understand how they were derived. Additionally, they should be able to download source code for pipelines that will allow them to compute their own distributions based on custom assumptions or run pipelines on our cloud.

We should build tools for notifying users as to substantial changes to data.

Where possible, we could even develop data visualizations on top of the data, allowing for things like private/public portals for municipalities to track how data has changed.


## Details

For now, we focus on respiratory illnesses, though this should be expanded to be more general purpose. The datasets that could be helpful include (but are not limited to - please feel free to suggest additions):

- Demographics
  - Static / Slow Changing
    - Age distributions
    - Distributions of underlying conditions
    - Cultural practices (including Hofstede style data)
    - Environmental data (including cleanliness of surfaces, etc)
    - Socioeconomic data
    - Healthcare capacity data
    - Initial immunity rates
    - Commute data (including mode)
  - Real Time
    - Contact rates
    - Demographic mixing data
    - Hand hygiene data
    - General hygiene data
    - Granular population density
    - Mobility data
    - Travel data (estimating imports)
- Clinical
  - Unmitigated infection rate
  - Latency
  - Time to symptom onset
  - Time to infectiousness
  - Length of symptoms
  - Length of infectiousness
  - Level of infectiousness at different stages of illness (including presymptomatic and asymptomatic infection)
  - Transmission methods
  - Fomite stability
  - Fraction severe / Hospitalization rate
  - Length of hospital stay
  - Fraction critical
  - Length of ICU stay
  - Fraction requiring ventilator
  - Time on ventilator
  - CFR
  - Seasonality
  - Time to recovery
  - Severity by demographic split
  - Test efficacy (precision + recall)
- Virological
    - Replication rate/dynamics
    - Mutation rate
    - Shedding dynamics (stratified by transmission method)
- Disease spread time series
  - Number of confirmed cases
  - Testing rate
  - Number of hospitalizations
  - Number of critical patients
  - Asymptomatic confirmed cases
  - Deaths
  - Recoveries
  - Time to death
  - Time to recovery
  - Patient demographics
  - News data around mitigation measures taken
  - Social media data
- Ancillary data
  - Evidence of reinfection
  - Evidence of inter-species transmission
  
The more granular that each of these datasets can get, the better. This includes granularity by age/population demographic, and by distribution. We need a way to qualify/quantify a given dataset's usefulness / how much we can trust it:
  - How likely is this to generalize to other populations?
    - Is the data broken down demographically?
      - Are there likely behavioral differences between those captured in the dataset and others?
    - Do we believe the population panel is representative?
  - What is our level of trust in this dataset?
    - Is it coherent with other similar datasets?
    - Are there red flags in experimental design?
      - Right censoring in time / distribution
      - Obvious confounding variables
    - Are the sample sizes, test statistics, and p values reasonable?
    - How well captured is the distribution of the data?
      - Location
        - Mean, median, mode, IQM
      - Spread
        - Std Dev/Variance, Range, IQR, MAE, Percentiles
      - Shape
        - Skewness, kurtosis, empirical distribution
      - Dependence
        - Pearson/Spearman correlation with potential confounding variables
      - Raw data
    - How recently was the experiment performed? How up to date are the time series?
    - How consistent was the measurement interval for time series is
    
We should build a data warehouse / data exchange that houses these datasets and allows for both push and pull based submissions.
  - Researchers can submit a study along with the relevant summary statistics / raw data (optional).
    - The study will be added to the repository, but flagged for review
    - Build a review prioritization scheme
      - Other researchers can vote on priority of review
      - Reviews in areas of current data weakness prioritized
      - Studies with good 'completion score' on summary statistics prioritized
    - If summary statistics are submitted, validate they agree with the study
    - If summary statistics are not submitted, add a task for someone to pull those stats out
    - Allow for discussion on a given study (about methodology, concerns)
    - Once summary stats are confirmed, pull into data pipeline for estimating parameters
    - In some cases, users should be able to add data manually without approval
      - Time series updates from municipalities should be easily input, without the municipality needing to develop their own data pipeline. 
  - Data warehouse browseable by parameter. All historical data of parameter estimates maintained
    - Build a 'best guess model' for each parameter that takes the various data sources and combines them
    - Allow accessing the specific studies and links to individual citations, so that researchers can easily cite things in their work
    - Build tool that allows for bayesian regression over several parameters + some user specified distributions in order to estimate distribution of another parameter
      - This should allow researchers to combine private datasets with publically available ones, even if they want to run a separate model than ours
    - Allow requesting a new type of parameter be added to the dataset
    
In addition to fundamental/source data, we should build a set of data pipelines on top of these datasets.
  - Pipelines should be reviewed before being queued for periodic computation. Ideally, pipelines could be re-run when their dependency datasets are changed.
  - Pipelines can include functions that pull source level data.
  - Pipelines can combine existing datasets to produce more tailored estimates for a given population.
  - Pipelines are also responsible for producing distributional data (i.e. given a few datasets, produce the posterior probability distribution of a parameter).
  - Pipelines can compute coherence from multiple datasets and flag them for review in an automated fashion (as below).
  - Any pipeline that produces a new input to the data warehouse should be traceable (VCS on pipelines?)
      
Additionally, we should try to assess where the weaknesses in our current data lie:
  - Build a (reddit-style?) tool that allows researchers to submit what they think the highest priority dataset to produce is
    - Include the ability to specify the level of demographic/population breakdown they think is necessary, the types of summary data they would prefer, and how they believe this data interacts/correlates with other data in a structured way.
  - Produce trust-weighted and unweighted estimates of the uncertainty, coherence, and completeness of our data for different model inputs
    - Where there are multiple studies producing a given parameter, compare the distributions produced
      - If the distributions are disagree by a large amount, flag this discrepancy for review
    - Compute some set of summary statistics about how well parameterized an model parameter is (demographic breakdown, full specification of informative prior).
      - For derived or latent model parameters like basic/effective reproduction number or # of undetected cases, break down the uncertainty into where that uncertainty is coming from (in a tree-like structure, if hierarchical).
      
When data is updated, it should trigger downstream actions:
  - Derived pipelines
  - Production of new visualizations dashboards
  - Notifications to users if parameters they're following have changed
      

