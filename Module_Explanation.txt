---------------------------------------------
There are 4 modules in the project folder:
---------------------------------------------
1. CACHE_CONTROLLER
2. L1_CACHE_MEMORY
3. L2_CACHE_MEMORY
4. MAIN_MEMORY

------------------------------------------
Brief Description of each module is given below: 
(For more specific information about the modules and flow of control, please refer to the verilog files in "Cache_Controller_Simulation_Project" which have been well commmented 
explaining the entire workflow of the code):
------------------------------------------
1. CACHE_CONTROLLER
This is the main module in the project which contains all the code regarding the operations performed by the Cache Controller.
The policies that has been implemented for the Cache Controller are:
    1. Write-Back Policy
    2. Write No-Allocate
    3. Least Recent used Policy
The Cache controller has been implemented for Direct-Mapped L1 Cache and four-way set-associative L2 Cache.
The size of L1 Cache here is 64 lines, 128 lines for L2 and 16384 lines for Main Memory. Each line in the Cache or main memory contains one or more blocks ,where each block is made up of many bytes.
The byte size is taken to be 8 bits. Each block consists of 4 bytes here, which are indexed using block-offset bits.
Given below is the workflow of the Code in brief:
    1. Checking the mode if read or write. Before taking the input the wait signal is checked to know whether the controller is busy executing the previous instruction.
    2. In READ operation, first the controller searches in the L1 Cache.
        3. If found in L1 Cache, give L1 hit signal as 1 and returns the read data to processor.
        4. If not found in L1 Cache, the address is searched in L2 Cache.
        5. While searching in L2 Cache, a delay of 2 cycles is implemented for the controller. If found in L2 Cache, the address is then promoted in L1 Cache.
        6. While promoting in L1 Cache, if there was some block stored in that location in L1, it needs to evicted to make space for the new block. Also the data of the evicted block needs to updated in L2 (if present in L2) or in Main memory (if not found in L2).
        7. In case the address to be read was not even present in L2 Cache, then the search in main memory begins, there has been a delay of 10 cycles implemented for searching in main memory.
        8. After getting the data from main memory, it needs to promoted in L2 and then to L1 Cache. Now there may be case that its position in L2 is occupied already. Then a block in L2 needs to evicted, whose value needs to be updated in main memory before.
        9. Now the block needs to be promoted to L1 Cache as well which may also be occupied by some block already. This process has been discussed before as well of evicting a block from L1 in point 6.
        10. Finally the data from the address is given to the processor.
    11. In WRITE operation, the address is first searched in L1 Cache.
        12. If found in L1 Cache, the data at address is modified and controller is ready for next instructions by processor.
        13. If not found in L1 Cache, the Cache controller searched in L2 Cache and modifies data there if the address was found.
        14. If not found in L2 Cache, the Cache controller searched in main memory and modifies data there.
        15. Note here in write operation, no promotion to L1 or L2 Cache are made as per the Write No-Allocate policy and hence no evictions in L1 and L2 Cache as well.

2. L1_CACHE_MEMORY
This module contains the implementation of the memory for L1 Cache, which briefly contains the memory array, tag array and the valid array.
It has been initialized in the module as well.

3. L2_CACHE_MEMORY
This module contains the implementation of the memory for L2 Cache, which briefly contains memory array, tag array, LRU array and the valid array.
It has been initialized in the module as well.

4. MAIN_MEMORY
This module contains the implementaton of the memory for main memory, which briefly contains a memory array.
It has been initialized in the module as well.

----------------------
Testbench Details:
----------------------
Please refer to the testbench file "TB_READ" present in "Cache_Controller_Simulation_Project". The test-cases and expected outcomes have been well explained in this file.  
