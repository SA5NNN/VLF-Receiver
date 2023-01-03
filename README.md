# VLF Receiver

KiCad schematics and docs for a VLF receiver aimed at 17.2 kHz SAQ reception.

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

Paul’s design builds on a design from Broesel, Villach, Austria, that he simplified a bit. The design as a resonance circuit with the
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
Use Paul's design of a ferrite rod as the antenna. Use 3 varicaps and no caps for the tuning. Use 2SK241 as my MOSFET as that is what
my supplier had available.
Use my signal generator for testing at 17.2 kHz. Use my RSPdx as the receiver (50 Ohm input).

## Building

### The antenna
The heart of this beast is the copper wound ferrite rod. It will couple magnetically with the electromagnetic field from SAQ.
The inductance needed I took from Paul's design. He has 82 mH, I was aming for the same (more or less).
The way you do this is by purchasing "enameled" copper wire and wind that around the rod. The copper wire is not enameled, if it where
the enamelign would crack and break off as we wound it. Instead it is in fact some type of plasic coating. The coating is very
important as without it the copper wire would short-circuit and we would not get any inductance at all. There are more expesive double
coated wire, this is for transmission when the voltage is high enough that you can get arcing. We do not need it in this design,
they are also more expensive. The wire has very little voltage and current, so we can use as thing wire as we like. The thinner they
are, the more fiddly everything gets. I opted for DASOL 155-17-339, 0.30 mm diameter, 1 hg roll.
The rod was the largest I could find in material 33, that is MnZn that is excellent at building inductance with each turn of the wire.
It is R33-050-750 at http://www.amidon.de/contents/de/d649.html. This is supposed to have a permiability of 800 wich is fantastic!

I took around 3 dm of wire out to the side to use as a connection, then started to wind the wire by rotating the rod in my hand and
using a finger to stear the wire slowly past the rods length. I wound only the center 2/3 of the rod, thinking the ends should be free
for some reason. According to Paul, this needs quite a few turns, like several hundreds, maybe even close to a thousand! It all
depends on the material. Pauls experience seems to indicate a lot of issues with wrong information when bying etc. I came to the
conclusion that I have to somehow meassure the inductance to see where I was. I sanded off the plastic coating of the end of the wire
then after many many turns sanded off as small part of the wire as I could so I got into electrical connection with the wire. Then
I meassured the inductance at 10 kHz and it was much to small (<30 mH). I decided not to retouch the coating, maybe nail polish
could have resealed the wire? But it appears very unlikely that the test patches I do, should come into contact with eachother and
short out, and even if they do, I can just wind more to compensate. This turned out to work, I never had a short on those small
patches so I believe as long as you deo not do them all the time, it's fine without resealing the coating.

Next I went for my power tool, opening the chuck and tried to insert the end of the rod padded with a bit of paper, it worked!
I tightened enough to remove slack, but put no preassure on the rod as it is pretty fragile.

