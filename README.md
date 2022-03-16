# Cheatsheet


adb devices -l (list of devices by product/model)

adb -s <deviceName> <command> (redirect command to specific device)\
adb -s <deviceName> shell (get shell to specific device)
  
adb install <apk> (install app)\
adb shell install <path> (install app from phone path)\
adb shell uninstall <name> (remove the app)

  
## File Operations
  
adb push <local> <remote> (copy file/dir to device)\
adb pull <remote> <local> (copy file/dir from device)\
run-as <package> cat <file> (access the private package files)

## Package Info
  
adb shell dumpsys package packages | grep  (grep after apk name)\
adb shell pm list packages (list package names)\
adb shell pm path <package> (path to the apk file)

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
  
## Log
  adb logcat
  
``` bash
  Usage: logcat [options] [filterspecs]

General options:
  -b, --buffer=<buffer>       Request alternate ring buffer(s):
                                main system radio events crash default all
                              Additionally, 'kernel' for userdebug and eng builds, and
                              'security' for Device Owner installations.
                              Multiple -b parameters or comma separated list of buffers are
                              allowed. Buffers are interleaved.
                              Default -b main,system,crash,kernel.
  -L, --last                  Dump logs from prior to last reboot from pstore.
  -c, --clear                 Clear (flush) the entire log and exit.
                              if -f is specified, clear the specified file and its related rotated
                              log files instead.
                              if -L is specified, clear pstore log instead.
  -d                          Dump the log and then exit (don't block).
  --pid=<pid>                 Only print logs from the given pid.
  --wrap                      Sleep for 2 hours or when buffer about to wrap whichever
                              comes first. Improves efficiency of polling by providing
                              an about-to-wrap wakeup.

Formatting:
  -v, --format=<format>       Sets log print format verb and adverbs, where <format> is one of:
                                brief help long process raw tag thread threadtime time
                              Modifying adverbs can be added:
                                color descriptive epoch monotonic printable uid usec UTC year zone
                              Multiple -v parameters or comma separated list of format and format
                              modifiers are allowed.
  -D, --dividers              Print dividers between each log buffer.
  -B, --binary                Output the log in binary.

Outfile files:
  -f, --file=<file>           Log to file instead of stdout.
  -r, --rotate-kbytes=<n>     Rotate log every <n> kbytes. Requires -f option.
  -n, --rotate-count=<count>  Sets max number of rotated logs to <count>, default 4.
  --id=<id>                   If the signature <id> for logging to file changes, then clear the
                              associated files and continue.

Logd control:
 These options send a control message to the logd daemon on device, print its return message if
 applicable, then exit. They are incompatible with -L, as these attributes do not apply to pstore.
  -g, --buffer-size           Get the size of the ring buffers within logd.
  -G, --buffer-size=<size>    Set size of a ring buffer in logd. May suffix with K or M.
                              This can individually control each buffer's size with -b.
  -S, --statistics            Output statistics.
                              --pid can be used to provide pid specific stats.
  -p, --prune                 Print prune white and ~black list. Service is specified as UID,
                              UID/PID or /PID. Weighed for quicker pruning if prefix with ~,
                              otherwise weighed for longevity if unadorned. All other pruning
                              activity is oldest first. Special case ~! represents an automatic
                              quicker pruning for the noisiest UID as determined by the current
                              statistics.
  -P, --prune='<list> ...'    Set prune white and ~black list, using same format as listed above.
                              Must be quoted.

Filtering:
  -s                          Set default filter to silent. Equivalent to filterspec '*:S'
  -e, --regex=<expr>          Only print lines where the log message matches <expr> where <expr> is
                              an ECMAScript regular expression.
  -m, --max-count=<count>     Quit after printing <count> lines. This is meant to be paired with
                              --regex, but will work on its own.
  --print                     This option is only applicable when --regex is set and only useful if
                              --max-count is also provided.
                              With --print, logcat will print all messages even if they do not
                              match the regex. Logcat will quit after printing the max-count number
                              of lines that match the regex.
  -t <count>                  Print only the most recent <count> lines (implies -d).
  -t '<time>'                 Print the lines since specified time (implies -d).
  -T <count>                  Print only the most recent <count> lines (does not imply -d).
  -T '<time>'                 Print the lines since specified time (not imply -d).
                              count is pure numerical, time is 'MM-DD hh:mm:ss.mmm...'
                              'YYYY-MM-DD hh:mm:ss.mmm...' or 'sssss.mmm...' format.

filterspecs are a series of
  <tag>[:priority]

where <tag> is a log component tag (or * for all) and priority is:
  V    Verbose (default for <tag>)
  D    Debug (default for '*')
  I    Info
  W    Warn
  E    Error
  F    Fatal
  S    Silent (suppress all output)

'*' by itself means '*:D' and <tag> by itself means <tag>:V.
If no '*' filterspec or -s on command line, all filter defaults to '*:V'.
eg: '*:S <tag>' prints only <tag>, '<tag>:S' suppresses all <tag> log messages.

If not specified on the command line, filterspec is set from ANDROID_LOG_TAGS.

If not specified with -v on command line, format is set from ANDROID_PRINTF_LOG
or defaults to "threadtime"

-v <format>, --format=<format> options:
  Sets log print format verb and adverbs, where <format> is:
    brief long process raw tag thread threadtime time
  and individually flagged modifying adverbs can be added:
    color descriptive epoch monotonic printable uid usec UTC year zone

Single format verbs:
  brief      — Display priority/tag and PID of the process issuing the message.
  long       — Display all metadata fields, separate messages with blank lines.
  process    — Display PID only.
  raw        — Display the raw log message, with no other metadata fields.
  tag        — Display the priority/tag only.
  thread     — Display priority, PID and TID of process issuing the message.
  threadtime — Display the date, invocation time, priority, tag, and the PID
               and TID of the thread issuing the message. (the default format).
  time       — Display the date, invocation time, priority/tag, and PID of the
             process issuing the message.

Adverb modifiers can be used in combination:
  color       — Display in highlighted color to match priority. i.e. VERBOSE
                DEBUG INFO WARNING ERROR FATAL
  descriptive — events logs only, descriptions from event-log-tags database.
  epoch       — Display time as seconds since Jan 1 1970.
  monotonic   — Display time as cpu seconds since last boot.
  printable   — Ensure that any binary logging content is escaped.
  uid         — If permitted, display the UID or Android ID of logged process.
  usec        — Display time down the microsecond precision.
  UTC         — Display time as UTC.
  year        — Add the year to the displayed time.
  zone        — Add the local timezone to the displayed time.
  "<zone>"    — Print using this public named timezone (experimental).
    
 ```
    
    
## activity
    
 am start-activity net.oneplus.launcher/net.oneplus.quickstep.RecentsActivity  (Oneplus open swipe to close ??) 
 am start-activity com.android.vending/com.google.android.finsky.activities.MainActivity (open google play store)
 
 ``` bash
start-activity [-D] [-N] [-W] [-P <FILE>] [--start-profiler <FILE>]
          [--sampling INTERVAL] [--streaming] [-R COUNT] [-S]
          [--track-allocation] [--user <USER_ID> | current] <INTENT>
      Start an Activity.  Options are:
      -D: enable debugging
      -N: enable native debugging
      -W: wait for launch to complete
      --start-profiler <FILE>: start profiler and send results to <FILE>
      --sampling INTERVAL: use sample profiling with INTERVAL microseconds
          between samples (use with --start-profiler)
      --streaming: stream the profiling output to the specified file
          (use with --start-profiler)
      -P <FILE>: like above, but profiling stops when app goes idle
      --attach-agent <agent>: attach the given agent before binding
      --attach-agent-bind <agent>: attach the given agent during binding
      -R: repeat the activity launch <COUNT> times.  Prior to each repeat,
          the top activity will be finished.
      -S: force stop the target app before starting the activity
      --track-allocation: enable tracking of object allocations
      --user <USER_ID> | current: Specify which user to run as; if not
          specified then run as the current user.
      --windowingMode <WINDOWING_MODE>: The windowing mode to launch the activity into.
      --activityType <ACTIVITY_TYPE>: The activity type to launch the activity as.
      --display <DISPLAY_ID>: The display to launch the activity into.
```
  
