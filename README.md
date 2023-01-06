# VLF Receiver

KiCad schematics and docs for a VLF receiver aimed at 17.2 kHz SAQ reception.

You can find all the documentation in the [[Wiki|https://github.com/SA5NNN/VLF-Receiver/wiki]]


This project started out with me noticing the UNESCO world heritage Grimeton https://grimeton.org/ facility south of Gothenburg Sweden.
Built in the 1920’s the facility communicated with New York where a similar station was located at Radio Central, Rocky Point, Long Island.
The station is the only station operating an Alexanderson alternator transmitter. The alternator generates a sine wave at 17.2 kHz
(17442 m wavelength) at a power of 200 kW. It uses a large field to house six 127 m high antennas. It is very difficult to get antennas
to be efficient at these low frequencies (maybe a few percent actually radiates, the rest turns into heat) and therefore interesting to
see how much I can hear from my QTH in Sweden.

My radio friends in SK5LF started by designing whip antennas, so it seemed a good idea to try something else. Whip antennas receive the
electric fields, so why not design something that connects to the magnetic field?
The obvious starting point www.vlf.it: http://www.vlf.it/looptheo7/looptheo7.htm. This page concludes that the mass of copper in the air
is the most important factor for a strong signal. It turns out getting the material is not trivial and building the construction is not
easy either. So, the project got left on the back burner over covid. At summer 2022 I decided to see if I could scale down the project
and actually do something!
I found Paul’s page https://www.prinz.nl/SAQ.html where he designs a receiver for a computer’s soundcard using a ferrite rod.
These components are much easier to use as they do not require much, if any, building. I missed the July transmission but had a plan.

I used a signal generator to generate 17.2 kHz sine wave and output it to a small piece of wire (10 cm or so),
this was detectable using my VNA! That means I can test my stuff before the actual transmission, quite a feat!
As a receiver I decided to use my SDR radio, a SDRplay RSPdx. It has special filtering to allow for VLF and a port for sub 200 MHz.
The SDR software can be used to record not only the sound, but the entire sampled radio data if I wanted to. Convenient as I do not
know morse so have to do a lot of decoding work after the reception.

Paul’s design builds on a design from Broesel, Villach, Austria, that he simplified a bit. The design is a resonance circuit with the
ferrite rod and capacitance provided by three varactors (or varicaps), then a MOSFET for amplification.

The ferrite rod took an eternity to google, had to wade through an ocean of commercial crap from all over the world.
But in the end a German company http://www.amidon.de/ had a rod of the correct material and dimensions. Also an Italian
company could provide the varactors https://www.rf-microwave.com/en/philips/bb112/varicap-diode/bb112/.
The ferrite rod took 1.5 weeks to arrive and the varactors took 2 days (!!). My “local” supplier had the rest, including one of the
MOSFETs Paul had tested and that worked. The MOSFET was 2SK241 https://www.electrokit.com/produkt/2sk241-spa-transistor-fet-n-ch-20v-30ma/
and was sold as surplus. Yes, availability of the components may be problematic since these stuff don’t get built much commercially any
more and the hobbyists do not build them to often either, but they do exist!

You may want to look at the schematics at this stage, download and install KiCad, then download the KiCad project files from
this github repository and you can follow along.

I made some changes to Paul’s original design due to components, final inductance and the choice of receiver, check his design instead
if you want to use a soundcard.

## Design parameters
* Use Paul's design of a ferrite rod as the antenna.
* Use 3 varicaps and no caps for the tuning. Use 2SK241 as my MOSFET as that is what my supplier had available.
* Use my signal generator for testing at 17.2 kHz. Use my RSPdx as the receiver (50 Ohm input).

## Building

### The antenna
The heart of this beast is the copper wound ferrite rod. It will couple magnetically with the electromagnetic field from SAQ.
The inductance needed I took from Paul's design. He has 82 mH, I was aiming for the same (more or less).
The way you do this is by purchasing "enamelled" copper wire and wind that around the rod. The copper wire is not enamelled, if it were
the enamelling would crack and break off as we wound it. Instead, it is in fact some type of plastic coating. The coating is very
important as without it the copper wire would short-circuit, and we would not get any inductance at all. There is more expensive double
coated wire, this is for transmission when the voltage is high enough that you can get arcing. We do not need it in this design,
they are also more expensive. The wire has very little voltage and current, so we can use as thin wire as we like. The thinner they
are though, the fiddlier everything gets, such as sanding. I opted for DASOL 155-17-339, 0.30 mm diameter, 1 hg roll.
The rod was the largest I could find in material 33, that is MnZn that is excellent at building inductance with each turn of the wire.
It is R33-050-750 at http://www.amidon.de/contents/de/d649.html. This is supposed to have a permeability of 800 which is fantastic!

I took around 3 dm of wire out to the side to use as a connection, then started to wind the wire by rotating the rod in my hand and
using a finger to steer the wire slowly past the rod’s length. I wound only the centre 2/3 of the rod, thinking the ends should be free
for some reason. According to Paul, this needs quite a few turns, like several hundreds, maybe even close to a thousand! It all
depends on the material. Paul’s experience seems to indicate a lot of issues with wrong information when buying etc. I came to the
conclusion that I must somehow measure the inductance to see where I was. I sanded off the plastic coating of the end of the wire
then after many many turns sanded off as small part of the wire as I could so I got into electrical connection with the wire. Then
I measured the inductance at 10 kHz, and it was much to small (<30 mH). I decided not to retouch the coating, maybe nail polish
could have resealed the wire? But it appears very unlikely that the test patches I do, should come into contact with each other and
short out, and even if they do, I can just wind more to compensate. This turned out to work, I never had a short on those small
patches so I believe as long as you do not do them often, it's fine without resealing the coating.

Next, I went for my power tool, opening the chuck and tried to insert the end of the rod padded with a bit of paper, it worked!
I tightened enough to remove slack but put no pressure on the rod as it is fragile. This worked well and went
very fast. At the next measurement I was just over 100 mH. I had to unwind some of the wire, then measured 88.5 mH and I thought
that was close enough to the target of 82. Using some kapton tape, I secured both ends and fixed the turns to the rod. The last end
needed a bit of sanding to remove the coating and this, the most laborious part was done.
Final rerrite rod measured: L=88.5 mH, C=-2.862 nF, R=1.0 mOhm, Z=5.56 kOhm, DCR=9.12 Ohm, Q=198, Phi=89.7 degrees.

### Breadboard
As I now had all the components for building, I wanted to set things up and test them out. A good way of doing this is to use a
breadboard. Breadboards can give unreliable connections and are useless at high frequencies, but this is a small build and a low
frequency so it should work well even if we might have to wiggle the components at times. Ben Eater did some testing
of reliable breadboards and recommended BB830 by BusBoard Prototype Systems that I used in my build. https://eater.net/breadboards
I attach the components as well as the antenna to the breadboard. Also, the breadboards power rails are powered from my lab power
supply set to 9 V with a 10 mA current limitation. It’s certainly possible, even recommended, to use a 9 V battery for as low
power rail noise as possible. The output was via a BNC coax with clamps so I could clamp on to the legs of the output component.

### Matching varicaps
The varicaps are the other part of the resonant circuit, see https://en.wikipedia.org/wiki/LC_circuit. The resonant frequency is described
by the formula $f_0 = \frac{1}{2 \pi \sqrt{L C}}$ where $f_0$ is the resonant frequency, $L$ is the inductance in Henry and $C$ is the
capacitance in Farad. In the schema $L$ is our rod antenna ferrite L1 and $C$ is the three varicaps D1, D2 and D3 in series.
We already know $L$, it’s 88.5 mH, because we have measured it. This means we can rewrite the expression to express $C$ as a function of $f_0$ as:
$C = \frac{1}{L (2 \pi f_0)^2}$ that evaluates to $C = \frac{1}{0.0885 (2 \pi 17200)^2}$ = 967 pF.
The varicaps BB112 has according to their datasheet a variable capacitance depending on the forward voltage ranging from 470 pF at 1 V to
 20 pF at 8 V. Since we have three in parallel, we got three times that. So almost 1500 pF to 60 pF.
The varicaps are small transistor like items that does not require much current or space. The alternative to the varicaps are
larger air capacitors costing several hundred euros to buy. The negative part is the DC needed over the varactors, we must have a capacitor
in the circuit to block any DC from going into the antenna ferrite or we will be in big troubles! The DC block cap is not an issue in our circuit,
so I think Paul’s choice of varactor is a stroke of genius in this circuit. It becomes very easy and small compared to the large heavy variable
air capacitors normally used. The BB112 came in a set of three matched varicaps and at a reasonable 9.25 Euro!
The actual value for the capacitance, 967 pF lies in the range of the varactors, so we should have no problem to tune for resonance at
17.2 kHz as well as quite a few more frequencies if needed.

### Amplifier
Paul use the BF987 MOSFET as the transistor, I however had already ordered the 2SK241 MOSFET, but BF987 can be found in Italy were the
varactors BB112 are also located: https://www.rf-microwave.com/en/siemens/bf987/n-channel-mosfet-amplifier/bf987/
The MOSFET needs to be a depletion mode N-channel MOSFET with low reverse transfer capacitance to not affect the resonant circuit.
Depletion mode means it can work around 0 V as it allows both positive and negative signals. The resonant circuit provides a very low
voltage signal around 17.2 kHz, so we need a very small amount of voltage around 0 V. If the received signal from SAQ is at signal strength S9,
a very strong and fine signal, then it equates to 50 &mu;V or -73 dBm. 50 millionth of a volt is not much and the sinus signal will oscillate between
–25 &mu;V and +25 &mu;V (RMS, not peak to peak), our FET must accept that.
