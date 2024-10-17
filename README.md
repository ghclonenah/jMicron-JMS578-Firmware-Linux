This repo contains F/W for the JMicron578 chip found in many USB3/SATA docks/adapters. These devices have some issues with USB3 commands in Linux, resulting in poor performance, disconnects, errors etc.

<https://ralimtek.com/posts/2021/jms578/>


Required software
I have re-hosted the required files to flash the firmware here for convenience in case they are ever lost to time. You can to use a Windows machine to use the JMicron software or you can use the jms578flash tool.

Process
The way that this works is that we want to reset all of the configuration memory to be blank (so that the device runs on defaults), and we also want to run a firmware that defaults to sane values and has auto-sleep defaulting to being turned off (some default to 10 minutes).

This does require you to have a drive in the enclosure, as it will not enumerate without a drive inserted, and the process I’m guessing uses SCSI commands to send the data to the ic. I’ve never had any issues at all with this, and have not had any damage to drives.

Connect the enclosure to your computer with an HDD inside it.
Open the Firmware update software
Check that your drive shows up in the “USB DISK” menu. If multiple show up, remove any other enclosures you have connected to be safe!
Tick the “RD Version” to enable extra options
Tick the option “Backup Old Firmware Only”
Click “Browse” and choose where to save a backup of your firmware
Click “Run” to take a backup of your firmware
Untick the “Backup Old Firmware Only”
Tick the option “Erase All Flash Only”
Click Run
When the process is done, a popup will prompt you to power cycle the drive. Hit OK
Hard power cycle the drive by pulling the power
Connect the power again and turn on the enclosure if required
Once the drive shows back up in the USB DISK Menu, continue below.
Untick the option at the bottom to Erase all of the flash
Now select the included firmware .bin file using the “Load File” button
Now untick “Include JMS577 NVRAM” option that will have automatically ticked itself
Click “Run” again
Now do a full power cycle again
You are done :D


Afterwards
After the flashing complete, you should have the following attributes of the enclosure:
<<<>>>>>>>>
No auto power off
Device enumerates with the drives correct model number
Device enumerates with a valid serial number (not always correct, but unique.. Suspect some byte scrambling going on)
Normal functionality of the enclosure.

Recovery
If for some reason you run into a bad flash of your unit you can take the following steps

If the unit enumerates to your pc
Just follow the above instructions to erase all of the firmware and power cycle your enclosure. You should be good to flash a new firmware afterwards.

If the unit does not enumerate to your pc
Don’t panic.

You will need to open your enclosure to get to the PCB. Near the main interface IC, there will be a small 8 pin SOIC that is storing the device firmware. Lookup the part number of the top of the chip to confirm its a flash IC.

With the drive powered down, you will need to short the MISO pin of the flash to the ground for the first ~1 second while the drive powers up. This will cause loading the firmware from the FLASH to fail. The unit will then boot from the internal ROM, and you can issue the erase command to then blank the flash IC and start again.

You may need to try this a few times, testing longer or shorter times of holding the pin to ground, not 1000% on this, but I have had it work every time for me eventually. I use thin-tipped tweezers to make this easier to perform.

Remember you will still need to have the drive connected, so it’s worth spending a few minutes to set up a jig for this.
