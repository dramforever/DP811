# The architecture of the DP811

## Overview

Byte-oriented variable-length instruction set

Encoded byte-by-byte. E.g. `Delay 5 seconds` is `DL 05, 01`, and we put in `02 05 01`.

8 registers: 00 .. 07

8 pins: P1.0 .. P1.7 = 00 .. 07.

The 8 pins are normally connected to 8 on-board LEDs through removable jumpers. The LEDs are common anode, so they turn *on* when the corresponding pin is *LOW*.

Ports: P1.{0...7} = 00. Ports 01 and 02 are on extension board (**TODO** what are those?). Accessible using `IN` (`0B`) and `OUT` (`0A`).

Timer: Pin 08 HIGH on timeout

## Instructions:

(*Note* The names are not official. The official names use Chinese Pinyin and look really bad in an English context. If you have an improvement idea feel free to open an issue. *End of note*)

<table>
<tr><th> #  <th> Name <th> Args       <th> Explaination
<tr><td> 00 <td> SL   <td> pin        <td> Set LOW on pin (Turns LED *on*)
<tr><td> 01 <td> SH   <td> pin        <td> Set HIGH on pin (Turns LED *off*)
<tr><td> 02 <td> DL   <td> time, unit <td> Wait for some time.

<table>
<tr><th colspan="2">unit
<tr><td> 00 <td> 0.1s
<tr><td> 01 <td> second
<tr><td> 02 <td> minute
<tr><td> 03 <td> hour
</table>

<tr><td> 03 <td> PL   <td> note, len  <td> Play note.

<table>
<tr><th colspan="2">note
<tr><td> 00 <td> rest (no sound)
<tr><td> 0x <td> low
<tr><td> 1x <td> mid
<tr><td> 2x <td> high
</table>

<table>
<tr><th colspan="2">len
<tr><td> 01 <td> 1/16
<tr><td> 02 <td> 1/8
<tr><td> 03 <td> 1.5/8
<tr><td> 04 <td> 1/4
<tr><td> 05 <td> 1.5/4
<tr><td> 06 <td> 1/2
<tr><td> 07 <td> 1.5/2
<tr><td> 08 <td> 1
<tr><td> 09 <td> long (<b>TODO</b> clarify)
</table>

<tr><td> 04 <td> ST   <td> reg, imm   <td> Set register to immediate
<tr><td> 05 <td> DISP <td> reg, en    <td> Display register content

<table>
<tr><th colspan="2">en
<tr><td> 00 <td> Blank display
<tr><td> 01 <td> Enable display
</table>

<tr><td> 06 <td> ADD  <td> reg, imm   <td> Add immediate to register
<tr><td> 07 <td> DJNZ <td> reg, addr  <td> Decrement register and jump if not zero
<tr><td> 08 <td> JH   <td> pin, addr  <td> Jump if pin is HIGH
<tr><td> 09 <td> JL   <td> pin, addr  <td> Jump if pin is LOW
<tr><td> 0A <td> OUT  <td> po, reg    <td> Write register bits to port
<tr><td> 0B <td> IN   <td> reg, po    <td> Read port to register bits
<tr><td> 0C <td> NOT  <td> reg        <td> Invert bits in register
<tr><td> 0D <td> NOP  <td>            <td> No-op
<tr><td> 0E <td> TIME <td> time, unit <td> Set timer.
<tr><td> 0F <td> HALT <td>            <td> Stop program
<tr><td> 10 <td> JMP  <td> addr       <td> Unconditional jump
<tr><td> 11 <td> JE   <td> imm, addr  <td> Jump if last register equals immediate
<tr><td> 12 <td> CALL <td> addr       <td> Call subroutine
<tr><td> 13 <td> RET  <td>            <td> Return from subroutine
<tr><td> 14 <td> ROT  <td> reg, dir   <td> Rotate (cyclic shift) register

<table>
<tr><th colspan="2">dir
<tr><td> 00 <td> Left
<tr><td> 01 <td> Right
</table>

</table>
