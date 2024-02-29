## FM_ADD

### **Description** 

Fixed Point Addition of Matrix A (Size: FM_COL * FM_ROW) and Matrix B (Size: FM_COL * FM_ROW). Implemented With the DDR4 Naive Interface and Simple Dual-Port BRAMs. 

### **Features**  

1. **Highly Parameterized.**
2. **Already Tested Under Several Scenarios,** Including (FM_COL, FM_ROW) = (8, 4) / (16, 4) / (16, 8) / (64, 4) /(64, 8) /(128, 4), and various fixed point number configurations. In the uploaded code, the author utilizes **signed** 64-bit fixed point number with 15 bits of fraction part. Other configurations are welcome! Btw, You can also change *real_setting* in Vivado wave window and set the *binary points* variable to 15 to verify the results yourself.
3. **Self-Check Results.** I have implemented self-check logics in the code to verify the correctness of addition results automatically. You can refer to the *error_num* signal to check whether error arises.  



### Procedure

1. Generate FM_COL * FM_ROW * 2 fixed point data and write them to DDR
2. Read the first FM_COL * FM_ROW data from DDR to BRAM0, the rest FM_COL * FM_ROW data to BRAM1
3. Read from BRAM0 and BRAM1 simultaneously, perform fixed point addition, write FM_COL * FM_ROW results to BRAM0
4. Read FM_COL * FM_ROW results from BRAM0 and write them to DDR
5. Read FM_COL * FM_ROW results from DDR and check correctness 



### Warning

1. Data tested are generated by *$random*, **NO** real matrix data are tested.
2. The fixed point adder engine **haven't** consider overflow and underflow conditions. In the uploaded version, the author copes with this problem by carefully generating **in-the-range** random numbers.
3. The uploaded version **only** has been tested under behavioral simulation. **No** guarantees in synthesis and post-synthesis simulation.
4. Data size larger than (128, 4) **haven't** been tested.



### Hierarchy

**E1_top**: top module & sim top module

---- **E1_gen_fixed_point**: Fixed point generation for A and B 

---- **E1_ddr_to_out**: Write generated numbers to DDR & Read results from DDR

---- **E1_catch_fixed_point:** Catch results from DDR and write them to a buffer, waiting for comparsion

---- **E1_validate_vadd:** Self-check module

---- **E1_bram_wr_rd:** BRAM top module, including two brams

--------- **E1_BRAM:** BRAM logic 

---- **E1_ddr_to_bram:** DDR vs. BRAM interface controller

--------- **E1_p2s:** Parellel to sequential conversion. Used in BRAM --> DDR

--------- **E1_s2p:** Sequential to parellel conversion. Used in DDR --> BRAM

---- **E1_bram_to_op:** BRAM vs. QADD interface controller

---- **E1_qadd_g:** QADD group

--------- **E1_qadd**: Fixed point adder

---- **E1_ddr_wrapper_top:** DDR4 top instantiation

--------- **E1_ddr_wrapper:** Interface between MIG ip and ddr4_model

--------- **ddr4_model:** Simulation model of DDR4



#### Others:

**ddr4_0.xci: ** DDR4 MIG IP file

**E1_qadd_tb:** Testbench of **E1_qadd** module


For more details, please refer to [https://www.cnblogs.com/georgelin/p/18045313].

*Please feel free to contact if you have any problems* ~😉

