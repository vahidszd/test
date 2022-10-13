# FBCM(Fast Beam Conditions Monitor) Simulation With CMSSW
Full CMSSW simulations, including an appropriate model for the front-end electronics, were performed to assess the performance of the proposed system and to optimize some of its parameters. The simulations were implemented in a private branch of CMSSW 11 2 0 pre10, including the FBCM geometry, detector simulation using GEANT4, and a customized digitizer. The results at the GEANT4 level show a good linearity; however, the digitization step describing the front-end electronics creates nonlinear effects in the simulations.
## In order to use this simulation, we need to follow the following steps:
* **Setup workspace for FBCM simulation in CMSSW_11_2_X**
  * Create your CMS_11_2_X
   ```sh
   cd <YourWorkDir>
   cmsrel CMSSW_11_2_0_pre10
   cd CMSSW_11_2_0_pre10/src
   cmsenv
   cd ..
   ```
   *    after setting scram environment variables, please go to release base directory. Copy the [setup          file](https://github.com/m-sedghi/cmssw/blob/CMSSW_11_2_FbcmBcm1f/SimFbcm) to the  CMSSW_11_2_0_pre10 directory and type 
         ```sh
         ./Setup_Fbcm_CMSSW_11_2_X.sh
        ```
   
* **Generating Minibias events with pythia**
   * Go to the  directory where Minibias generating configuration file located
     ```sh
     cd <YourWorkDir>/CMSSW_11_2_0_pre10/src/SimFbcm/SampleConfigs/Fbcm2022Aug
     ls
     ```
      then you can find **MinBias_14TeV_pythia8_TuneCUETP8M1_cfg.py** Python script which generated by 
       ```sh
        cmsDriver.py --evt_type MinBias_14TeV_pythia8_TuneCUETP8M1_cfi -s GEN,SIM --mc --fileout file:MinBias_14TeV_pythia8_TuneCUETP8M1_GEN_SIM.root --conditions auto:phase2_realistic --era Phase2  --datatier GEN-SIM --geometry Extended2026D81 --eventcontent FEVTDEBUG --pileup=NoPileUp --python_filename MinBias_14TeV_pythia8_TuneCUETP8M1_cfg.py --customise Configuration/DataProcessing/Utils.addMonit oring --nThreads 8 -n 40 --no_exe
        ```
       command. Now you can generate your own configuration file with other options than used in abow. For example change the  `--python_filename MinBias_14TeV_pythia8_TuneCUETP8M1_cfg.py` to `--python_filename New_MinBias_14TeV_pythia8_TuneCUETP8M1_cfg.py` to generate another configuration file named **New_MinBias_14TeV_pythia8_TuneCUETP8M1_cfg.py**.
    * Let's test it. Type 
      ```sh
      cmsRun  MinBias_14TeV_pythia8_TuneCUETP8M1_cfg.py
      ```
      to generate MinBias events. It take few minute for the script to do it's job (please be patient). When the script done it's job successfully, you should find a root file in the same directory named **MinBias_14TeV_pythia8_TuneCUETP8M1_GEN_SIM.root** in which the generated MiniBias events stored.
     * We need many of MiniBias events and we can't generate them locally as we have done. We need to submit the **MinBias_14TeV_pythia8_TuneCUETP8M1_cfg.py** to the crab or condor to generate millions of MiniBias events. To do so we need to wright a crab or condor submit script. Here is the sample of the crab submit file which you can find it in  **CMSSW_11_2_0_pre10/src/SimFbcm/SampleConfigs/CrabSamples2SubmitJobs/v0** directory.
       ```py
       from WMCore.Configuration import Configuration
       import os,sys
       config = Configuration()

       config.section_('General')
       config.General.requestName = 'FBCMMinBiasGeneration'
       config.General.workArea = 'crab_projects'
       config.General.transferOutputs = True

       config.section_('JobType')
       config.JobType.pluginName = 'PrivateMC'
       config.JobType.psetName = 'MinBias_14TeV_pythia8_TuneCUETP8M1_cfg.py'
       config.JobType.allowUndistributedCMSSW = True
       config.JobType.maxJobRuntimeMin = 3000
       config.JobType.sendPythonFolder	 = True
       config.JobType.numCores = 8
       config.JobType.maxMemoryMB = 10000
       config.JobType.maxJobRuntimeMin = 5000

       config.section_('Data')
       config.Data.outputPrimaryDataset = 'MinBias'
       config.Data.splitting = 'EventBased'
       config.Data.unitsPerJob = 2000
       config.Data.totalUnits = 1000000 #config.Data.unitsPerJob * NJOBS
       config.Data.publication = True
       config.Data.outputDatasetTag = 'FBCMMinBias'
       config.Data.outLFNDirBase = '/store/group/dpg_bril/comm_bril/phase2-sim/FBCM/'

       config.section_("Site")
       config.Site.storageSite = "T2_CH_CERN"
       ```
      
* **Simulation of bunches with different pileupes by mixing the Minibias events generated in first step**
- 
