# Dual Core example for STM32H7

Example demonstrating STM32H7 Dual Core setup and running a simple Blinky application.

## Prerequisites

Tools:
 - Keil MDK 5.36
 - Arm Compiler 6
 - bash environment
 - CMSIS-Toolbox 0.9.3

CMISIS packs:
 - ARM::CMSIS
 - Keil::STM32H7xx_DFP@3.0.0


## Step 0 (Subdirectory 0): .uv* based project

Original reference example:
 - copied from STM32H7 DFP
 - using uVision project files
 - working STM32CubeMX integration
 - STM32CubeMX generated code (device configuration)


## Step 1 (Subdirectory 1): .yml based project

Example using CMSIS project .yml files:
 - copied from subdirectory 0
 - uVision workspace file replaced with csolution .yml file (manually constructed)
 - uVision project files replaced with cproject .yml files (manually constructed)
 - STM32CubeMX integration retained
 - removed download/debug initialization files


## Step 1G (Subdirectory 1G): .yml based project without initial generator run

Similar as Step 1 but without initial STM32CubeMX generator run.

Example using CMSIS project .yml files:
 - copied from subdirectory 1
 - removed STM32CubeMX generated files:
   - directory: ./CM4/STM32CubeMX
   - directory: ./CM7/STM32CubeMX
   - directory: ./STM32CubeMX

Change to subdirectory 1G:  
`$ cd 1G`

List generator for CM4 target:  
`$ csolution list generators -s DualCore.csolution.yml -c CM4.Debug+CM4`  
`$ csolution list generators -s DualCore.csolution.yml -c CM4.Release+CM4`  
STM32CubeMX_CM4

List generator for CM7 target:  
`$ csolution list generators -s DualCore.csolution.yml -c CM7.Debug+CM7`  
`$ csolution list generators -s DualCore.csolution.yml -c CM7.Release+CM7`  
STM32CubeMX_CM7

Run generator for a single context (any of the commands below):  
`csolution run -g STM32CubeMX_CM4 -s DualCore.csolution.yml -c CM4.Debug+CM4`  
`csolution run -g STM32CubeMX_CM4 -s DualCore.csolution.yml -c CM4.Release+CM4`  
`csolution run -g STM32CubeMX_CM7 -s DualCore.csolution.yml -c CM7.Debug+CM7`  
`csolution run -g STM32CubeMX_CM7 -s DualCore.csolution.yml -c CM7.Release+CM7`  

STM32CubeMX will generate all the files (CM4, CM7 and common) regardless of the context.

STM32CubeMX launcher issue:  
Launcher cannot handle filenames with multiple '.' (ex: CM4.Debug+CM4.cprj). 
Therefore some files will be created in the drive root directory in subdirectory CM4 and CM7.

Note: .cprj file is currently required by the launcher. Should be changed to .yml file.


## Step 1X (Subdirectory 1X): .yml based future project (proposal)

Proposal for DualCore directory structure (natural and clean concept).

Similar as Step 1 but with different project structure.

Note: not compatible with current STM32CubeMX and integration with MDK

Example using CMSIS project .yml files:
 - copied from subdirectory 1
 - restructured directories:
   - all CM4 target files in CM4 project directory
   - all CM7 target files in CM7 project directory
   - common ST project .ioc file in solution directory
 - STM32CubeMX integration not retained


## Step 2 (Subdirectory 2): .cprj within source tree

Copy from subdirectory 1:  
`$ cp -r 1 2`

Change to subdirectory 2:  
`$ cd 2`

Generate CMSIS project .cprj files using CMSIS Project Manager:  
`$ csolution convert -s DualCore.csolution.yml`

Generated .cprj files:
 - CM4/CM4.Debug+CM4.cprj:
   - Target: Board STM32H747I-EVAL, Device STM32H747XIHx:CM4
   - Build: Debug
 - CM4/CM4.Release+CM4.cprj:
   - Target: Board STM32H747I-EVAL, Device STM32H747XIHx:CM4
   - Build: Release
 - CM7/CM7.Debug+CM7.cprj:
   - Target: Board STM32H747I-EVAL, Device STM32H747XIHx:CM7
   - Build: Debug
 - CM7/CM7.Release+CM7.cprj:
   - Target: Board STM32H747I-EVAL, Device STM32H747XIHx:CM7
   - Build: Release

Note: STM32CubeMX integration retained

CMSIS projects can be built using CMSIS-Build:  
`$ cbuild.sh CM4/CM4.Debug+CM4.cprj`  
`$ cbuild.sh CM4/CM4.Release+CM4.cprj`  
`$ cbuild.sh CM7/CM7.Debug+CM7.cprj`  
`$ cbuild.sh CM7/CM7.Release+CM7.cprj`  

CMSIS projects can also be imported in uVision.  
Following project settings needs to be changed in order to build it with uVision:
 - Options for Target - C/C++ (AC6) - Language: c99
 - Options for Target - C/C++ (AC6) - Warnings: AC5-like Warnings

Note: Debugger is not automatically selected (selection based on board is not working)


## Step 2X (Subdirectory 2X): .cprj within source tree (future project)

Similar as Step 2 but with different project structure (derived from Step 1X).

Copy from subdirectory 1X:  
`$ cp -r 1X 2X`

Change to subdirectory 2X:  
`$ cd 2X`

Generate CMSIS project .cprj files using CMSIS Project Manager:  
`$ csolution convert -s DualCore.csolution.yml`

Note: STM32CubeMX integration not retained

CMSIS projects can be built using CMSIS-Build:  
`$ cbuild.sh CM4/CM4.Debug+CM4.cprj`  
`$ cbuild.sh CM4/CM4.Release+CM4.cprj`  
`$ cbuild.sh CM7/CM7.Debug+CM7.cprj`  
`$ cbuild.sh CM7/CM7.Release+CM7.cprj`  

CMSIS projects can also be imported in uVision.  


## Step 2O (Subdirectory 2O): .cprj out-of source tree

Change to subdirectory 1:  
`$ cd 1`

Generate CMSIS project .cprj files in specified output directory using CMSIS Project Manager:  
`$ csolution convert -s DualCore.csolution.yml -o ../3`

Generated project directories:
 - CM4.Debug+CM4: project for CM4 target (Debug)
 - CM4.Release+CM4: project for CM4 target (Release)
 - CM7.Debug+CM7: project for CM7 target (Debug)
 - CM7.Release+CM7: project for CM7 target (Release)

csolution issues:
 - generator .gpdsc file processing fails which results in generated files not copied

Note: all files need to specified in order to be copied to the output directory

Note: STM32CubeMX integration not retained due to incompatible directory structure


## Step 3 (Subdirectory 3): .yml based project with layers (proposal)

Proposal:
 - `_layer_`: directory with layer repo (eventually part of pack and described in .pdsc)
 - CM4: Project for CM4 target
 - CM7: Project for CM7 target

Layer description example in .pdsc:  
```
<csolution>
  <clayer type="Board" board="STM32H747I-EVAL" name="./CM4/Board/STM32H747I-EVAL/Board.clayer.yml" directory="./_layer_/Board/STM32H747I-EVAL" condition="CM4"/>
  <clayer type="Board" board="STM32H747I-EVAL" name="./CM7/Board/STM32H747I-EVAL/Board.clayer.yml" directory="./_layer_/Board/STM32H747I-EVAL" condition="CM7"/>
</csolution>
```

Composed project should use similar directory structure as Step 2 and is described in Step 4.

Note: `csolution convert` currently cannot be used to generated the desired directory structure


## Step 4 (Subdirectory 4): .cprj for project with layers (proposal)

Composed project from Step 3 (currently manually constructed):
 - find Board clayer from .pdsc (evaluate conditions)
 - create subdirectory <board_directory>=<layer_type>/<layer_board> in project directory
 - copy layer .yml file to <board_directory> subdirectory
 - copy RTE directory (from layer .yml directory) to project directory
 - process .yml file and copy referenced file (retain directory tree)



