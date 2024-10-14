# funni-fullscreen-coloring

Your screen flickering and speakers beeping for some times ... for a BSoD ?

## Build
`gcc -O2 -mwindows funni-fullscreen-coloring.c -o funni-fullscreen-coloring.exe -lgdi32`

## Detail

The program get the whole screen's device context, then draw a colored rectangle on it directly. During the process, beep sound with different frequencies and lengths are played. After a few moment, a BSOD will occurred by making a call to `NtRaiseHardError` and the machine restarts. 

The code runs on 4 threads:
- The main thread draw a fullscreen rectangle with random color every 100ms (determine by the `flickertime` constant) in between 4.5-6s
- The "single color" thread: Run after main thread ends to set the screen to a single color, also act as "waiting" for the BSOD thread
- The "BSOD" thread:  Run last. It takes some for the call to be in effect
- The beep thread run seperately until the computer restarts (or at least the sound drivers are stopped and audioloop occurs) 

A global hotkey `Ctrl+Alt+E` is also installed to stop the program immediately. Note that the drawn rectangle would still on the screen, a small refresh (e.g. switch to another progeam) would bring everything back to normal

Due to the fact the program draw directly to the screen, anything graphics that update in process would still be visible as well, same with the mouse pointer (this could be a feature request). High DPI & anything-not-but-not 100% scaling would also break

## Demo (from first build May 6 2024)

https://github.com/user-attachments/assets/f6f02e5a-23cc-4a22-9e18-36335dcf8385

