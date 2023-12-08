# Dell-Drive-Updates-PSADT Revised
Really like where this script started, but wanted to use it with newer versions of PSADT. This script is a WIP. I don't know what I'm doing, but as a sysadmin who uses PSADT for his work, this was a good jumping off point for getting a DCU update script working without reinventing the wheel like I tend to.

## Changes:

### Encrypted BIOS Passwords Problem
There was a request to add an option to do all but BIOS updates during updates. I think this may have stemmed from the inability to use `dcu-cli.exe` to add the encrypted BIOS password to the registry in the same way you can with the DCU GUI, making the need for admin execution moot. Unfortunately, this has been an oversight from Dell, for whatever reason, for many versions of DCU!

Of course, you can use the `/generateencryptedpassword` flag with the CLI, but the encrypted output is not the same as the GUI. Simply generating it, and storing it in the registry won't work. Further, the generated encrypted BIOS passwords are specific to the machine preventing you from adding the encrypted password registry key to other devices and expecting it to work the same.

In conclusion. unless you want to run the DCU GUI as admin and set the BIOS password within the settings for every machine you manage, you'll need a different method or to avoid BIOS updates entirely.

### Encrypted BIOS Passwords Solution
**Disclaimer: This solution assumes you are using the same BIOS password for every machine you manage. If that's not the case, you'll want to use the flag to ignore BIOS updates or devise a solution that works for you.**

Provided with this revised script is a separate PowerShell script that you can run. This script will prompt for your BIOS password twice as a secure string, compare them, and then generate three encryption files required for the BIOS update portion of the revised script. These files are saved to the user's TEMP folder, but can be changed. This script only needs to be ran for each BIOS password in the environment. It should be noted that the encryption files can be named anything so long as the encrypted password files contains the word `pass`, the key file contains the word `key`, and the initial vector file contains the word `iv`.

---

# Original : Dell-Driver-Updates-PSADTK
Dell Driver update w/ Powershell Application Deployment ToolKit. This package for SCCM will use Dell's Command Update command line to scan the computers for hardware, then cross check it with Dell's model catalog and install the drivers from Dell's site. This includes Bios and Drivers. 

Added switch for UpdateType (All, Bios, Network, Display)
  Default: All

Example: Deploy-Application.exe -UpdateType Bios  
  Result: Updates ONLY the bios on the workstation
  
Example: Deploy-Application.exe -UpdateType Network  
  Result: Updates ONLY the network driver(s) on the workstation, Wifi and/or Lan
  
Example: Deploy-Application.exe -UpdateType Display  
  Result: Updates ONLY the display drivers on the workstation
  
Example: Deploy-Application.exe  
  Result: Updates ALL the drivers on the workstation

Note: Make sure if you have a BIOS password on the system you update the PW in the script, if you do not employ a bios pw commment out the cctk lines of the code.
