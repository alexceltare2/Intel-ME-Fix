You esentially need to Dump the BIOS>Modify it in FITC>Flash it back. Prerequisites: ME Analyzer, install plugins for python: "pip3 install colorama" "pip3 install crccheck" "pip3 install pltable", FITC. ME Firmware repositories, fpt. (check them all in win-raid me github)

1. Download/dump the bios.
2. Identify the ME version it contains with ME Analyser.
3. Find the same version number and SKU that matches the bios in the ME Firmware Repository.
4. Rename it to ME Region.bin
5. In fitc.exe, open the dumped BIOS and go to build>build settings and untick "Generate immediate build files"
6. Flash Image > ME Region > Configuration > Features Supported > Intel (R) Anti-Theft Tech Permanently Disabled > Yes
7. Flash Image > Descriptor Region > PCH Straps > PCH Strap 2 > Intel (R) ME SMBus MCTP Address Enable > false
8. Flash Image > Descriptor Region > PCH Straps > PCH Strap 2 > Intel (R) ME SMBus MCTP Address > 0x00
! Descriptor region are not part of ME Region so you need to rebuild the entire dump.
9. Go to "File > Save As" and save the configuration xml file, in this case it's named "config.xml". Afterwards, close the FITC window.
10. In the Intel Flash Image Tool folder is the bios image folder, Open the bios folder name>Decomp>and replace ME Region.bin with the RGN release from step 4.
11. Open FITC and go to File>Open>config.xml from step 9.
12. Click the Build or go to Build > Build Image.
13. You should have a sucessful image build in the FITC folder>Build>outimage.bin
14. Verify by opening the old bios with FITC, then save the XML as before.xml and open the new bios and save as after.xml. Do these while removine the temp files (folders, ini, log...). Nothing should be different than Input files, Intel AT, and the 2 PCH Step 2 configs.
Verify by openning the new bios with ME Analyzer too and make sure is Type is reported as "Region, Extracted" meaning that is configured. 
15. Now you're ready to flash, boi. Flash the new bios.
16. Run Flash Programming Tool with command fpt -greset and wait for the system to reset (no settings are lost). This step is very important because it forces the Engine co-processor to re-initialize and properly accept any changes to its SPI/BIOS image region counterpart. DONE!