# Experiments with Nucleo H743ZI2

## required tools

- ARM GCC Compiler Toolchain
- STM32CubeMX
- STM32CubeProgrammer
 
## What I did
 
- Generating a Makefile Project using STM32CubeMX
- Compiling the project with ARM GCC
- Flashing the board using STM32CubeProg

## GENERATE THE PROJECT SOURCE CODE

The STM32CubeMX tool can be use to generate a Makefile project ready to use. I made one (LEDBlink).

## COMPILING THE PROJECT

From the project folder, we can run `make`.
We might need to specify the location of the ARM GCC Toolchain with `GCC_PATH=/path/to/arm/gcc/`.

The result is a binary: `build/LEDBlink.bin`


## FLASH THE BOARD

### Prob devices

```
 ./STM32_Programmer_CLI -l
      -------------------------------------------------------------------
                        STM32CubeProgrammer v2.6.0                  
      -------------------------------------------------------------------

=====  DFU Interface   =====

No STM32 device in DFU mode connected

===== STLink Interface =====

-------- Connected ST-LINK Probes List --------

ST-Link Probe 0 :
   ST-LINK SN  : 004400323438511134313939
   ST-LINK FW  : V3J5M2
-----------------------------------------------

=====  UART Interface  =====

Total number of serial ports available: 1

Port: ttyACM0
Location: /dev/ttyACM0
Description: STLINK-V3
Manufacturer: STMicroelectronics
```

### Connecting with STM32CubeProg

```
 ./STM32_Programmer_CLI -c port=SWD
      -------------------------------------------------------------------
                        STM32CubeProgrammer v2.6.0                  
      -------------------------------------------------------------------

ST-LINK SN  : 004400323438511134313939
ST-LINK FW  : V3J5M2
Board       : NUCLEO-H743ZI
Voltage     : 3.30V
SWD freq    : 24000 KHz
Connect mode: Normal
Reset mode  : Software reset
Device ID   : 0x450
Revision ID : Rev V
Device name : STM32H7xx
Flash size  : 2 MBytes
Device type : MCU
Device CPU  : Cortex-M7
```

### Flashing the board

You can flash the board with the binary file you compiled (assuming `LEDBlink.bin` is the file).

```
 ./STM32_Programmer_CLI -c port=SWD -d LEDBlink.bin 0x08000000 -s
      -------------------------------------------------------------------
                        STM32CubeProgrammer v2.6.0                  
      -------------------------------------------------------------------

ST-LINK SN  : 004400323438511134313939
ST-LINK FW  : V3J5M2
Board       : NUCLEO-H743ZI
Voltage     : 3.30V
SWD freq    : 24000 KHz
Connect mode: Normal
Reset mode  : Software reset
Device ID   : 0x450
Revision ID : Rev V
Device name : STM32H7xx
Flash size  : 2 MBytes
Device type : MCU
Device CPU  : Cortex-M7



Memory Programming ...
Opening and parsing file: LEDBlink.bin
  File          : LEDBlink.bin
  Size          : 16824 Bytes
  Address       : 0x08000000 


Erasing memory corresponding to segment 0:
Erasing internal memory sector 0
Download in Progress:
[==================================================] 100% 

File download complete
Time elapsed during download operation: 00:00:00.913

RUNNING Program ... 
  Address:      : 0x8000000
Application is running
Start operation achieved successfully
```

Details about the parameters:
- `-c port=SWD`: Connect to the board,
- `-d LEDBlink.bin 0x08000000` : Specify the binary location an the address to start writing to,
- `-s`: Start runing the board right after the operation.
