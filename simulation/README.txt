UNIFIED ANALOG COMMAND-CURRENT SUPERVISOR - REV F
Simulation files
=====================================================================

USE
  Keep every file in this folder together. Open Unified_Supervisor.asc
  in LTspice and press Run. The .asc includes UnifiedSupervisor20261028.lib
  and uses the 19 custom .asy symbols in this folder.

ABOUT THE MODELS
  The model library and symbols were rebuilt from the schematic. The
  parts are functional approximations, not vendor models. Three values
  were tuned to reproduce the report's results: LM358 offset, MOSFET Kp
  and LM339 low-level output.

SYMBOLS (pins in LTspice grid units, relative to the SYMBOL origin)
  Supply  V  +(0,0) -(0,128)
  RH      R  A(0,0) B(128,0)          RV  R  A(0,0) B(0,128)
  CV      C  A(0,0) B(0,128)          Coil L A(0,0) B(128,0)
  DH      D  A(0,0) K(128,0)          DV  D  A(0,0) K(0,128)
  USNPN   Q  C(96,0) B(0,64) E(96,128)
  MOS     X  D(96,0) G(0,64) S(96,128)              -> IRLZ44N_APPROX
  Reg     X  IN(0,32) GND(112,128) OUT(224,32)      -> REG7805
  POT     X  TOP(0,0) W(64,80) BOT(0,160)           -> POT10K K=
  Buzzer  X  +(0,0) -(0,128)                        -> BUZZER5
  SPST    X  A(0,0) B(128,0)                        -> SPST MODE=
  SPDT    X  A(0,0) B(0,96) COM(128,48)             -> SPDT MODE=
  LM358   X  DIP-8 order, inputs x=0, outputs x=320, V+(160,-32) GND(160,352)
  LM339   X  DIP-14 order, inputs x=0 at y=64/112/192/240/320/368/448/496,
             outputs x=384 at y=80/208/336/464, V+(192,-32) GND(192,576)
  HC08/HC32  DIP-14, same grid as LM339, VCC(192,-32) GND(192,576)
  HC04       inputs x=0 / outputs x=384 at y=64/144/224/304/384/464,
             VCC(192,-32) GND(192,512)

SWITCH MODES (SPDT: COM-A default, COM-B while active, break-before-make)
  MODE=0 S0  SPST closed throughout
  MODE=1 S1  SPST closed, OPEN 1.5-2.2 s          (open coil)
  MODE=3 S3  SPST open, CLOSED 3.9-4.3 s          (HIGH test)
  MODE=2 S2  SPDT NORMAL, TEST 3.5-4.3 s
  MODE=4 S4  SPDT OFF(0 V), ON(+5 V) 0.5-2.5 s and 3.0-4.6 s
  MODE=5 S5  SPDT C, STUCK(+5 V) 5.0-5.8 s
  Edges take 10 us. Switch Ron 10 mohm, Roff 1 Gohm.

MODEL VALUES
  REG7805     5.000 V, 10 mohm, 2 V dropout, 5 mA Iq
  LM358_DUAL  Aol 1e5, GBW 1 MHz, slew 0.3 V/us, Vos +2.195 mV
  LM339_QUAD  open collector, 66 ohm saturated sink, ~0.3 us response
  HC08/32/04  VCC/2 threshold, 25 ohm output
  IRLZ44N     level-1 core, Vto 1.8 V, Kp 8.3 (~38 mohm at Vgs 5 V)
  LEDs ~2.0/1.9/1.7 V at 3 mA (green/yellow/red); buzzer 200 ohm

LTSPICE 26.0.2 RESULTS (.meas)
  Coil_Normal     0.395632 A       HighTest_Coil   0.395583 A
  Sense_Normal    1.427513 V       V5_Min          4.999514 V
  Delay_Rise      79.07 ms
  Open_Under      4.925 V          LowTest_Under   4.925 V
  HighTest_Over   4.919 V          Stuck_Fault     4.919 V
  Idle_Fault / Normal_Fault / Turnoff_Fault   < 1 nV (no alarm)
  The report's figures (e.g. 0.39565 A, 1.42756 V, 79.08 ms) were
  recorded in LTspice 17.2.4; small differences come from the solver.
  The two "level 1 MOSFET" length/width warnings in the log are
  harmless: only the W/L ratio matters in this model.
