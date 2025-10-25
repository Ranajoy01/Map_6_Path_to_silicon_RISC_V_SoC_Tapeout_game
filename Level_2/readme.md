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

 ## :trophy: Level Status: 

- All objectives completed.
- I have perfromed floorplan and placement for VsdBabySoC design using OpenROAD.
- :white_check_mark: Map Completed.
  
<div align="center">:star::star::star::star::star::star:</div> 


