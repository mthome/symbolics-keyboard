Symbolics keyboard adapter, based on Teensy keyboard example.

The Symbolics keyboard acts as a shift register with 128 bits.  Each
key is represented by one bit in the shift register.  The hardware
interface consists of a clear line which is used to signal the
beginning of a read cycle, a clock line, and a data line.  All signals
are active low.  The keyboard changes the data line on the rising edge
of the clock.  It should be read near the falling edge of the clock by
the host.

The keyboard needs to be interfaced to the Teensy board as
follows. The wire colors specified are those used in the original
modular cable supplied with the keyboard:

blue  GND
green 5V
red   D4   DIN
black D5   CLK
white D6   CLR

The keyboard implements two locking functions, caps lock and mode
lock.  Both of these are implemented as switches, not as buttons.
Host systems do not usually expect switches on keyboards, so
precautions must be taken to synchronize their state to the host's
state.

The "Caps Lock" key is implemented so that it works as usual, i.e. it
is transmitted to the host as if it were a button.  The host sends
back its caps lock state through the keyboard LEDs.  Thus, the
controller firmware can synchronize the host's state with the state of
the caps lock switch on the keyboard.

The "Mode Lock" key is used to switch the keyboard between the classic
Symbolics layout and a variant that assigns the modifier keys on the
right side of the space bar to be cursor keys.  This mode is called
f_mode.

The "Local" key is used as a modifier key to trigger functions in the
converter firmware.  The following functions are implemented:

Local-B boots the AVR into the boot loader so that it can be
reprogrammed through USB by the host.

Local-V sends the Subversion revision number of this file to the host.

Mapping of the symbolics key number to an USB key number is done
through the mapping table defined in the file keymap.inc.  There are
two separate tables, one for normal mode and one for f_mode.  The
mapping table is normally autogenerated by the keymap generation
program contained in make-keymap.lisp, but it can be manually edited
if no Lisp evironment is available.

Author: Hans Huebner (hans.huebner@gmail.com).
