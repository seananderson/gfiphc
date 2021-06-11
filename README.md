# gfiphc: An R package for extracting and analysing groundfish data from the IPHC longline survey in BC

<!-- badges: start -->
[![Travis build status](https://travis-ci.com/pbs-assess/gfiphc.svg?branch=master)](https://travis-ci.com/pbs-assess/gfiphc)

<!-- badges: end -->

The International Pacific Halibut Commission (IPHC) conducts an annual stock assessment longline survey in waters from California to Alaska, including British Columbia (BC) waters. The survey's main goal is to provide data on Pacific Halibut (*Hippoglosus stenolepis*) for stock assessment purposes. However, data are also recorded on other species caught, making it the longest ongoing groundfish survey in BC waters and hence a valuable source of data for many species. 

The gfiphc package contains functions for: 

- extracting data from the GFBio (GroundFish Biology) database, housed at the Pacific Biological Station, Nanaimo, BC, Canada
- plotting maps (and movies) showing locations of stations each year
- deriving relative biomass index trends from the IPHC survey for non-halibut groundfish species along the full BC coast, taking into account the changing survey methodologies
- input into the [groundfish synopsis report](https://github.com/pbs-assess/gfsynopsis) for over 100 species of BC groundfish, which is to be updated every couple of years
- deriving relative biomass index trends for a specified area within BC waters, again taking into account the changing survey methodologies. 

The latter example is being used to generate predators of Haida Gwaii Pacific Herring as information for a Case Study in DFO's Ecoystem Approach to Fisheries Management initiative. 

Users outside of PBS can request someone at PBS to extract the data (try Andrew Edwards first) and then the analyses within the package will work. The set-level information (not species specific) is built into the package, as is some species-specific data.

The functions in this package were originally part of gfplot and have now been split out into their own package.

## Vignettes

The vignettes are the best place to start, as they should cover many analyses users would want to do. The links here go to  the saved .html versions on GitHub. The raw Rmarkdown code is available in the [vignettes](vignettes/) folder and should be used as a template.

- [Single species](vignettes/data_for_one_species.Rmd) - shows example data that are saved in the package, and for extracting and generating the longest index possible for a single species for the whole BC coast.
- [Restricted area](vignettes/analysis_for_restricted_area.Rmd) - for extracting and generating the longest index possible for a restricted area of interest.
- [Herring predators](vignettes/analysis_for_HG_herring_predators.Rmd) - extraction of multiple species, and groups of species, for a restricted area. Then saved to be inputs for the Haida Gwaii Pacific Herring Case Study. 
- [All species](vignettes/analysis_all_species.Rmd) - extraction of 113 species, to test updating of the groundfish synopsis report [(Anderson et al. 2019)](https://www.dfo-mpo.gc.ca/csas-sccs/Publications/ResDocs-DocRech/2019/2019_041-eng.html).

## Methods

Methods are described in detail in Appendix G of the groundfish synopsis report [(Anderson et al. 2019)](https://www.dfo-mpo.gc.ca/csas-sccs/Publications/ResDocs-DocRech/2019/2019_041-eng.html). The methods build on those developed for assessments of
Redbanded Rockfsh [(Edwards et al. 2017)](https://www.dfo-mpo.gc.ca/csas-sccs/Publications/ResDocs-DocRech/2017/2017_058-eng.html) and Yelloweye Rockfsh [(Yamanaka et al. 2018)](https://www.dfo-mpo.gc.ca/csas-sccs/Publications/ResDocs-DocRech/2018/2018_001-eng.html) . The Redbanded assessment was the first to develop an abundance index from the IPHC survey that went back to 1995, and included data up to 2012. For the Yelloweye assessment the methods were extended to demonstrate that the index based on waters north of Vancouver Island could be considered representative of the coastwide population. The synopsis report includes preliminary investigations into hook competition.

## Differing data-collection protocols

A particular issue is that the data resolution is not the same across the years. This is explained in Table G.1 in the synopsis report. An updated version of that table is given here:


|Year         |Hooks enumerated          |Data resolution    |Location of data       |WCVI? |
|-------------|--------------------------|-------------------|-----------------------|-------|
|1995         |All                       |Set-by-set         |Spreadsheets           |N      |
|1996         |All                       |Set-by-set         |Spreadsheet            |N      |
|1997-1998    |First 20 of each skate    |Set-by-set         |Spreadsheet            |N      |
|1999         |First 20 of each skate    |Set-by-set         |Spreadsheet            |Y      |
|2000         |First 20 of each skate    |Set-by-set         |Spreadsheet            |N      |
|2001-2002    |First 20 of each skate    |Set-by-set         |Spreadsheet            |Y      |
|2003-2011    |All                       |Hook-by-hook       |DFO database GFBio     |Y      |
|2012         |All (bait experiment)     |Hook-by-hook       |DFO database GFBio     |Y      |
|2013         |First 20 of each skate    |Set-by-set         |Spreadsheet            |Y      |
|2014-2017    |All                       |Hook-by-hook       |DFO database GFBio     |Y      |
|2018         |All (+ expansions stns)   |Hook-by-hook       |DFO database GFBio     |Y      |
|2019         |All                       |Hook-by-hook       |DFO database GFBio     |Y      |
|2020         |First 20 of each skate    |Set-by-set         |TODO                   |Y      |

<!-- for putting back into gfsynopsis, note that 2018 is now separated out) -->
In 2018 there were extra expansion stations surveyed (see the vignettes), and in 2020 only the first 20 hooks were enumerated.

The structure of the data in GFBio is described by [Cooke and Olsen (2020)](https://waves-vagues.dfo-mpo.gc.ca/Library/40879100.pdf), but gfiphc's R functions and built-in SQL queries mean that gfiphc users do not need to have knowledge of GFBio. 

## Summary of Series that can be constructed

Due to the differing data-collection protocols, we developed different ways to obtain as long a time series as possible (Appendix G again). We first defined Series A, B, C, and D in Table G.2 (reproduced below), combined and compared them as possible to define Series AB for the north of West Coast of Vancouver Island (WCVI), and tested whether Series AB could be considered as representing the full coast (see Appendix G for full details). Here we also look at a user-defined restricted area of interest, defining Series E to be based on the first 20 hooks from each skate, and Series F to be based on all hooks from each skate (see the vignette TODO(add link) for details). Series A-F are defined as:

|                                | Only north of WCVI | Full coast | Restricted area of interest |
|--------------------------------|--------------------|------------|-----------------------------|
| First 20 hooks from each skate | A (22)             | D (19)     | E                           |
| All hooks from each skate      | B (17)             | C (15)     | F                           |
 
with numbers in parentheses indicating the number of years for which data for each Series are available. For E and F this depends on the particular area of interest. 

## Updating with new data each year

For 2020, only the first 20 hooks were evaluated from each skate. So, like for 2013, the data have been included in this package. For future years, copy the code `data-raw/iphc-2020-data.Rmd` (and rename for a new year) and follow the instructions for downloading the data from the IPHC website, checking the data, and saving it formatted for this package. The pdf `data-raw/iphc-2020-data.pdf` is also available for easier reading (only commit a final version of future .pdf's, not as you are working on it).

So,

1. Adapt `data-raw/iphc-2020-data.Rmd` as just described. 

1. Create/adapt new functions to include the data in the calculations. For data in GFbio this should be automatic, else see what was done just after commit a5ecd2a for 2020 (may make notes here). 

1. Change `get_all_iphc_set_counts()` to include new year, checking that `get_iphc_from_gfiphc()` works.

1. Re-run (line by line to check) `data-raw/sets-skates-hooks-yelloweye.R` to save some more new data into package.

1. Add lines for new year, `data_names_XXXX` in `check_iphc_spp_name()`.
 
1. Update documentation of data sets in `R/data.R` if necessary.

1. Go through and rerun the vignettes, adapting any called functions or printed results as necessary to include latest year of data, making sure the latest year is included and the maps make sense. 

1. May have to repeat this process as necessary. For example, in 2020 I saved the data, but then kept examining it in `data-raw/iphc-2020-data.Rmd`, realising need to change a `standard` designation in `setDataExpansion`, so then had to rerun all of these steps to update all .rda files. 
 
## Installation

You can install the latest version of the package with:

``` r
devtools::install_github("pbs-assess/gfiphc")
```

## Issues, problems

Please report any problems as a [GitHub Issue](https://github.com/pbs-assess/gfiphc/issues), using a minimal working example if possible (and please check the closed issues first).