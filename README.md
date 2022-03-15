# Cheatsheet


adb devices -l (list of devices by product/model)

adb -s <deviceName> <command> (redirect command to specific device)\
adb -s <deviceName> shell (get shell to specific device)
  
adb shell install <apk> (install app)\
adb shell install <path> (install app from phone path)\
adb shell install -r <path> (install app from phone path)\
adb shell uninstall <name> (remove the app)

  
## File Operations
  
adb push <local> <remote> (copy file/dir to device)\
adb pull <remote> <local> (copy file/dir from device)\
run-as <package> cat <file> (access the private package files)

## Package Info
  
adb shell dumpsys package packages | grep  (grep after apk name)\
adb shell pm list packages (list package names)\
adb shell path <package> (path to the apk file)

``` bash
 list packages [-f] [-d] [-e] [-s] [-3] [-i] [-l] [-u] [-U]
      [--show-versioncode] [--apex-only] [--uid UID] [--user USER_ID] [FILTER]
    Prints all packages; optionally only those whose name contains
    the text in FILTER.  Options are:
      -f: see their associated file
      -a: all known packages (but excluding APEXes)
      -d: filter to only show disabled packages
      -e: filter to only show enabled packages
      -s: filter to only show system packages
      -3: filter to only show third party packages
      -i: see the installer for the packages
      -l: ignored (used for compatibility with older releases)
      -U: also show the package UID
      -u: also include uninstalled packages
      --show-versioncode: also show the version code
      --apex-only: only show APEX packages
      --uid UID: filter to only show packages with the given UID
      --user USER_ID: only list packages belonging to the given user
 ```
  
  
## Configure Settings Commands
  
adb shell dumpsys battery set level <n> (change the level from 0 to 100)\
adb shell dumpsys battery set status<n> (change the level to unknown, charging, discharging, not charging or full)\
adb shell dumpsys battery reset (reset the battery)\
adb shell dumpsys battery set usb <n> (change the status of USB connection. ON or OFF)\
adb shell wm size WxH (sets the resolution to WxH)
  
``` bash
  usage: dumpsys
         To dump all services.
or:
       dumpsys [-t TIMEOUT] [--priority LEVEL] [--pid] [--help | -l | --skip SERVICES | SERVICE [ARGS]]
         --help: shows this help
         -l: only list services, do not dump them
         -t TIMEOUT_SEC: TIMEOUT to use in seconds instead of default 10 seconds
         -T TIMEOUT_MS: TIMEOUT to use in milliseconds instead of default 10 seconds
         --pid: dump PID instead of usual dump
         --proto: filter services that support dumping data in proto format. Dumps
               will be in proto format.
         --priority LEVEL: filter services based on specified priority
               LEVEL must be one of CRITICAL | HIGH | NORMAL
         --skip SERVICES: dumps all services but SERVICES (comma-separated list)
         SERVICE [ARGS]: dumps only service SERVICE, optionally passing ARGS to it
 ``` 
## Device Related Commands
  
adb reboot-recovery (reboot device into recovery mode)\
adb reboot fastboot (reboot device into recovery mode)\
adb shell screencap -p "/path/to/screenshot.png" (capture screenshot)\
adb shell screenrecord "/path/to/record.mp4" (record device screen)\
adb backup -apk -all -f backup.ab (backup settings and apps)\
adb backup -apk -shared -all -f backup.ab (backup settings, apps and shared storage)\
adb backup -apk -nosystem -all -f backup.ab (backup only non-system apps)\
adb restore backup.ab (restore a previous backup)\
adb shell am start|startservice|broadcast <INTENT>[<COMPONENT>]\
-a <ACTION> e.g. android.intent.action.VIEW\
-c <CATEGORY> e.g. android.intent.category.LAUNCHER (start activity intent)\

adb shell am start -a android.intent.action.VIEW -d URL (open URL)\
adb shell am start -t image/* -a android.intent.action.VIEW (opens gallery)
  
