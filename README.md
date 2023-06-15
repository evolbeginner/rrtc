# rrtc
A script to do probability-based relaxed time constraint (RTC)-based dating.



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

`
symbiont1,symbiont2 euk1,euk2:0.0037	euk1,euk3:0.9654
symbiont1,symbiont3 euk1,euk2:0.9648	euk1,euk3:0.0350
`

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

