# Protein-Expression-Dynamics
Numerically simulating a protein expression system using first order differential equation from the papers [Global signatures of protein and mRNA expression levels ](https://www.ncbi.nlm.nih.gov/labs/pmc/articles/PMC4089977/) and [The role of mRNA and protein stability in gene expression](https://pubmed.ncbi.nlm.nih.gov/2676679/). 

![differential equation](https://render.githubusercontent.com/render/math?math=%5Cfrac%7BdP%7D%7Bdt%7D%20%3D%20k_%7BProteinProduction%7D%20-%20k_%7BProteinDegradation%7D%5Ctimes%20P&mode=display)

The numerical analysis uses [`solve_ivp`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.solve_ivp.html) (this is so much better than `odeint`) from `SciPy`. First, a simple model, which assumes a constant mRNA, is simulated. Then a more complicated model with a system of differential equations for transcription, mRNA decay and protein expression is solved.

#### Assuming a constant mRNA

An obvious result is the increase of protein levels with the increase of translation rates.

<img src="figs/protein_vs_translation_rates.png" alt="protein vs translation rates"/>

A _not so obvious_ result is that despite the lower translation rates (k_transl), the Spearman's correlation (corr) between mRNA and protein levels will still remain somewhat constant. This simulation uses 1,000 different mRNAs with different concentration and degradation rates.

<img src="figs/protein_vs_translation_rates_for_different_mRNA.png" alt="protein vs translation rates for different mRNA"/>

And the protein at steady state (shown as colour bar below) might be high despite low- _ish_ mRNA levels, if the translation rate is high. Of course, the output maybe limited in real world because of a finite production capacity of cellular systems.

<img src="figs/protein_vs_translation_rates_vs_mRNA_steady_state.png" alt="protein vs translation rates vs mRNA at steady state"/>


#### Including transcription and mRNA decay
This complicates things a bit, because we are now looking at a set of coupled differential equations. First one is the equation for mRNA:

![differential equation mRNA](https://render.githubusercontent.com/render/math?math=%5Cfrac%7BdR%7D%7Bdt%7D%20%3D%20k_%7BTranscription%7D%20-%20k_%7BmRNADegradation%7D%5Ctimes%20R&mode=display)

Followed by equations for protein:

![differential equation protein](https://render.githubusercontent.com/render/math?math=%5Cfrac%7BdP%7D%7Bdt%7D%20%3D%20k_%7BProteinProduction%7D%20-%20k_%7BProteinDegradation%7D%5Ctimes%20P&mode=display)

where,

![protein production rate](https://render.githubusercontent.com/render/math?math=k_%7BProteinProduction%7D%20%3D%20k_%7BTranslation%7D%5Ctimes%20R&mode=display)

These equations are eqn 3 and 1 from the paper [The role of mRNA and protein stability in gene expression](https://pubmed.ncbi.nlm.nih.gov/2676679/).

Both mRNA and protein reach a steady state. 

<img src="figs/decaying_mRNA_and_protein.png" alt="steady state mRNA and protein" />

At steady state, the proportionality factor changes for different translation rates, but the Spearman's correlation still is somewhat constant.

<img src="figs/mRNA_and_protein_with_decay.png" alt="steady state protein and mRNA for different translation rates for a model with mRNA" />
