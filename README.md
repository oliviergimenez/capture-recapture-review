Context
=======

-   Invited to the [Wildlife Research and Conservation 2019
    conference](http://www.izw-berlin.de/welcome-234.html) in Berlin,
    and at the time I thought clever to propose a plenary in which I
    would review recent advances in capture-recapture… Now I need to
    actually do something. Session
    [here](http://www.izw-berlin.de/session-recent-advances-in-capture-recapture-studies.html).
    Abstract of the session: *Studying wildlife populations is
    challenging because not all individuals can be captured, identified
    and monitored exhaustively and continuously. Capture-recapture
    methods (CR) are a powerful tool to study individual life-history
    and behavioral traits and connect them to the dynamic of
    free-ranging populations. CR is also key to evolutionary demography
    through the inference of demographic parameters and establishment of
    causal assumptions between biological processes and environment,
    while accounting for individual heterogeneity and imperfect
    detection. In this session, recent applications of CR methods are
    presented, including the study of trade-offs between reproduction
    and survival, dominance and parental care, foraging and
    anti-predation vigilance, resistance and tolerance to pathogens,
    providing a better understanding of changes in population size and
    composition, inter-specific interactions and coevolution as well as
    insights for conservation.*

-   Explain capture-recapture: marking, then recapture (image ours avec
    nvx transparence) - info on abundance and demog parameters

Pitch
=====

-   We won’t need capture-recapture models anymore in 10 years! I will
    make a bet. In 16 years, still there. I’ll be 60 years (damn!).
    Follow advice on storytelling
    [here](http://stephanieschuttler.com/storytelling/).

Analyses
========

I carry out a biliometric exploration in the spirit of [this
paper](https://www.cell.com/trends/ecology-evolution/fulltext/S0169-5347(18)30278-7).
Examples
[here](https://www.researchweaving.com/meta_2/web/index.php?r=site%2Fgraphical-summaries).
See
[here](file:///Users/oliviergimenez/Dropbox/OG/BOULOT/RECHERCHE/CONFERENCES&SEMINAIRES&POSTERS/CONF/IZWBerlin2019/reviewCMR/bibliometrix/bibliometric_analysis.html)
and
[there](file:///Users/oliviergimenez/Dropbox/OG/BOULOT/RECHERCHE/CONFERENCES&SEMINAIRES&POSTERS/CONF/IZWBerlin2019/reviewCMR/bibliometrix/topic_modelling.html)

Trendy
======

Questions
---------

-   Quelles sont les questions? Quelles disciplines concernees?

-   Dispersal w/ Hugo’s review, papers by Jean-Do

-   If interest in climate change, ok, mais attention, nb occz is nb
    annees (cf Frederiksen). Du coup deux reflexions :
    -   long-term monitoring (paper Sheldon et Clutton-Brock. Nichols)
    -   IPM et go for adaptive monitoring et management; learning while
        doing

Methods
-------

-   uptake of HMMs, and models w/ latent variables; turning point w/
    realization of hidden structure (partially-observed process - graph
    of HMM). State-space and HMM. Why? Deal w/ complexity and
    uncertainty, while offering great flexibility by decomposing obs vs
    state!

-   Model fitting, selection and validation (Burnham, White, Choquet)

-   A plethora of software: E-SURGE, MARK, marked, etc

-   Multistate (Lebreton) and multievent (Pradel) are actually HMM
    (particular case of SSM), and I recommend we use this terminology as
    tools are readily available. (Le fils tue le pere 💀)

-   Bayes analysis: we now can fit complex models with, e.g., random
    effects or spatial component and estimate latent variables (disease
    or breeding status in multistate models, home range (activity)
    centers in spatial explicit models)

-   Spatial capture-recapture

-   New methods need to be evaluated - there is an actual niche there

-   Importance of unmarked individuals (unmarked, occupancy,
    mark-resight)

-   Combination of information! IPMs. According to Andy Royle:
    ![](raft.gif)

-   Continuous? Opportunist data? Citizen science data?

-   Importance of workshops to disseminate and form new collab

-   Genetics: see excellent paper by C.T. Lamb
    <a href="https://esajournals.onlinelibrary.wiley.com/doi/full/10.1002/eap.1876" class="uri">https://esajournals.onlinelibrary.wiley.com/doi/full/10.1002/eap.1876</a>
    ; see also review by Andy in Ecography

![](dna_ClaytonTLamb.png)

-   N-mixture

What do we miss
===============

-   Sampling design: Model- and design-based design (adaptive sampling
    proposed, power analyses)
-   A proper treatment of missing and censored data
-   Don’t reinvent the wheel - read your classics: Lebreton. Cormack.
    Jolly. Arnason. Otis.
-   Population dynamics to be coupled with movement ecology,
    ecophysiology, etc; see figure in network paper by Réale in which
    capture-recapture cluster à part… Foster collaboration.

Perspectives
============

-   eDNA
-   multispecies
-   machine learning
-   social sciences to improve detection (cf papier X Lambin)
-   mark-resighting?
-   technology doesn’t have to be about mechanic, organic as well,
    e.g. detection dogs (mettre def technologie, et photo iris ours
    phyrénées)
-   Marking evolves: DNA, cameras, acoustics, drone. Methods need to
    keep up.
-   Big data? Citizen science protocols? Improve MCMC - Nimble? Stan?
    TMB?
-   Voir mon repertoire futur.

Remember to
===========

-   Ask feedback on Twitter (hashtag academictwiiter) and FB (do I miss
    something, who’d like to join if I write a paper?)

-   Refer to talks and posters of the session
    -   Talk 1 : Lucile Marescot (disease ecology, spotted hyenas)
    -   Talk 2 : Antica Culina (evolutionary trade-offs, parental care,
        birds)
    -   Talk 3 : Christophe Barbraud (impact of fishing and climat on
        seabird demography)
    -   Talk 4 : Ana Sanz (foraging, behaviour, birds)
    -   Talk 5 : Pierre Dupont and Cyril Milleret (spatial dynamics of
        large carnivore populations)
    -   026 Pilot estimation of leopard density in the Nama-Karoo
        habitat in Southern Namibia
    -   049 Does site occupancy predict local abundance? A comparison of
        different methods
    -   058 Small mammals as model species to evaluate environmental and
        climatic effects of climate change across European habitat
        heterogeneity
    -   133 Predator-prey relations and density estimations based on
        camera trap data in Bükk National Park, Hungary
    -   161 Estimating density for the endangered Himalayan brown bear
        by integrating non-invasive DNA-sampling and camera traps
-   Show people faces throughout talk, as well as key ref
-   Emphasize a more balanced gender balance in capture-recapture
    literature (true?); in books (Ruth, Rachel, Beth, Elise, …), in
    papers?

What to do with
===============

-   Telemetry
-   The Human Rights Data Analysis Group is a non-profit, non-partisan
    organization that applies rigorous science to the analysis of human
    rights violations around the world. ![](25-Years-Logo.png)
-   Check Rachel’s poster

Gifs
====

![](benaffleck.gif)

See pictures at \[Pixabay\]
