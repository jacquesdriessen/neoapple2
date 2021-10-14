
# Apple II+ Emulation on PYNQ-Z2
## [Feng Zhou's port](https://github.com/zf3/neoapple2/) but for the PYNQ-Z2, which is in turn based on Stephen A. Edwards's [Apple2fpga](http://www.cs.columbia.edu/~sedwards/apple2fpga/)

- For the full guide / explanation etc - please refer to [Feng Zhou's port](https://github.com/zf3/neoapple2/)
- Audio is not working - the raw PWM signal is available though on bit 6 of PMODA (beneath where the PS/2 adapter goes) so technically some circuitry with a low pass filter would probably work (Z2 has different audio circuitry)
- The only changes are:
    - The source was looking for an initial nib file - added an empty one.
    - Added a release folder with a BOOT.BIN (see original on what to do with that)
    - For whatever reason the .nib extension was case sensitive on my setup / now accepts both .nib and .NIB.
    - For simulation purposes - reset came after < 1 micro second in the source - back to what it was (0.25 seconds)
    - And remove the "keyboard press unpause" as more often than not there would be some spurious keyboard input - e.g. you really need to load a disk image, then press button 0 and then you are in business.

## Some additional tips
To make the tcl script work as per the original, I needed to:
- First do a "set_param board.repoPaths="C:\\fullpathto\\boardfilesdirectory" - I found those here [Vendor board files](https://www.tul.com.tw/productspynq-z2.html)
- Make sure the Digilent IP Library "top level" directory is the same as this repository (the source expects this)
- SW1 to enable colour

For the vitis part - basically had to recreate that, so here is the steps (likely also required for this)
- Move the neoapple2ui directory somewhere else (you will need it) and create a new workspace neoapple2ui in that location as per the instructions.
- Select the platform.spr, under the standalone on ps7 you can modify the BSP settings (starts greyed out but if you give it a sec that should work).
- Add xilffs
- Then File / New Application Project, call the project neoapple2ui (it should default to neoapple2ui_system as the system, but if not - use that), use the hello world template.
- Under neoapple2ui_system/neoapple2ui/src there are four source files, three called platform something and a fourth one you need to rename to main.c
- Exit vitis / then copy the originals from the repository (this one as otherwise it can't find NIB files), restart vitis.
- The do project/clean, select clean all and build the entire workspace.
- This should result in a BOOT.BIN in neoapple2\neoapple2ui\neoapple2ui_system\Debug\sd_card, see original Feng Zhou's project on what to do with that.

What had me fooled for a bit, is getting a keyboard going. I had an adapter USB -> PS/2 lying around and thought this would work with any keyboard. That's not the case (these adapters are fully passive, e.g. you need a keyboard that can switch between USB and PS/2 - e.g. the ones that ship with such an adapter) - and also hope they are able to work off a 3.3V VCC.

Lastly - I wouldn't get a prompt without having a minimal working disk image.

