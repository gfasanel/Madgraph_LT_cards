# Madgraph_LT_cards
https://twiki.cern.ch/twiki/bin/viewauth/CMS/QuickGuideMadGraph5aMCatNLO


```
cd scratch1/
mkdir card_madgraph
cd card_madgraph/
cmsrel CMSSW_8_0_8
cd CMSSW_8_0_8/src
git clone https://github.com/cms-sw/genproductions genproductions
cd genproductions/bin/MadGraph5_aMCatNLO
#./gridpack_generation.sh wplustest_4f_LO cards/examples/wplustest_4f_LO
# this produce a cuts.f, that you can open and modify
#emacs wplustest_4f_LO/wplustest_4f_LO_gridpack/work/MG5_aMC_v2_3_3/Template/LO/SubProcesses/cuts.f
#modify cuts.f to carve your phase space
prepare a directory with a *cuts.f a proc_card.dat and a run_card.dat, like I did here:
```
https://github.com/gfasanel/genproductions/tree/master/bin/MadGraph5_aMCatNLO/cards/production/13TeV/DYJets_LT_cards/DYJets_M50_LT_100to200
```
./submit_gridpack_generation_local.sh 50000 1nw DYJets_M50_LT_75to80 LT_cards/DYJets_M50_LT_75to80 1nw
o meglio ancora, evita di girare in locale perche' prende un sacco di spazio
./submit_gridpack_generation.sh 30000 50000 1nw DYJets_M50_LT_75to80 LT_cards/DYJets_M50_LT_75to80 1nw

cmsenv #not before
#create a directory
mkdir 75_80
#untar tje gridpack INSIDE this directory
cd 75_80
tar -xaf ../DYJets_M50_LT_75to80_tarball.tar.xz
./runcmsgrid.sh 10 123456 2 &> debug.log
#clone the tool by Andrea Massironi to check that the phase space if well carved
cd ..
git clone git@github.com:GiuseppeFasanella/LatinoTreesLHE.git
cd LatinoTreeLHE
#compile it following the README
./ntupleMaker.exe fileLHE outputFileRoot
./ntupleMaker.exe 75_80/cmsgrid_final.lhe output.root

###############################################
Quick test: in the proc card switch off the extra jets and run interactively
cd /afs/cern.ch/work/g/gfasanel/card_madgraph/CMSSW_8_0_8/src/genproductions/bin/MadGraph5_aMCatNLO
 ./gridpack_generation.sh DYJets_M50_LT_75to80 LT_cards/DYJets_M50_LT_75to80/
 tar -xaf DYJets_M50_LT_75to80_tarball.tar.xz
./runcmsgrid.sh 25 123456 2 
./LatinoTreesLHE/ntupleMaker.exe cmsgrid_final.lhe output.root
```
