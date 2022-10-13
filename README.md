# FBCM(Fast Beam Conditions Monitor) Simulation With CMSSW
Full CMSSW simulations, including an appropriate model for the front-end electronics, were performed to assess the performance of the proposed system and to optimize some of its parameters. The simulations were implemented in a private branch of CMSSW 11 2 0 pre10, including the FBCM geometry, detector simulation using GEANT4, and a customized digitizer. The results at the GEANT4 level show a good linearity; however, the digitization step describing the front-end electronics creates nonlinear effects in the simulations.
## In order to use this simulation, we need to follow the following steps:
* Setup workspace for FBCM simulation in CMSSW_11_2_X
  * Create your CMS_11_2_X
   ```sh
   cd <YourWorkDir>
   cmsrel CMSSW_11_2_0_pre10
   cd CMSSW_11_2_0_pre10/src
   cmsenv
   cd ..
   ```
   *    after setting scram environment variables, please go to release base directory. Copy the [setup          file](https://github.com/m-sedghi/cmssw/blob/CMSSW_11_2_FbcmBcm1f/SimFbcm/Setup_FbcmBcm1F_CMSSW_11_2_X.s) to the  CMSSW_11_2_0_pre10 directory and type 
  ```sh
      ./Setup_Fbcm_CMSSW_11_2_X.sh
     ```
   
- Generating Minibias events with pythia
- Simulation of bunches with different pileupes by mixing the Minibias events generated in first step
- 
