[Go to Map List of the Game](https://github.com/Ranajoy01/Map_List_Path_to_silicon_RISC_V_SoC_Tapeout_game)

---

[Go to Level List of the Map-6](https://github.com/Ranajoy01/Map_6_Path_to_silicon_RISC_V_SoC_Tapeout_game)

---

[Go to Previous Level](../Level_1/readme.md)

<div align="center">:star::star::star::star::star::star:</div> 

# Level-2: Floorplan and Placement of VsdBabySoC design using OpenROAD-flow-script

## List of Objectives

- :microscope: <b>Practical Objective-1:</b> []()
  
 <div align="center">:star::star::star::star::star::star:</div> 

## :microscope: Setup VSDBabySoC directories for physical design using OpenROAD-flow-script
### :zap: Create two vsdbabysoc directories-
- For Verilog files `~/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc`
- For config, include, macro, lib files `~/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc`

### :zap: Copy the module  directory from VSDBabySoC folder (used in previous weeks) to `~/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc`
- All verilog files (vsdbabysoc.v, rvmyth.v , clk_gate.v) are copied.

### :zap: Copy gds,  include,  layout_conf,  lef,  lib,  sdc directories from VSDBabySoC folder (used in previous weeks) to `~/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc`
- `gds` contain dac and pll gds files
- `include` contain header filess
- `layout_conf` contain macro and pin configuration files
- `lib` contain  sky130 PDK technology library files
- `sdc` contain constraints file

### :zap: Create `config.mk` file in `~/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc` directory with the following content
<details>
<summary><mark>config.mk (Expand this for configuration file content)</mark></summary>
	
```mk
  # Design and Platform Configuration
   export DESIGN_NICKNAME = vsdbabysoc
   export DESIGN_NAME = vsdbabysoc
   export PLATFORM    = sky130hd

  # Design Paths
  export vsdbabysoc_DIR = /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/$(DESIGN_NICKNAME)

  # Explicitly list Verilog files for synthesis
   export VERILOG_FILES = /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/module/vsdbabysoc.v \
                         /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/module/rvmyth.v \
                         /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/module/clk_gate.v


  # Include Directory for Verilog Header Files
   export VERILOG_INCLUDE_DIRS = $(vsdbabysoc_DIR)/include

  # Constraints File
    export SDC_FILE = $(vsdbabysoc_DIR)/sdc/vsdbabysoc_synthesis.sdc

  # Additional GDS Files
    export ADDITIONAL_GDS = $(vsdbabysoc_DIR)/gds/avsddac.gds \
                            $(vsdbabysoc_DIR)/gds/avsdpll.gds

  # Additional LEF Files
   export ADDITIONAL_LEFS = $(vsdbabysoc_DIR)/lef/avsddac.lef \
                            $(vsdbabysoc_DIR)/lef/avsdpll.lef

  # Additional LIB Files
   export ADDITIONAL_LIBS = $(vsdbabysoc_DIR)/lib/avsddac.lib \
                            $(vsdbabysoc_DIR)/lib/avsdpll.lib

 # Pin Order and Macro Placement Configurations
   export FP_PIN_ORDER_CFG = $(vsdbabysoc_DIR)/layout_conf/pin_order.cfg
   export MACRO_PLACEMENT_CFG = $(vsdbabysoc_DIR)/layout_conf/macro.cfg

 # Clock Configuration
   export CLOCK_PORT = CLK
   export CLOCK_NET  = $(CLOCK_PORT)
   export CLOCK_PERIOD = 20.0

# Floorplanning Configuration
  export DIE_AREA   = 0 0 1600 1600
  export CORE_AREA  = 20 20 1590 1590

# Placement Configuration
  export PLACE_PINS_ARGS = -exclude left:0-600 -exclude left:1000-1600 -exclude right:* -exclude top:* -exclude bottom:*

# Tuning for Timing and Buffers
  export TNS_END_PERCENT     = 100
  export REMOVE_ABC_BUFFERS  = 1
  export CTS_BUF_DISTANCE    = 600
  export SKIP_GATE_CLONING   = 1

 # Magic Tool Configuration
   export MAGIC_ZEROIZE_ORIGIN = 0
   export MAGIC_EXT_USE_GDS    = 1
```

</details>

#### config.mk file is important for physical design using OpenROAD-flow-script. It contains-
- Configuration file contains the path for verilog files, constraints, header files, macro .
- Die area and core area.
- Pin and macro placement configuration.
- Clock configuration.
- Timing and buffer configuration.
- Optimization rules.
---

### :zap: Final directory tree structure-

:100: After all these steps the `~/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc` directory will have the structure like this-
```
.
└── module
    ├── a.out
    ├── avsddac.v
    ├── avsdpll.v
    ├── clk_gate.v
    ├── hello.png.pdf
    ├── post_synth.out
    ├── pre_synth_sim.vcd
    ├── pseudo_rand_gen.sv
    ├── pseudo_rand.sv
    ├── rvmyth_gen.v
    ├── rvmyth.tlv
    ├── rvmyth.v
    ├── testbench.rvmyth.post-routing.v
    ├── testbench.v
    ├── vsd_babysoc_net.v
    ├── vsdbabysoc.synth.v
    └── vsdbabysoc.v

```
:100: After all these steps the `~/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc` directory will have the structure like this-
```
.
├── config.mk -> configuration file for vsdbabysoc physical design
├── gds
│   ├── avsddac.gds
│   └── avsdpll.gds
├── include
│   ├── sandpiper_gen.vh
│   ├── sandpiper.vh
│   ├── sp_default.vh
│   └── sp_verilog.vh
├── layout_conf
│   ├── rvmyth
│   │   ├── config.tcl
│   │   └── pin_order.cfg
│   └── vsdbabysoc
│       ├── config.tcl
│       ├── macro.cfg
│       └── pin_order.cfg
├── lef
│   ├── avsddac.lef
│   └── avsdpll.lef
├── lib
│   ├── avsddac.lib -> dac macro technology library
│   ├── avsdpll.lib -> pll macro technology library
│   └── sky130_fd_sc_hd__tt_025C_1v80.lib
└── sdc
    ├── vsdbabysoc_layout.sdc
    └── vsdbabysoc_synthesis.sdc -> this sdc file is used for floorplan and placement

9 directories, 19 files

```

 <div align="center">:star::star::star::star::star::star:</div> 

## :microscope: Floorplan for VSDBabySoC design
### :zap: Perform floorplan and analyze the floorplan log

#### Inside `~/OpenROAD-flow-scripts/flow/` directory run this command-
```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```
- Use the Makefile in flow directory.
- We use vsdbabysoc configuraton file instead of default configuration file.
- `floorplan` .PHONY is ued.(target file is not real but a command name used in Makefile).

#### Analysis of the floorplan.log
We get the following log in the terminal.I have annotated this log file-
<details>
<summary><mark>FLORPLAN LOG (Expand this for the log file content)</mark></summary>
	
```
mkdir -p results/sky130hd/vsdbabysoc/base/
echo 11 > results/sky130hd/vsdbabysoc/base/clock_period.txt
-----------------------------------------------------------------------------------------------------------------------------------------------
----------------------SYNTHESIS START---------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
/home/ranajoy01/OpenROAD-flow-scripts/flow/scripts/synth.sh /home/ranajoy01/OpenROAD-flow-scripts/flow/scripts/synth_canonicalize.tcl ./logs/sky130hd/vsdbabysoc/base/1_1_yosys_canonicalize.log
Using ABC speed script.
Extracting clock period from SDC file: ./results/sky130hd/vsdbabysoc/base/clock_period.txt
Setting clock period to 11
1. Executing Liberty frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
2. Executing Liberty frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsddac.lib
3. Executing Liberty frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsdpll.lib
4. Executing Liberty frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
5. Executing Liberty frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsddac.lib
6. Executing Liberty frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsdpll.lib
7. Executing Verilog-2005 frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/module/vsdbabysoc.v
8. Executing Verilog-2005 frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/module/rvmyth.v
9. Executing Verilog-2005 frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/module/clk_gate.v
10. Executing Verilog-2005 frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/cells_clkgate_hd.v
11. Executing HIERARCHY pass (managing design hierarchy).
12. Executing AST frontend in derive mode using pre-parsed AST for module `\vsdbabysoc'.
12.1. Analyzing design hierarchy..
12.2. Executing AST frontend in derive mode using pre-parsed AST for module `\rvmyth'.
Warning: Replacing memory \CPU_Dmem_value_a4 with list of registers. See /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/module/rvmyth.v:300
Warning: Replacing memory \CPU_Xreg_value_a3 with list of registers. See /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/module/rvmyth.v:284
Warning: Replacing memory \CPU_Imem_instr_a1 with list of registers. See /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/module/rvmyth.v:273
Warning: Replacing memory \CPU_Xreg_value_a5 with list of registers. See /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/module/rvmyth_gen.v:693
Warning: Replacing memory \CPU_Xreg_value_a4 with list of registers. See /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/module/rvmyth_gen.v:692
Warning: Replacing memory \CPU_Dmem_value_a5 with list of registers. See /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/module/rvmyth_gen.v:683
12.3. Analyzing design hierarchy..
12.4. Executing AST frontend in derive mode using pre-parsed AST for module `\clk_gate'.
12.5. Analyzing design hierarchy..
12.6. Analyzing design hierarchy..
13. Executing OPT_CLEAN pass (remove unused cells and wires).
Warning: Ignoring module rvmyth because it contains processes (run 'proc' command first).
14. Executing RTLIL backend.
Warnings: 7 unique messages, 7 total
End of script. Logfile hash: b1f23d6ca1, CPU: user 0.29s system 0.03s, MEM: 52.14 MB peak
Yosys 0.58+35 (git sha1 89f32a415, g++ 13.3.0-6ubuntu2~24.04 -fPIC -O3)
Time spent: 63% 8x read_liberty (0 sec), 17% 1x hierarchy (0 sec), ...
Elapsed time: 0:00.45[h:]min:sec. CPU time: user 0.40 sys 0.04 (99%). Peak memory: 56756KB.
/home/ranajoy01/OpenROAD-flow-scripts/flow/scripts/synth.sh /home/ranajoy01/OpenROAD-flow-scripts/flow/scripts/synth.tcl ./logs/sky130hd/vsdbabysoc/base/1_2_yosys.log
Using ABC speed script.
Extracting clock period from SDC file: ./results/sky130hd/vsdbabysoc/base/clock_period.txt
Setting clock period to 11
1. Executing RTLIL frontend.
2. Executing HIERARCHY pass (managing design hierarchy).
2.1. Analyzing design hierarchy..
2.2. Analyzing design hierarchy..
3. Executing SYNTH pass.
3.1. Executing HIERARCHY pass (managing design hierarchy).
3.1.1. Analyzing design hierarchy..
3.1.2. Analyzing design hierarchy..
3.2. Executing PROC pass (convert processes to netlists).
3.2.1. Executing PROC_CLEAN pass (remove empty switches from decision trees).
3.2.2. Executing PROC_RMDEAD pass (remove dead branches from decision trees).
3.2.3. Executing PROC_PRUNE pass (remove redundant assignments in processes).
3.2.4. Executing PROC_INIT pass (extract init attributes).
3.2.5. Executing PROC_ARST pass (detect async resets in processes).
3.2.6. Executing PROC_ROM pass (convert switches to ROMs).
3.2.7. Executing PROC_MUX pass (convert decision trees to multiplexers).
3.2.8. Executing PROC_DLATCH pass (convert process syncs to latches).
3.2.9. Executing PROC_DFF pass (convert process syncs to FFs).
3.2.10. Executing PROC_MEMWR pass (convert process memory writes to cells).
3.2.11. Executing PROC_CLEAN pass (remove empty switches from decision trees).
3.2.12. Executing OPT_EXPR pass (perform const folding).
3.3. Executing FLATTEN pass (flatten design).
3.4. Executing OPT_EXPR pass (perform const folding).
3.5. Executing OPT_CLEAN pass (remove unused cells and wires).
3.6. Executing CHECK pass (checking for obvious problems).
3.7. Executing OPT pass (performing simple optimizations).
3.7.1. Executing OPT_EXPR pass (perform const folding).
3.7.2. Executing OPT_MERGE pass (detect identical cells).
3.7.3. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
3.7.4. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
3.7.5. Executing OPT_MERGE pass (detect identical cells).
3.7.6. Executing OPT_DFF pass (perform DFF optimizations).
3.7.7. Executing OPT_CLEAN pass (remove unused cells and wires).
3.7.8. Executing OPT_EXPR pass (perform const folding).
3.7.9. Rerunning OPT passes. (Maybe there is more to do..)
3.7.10. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
3.7.11. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
3.7.12. Executing OPT_MERGE pass (detect identical cells).
3.7.13. Executing OPT_DFF pass (perform DFF optimizations).
3.7.14. Executing OPT_CLEAN pass (remove unused cells and wires).
3.7.15. Executing OPT_EXPR pass (perform const folding).
3.7.16. Finished fast OPT passes. (There is nothing left to do.)
3.8. Executing FSM pass (extract and optimize FSM).
3.8.1. Executing FSM_DETECT pass (finding FSMs in design).
3.8.2. Executing FSM_EXTRACT pass (extracting FSM from design).
3.8.3. Executing FSM_OPT pass (simple optimizations of FSMs).
3.8.4. Executing OPT_CLEAN pass (remove unused cells and wires).
3.8.5. Executing FSM_OPT pass (simple optimizations of FSMs).
3.8.6. Executing FSM_RECODE pass (re-assigning FSM state encoding).
3.8.7. Executing FSM_INFO pass (dumping all available information on FSM cells).
3.8.8. Executing FSM_MAP pass (mapping FSMs to basic logic).
3.9. Executing OPT pass (performing simple optimizations).
3.9.1. Executing OPT_EXPR pass (perform const folding).
3.9.2. Executing OPT_MERGE pass (detect identical cells).
3.9.3. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
3.9.4. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
3.9.5. Executing OPT_MERGE pass (detect identical cells).
3.9.6. Executing OPT_DFF pass (perform DFF optimizations).
3.9.7. Executing OPT_CLEAN pass (remove unused cells and wires).
3.9.8. Executing OPT_EXPR pass (perform const folding).
3.9.9. Rerunning OPT passes. (Maybe there is more to do..)
3.9.10. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
3.9.11. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
3.9.12. Executing OPT_MERGE pass (detect identical cells).
3.9.13. Executing OPT_DFF pass (perform DFF optimizations).
3.9.14. Executing OPT_CLEAN pass (remove unused cells and wires).
3.9.15. Executing OPT_EXPR pass (perform const folding).
3.9.16. Rerunning OPT passes. (Maybe there is more to do..)
3.9.17. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
3.9.18. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
3.9.19. Executing OPT_MERGE pass (detect identical cells).
3.9.20. Executing OPT_DFF pass (perform DFF optimizations).
3.9.21. Executing OPT_CLEAN pass (remove unused cells and wires).
3.9.22. Executing OPT_EXPR pass (perform const folding).
3.9.23. Finished fast OPT passes. (There is nothing left to do.)
3.10. Executing WREDUCE pass (reducing word size of cells).
3.11. Executing PEEPOPT pass (run peephole optimizers).
3.12. Executing OPT_CLEAN pass (remove unused cells and wires).
3.13. Executing ALUMACC pass (create $alu and $macc cells).
3.14. Executing SHARE pass (SAT-based resource sharing).
3.15. Executing OPT pass (performing simple optimizations).
3.15.1. Executing OPT_EXPR pass (perform const folding).
3.15.2. Executing OPT_MERGE pass (detect identical cells).
3.15.3. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
3.15.4. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
3.15.5. Executing OPT_MERGE pass (detect identical cells).
3.15.6. Executing OPT_DFF pass (perform DFF optimizations).
3.15.7. Executing OPT_CLEAN pass (remove unused cells and wires).
3.15.8. Executing OPT_EXPR pass (perform const folding).
3.15.9. Rerunning OPT passes. (Maybe there is more to do..)
3.15.10. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
3.15.11. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
3.15.12. Executing OPT_MERGE pass (detect identical cells).
3.15.13. Executing OPT_DFF pass (perform DFF optimizations).
3.15.14. Executing OPT_CLEAN pass (remove unused cells and wires).
3.15.15. Executing OPT_EXPR pass (perform const folding).
3.15.16. Finished fast OPT passes. (There is nothing left to do.)
3.16. Executing MEMORY pass.
3.16.1. Executing OPT_MEM pass (optimize memories).
3.16.2. Executing OPT_MEM_PRIORITY pass (removing unnecessary memory write priority relations).
3.16.3. Executing OPT_MEM_FEEDBACK pass (finding memory read-to-write feedback paths).
3.16.4. Executing MEMORY_BMUX2ROM pass (converting muxes to ROMs).
3.16.5. Executing MEMORY_DFF pass (merging $dff cells to $memrd).
3.16.6. Executing OPT_CLEAN pass (remove unused cells and wires).
3.16.7. Executing MEMORY_SHARE pass (consolidating $memrd/$memwr cells).
3.16.8. Executing OPT_MEM_WIDEN pass (optimize memories where all ports are wide).
3.16.9. Executing OPT_CLEAN pass (remove unused cells and wires).
3.16.10. Executing MEMORY_COLLECT pass (generating $mem cells).
3.17. Executing OPT_CLEAN pass (remove unused cells and wires).
4. Executing SYNTH pass.
4.1. Executing OPT pass (performing simple optimizations).
4.1.1. Executing OPT_EXPR pass (perform const folding).
4.1.2. Executing OPT_MERGE pass (detect identical cells).
4.1.3. Executing OPT_DFF pass (perform DFF optimizations).
4.1.4. Executing OPT_CLEAN pass (remove unused cells and wires).
4.1.5. Rerunning OPT passes. (Removed registers in this run.)
4.1.6. Executing OPT_EXPR pass (perform const folding).
4.1.7. Executing OPT_MERGE pass (detect identical cells).
4.1.8. Executing OPT_DFF pass (perform DFF optimizations).
4.1.9. Executing OPT_CLEAN pass (remove unused cells and wires).
4.1.10. Finished fast OPT passes.
4.2. Executing MEMORY_MAP pass (converting memories to logic and flip-flops).
4.3. Executing OPT pass (performing simple optimizations).
4.3.1. Executing OPT_EXPR pass (perform const folding).
4.3.2. Executing OPT_MERGE pass (detect identical cells).
4.3.3. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
4.3.4. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
4.3.5. Executing OPT_MERGE pass (detect identical cells).
4.3.6. Executing OPT_SHARE pass.
4.3.7. Executing OPT_DFF pass (perform DFF optimizations).
4.3.8. Executing OPT_CLEAN pass (remove unused cells and wires).
4.3.9. Executing OPT_EXPR pass (perform const folding).
4.3.10. Rerunning OPT passes. (Maybe there is more to do..)
4.3.11. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
4.3.12. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
4.3.13. Executing OPT_MERGE pass (detect identical cells).
4.3.14. Executing OPT_SHARE pass.
4.3.15. Executing OPT_DFF pass (perform DFF optimizations).
4.3.16. Executing OPT_CLEAN pass (remove unused cells and wires).
4.3.17. Executing OPT_EXPR pass (perform const folding).
4.3.18. Rerunning OPT passes. (Maybe there is more to do..)
4.3.19. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
4.3.20. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
4.3.21. Executing OPT_MERGE pass (detect identical cells).
4.3.22. Executing OPT_SHARE pass.
4.3.23. Executing OPT_DFF pass (perform DFF optimizations).
4.3.24. Executing OPT_CLEAN pass (remove unused cells and wires).
4.3.25. Executing OPT_EXPR pass (perform const folding).
4.3.26. Rerunning OPT passes. (Maybe there is more to do..)
4.3.27. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
4.3.28. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
4.3.29. Executing OPT_MERGE pass (detect identical cells).
4.3.30. Executing OPT_SHARE pass.
4.3.31. Executing OPT_DFF pass (perform DFF optimizations).
4.3.32. Executing OPT_CLEAN pass (remove unused cells and wires).
4.3.33. Executing OPT_EXPR pass (perform const folding).
4.3.34. Rerunning OPT passes. (Maybe there is more to do..)
4.3.35. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
4.3.36. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
4.3.37. Executing OPT_MERGE pass (detect identical cells).
4.3.38. Executing OPT_SHARE pass.
4.3.39. Executing OPT_DFF pass (perform DFF optimizations).
4.3.40. Executing OPT_CLEAN pass (remove unused cells and wires).
4.3.41. Executing OPT_EXPR pass (perform const folding).
4.3.42. Rerunning OPT passes. (Maybe there is more to do..)
4.3.43. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
4.3.44. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
4.3.45. Executing OPT_MERGE pass (detect identical cells).
4.3.46. Executing OPT_SHARE pass.
4.3.47. Executing OPT_DFF pass (perform DFF optimizations).
4.3.48. Executing OPT_CLEAN pass (remove unused cells and wires).
4.3.49. Executing OPT_EXPR pass (perform const folding).
4.3.50. Rerunning OPT passes. (Maybe there is more to do..)
4.3.51. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
4.3.52. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
4.3.53. Executing OPT_MERGE pass (detect identical cells).
4.3.54. Executing OPT_SHARE pass.
4.3.55. Executing OPT_DFF pass (perform DFF optimizations).
4.3.56. Executing OPT_CLEAN pass (remove unused cells and wires).
4.3.57. Executing OPT_EXPR pass (perform const folding).
4.3.58. Finished fast OPT passes. (There is nothing left to do.)
4.4. Executing TECHMAP pass (map to technology primitives).
4.4.1. Executing Verilog-2005 frontend: /usr/local/bin/../share/yosys/techmap.v
4.4.2. Executing Verilog-2005 frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/common/lcu_kogge_stone.v
4.4.3. Continuing TECHMAP pass.
4.5. Executing OPT pass (performing simple optimizations).
4.5.1. Executing OPT_EXPR pass (perform const folding).
4.5.2. Executing OPT_MERGE pass (detect identical cells).
4.5.3. Executing OPT_DFF pass (perform DFF optimizations).
4.5.4. Executing OPT_CLEAN pass (remove unused cells and wires).
4.5.5. Rerunning OPT passes. (Removed registers in this run.)
4.5.6. Executing OPT_EXPR pass (perform const folding).
4.5.7. Executing OPT_MERGE pass (detect identical cells).
4.5.8. Executing OPT_DFF pass (perform DFF optimizations).
4.5.9. Executing OPT_CLEAN pass (remove unused cells and wires).
4.5.10. Rerunning OPT passes. (Removed registers in this run.)
4.5.11. Executing OPT_EXPR pass (perform const folding).
4.5.12. Executing OPT_MERGE pass (detect identical cells).
4.5.13. Executing OPT_DFF pass (perform DFF optimizations).
4.5.14. Executing OPT_CLEAN pass (remove unused cells and wires).
4.5.15. Finished fast OPT passes.
4.6. Executing ABC pass (technology mapping using ABC).
4.6.1. Extracting gate netlist of module `\vsdbabysoc' to `<abc-temp-dir>/input.blif'..
4.7. Executing OPT pass (performing simple optimizations).
4.7.1. Executing OPT_EXPR pass (perform const folding).
4.7.2. Executing OPT_MERGE pass (detect identical cells).
4.7.3. Executing OPT_DFF pass (perform DFF optimizations).
4.7.4. Executing OPT_CLEAN pass (remove unused cells and wires).
4.7.5. Finished fast OPT passes.
4.8. Executing HIERARCHY pass (managing design hierarchy).
4.8.1. Analyzing design hierarchy..
4.8.2. Analyzing design hierarchy..
4.9. Printing statistics.
4.10. Executing CHECK pass (checking for obvious problems).
5. Executing OPT pass (performing simple optimizations).
5.1. Executing OPT_EXPR pass (perform const folding).
5.2. Executing OPT_MERGE pass (detect identical cells).
5.3. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
5.4. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
5.5. Executing OPT_MERGE pass (detect identical cells).
5.6. Executing OPT_DFF pass (perform DFF optimizations).
5.7. Executing OPT_CLEAN pass (remove unused cells and wires).
5.8. Executing OPT_EXPR pass (perform const folding).
5.9. Rerunning OPT passes. (Maybe there is more to do..)
5.10. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
5.11. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
5.12. Executing OPT_MERGE pass (detect identical cells).
5.13. Executing OPT_DFF pass (perform DFF optimizations).
5.14. Executing OPT_CLEAN pass (remove unused cells and wires).
5.15. Executing OPT_EXPR pass (perform const folding).
5.16. Finished fast OPT passes. (There is nothing left to do.)
6. Executing EXTRACT_FA pass (find and extract full/half adders).
7. Executing TECHMAP pass (map to technology primitives).
7.1. Executing Verilog-2005 frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/cells_adders_hd.v
7.2. Continuing TECHMAP pass.
8. Executing TECHMAP pass (map to technology primitives).
8.1. Executing Verilog-2005 frontend: /usr/local/bin/../share/yosys/techmap.v
8.2. Continuing TECHMAP pass.
9. Executing OPT pass (performing simple optimizations).
9.1. Executing OPT_EXPR pass (perform const folding).
9.2. Executing OPT_MERGE pass (detect identical cells).
9.3. Executing OPT_DFF pass (perform DFF optimizations).
9.4. Executing OPT_CLEAN pass (remove unused cells and wires).
9.5. Finished fast OPT passes.
10. Executing TECHMAP pass (map to technology primitives).
10.1. Executing Verilog-2005 frontend: /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/cells_latch_hd.v
10.2. Continuing TECHMAP pass.
11. Executing DFFLIBMAP pass (mapping DFF cells to sequential cells from liberty file).
11.1. Executing DFFLEGALIZE pass (convert FFs to types supported by the target).
12. Executing OPT pass (performing simple optimizations).
12.1. Executing OPT_EXPR pass (perform const folding).
12.2. Executing OPT_MERGE pass (detect identical cells).
12.3. Executing OPT_MUXTREE pass (detect dead branches in mux trees).
12.4. Executing OPT_REDUCE pass (consolidate $*mux and $reduce_* inputs).
12.5. Executing OPT_MERGE pass (detect identical cells).
12.6. Executing OPT_DFF pass (perform DFF optimizations).
12.7. Executing OPT_CLEAN pass (remove unused cells and wires).
12.8. Executing OPT_EXPR pass (perform const folding).
12.9. Finished fast OPT passes. (There is nothing left to do.)
13. Executing SETUNDEF pass (replace undef values with defined constants).
abc -script /home/ranajoy01/OpenROAD-flow-scripts/flow/scripts/abc_speed.script -liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsddac.lib -liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsdpll.lib -dont_use sky130_fd_sc_hd__probe_p_8 -dont_use sky130_fd_sc_hd__probec_p_8 -dont_use sky130_fd_sc_hd__lpflow_bleeder_1 -dont_use sky130_fd_sc_hd__lpflow_clkbufkapwr_1 -dont_use sky130_fd_sc_hd__lpflow_clkbufkapwr_16 -dont_use sky130_fd_sc_hd__lpflow_clkbufkapwr_2 -dont_use sky130_fd_sc_hd__lpflow_clkbufkapwr_4 -dont_use sky130_fd_sc_hd__lpflow_clkbufkapwr_8 -dont_use sky130_fd_sc_hd__lpflow_clkinvkapwr_1 -dont_use sky130_fd_sc_hd__lpflow_clkinvkapwr_16 -dont_use sky130_fd_sc_hd__lpflow_clkinvkapwr_2 -dont_use sky130_fd_sc_hd__lpflow_clkinvkapwr_4 -dont_use sky130_fd_sc_hd__lpflow_clkinvkapwr_8 -dont_use sky130_fd_sc_hd__lpflow_decapkapwr_12 -dont_use sky130_fd_sc_hd__lpflow_decapkapwr_3 -dont_use sky130_fd_sc_hd__lpflow_decapkapwr_4 -dont_use sky130_fd_sc_hd__lpflow_decapkapwr_6 -dont_use sky130_fd_sc_hd__lpflow_decapkapwr_8 -dont_use sky130_fd_sc_hd__lpflow_inputiso0n_1 -dont_use sky130_fd_sc_hd__lpflow_inputiso0p_1 -dont_use sky130_fd_sc_hd__lpflow_inputiso1n_1 -dont_use sky130_fd_sc_hd__lpflow_inputiso1p_1 -dont_use sky130_fd_sc_hd__lpflow_inputisolatch_1 -dont_use sky130_fd_sc_hd__lpflow_isobufsrc_1 -dont_use sky130_fd_sc_hd__lpflow_isobufsrc_16 -dont_use sky130_fd_sc_hd__lpflow_isobufsrc_2 -dont_use sky130_fd_sc_hd__lpflow_isobufsrc_4 -dont_use sky130_fd_sc_hd__lpflow_isobufsrc_8 -dont_use sky130_fd_sc_hd__lpflow_isobufsrckapwr_16 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_hl_isowell_tap_1 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_hl_isowell_tap_2 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_hl_isowell_tap_4 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_isowell_4 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_isowell_tap_1 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_isowell_tap_2 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_isowell_tap_4 -constr ./objects/sky130hd/vsdbabysoc/base/abc.constr -D 11
14. Executing ABC pass (technology mapping using ABC).
14.1. Extracting gate netlist of module `\vsdbabysoc' to `<abc-temp-dir>/input.blif'..
14.1.1. Executed ABC.
14.1.2. Re-integrating ABC results.
Took 6 seconds: abc -script /home/ranajoy01/OpenROAD-flow-scripts/flow/scripts/abc_speed.script -liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsddac.lib -liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsdpll.lib -dont_use sky130_fd_sc_hd__probe_p_8 -dont_use sky130_fd_sc_hd__probec_p_8 -dont_use sky130_fd_sc_hd__lpflow_bleeder_1 -dont_use sky130_fd_sc_hd__lpflow_clkbufkapwr_1 -dont_use sky130_fd_sc_hd__lpflow_clkbufkapwr_16 -dont_use sky130_fd_sc_hd__lpflow_clkbufkapwr_2 -dont_use sky130_fd_sc_hd__lpflow_clkbufkapwr_4 -dont_use sky130_fd_sc_hd__lpflow_clkbufkapwr_8 -dont_use sky130_fd_sc_hd__lpflow_clkinvkapwr_1 -dont_use sky130_fd_sc_hd__lpflow_clkinvkapwr_16 -dont_use sky130_fd_sc_hd__lpflow_clkinvkapwr_2 -dont_use sky130_fd_sc_hd__lpflow_clkinvkapwr_4 -dont_use sky130_fd_sc_hd__lpflow_clkinvkapwr_8 -dont_use sky130_fd_sc_hd__lpflow_decapkapwr_12 -dont_use sky130_fd_sc_hd__lpflow_decapkapwr_3 -dont_use sky130_fd_sc_hd__lpflow_decapkapwr_4 -dont_use sky130_fd_sc_hd__lpflow_decapkapwr_6 -dont_use sky130_fd_sc_hd__lpflow_decapkapwr_8 -dont_use sky130_fd_sc_hd__lpflow_inputiso0n_1 -dont_use sky130_fd_sc_hd__lpflow_inputiso0p_1 -dont_use sky130_fd_sc_hd__lpflow_inputiso1n_1 -dont_use sky130_fd_sc_hd__lpflow_inputiso1p_1 -dont_use sky130_fd_sc_hd__lpflow_inputisolatch_1 -dont_use sky130_fd_sc_hd__lpflow_isobufsrc_1 -dont_use sky130_fd_sc_hd__lpflow_isobufsrc_16 -dont_use sky130_fd_sc_hd__lpflow_isobufsrc_2 -dont_use sky130_fd_sc_hd__lpflow_isobufsrc_4 -dont_use sky130_fd_sc_hd__lpflow_isobufsrc_8 -dont_use sky130_fd_sc_hd__lpflow_isobufsrckapwr_16 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_hl_isowell_tap_1 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_hl_isowell_tap_2 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_hl_isowell_tap_4 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_isowell_4 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_isowell_tap_1 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_isowell_tap_2 -dont_use sky130_fd_sc_hd__lpflow_lsbuf_lh_isowell_tap_4 -constr ./objects/sky130hd/vsdbabysoc/base/abc.constr -D 11
15. Executing SPLITNETS pass (splitting up multi-bit signals).
16. Executing OPT_CLEAN pass (remove unused cells and wires).
17. Executing HILOMAP pass (mapping to constant drivers).
18. Executing INSBUF pass (insert buffer cells for connected wires).
19. Executing CHECK pass (checking for obvious problems).
20. Printing statistics.
21. Executing CHECK pass (checking for obvious problems).
22. Executing Verilog backend.
22.1. Executing BMUXMAP pass.
22.2. Executing DEMUXMAP pass.
exec cp /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/sdc/vsdbabysoc_synthesis.sdc ./results/sky130hd/vsdbabysoc/base/1_synth.sdc
End of script. Logfile hash: 8ec147a303, CPU: user 5.09s system 0.07s, MEM: 72.62 MB peak
Yosys 0.58+35 (git sha1 89f32a415, g++ 13.3.0-6ubuntu2~24.04 -fPIC -O3)
Time spent: 56% 2x abc (6 sec), 10% 34x opt_clean (1 sec), ...
Elapsed time: 0:11.45[h:]min:sec. CPU time: user 11.28 sys 0.17 (100%). Peak memory: 75812KB.
mkdir -p ./results/sky130hd/vsdbabysoc/base ./logs/sky130hd/vsdbabysoc/base ./reports/sky130hd/vsdbabysoc/base
cp ./results/sky130hd/vsdbabysoc/base/1_2_yosys.v ./results/sky130hd/vsdbabysoc/base/1_synth.v
-----------------------------------------------------------------------------------------------------------------------------------------------
----------------------SYNTHESIS END---------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
---------------------FLOORPLAN START---------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
/home/ranajoy01/OpenROAD-flow-scripts/flow/scripts/flow.sh 2_1_floorplan floorplan
Running floorplan.tcl, stage 2_1_floorplan
read_liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsddac.lib
read_liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsdpll.lib
[INFO ODB-0227] LEF file: /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/lef/sky130_fd_sc_hd.tlef, created 13 layers, 25 vias
[INFO ODB-0227] LEF file: /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/lef/sky130_fd_sc_hd_merged.lef, created 441 library cells
[WARNING ODB-0220] WARNING (LEFPARS-2008): NOWIREEXTENSIONATPIN statement is obsolete in version 5.6 or later.
The NOWIREEXTENSIONATPIN statement will be ignored. See file /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lef/avsddac.lef at line 2.

[INFO ODB-0227] LEF file: /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lef/avsddac.lef, created 1 library cells
[WARNING ODB-0220] WARNING (LEFPARS-2008): NOWIREEXTENSIONATPIN statement is obsolete in version 5.6 or later.
The NOWIREEXTENSIONATPIN statement will be ignored. See file /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lef/avsdpll.lef at line 2.

[WARNING ODB-0186] macro avsdpll references unknown site unithddb1
[INFO ODB-0227] LEF file: /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lef/avsdpll.lef, created 1 library cells
link_design vsdbabysoc
Master sky130_ef_sc_hd__decap_12 is loaded but not used in the design

==========================================================================
Floorplan check_setup
--------------------------------------------------------------------------
Warning: There are 7 input ports missing set_input_delay.
Warning: There is 1 output port missing set_output_delay.
Warning: There are 2 unconstrained endpoints.
-----------------------------------------------------------------------------------------------------------------------------------------------
--------------Instances in verilog design---------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
number instances in verilog is 6605
[WARNING IFP-0028] Core area lower left (20.000, 20.000) snapped to (20.240, 21.760).
---------------------------------------------------------------------------------------------------------------------------------------------
-----------No of rows and site----------------------------
----------------------------------------------------------------------------------------------------------------------------------------------
[INFO IFP-0001] Added 576 rows of 3412 site unithd.
source /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/make_tracks.tcl
source /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/fastroute.tcl
Repair tie lo fanout...
[INFO RSZ-0042] Inserted 4 tie sky130_fd_sc_hd__conb_1 instances.
Repair tie hi fanout...
[INFO RSZ-0026] Removed 595 buffers.
Default units for flow
 time 1ns
 capacitance 1pF
 resistance 1kohm
 voltage 1v
 current 1mA
 power 1nW
 distance 1um
Report metrics stage 2, floorplan final...

==========================================================================
floorplan final report_design_area
--------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
--------------Design area utilization-------------
-----------------------------------------------------------------------------------------------------------------------------------------------
Design area 722267 um^2 29% utilization.
Elapsed time: 0:03.58[h:]min:sec. CPU time: user 3.50 sys 0.70 (117%). Peak memory: 141124KB.
Log                        Elapsed/s Peak Memory/MB  sha1sum .odb [0:20)
2_1_floorplan                      3            137 a8e0dc991bf12e8968c5
/home/ranajoy01/OpenROAD-flow-scripts/flow/scripts/flow.sh 2_2_floorplan_macro macro_place
Running macro_place.tcl, stage 2_2_floorplan_macro
read_liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsddac.lib
read_liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsdpll.lib
read_db ./results/sky130hd/vsdbabysoc/base/2_1_floorplan.odb
rtl_macro_placer -max_num_level 2 -halo_width 40 -halo_height 40 -min_ar 0.33 -signature_net_threshold 50 -area_weight 0.1 -wirelength_weight 100.0 -outline_weight 100.0 -boundary_weight 50.0 -notch_weight 10.0 -target_dead_space 0.05 -report_directory ./objects/sky130hd/vsdbabysoc/base/rtlmp -fence_lx 0.0 -fence_ly 0.0 -fence_ux 100000000.0 -fence_uy 100000000.0 -target_util 0.6
Die Area: (0.00, 0.00) (1600.00, 1600.00),  Floorplan Area: (20.24, 21.76) (1589.76, 1588.48)
-----------------------------------------------------------------------------------------------------------------------------------------------
 -------------------Standard cell Macros-------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
	Number of std cell instances: 6011
	Area of std cell instances: 50614.96
	Number of macros: 2
	Area of macros: 671652.31
	Halo width: 40.00
	Halo height: 40.00
	Area of macros with halos: 828913.94
	Area of std cell instances + Area of macros: 722267.25
	Floorplan area: 2458998.25
	Design Utilization: 0.29
	Floorplan Utilization: 0.03
	Manufacturing Grid: 5

[WARNING MPL-0002] Couldn't align pin dac/D[0] from layer li1 to the track-grid.
[WARNING MPL-0002] Couldn't align pin dac/D[1] from layer li1 to the track-grid.
[WARNING MPL-0002] Couldn't align pin dac/D[2] from layer li1 to the track-grid.
[WARNING MPL-0002] Couldn't align pin dac/D[4] from layer li1 to the track-grid.
[WARNING MPL-0002] Couldn't align pin dac/D[5] from layer li1 to the track-grid.
[WARNING MPL-0002] Couldn't align pin dac/D[6] from layer li1 to the track-grid.
[WARNING MPL-0002] Couldn't align pin dac/D[7] from layer li1 to the track-grid.
[WARNING MPL-0002] Couldn't align pin dac/D[8] from layer li1 to the track-grid.
[WARNING MPL-0002] Couldn't align pin dac/D[9] from layer li1 to the track-grid.
[WARNING MPL-0002] Couldn't align pin pll/CLK from layer li1 to the track-grid.
[WARNING MPL-0002] Couldn't align pin pll/ENb_CP from layer li1 to the track-grid.
[WARNING MPL-0002] Couldn't align pin pll/ENb_VCO from layer li1 to the track-grid.
[WARNING MPL-0002] Couldn't align pin pll/REF from layer li1 to the track-grid.
Took 22 seconds: rtl_macro_placer -max_num_level 2 -halo_width 40 -halo_height 40 -min_ar 0.33 -signature_net_threshold 50 -area_weight 0.1 -wirelength_weight 100.0 -outline_weight 100.0 -boundary_weight 50.0 -notch_weight 10.0 -target_dead_space 0.05 -report_directory ./objects/sky130hd/vsdbabysoc/base/rtlmp -fence_lx 0.0 -fence_ly 0.0 -fence_ux 100000000.0 -fence_uy 100000000.0 -target_util 0.6
Elapsed time: 0:23.18[h:]min:sec. CPU time: user 33.64 sys 26.70 (260%). Peak memory: 168092KB.
Log                        Elapsed/s Peak Memory/MB  sha1sum .odb [0:20)
2_2_floorplan_macro               23            164 ed43eed0ebb4b136e599
-----------------------------------------------------------------------------------------------------------------------------------------------
----------------TAPCELL----------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
/home/ranajoy01/OpenROAD-flow-scripts/flow/scripts/flow.sh 2_3_floorplan_tapcell tapcell
Running tapcell.tcl, stage 2_3_floorplan_tapcell
read_liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsddac.lib
read_liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsdpll.lib
read_db ./results/sky130hd/vsdbabysoc/base/2_2_floorplan_macro.odb
[INFO ODB-0303] The initial 576 rows (1965312 sites) were cut with 2 shapes for a total of 793 rows (1420372 sites).
[INFO TAP-0005] Inserted 23780 tapcells.
Elapsed time: 0:00.86[h:]min:sec. CPU time: user 0.79 sys 0.06 (99%). Peak memory: 125992KB.
Log                        Elapsed/s Peak Memory/MB  sha1sum .odb [0:20)
2_3_floorplan_tapcell              0            123 4bd4213c751fb968c07c
-----------------------------------------------------------------------------------------------------------------------------------------------
------------------POWER DISTRIBUTION NETWORK---------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
/home/ranajoy01/OpenROAD-flow-scripts/flow/scripts/flow.sh 2_4_floorplan_pdn pdn
Running pdn.tcl, stage 2_4_floorplan_pdn
read_liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/platforms/sky130hd/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsddac.lib
read_liberty /home/ranajoy01/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsdpll.lib
read_db ./results/sky130hd/vsdbabysoc/base/2_3_floorplan_tapcell.odb
[WARNING PDN-0189] Supply pin pll/GND is not connected to any net.
[WARNING PDN-0189] Supply pin pll/GND#2 is not connected to any net.
[WARNING PDN-0189] Supply pin pll/VDD#2 is not connected to any net.
[WARNING PDN-0189] Supply pin pll/VDD#3 is not connected to any net.
[INFO PDN-0001] Inserting grid: grid
[INFO PDN-0001] Inserting grid: CORE_macro_grid_1 - dac
[INFO PDN-0001] Inserting grid: CORE_macro_grid_1 - pll
Elapsed time: 0:02.68[h:]min:sec. CPU time: user 2.55 sys 0.13 (100%). Peak memory: 210532KB.
Log                        Elapsed/s Peak Memory/MB  sha1sum .odb [0:20)
2_4_floorplan_pdn                  2            205 2658412dd4617b4200aa
cp ./results/sky130hd/vsdbabysoc/base/2_4_floorplan_pdn.odb ./results/sky130hd/vsdbabysoc/base/2_floorplan.odb
cp ./results/sky130hd/vsdbabysoc/base/2_1_floorplan.sdc ./results/sky130hd/vsdbabysoc/base/2_floorplan.sdc
-----------------------------------------------------------------------------------------------------------------------------------------------
----------------------FLOORPLAN END---------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------
```
</details>
- If synthesis is not done before then synthesis is performed before floorplan.
- Die and core area is defined.
- In this log we can see at first synthesis part is performed using yosys.
- Then Instance and macro area calculation done.
- Then macros are positioned on core.
  - Here are 2 macros - `dac` and `pll` (analog IPs)
- Then Tapcells are positioned on core (Tapcells are essential in CMOS physical design to ensure electrical reliability and prevent destructive effects such as latch-up).
-  Power distribution network is positioned on core.

#### :100: So floorplan perform-
- Die and core generation.
- Instance and macro check and macro positioning.
- Tapcell positioning.
- Area utilization.
- Power Distribution network (PDN) positioning.


 <div align="center">:star::star::star::star::star::star:</div> 

 ## :trophy: Level Status: 

- All objectives completed.
- I have perfromed floorplan and placement for VsdBabySoC design using OpenROAD.
- :white_check_mark: Map Completed.
  
<div align="center">:star::star::star::star::star::star:</div> 


