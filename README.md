# Oregon-scientific-OOK-Decoder
Port of RFLink Oregon Scientific plugin in C for NXP LPC1xxx MCUs

This is heavily based on the work on RFLink project, for frame decoding, and also 
on work from https://github.com/Cactusbone/ookDecoder
for decoding the manchester bit stream coming from the RF 433MHz receiver.
Only the decoding of Oregon Scientific devices is included, but based on this and
the code from the original git repository, other kind of equipments could be
easily added.

I use this code included in my other projet on github related to Viessmann
heater remote controller.

I ported the code to C as the original C++ manchester decoder was too large
once compiled to fit on my LPC1343 MCU. This code compiles to about 10kbytes on that
target.

The OOK decoder is based on a capture timer, with timer counting at 1MHz.
Data is sent through a USB CDC interface to some remote controlling unit, PC or Raspberry
or whatever.

So the TIMER32_0_IRQHandler as the name states is the interrupt handler for the timer.
The timer is configured to capture the value for rising or falling edge events on the
external capture pin, then trig an interrupt. The function saves the time (in µs) between
the two last capture events, so the length of any pulse out of the RF receiver.

Output frame format is kept from RFLink plugin with an added RFL header to fit in
my existing remote control software.
