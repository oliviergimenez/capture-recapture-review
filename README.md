Why a review on capture-recapture?
=================================

I will be attending the [Wildlife Research and Conservation 2019
conference](http://www.izw-berlin.de/welcome-234.html) in Berlin end of
September. At the time I was proposed to come, I thought it’d be cool to
review recent advances in capture-recapture, which I have been thinking
to do for a while. Now time to actually do something. As a
capture-recapture aficionado, I have witnessed the awesomeness of
the last 10 years: clever methods have been developed and exciting
ecological questions have been asked and answered. Let’s have a look in more details
to the period 2009-2019.

![](images/Recapture.png)

Bibliometric and textual analyses
=================================

To determine the questions and methods folks have been interested
in, I searched for capture-recapture papers in the Web of Science. I
found more than 5000 relevant papers on the 2009-2019 period. These
papers are gathered in the file
[crdat.csv](https://github.com/oliviergimenez/capture-recapture-review/blob/master/crdat.csv).
To make sense of this big corpus, I carried out biliometric and textual
analyses in the spirit of [this
paper](https://www.cell.com/trends/ecology-evolution/fulltext/S0169-5347(18)30278-7). Explanations along with the code and results are [here](https://github.com/oliviergimenez/capture-recapture-review/blob/master/bibliometric_analysis.md). I also inspected a sample of methodological and ecological papers, see [there](https://github.com/oliviergimenez/capture-recapture-review/blob/master/make_sense.md) for more details. 

Below is a synthesis of my findings. I will add the references later on.

Ecological questions
====================

- Note: In the 80s and 90s, there was a shift in emphasis from abundance estimation to survival estimation (Lebreton et al. 1992). Well, it seems like things are more balanced, and there is clearly a lot of work on both topics now. This is probably thanks to spatial capture-recapture models and their ability to make use of all available data (spatial info above all), produce density estimates and account for individual heterogeneity regain of interest in estimating abundance and density.

- Question 1: The study of **dispersal** is a big deal.

- Question 2: There is been a growing interest in studying the **threats on biodiversity** by determining the impact of climate change, pollution, or overexploitation on animal demography. (what about invasive species? deforestation and habitat loss?).

- Question 3: The field of **evolutionary ecology** has been using capture-recapture to quantifying life-history trade-offs, estimating heritability of demographic parameters, describing senescence patterns or assessing selection gradients.

Capture-recapture methods
=========================

-   Method 1: uptake of hidden Markov models, and models w/ **hidden
    variables**; turning point w/ realization of hidden structure
    (partially-observed process - graph of HMM). State-space and HMM.
    Why? Deal w/ complexity and uncertainty, while offering great
    flexibility by decomposing obs vs state! Multistate (Lebreton) and
    multievent (Pradel) are actually HMM (particular case of SSM), and I
    recommend we use this terminology as tools are readily available.
    (Le fils tue le pere 💀). King (2012): 'The application of state-space models to capture–recapture–
    recovery data is very appealing due to its simplicity
    and ease with which models can be generalized. Partitioning
    the system and observation processes into
    distinct components leads to the natural identification
    of each individual process operating. This allows a complex
    model to be constructed using a sequence of simpler
    models, corresponding to each individual process acting
    on the study population'.

-   Method 2: **Bayes analysis**: we now can fit complex models with,
    e.g., random effects or spatial component and estimate latent
    variables (disease or breeding status in multistate models, home
    range (activity) centers in spatial explicit models)

-   Method 3: **spatial capture-recapture**

-   Method 4: Importance of methods for **unmarked individuals**
    (N-mixture, occupancy, mark-resight)

-   Method 5: **Random effects**

-   Method 6: Combination of information! **Integrated population
    models** as seen by Andy Royle:

![](images/raftstack.jpg)

-   Method 7: **Continuous capture-recapture**? Opportunist data?
    Citizen science data?

-   Method 8: **Non-invasive methods** - Genetics: see excellent paper
    by C.T. Lamb
    <a href="https://esajournals.onlinelibrary.wiley.com/doi/full/10.1002/eap.1876" class="uri">https://esajournals.onlinelibrary.wiley.com/doi/full/10.1002/eap.1876</a>
    ; see also review by Andy in Ecography + Camera-traps.

![](images/dna_ClaytonTLamb.png)

What do we miss? My two cents insights.
=======================================

**Methods**

- Data entry error? Data wrangling, procedures. Everything that happens before the actual analysis?!!!!

- Sampling design: Model- and design-based design (adaptive sampling
    proposed, power analyses). Lebreton et al. 1992: 'An applied statistician knowledgeable in capture-recapture should often be consulted for both the design and analysis of capturerecapture or capture-resighting studies, especially if the population issues are complex.'

- A proper treatment of missing and censored data

- Covariates in non-spatial models. What about in SCR models?

- Visualisation tools - how to get. back to raw data?

- New data technology. Big data (work by S. Bonner) mais aussi small data? Model selection via reduction techniques (Lasso, papier par je sais plus qui qui fait du boosting).

**Ecology**

-  Population dynamics to be coupled with movement ecology,
    ecophysiology, etc; see figure in network paper by Réale in which
    capture-recapture cluster à part… Foster collaboration. Pollock (1981 in JASA): 'Cross-fertilization with human 
    capturerecapture. Cell phone e.g., and movements.'

- Not so new in disease ecology, mais underused. Link of population dynamics w/ other disciplines of ecology (functional ecology [see our special issue], quantitative genetics, …).

- What about predictive ecology? Context of global change. Link with mechanistic approaches.

- Faire un parallèle. Avec ce qui se passe dans les survival models en médecine? C’est quoi qui se développe?

Big deal. Heterogeneity in individuals. Cf vol special Sandra Hamel. 

- Metanalyses. Of survival. Other demos parameters. Effect of climate on demography?

- Big databases. Comme les trucs de Rob. Citer le truc de Sébastien DEvillard.  

- Reproducibility crisis. Monopole de l’AIC?

- Brian McGill attack on detection; statistical machismo

- Animal welfare? Impact of marking on animals?


Capture-recapture is dead, long live the capture-recapture!
===========================================================

- In 1992, Lebreton et al. predicted : 'New developments will appear, in at least four directions: first, new methods of marking are appearing; second, the model-building and computations will be made easy for the users of estimators and test statistics; third, some technical (statistical) shortcomings of the present procedures will be overcome; and fourth, more
sophisticated models will appear as deeper biological questions will be asked through more sophisticated experiments.' Pretty much happened. Shall i take the risk to make a prediction for the next 10 years? Heeeeeeelp.

- About the fourth point, more sophisticated models will broaden biological relevance. One of the most
promising directions will be to allow capture-recapture models to include recruitment and dispersal. Such
models would be able to address questions relative to trade-offs between dispersal and survival, and to the
regulation of spatially organized populations. More generally, multi-site models, and models considering
individual covariates susceptible of changing through time, provide a challenging direction of development

-   eDNA

-   multispecies

-   machine learning

-   social sciences to improve detection (cf papier X Lambin)

-   technology doesn’t have to be about mechanic, organic as well,
    e.g. detection dogs (mettre def technologie, et photo iris ours
    phyrénées)

-   Marking evolves: DNA, cameras, acoustics, drone. Methods need to
    keep up.

-   Big data? Citizen science protocols? Improve MCMC - Nimble? Stan?
    TMB?

- Intake of Bayesian analyses. Kéry and Schaub books. Importance of workshops. And software. And small conferences (Euring). Now ISEC. And people. Friendly. Inclusive. 

Recommandations (if I may)
==========================

-   New methods need to be evaluated - there is an actual niche there

-   Importance of **workshops** to disseminate and form new
    collaborations

-   A plethora of **software**: E-SURGE, MARK, marked, etc; some
    homogeneization, importance of portage in R, how to value the work
    of these folks investing months and years in this.

-   Don’t reinvent the wheel - read your classics: Lebreton. Cormack.
    Jolly. Arnason. Otis.
    
-   Relatively short time-series (nb occz is nb annees cf Frederiksen). Du coup deux reflexions :
    -   long-term monitoring (paper Sheldon et Clutton-Brock. Nichols)
    -   IPM et go for adaptive monitoring et management; learning while
        doing


Take-home message
=================

> Even the most general mathematical model is a plaything relative to the complexity of an animal population (Cormack 1968 cited in Lebreton et al. 1992).
