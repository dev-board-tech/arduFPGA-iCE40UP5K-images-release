# arduFPGA-iCE40UP5K-images-release

Precompiled images for uSD and design FLASH

This repository will have the latest stable design FLASH images and uSD memory card tree of designs and applications.

### uSD memory card tree

In order GUI boot-loader to behave as intended there need to fallow a directory tree rule on the uSD memory card.

A practical example is in uSD directory of current repository ( this directory will be updated from time to time).

The directory tree can be as fallow:

```
root --->current.txt  *
      |->des.bin  **
      |->exp.bin  **
      |-> mega ----> arduboy ----> des.bin  ***
      |         |             |--> exp.bin  ***
      |         |             |--> app1.app
      |         |             |--> app1.eep  ****
      |         |             |--> app2.app
      |         |             |--> app2.eep  ****
      |         |
      |         |--> core1 ------> des.bin  ***
      |                       |--> exp.bin  ***
      |                       |--> app1.app
      |                       |--> app2.app
      |
      |-> riscv ---> core1 ------> des.bin  ***
                |             |--> exp.bin  ***
                |             |--> app1.app
                |             |--> app2.app
                |
                |--> core2 ------> des.bin  ***
                              |--> exp.bin  ***
                              |--> app1.app
                              |--> app2.app

```
```
*     Is the file that contain the path of last opened application, this file is on the root directory of
uSD and is used at GUI-boot-loader load to know what directory was opened last time and show the files
inside that directory, the same file is used if the design is updated when an user APP is opened to know
after reset what user application need to load and after power up to directly launch the latest opened
application.

**    Are the design and GUI-boot-loader image files, if both of this files are present on the root
directory of the uSD, after reset, the GUI-boot-loader will ask the user if he want to update the
design and the GUI-boot-loader, after update or if user cancel the update, these files are deleted
from the uSD, so, next time after reset will not ask for update, each file is checked against FLASH
content to see if the FLASH need to be updated, avoiding wearing out the FLASH memory.

***   Are the design and GUI-boot-loader for the applications inside that directory, each time an 
application is opened, the GUI-boot-loader check this files against FLASH content to see if there
is a design update, design change and/or GUI-boot-loader was changed/updated, if only one bit is 
changed in one of the files the des.bin and/or exp.bin are rewritten on the FLASH, the applications
in that directory will always run on the des.bin and exp.bin from that directory.

****  These are the saved EEPROM files of the application with the same name, when opening an application
the GUI-boot-loader will load this file into the FLASH, if the content in  FLASH differs from the uSD one,
from there the first stage boot-loader will load the content from FLASH into the emulated EEPROM from the
design.
      When the user application is interrupted by the user thru INTERRUPT button, the GUI-boot-loader check
      the emulated EEPROM against uSD .eep file content and update the file only if there is a difference
      to avoid wearing out the uSD.
```

### FLASH images

The image contain the design with the prewritten first stage boot-loader inside and the GUI-boot-loader at the precise address where both, first stage-bootloader and GUI-boot-loader expect des.bin and exp.bin to be.

This image can be written directly to the FLASH thru ARDVARK programming header using every SPI interface found on the market or a LATTICE programmer, *** THIS PROCEDURE WILL ERASE THE ENTIRE FLASH CONTENT ***.
