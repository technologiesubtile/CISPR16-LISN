# CISPR16-LISN
Coming soon : A LISN for 230 VAC line powered equipment up to 6A

![cispr16_lisn_preproduction](https://github.com/user-attachments/assets/3fff20d5-c632-4f67-ab1c-a45b64539c22)

This device will allow testing the conducted emissions for 230 V and 115 V (or even 100 V, Japan) equipment.

Motivation : Up to 60% of electrical equipment in use in european countries, despite carrying the CE label, does not comply with the regulations, for different reasons, either technical or documentation related (applicability of standards etc.). In some cases, manufacturers eliminate components related to safety and electromagnetic compliance, after the preproduction units have passed the certification, to reduce costs.

Electrical measurements are a non-invasive and non-destructuve way of assuring EMC compliance, in contrast to opening the case and checking for components, which often is destructive since an increasing number of household appliances are ultrasonic-welded. A good starting point is the measurement of conducted emissions, since failure of this test usually implies that the test candidate would also fail radiated emissions, and less test equipment is needed for conducted emission measurement.

The task of the LISN is to conduct 50 /60 Hz AC power from the AC outlet to the EUT (equipment under test) but to block RF interferences between the AC input and output, and to couple out the RF produced by the EUT to a spectrum analyzer. It can therefore be considered as a frequency splitter, similarly to what is inside a multiple way speaker or an ADSL splitter.

Conducted emission is often the test to begin with, before measuring radiated emissions in anechoic and reverberation chambers. For verifying the population of components within the device, some test laboratories may also perform X-ray or even computer tomography imaging.

LISNs are commercially available and a renowned manufacturer is Tekbox. While these LISNs have an exceptional build quality, they also come with their price tag, and with some moderate effort they can be homebrew and brought within the specifications of the measurment standard.

The compromise made in this implementation shall be the use of simple coils, in contrast to the coils of the commercial units, that have 3 intermediate taps connected to resistors, to dampen the self-resonance of the coil. While this commercial implementation is a good approach to obtain a very flat 50 Ohm impedance over a wide range, it is not strictly necessary to keep the impedance within the wide (~ 20 %) boundaries of the CISPR16 standard. Finally, since the spectra of the interference caused by the EUT typically extend over some 10s of dB µV, a 15 % deviation from a perfectly flat LISN impedance would not have a noticeable effect on this final result. Therefore we shall adopt a simpler LISN architecture by still using single layer coils for the artificial network, but without the intermediate taps.
