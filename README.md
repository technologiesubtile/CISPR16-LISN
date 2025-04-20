# CISPR16-LISN
A LISN for 230 VAC line powered equipment up to 6A

![P4201888](https://github.com/user-attachments/assets/df4e97fd-c0b7-4463-a6cc-72a9f0767e50) ![finishcabled](https://github.com/user-attachments/assets/14c89458-e1be-4569-aa79-b89adeecce67)


This device will allow testing the conducted emissions for 230 V and 115 V (or even 100 V, Japan) equipment.

Motivation : Up to 60% of electrical equipment in use in european countries, despite carrying the CE label, does not comply with the regulations, for different reasons, either technical or documentation related (applicability of standards etc.). In some cases, manufacturers eliminate components related to safety and electromagnetic compliance, after the preproduction units have passed the certification, to reduce costs.

Electrical measurements are a non-invasive and non-destructive way of assuring EMC compliance, in contrast to opening the case for visual inspection, which often is destructive since an increasing number of consumer and household appliances are ultrasonic-welded. A good starting point is the measurement of conducted emissions, since failure of this test usually implies that the test candidate would also fail radiated emissions, and less test equipment is needed for conducted emission measurement.

The task of the LISN is to conduct 50 /60 Hz AC power from the AC outlet to the EUT (equipment under test) and present a defined impedance to the EUT, but to block RF interferences between the AC input and output, and to couple out the RF produced by the EUT to a spectrum analyzer. It can therefore be considered as a frequency splitter, similarly to what is inside a multiple way speaker or an ADSL splitter.

Conducted emission is often the test to begin with, before measuring radiated emissions in test chambers. For verifying the population of components within the device, some test laboratories may also perform X-ray or even computer tomography imaging.

LISNs are commercially available and a renowned manufacturer is Tekbox. While these LISNs have an exceptional build quality, they also come with their price tag, and with some moderate effort they can be homebrew and brought within the specifications of the measurment standard.

The compromise made in this implementation shall be the use of simple coils, in contrast to the coils of the commercial units, that have 3 intermediate taps connected to resistors, to dampen the self-resonance of the coil. While this commercial implementation is a good approach to obtain a very flat 50 Ohm impedance over a wide range, it is not strictly necessary to keep the impedance within the wide (~ 20 %) boundaries of the CISPR16 standard. Finally, since the spectra of the interference caused by the EUT typically extend over some 10s of dB µV, a 15 % deviation from a perfectly flat LISN impedance would not have a noticeable effect on this final result. Therefore we shall adopt a simpler LISN architecture by still using single layer coils for the artificial network, but without the intermediate taps.

![informative_schematics_sm](https://github.com/user-attachments/assets/5ca811f7-7cb7-415b-bc85-efa82a105afc)

The replacement circuit is this (courtesy EEVblog). At the output it is a bit wrong since the spectrum analyzer, itself being a 50 Ohm load, is connected to one branch and the other has to be terminated by an internal 50 ohm resistor.

![kicad_circuit_sm](https://github.com/user-attachments/assets/2f514313-fe99-48f6-8004-219796ece34a)

ABove is shown the circuit diagram on KiCAD. The 50 Ohm problem is not visible on my circuit but will be adressed by the DPDT switch that does the job when connected correspondingly. It assumes the task of connecting one output to the RF output, and to terminate the other via 50 Ohm. If the DPDT rocker switch is in the middle zero position, neither output is connected to the upper RF output nor terminated, and it is the users task to use and/or terminate the signals on the lower RF ouputs. This allows performing arithmetics to distinguish common and differential mode.

![P4201879](https://github.com/user-attachments/assets/dd4885bc-3fc7-4dee-af39-0a8d74f6c2ff) ![P4201878](https://github.com/user-attachments/assets/cfb5b6eb-7c66-4b39-ae2c-7089bd6f81c3)

Above is the cabling of the rocker switch and the 3 RF N-type output connectors.

The big coils are not onboard but connected by clamp terminals. The higher parts count stems from making 8 uF and 5 Ohm resistors by parallel circuit of 2 x 4 uF and 2 x 10 Ohm respectively, adding varistors as overvoltage protection, 2 capacitors in parallel for the outcoupling capacitor because of parasitics, the parts for the artificial hand and the option of having the PE float on 50 uH.

![P4201871](https://github.com/user-attachments/assets/bcdcc090-6d13-4264-927b-2b508ce3e617)  ![P4201870](https://github.com/user-attachments/assets/cdb38440-55e5-4799-bd56-ce7b6ac3c377)

Above is shown the measurement of the component value of an artificial network coil by the series-through 2-port method. The measurement is performed through two 6dB attenuators. Optionally, we could have made a "test fixture" type of calibration that would take into account the air wires, but this is not done here because the leads are also part of the final intended setup. Note that the inductance of the coils drops a little when they are inside the metal cabinet versus the value in free space because the cabinet behaves as a secondary winding with bad coupling. This effect accounts for a drop from 58 uH to 54 uH. For the moment we leave it as is.

![P4201881](https://github.com/user-attachments/assets/bde0ca7c-efca-44be-83c5-5ff68280ec65)

The effect is stronger for the vertically mounted 220 uH line filter coils. They are mounted on 1 cm plastic spacers to limit the effect of the inductance drop to 200 uH.

![tekbox_front_sm](https://github.com/user-attachments/assets/928bf441-2746-43fe-946f-5e7c84e7800e)

The original from which my DIY project is inspired is the image above. Note the switch allowing to let the PE float. What i do not implement is the internal attenuator. Mine will be used permanently with an external one, even 20 dB or more.

In the end we perform the calibration of the LISN as a whole. We perform the measurements as given in the Tekbox manual :

![calibration_setups](https://github.com/user-attachments/assets/c79731e6-b178-4891-85a3-2a092f6ac03e)

The left measurement is a shunt-through measurement of the input impedance. The right is a transmission measurement from DUT to RF. The S21 parameter of the left measurement is then also used as a reference to normalize the S21 measurement of the transmission measurement and obtain the insertion loss 10*log(|S21,trans|/|S21,ref|).

![P4201902](https://github.com/user-attachments/assets/47d27624-2e76-449a-9a24-22d8951756a7) ![P4201901](https://github.com/user-attachments/assets/3ad5e47e-a442-470f-998f-613d6b6504f6)

Above is shown the experimental implementation of the reference and transmission measurement. We extract two calibration curves, the impedance presented to the EUT, and the insertion loss :

![Impedance](https://github.com/user-attachments/assets/3da23c24-34fb-4312-850c-4e65a70f5a4f) ![Insertion](https://github.com/user-attachments/assets/3487a0c1-5284-4825-be3f-35cfc313a1f8)

The result clearly shows a problem between 10 MHz and 30 MHz. The impedance rises to > 100 Ohm and the insertion loss approaches 3 dB. We attribute this to the length of the air wires between the board and the rocker switch, and the effect of their inductance. It would be advantageous to connect the rocker switch to the board with coaxial wires and also to put the terminating resistor for the unused signal directly on the back of the switch instead of re-routing the unused signal back to the board. However, the LISN should be good enough for some precompliance testing. We are not sure whether commercial software such as EMCview would accept the calibration files since they are not within the bounds of the CISPR specification.

A final remark about the capacitors :

![P4201887](https://github.com/user-attachments/assets/9d48455a-4d09-406e-9be9-14737cf1c724) ![P4201884](https://github.com/user-attachments/assets/e4a14d56-5171-4c1f-8543-a0225ffd19ab)

Their terminals are placed somewhat randomly at their bottom and a custom footprint with two pads on each side had been made to place them as straight as possible on the board but this was not exactly possible and therefore 2 capacitors are slightly rotated on the prototype. The capacitors are motor capacitors, polyproylene film, type CBB61. If the LISN is operated via a safety isolation transformer, the mains voltage shall divide onto the two branches, dividing the voltage by 2, and leave a large safety margin in case the indicated rating is not for permanent use.



