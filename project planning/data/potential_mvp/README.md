# Data Exchange MVP

As a start, we build a [folder in this repo](../../parameters) wherein the README.md contains our best estimates at the parameters, as well as our sources for those beliefs.

For any derived parameters, the source column of the table should link to a notebook that shows the derivation of that parameter, in the same directory. 

For any parameters we believe to have relationships to other variables, we detail these relationships under the [relationships](../../parameters/#relationships) section, and a long-lived issue (with the discussion label. When discussions are available to the repo, we will convert to that) will be started and linked on the topic.

Changes to parameters can be viewed by looking at the git history. 



## Next tasks:

1. Pull known existing datasets into the repo, and add those parameters with sources into a [table](../../parameters/#estimates) 
2. Develop a full ontology of the types of data we would like to have and the relationships between the data-sets.
3. Build a separate repository for housing this information, and start building tooling for the data exchange. 
4. Build tools that allow for the generation of a full distribution given a few summary statistics
5. Build tools that combine a statistic with demographic data and give expected values/distributions for different demographics, or output additive/multiplicative modifiers for those demographics.