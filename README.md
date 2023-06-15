# rrtc
A script to do probability-based relaxed time constraint **(pRTC)**-based dating.

One of the main challenges in molecular dating is the lack of maximum calibrations. To overcome this difficulty, the concept of relative time constrain (RTC) was proposed by using information beyond fossils to provide additional time constraints [1,2] of the **divergence order**, e.g., by forcing the age of a specific node younger than another. This is commonly based on horizontal gene transfer (HGT) that the receipient should originate no earlier than the donor. However, up to now, all approaches that implement RTC treat RTCs as "hard" constraints. This ignores the uncertainty in inference of the divergence order.

This tool named "rrtc" (relaxed RTC) or "prtc" (probability-based RTC) weighted RTCs by their probability inferred by ancestral state reconstruction (ASR) in a **rejection sampling** framework. For example, the last common ancestor (LCA) of symbiont clade 1 may have a probability of 0.7 to be associated with bats, 0.2 with primates, and 0.1 being free-living, as inferred by (ASR). Hence, for any posterior samples of the times inferred by a MCMC molecular dating program, the tool considers the different probabilities of the ancestral hosts (states) and discard the posterior time samples accordingly.

# Installation
Make sure RUBY is installed. Ensure the following RUBY packages have been installed. Otherwise, please use "gem install package_name" to install.
* bio-nwk
* colorize
* parallel
* csv

# Usage
`ruby ~/project/Rhizobiales/scripts/dating/rrtc/do_rrtc.rb --mcmctxt mcmctree/mcmc.txt -i mcmctree/out --rrtc rrtc_folder`
Arguments:
  * `--mcmctxt`: the file "mcmc.txt" generated by MCMCTree
  * `-i`: the file "out" generated by MCMCTree
  * `--rrtc`: the folder that contains the info of all RTCs

The format of the files included in the rrtc_folder should be as follows.

For analysis based on the marginal probability of ancestral states:


`symbiont1,symbiont2 euk1,euk2:0.0037	euk1,euk3:0.9654

symbiont1,symbiont3 euk1,euk2:0.9648	euk1,euk3:0.0350`


The above example indicates that the LCA of sym1 and sym2 is no older than those of eukl & euk2, and euk1 & euk3 (hosts) with a probability of 0.0037 and 0.9654 respectively. You may note that the probabilities in the row do not add up to 1.0, which is because the rest corresponds to the probability of being free-living. You can consider being free-living means the age of the symbiont node should be younder than only the "root" of the tree.

For join probability of ancestral states:

`
sym1,sym2 sym1,sym3

euk1,euk2 euk1,euk2 0.0037

euk1,euk2 euk1,euk3	0.9311

euk1,euk3 euk1,euk2 0.0341

euk1,euk3 root	0.0002

root	euk1,euk2	0.0300

root	euk1,euk3	0.0009
`

The first wor indicates the names of the internal symbiont node which in the above example are sym1&sym2, and sym2&sym3. The remaining rows specify the probabilities that the two symbiont nodes are younger than each of the combinations of the host nodes or being free-living.

# References
1. Davín, A.A., Tannier, E., Williams, T.A. et al. Gene transfers can date the tree of life. Nat Ecol Evol 2, 904–909 (2018). https://doi.org/10.1038/s41559-018-0525-3
2. Magnabosco C, Moore KR, Wolfe JM, Fournier GP. Dating phototrophic microbial lineages with reticulate gene histories. Geobiology. 2018 Mar;16(2):179-89.


# How to cite

# More details about the model
![image](https://github.com/evolbeginner/rrtc/assets/8715751/b2e1cefa-638a-47b6-9fc8-9fcf30f07f5e)
