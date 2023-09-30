---
title: "Turning off WiFi at night on Mikrotik RouterOS"
related : true
categories:
  - HowTo
tags: 
  - RouterOS
  - Mikrotik
  - WiFi
  - script
  - schedule
---

If you think that you must turn off WiFi at night due to security reasons or
looking for a healthy rest time, here is how to do that on Mikrotik RouterOS.

![CRS125 MikroTik's popular router](/assets/images/2017/05/CRS125.png){: .align-left} 

To program a Mikrotik router for turning off and on WiFi you must use scripting and scheduling capabilities fo RouterOS.

The command you must run to turn off WiFi is:

    interface wireless disable <interface-name>

So you have to wrote a very small script with just that command. Using WinBox:

1. `Open System` -> `Scripts` window
2. Press `Add` button and the script editor window will open
  - Type a script name like `DisableWLAN`
  - Select required policies or check all of them
  - Type script into the `Source` text area: `interface wireless disable <interface-name>`
  - You can to test the script with `Run Script` button
  - Save the script with `Ok` button

Now create a similar `EnableWLAN` script with the opposite command:

    interface wireless enable <interface-name>
    
Next add the schedule scripts:

1. `Open System` -> `Scheduler` window
2. Press `Add` button to open the `Scheduler` window
  - Type some meaningful name for the scheduled task like `CronDisableWLAN` (can be the same as the script's)
  - Type the name of your script, `DisableWLAN`, into the `On Event` text area 
  - Select the `start date` (i.e today), `start time` (`hh:mm:ss` format, i.e. `23:00:00`) and the `interval` for repeating the action like `1d 00:00:00`
  - Select required policies or check them all
  - Save the schedule with `Ok` button

Repeat the same to schedule the enabling script (i.e. `CronEnableWLAN` starting at `09:00:00` with an interval of `1d 00:00:00`)

Now your Mikrotik router will turn off and on at your desired times. 

Good rest !
