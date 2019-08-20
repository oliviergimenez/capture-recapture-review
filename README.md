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

- To list the relevant questions, I used the results of a [topic modeling analysis](https://github.com/oliviergimenez/capture-recapture-review/blob/master/bibliometric_analysis.md#topic-modelling) and [my reading of the papers](https://github.com/oliviergimenez/capture-recapture-review/blob/master/make_sense.md) published in the most population journals.  

- In the 80s and 90s, there was a shift in emphasis from abundance estimation to survival estimation (Lebreton et al. 1992). Well, My general impression is that things are more balanced nowadays, with a lot of work on both topics. This is probably due to the uptake of spatial capture-recapture models and their ability to make use of all available data (spatial info above all), produce density estimates and account for individual heterogeneity regain of interest in estimating abundance and density.

- Question 1: The study of **dispersal** is a big deal, as well as **population growth**, **recruitment** and **stopover duration** in relation to migration; **species richness** is also of interest (big influence of A. Chao's work). Obviously, the study of survival remains the focus of much interest, in particular the consideration of competing risks and estimation of cause-specific mortality rates.

- Question 2: There is been a growing interest in studying the **threats on biodiversity** by determining the impact of climate conditions and change, pollution, poaching, invasive species, habitat loss, deforestation, human infrastructure (road, power lines, wind turbine) or overexploitation (fishing, hunting) on animal demographic parameters and performance. 

- Question 3: The field of **evolutionary ecology** has resorted to capture-recapture to quantifying life-history trade-offs, estimating heritability of demographic parameters, describing senescence patterns or assessing selection gradients.

- Question 4: In relation to the previous item, **individual heterogeneity** has become the focus of interest - no longer considered as a nuisance parameter only.

- Question 5: The demographic component of interest varies according to animal class: in **birds**, the interest is in studying migration and breeding; in **fish**, folks are more into individual growth, movement and survival; in **marine mammals** and **large carnivores**, it is all about estimating abundance/density using photo-identification, camera-trapping and genetic tagging; 

- Question 6: The field of **disease ecology** is using capture-recapture more and more for estimating prevalence, infection rates, studying transmission, exposure, etc...

- Question 7: Lots of **different organisms** are studied: birds, mammals, fishes, insects, reptiles, amphibians, plants.

- Question 8: How to **manage populations** (conservation, regulation, harvesting, reintroduction) is keeping folks busy: population size and population trends are used as ecological indicators.

- Question 9: Ecologists are concerned about the **effects of marking on animals** and devote efforts to quantify these effects.

- Question 10: Folks are using **genetic information** in new innovative ways to estimate demographic parameters (close-kin method, population assignment) 


Capture-recapture methods
=========================

- Note: By focusing on journals where folks mostly published, we might miss methodological papers in non common journals. For example, Cole and Choquet have published important work on identifiability that does not appear here (advice for young researchers: if you want your work to be seen and hopefully read by colleagues, publish in journals that they’re familiar with; ok, somehow, if the work is useful, it'll get spotted and cited anyway). 

- Method 0: **Non-invasive methods** have revolutionalized the field: photo-identification, acoustic, camera-trapping or genetic tagging. This has stimulated a lot of work on **identification issues** including misidentification, incomplete identification, partial identification, non-identification (dna not amplified, allelic dropout), missed matches (in photo-id).

- Method 1: There has been an uptake of **hidden Markov models (HMM)** (possibly >1st order - memory), and models w/ **hidden
    variables** in general; we can deal w/ complexity and uncertainty, while having great
    flexibility in the modelling by i) distinguishing what we see, what we observe from what is actually going on and ii) 
    decomposing a complex problems in several simpler problems; note that HMM are a special case of **state-space models**, 
    and that multistate/multievent models are HMMs (shall we use only the HMM .
    terminology?). 

- Method 2: **Bayesian analyses** of capture-recapture data are quite common now: we can fit complex models with,
    e.g., random effects or a spatial component, and estimate latent variables (disease or breeding status in multistate 
    models, home ranges (activity centers) in spatial explicit models).

- Method 3: **Spatially-explicit models** are a bid deal, for closed and open population, frequentist and Bayes framework, can help answering many ecological questions (see recent reviews by Clayton Lamb and Andy Royle).

- Method 4: It is not only about individual identification, methods for **unmarked individuals** (N-mixture, occupancy, random-encounter, batch marking, and mark-resight models) are widely used to say something about abundance and demography.

- Method 5: We can now consider **random effects** like in generalized mixed models (time random effects are still difficult to fit in a frequentist framework though).

- Method 6: **Combination of information** makes a lof of sense, i) from different protocols like recaptures, recoveries or telemetry (not to be called 'integrated population models' in my opinion) and ii) using a matrix pop model at its core to combine individual- and population-level information in **integrated population models (IPM)** (not to be confused w/ integral projection models). In passing, I loved Andy Royle's tweet about his visual representation of an IPM:

![](images/raftstack.jpg)

- Method 7: The has been some interest in developing methods for **continuous capture-recapture** to deal w/ opportunistic data.

- Method 8: Folks have spent a lot of energy on the issue of **heterogeneity** (individual, temporal). Often, mixtures or random effects are used if unobserved.

- Method 9: As anticipated by Lebreton et al. (1992), how to properly deal with **covariates** has generated a lot of work (measurement error, missing data, flexible functional form, covariate selection, collinearity, indirect/direct effects).

- Method 10: **Temporary emigration** is a serious concern (combine w/ telemetry data, use robust design, consider unobservable states).

- Method 11: Folks spend a lot of energy the **evaluation of methods** (often via simulations), thinking about assumptions, developing goodness-of-fit procedures.

- Method 12: In relation to the previous item, the **lack of independence between individuals** is of interest, in particular for social species (random effects, social network, sampling design).

- Method 13: There are remarkable efforts made to make methods available to ecologists via the development of computer programs, mainly **R packages**. 


What do we miss? My two cents insights.
=======================================

**Methods**

- Data entry error? Data wrangling, procedures. Everything that happens before the actual analysis?!!!!

- Sampling design: Model- and design-based design (adaptive sampling proposed, power analyses).

- Covariates in non-spatial models. What about in SCR models?

- Visualisation tools - how to get. back to raw data?

- New data technology. Big data (work by S. Bonner) mais aussi small data? Model selection via reduction techniques (Lasso, papier par je sais plus qui qui fait du boosting).

**Ecology**

-  Population dynamics to be coupled with movement ecology,
    ecophysiology, etc; see figure in network paper by Réale in which
    capture-recapture cluster à part… Foster collaboration. Pollock (1981 in JASA): 'Cross-fertilization with human 
    capturerecapture. Cell phone e.g., and movements.' Byron dans Preface au bouquin sur Capture-recapture pour social and medical science 'I would expect this book to facilitate cross-fertilisation of new methods between ecological
and non-ecological areas, for instance in the area of spatial capture recapture.' epidemiology, public health

- Link of population dynamics w/ other disciplines of ecology (functional ecology [see our special issue], quantitative genetics, …).

- What about predictive ecology? Context of global change. Link with mechanistic approaches.

- Faire un parallèle. Avec ce qui se passe dans les survival models en médecine? C’est quoi qui se développe?

- Metanalyses. Of survival. Other demos parameters. Effect of climate on demography?

- Big databases. Comme les trucs de Rob. Citer le truc de Sébastien DEvillard.  

- Reproducibility crisis. Monopole de l’AIC?

- Brian McGill attack on detection; statistical machismo

- Animal welfare? Impact of marking on animals?

- Stable isotope?

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

-   multispecies - ecosystem based approach (SNA at species level), cf Forcada’s paper

-   machine learning

-   social sciences to improve detection (cf papier X Lambin)

-   technology doesn’t have to be about mechanic, organic as well,
    e.g. detection dogs (mettre def technologie, et photo iris ours
    phyrénées)

-   Marking evolves: DNA, cameras, acoustics, drone. Methods need to
    keep up. Wildlife monitoring technology is advancing rapidly.

-   Big data? Citizen science protocols? Improve MCMC - Nimble? Stan?
    TMB?

- Intake of Bayesian analyses. Kéry and Schaub books. Importance of workshops. And software. And small conferences (Euring). Now ISEC. And people. Friendly. Inclusive. 

Random thoughts
==========================

- What I really like w/ capture-recapture (and science in general I guess) is that you can work with folks from (almost) anywhere in the world on basically anything you like: ecology or statistics; terrestrial or marine species; plants (yes!), insects, birds, fishes, mammals (including humans), reptiles, amphibians; conservation biology, population management, behavioral ecology, evolutionary ecology, population dynamics, population genetics, etc.

- New methods need to be evaluated (virtual ecologist approach ie w/ simulation, or empirically) - there is an actual niche there.

- Clearly, workshops are important to disseminate methods, foster new collaborations and build a community.

- A plethora of software: E-SURGE, MARK, marked, etc; we don't necessarily need the mother-of-all (reference to a paper by Matt and Richard) program, diversity is nice, but I guess at some stage we'll need a taxonomy. Also, we need to thank in one way or another and anytime we. have the opportunity fols who invest time in building these tools.

- It might be because I'm getting old, but sometimes I feel like we reinvent the wheel - read the classics: Cormack. Jolly. Seber. Burnham. Lebreton. Arnason. Otis. And many others. It's also an inspiration on how to write. 
    
- When it comes to determine the effect of time-varying covariates (e.g. climatic conditions), we need to remember that we have relatively short time-series and that it might be difficult to detect anything (see review by Morten Frederiksen).

- In relation the previous item, we will never say it often enough: long-term monitoring are so so important (see TREE paper by Sheldon and Clutton-Brock and recent ARES paper)
        
- Methods are getting more and more complex (true?), and this should reflect in the training we give to our students: how to teach spatial statistics, point process, HMM, Bayesian thinking, mixed models on top of everything else?

Useful quotes
=================

> Even the most general mathematical model is a plaything relative to the complexity of an animal population (Cormack 1968 cited in Lebreton et al. 1992).

> Counting fish is just like counting trees — except that they are invisible and keep moving (John Sheperd; [original statement](http://jgshepherd.com/thoughts/) a bit different)

> An applied statistician knowledgeable in capture-recapture should often be consulted for both the design and analysis of capture-recapture or capture-resighting studies, especially if the population issues are complex. (Lebreton et al. 1992)
