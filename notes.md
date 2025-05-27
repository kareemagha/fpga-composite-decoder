# Notes
## PAL Basics
* 25 fps, with 50 fields being interlaced
* Phase of each field alternates by $180\degree$
    * technically each line alternates in each phase, but because of interlacing this means each field alternates in phase
* A PAL signal contains 2 minor signals: the chrominance and luminance
    * the luminance is the B/W i.e. the image
    * the chrominance is the colour
* The chrominance consits of two other signals that are modulated onto a 4.43MHz subcarier - the colour burst.
    * these two signals are the B-Y (U) and R-Y (V) signals.
    * $\text{Y} = 0.299\text{R'} + 0.587\text{G'} + 0.114\text{B'}\\
\text{U} = -0.147\text{R'} -0.289\text{G'} + 0.436\text{B'} = 0.492(\text{B' - Y})\\
\text{V} = 0.615\text{R'} - 0.515\text{G'} - 0.100\text{B'} = 0.877(\text{R'-Y})
$
* In PAL, these components are quadrature modulated, meaning:
    * U modulates the subcarrier in-phase (0°), and
    * V modulates the subcarrier in quadrature (90° phase shift).
* BUT — in PAL, the V component is phase-inverted on every other line - so on alternate lines, the V axis flips, meaning the modulated signal has its phase shifted by 180° in that component.

* Formally, the U and V signals are modulated via surpressed carried quadrature amplitude modulation
    * mathematically just means the U and V siganls are spaced $90\degree$ apart, in the absence of the carrier signal (however this carrier signal is sent as a colour burst somewhere else - more on this later)
    * i.e. Chrom. = $\underbrace{a\cos{(2\pi f + \phi_1)}}_U + \underbrace{b\sin{(2\pi f + \phi_2)}}_{V}$
* The phase of the U and V signals determine the colour/hue of the "pixel"


## Colour burst 
* For a TV to decode the transmitted colour signal, the colour subcarrier must be reinserted i.e. through a local oscillator
* This oscillator must be exactly locked in phase with the oscillator that encoded the signal - done with a PLL and a VCO
* As such $10 \pm 1$ cycles of 4.43Mhz burst information are sent with each line of signal info - this is found on the back porch. 
* This burst signal is referred to as the 'PAL swining burst' because the phase 'swings' $135\degree$ to $225\degree$ with respect to (wrt) the U signal.
* When decoding we dont need to do anything about this swing - the swing actually helps us since it was intentionally added to "average out" colour discrepancies.

## Interleaving the signal
* A requirement of colour TV was that it shouldnt increace the bandwidth of the transmitted signal. i.e. if a TV channel had B/W transmission, they should not need to increase their bandwidth for the colour transmission.
* In Australia the total bandwidth of 625 line transmitters is 7MHz (includeing the sound carrier aswell), with the Y taking up 5MHz of the bandwidth. 
* Monochrome signals do not occupy the entire bandwidth of the signal, but occur in clusters of energy at multiples (harmonics) of the line frequency. This is because the monochrome picture is scanned one line at a time, building up the complete picture. This means that the transmitted colour information can be interleaved between peaks of monochrome energy

    ![The colour signal’s subcarrier sidebands are interleaved with the monochrome signal sidebands.](images/interleaving.png)

* The ‘distance’ between each peak of colour signal energy and the last and next cluster of monochrome signal energy is half the line frequency.
    * This just means the chroma peaks fall halfway between luma peaks.
* The value for the colour subcarrier frequency is found mathematically via:
    * $f_{sc} = 0.5\times f_{field} + (283.5 + 0.25) \times f_{line} $
        * $f_{field} = 50\text{Hz}$
        * $f_{line} = 15,625\text{Hz}$


## Decoding
* The following notes will be based off this block diagram.
![alt text](images/decoderblockdiagram.png)
