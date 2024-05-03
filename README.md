
Lenovo TB-X605F Rooting and Flashing Guide
---------------------------------------------------------------------------------
Note: If tablet ever gets stuck booting. Hold Vol Down + Vol up + Power until it resets. Mine did lock up on a few reboots during the process.

Install Lenovo Drivers
System > About > tap build number 10 times to enable developer options
System > Developer Options > Enable USB Debugging
				> Disable Verify Apps Over ADB
				> Enable OEM Unlocking
Security Settings > Remove Device Lock (PIN, fingerprint, whatever you are using) and set it to NONE.

1. Unlock Bootloader:
```
adb reboot bootloader
fastboot devices (make sure its found)
fastboot getvar info (optional)
fastboot oem unlock-go
```

!!!-- Device will reboot and reset --!!!

If above fails. Use RSA to 'recover' the tablet by flashing the lastest stock rom. I flashed TB_X605F_S210273_221107_ROW Did not note previous version but IIRC had a build date of 2019? Tablet was bought new around 2019 and never used until 2024
Note: Can copy TB_X605F_S210273_221107_ROW into C:\ProgramData\RSA\Download\RomFiles to skip downloading twice if you already downloaded both archives.

Note 2: I was able to enable OEM Unlocking in Developer Tools on my previous rom. However the unlock commands would not work, tried all iterations. Command not found. Flashed TB_X605F_S210273_221107_ROW and OEM Unlock was off and greyed out. Checked devinfo with BLUnlocker, and hex was set to be unlocked (flashed it back anyways). Developer Tools still showed off and greyed out. Rebooted to bootloader, ran fastboot oem unlock-go, and unlocked successfully. 

See appendix for information on BLUnlocker

2. Flash TWRP (recovery.img in TWRP-3.3.1_Lenovo-TBX605F)
```
adb reboot bootloader
fastboot flash recovery twrp.img (recovery.img in TWRP archive)
fastboot reboot
```

!!!-- Device will reboot --!!!

-------------------------------------------------------------------------------------------------------------
Option A - Magisk root by patching your boot.img
-------------------------------------------------------------------------------------------------------------

```
adb push boot.img /mnt/sdcard/Download/boot.img
adb install Magisk-v26.0.apk
```

- Open Magisk and click 'Install' top right
- Choose file to patch
- Navigate to Download and select boot.img
- Magisk will patch the boot.img and save as magisk_patched-XXXXXX.img
  
  Replace magisk_patched-XXXXX.img with your file name

```
adb pull /storage/emulated/0/Download/magisk_patched-XXXXX.img
adb reboot bootloader
```

Flash the patched boot image

```
fastboot flash boot magisk_patched-XXXXX.img
```



-------------------------------------------------------------------------------------------------------------
Option B - Magisk root by flashing pre-patched boot.img (Rom: TB_X605F_S210273_221107_ROW)
-------------------------------------------------------------------------------------------------------------

```
adb reboot bootloader
fastboot flash boot magisk_patched-26000_S5cIp.img
```


Final Steps
--------------------
Open Magisk Manager and Update the app
Open again and update the Magisk install
Choose 'Direct Install' this time instead of 'Patch File'
After updates and reboot, you're done!

If you would like to debloat, run the LenovoM10DebloatTool.bat file.


##########################################################################
End
##########################################################################


-------------------------------------------------------------------------------------------------------------
==**BLUnlocker Informations**==
-------------------------------------------------------------------------------------------------------------

**Prerequisites:**  
QDLoader_HS_USB_Driver (Google it and find)  
Hex Editor (eg. HxD)  
PC and some brain to understand....  
  
**Procedure:**  

1. Install the drivers and hex editor.
2. Extract BLUnlocker-Master
3. Step 3!
4. Boot into edl mode using key combo  
    or using adb
    
    Code:
    
    ```
    adb reboot edl
    ```
    
5. Check the port number in device manager and run " dump_devinfo.bat ".
6. Mention port number and full path to "*.mbn" file (eg. D:\where\every\it\is\prog_emmc_firehose_8917_ddr.mbn)
7. A file named " devinfo.img " will be dumped to the working dir. Edit it using an HEX editor as shown below.

![[1607766281870.png]]

8. Save the devinfo.img after editing and run " unlock.bat "
- Reboot after success message.
- Voila !! You have Successful unlocked the bootloader. Reboot to system by holding the power button.




NOTES:
https://xdaforums.com/t/rescue-and-smart-assistant-lmsa-motorola-lenovo-only.3951714/
https://xdaforums.com/t/root-tb-x605f-l-magisk-for-those-who-cant-use-twrp.4296469/
https://xdaforums.com/t/lenovo-tb-x605f-magisk-root.3992477/
https://xdaforums.com/t/guide-unlock-bootloader-of-lenovo-tab-4-10-with-oem-unlock-greyed-out-tb-x304f-l-and-other-qcom-tablets.4201857/
