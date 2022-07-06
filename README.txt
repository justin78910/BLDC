Justin Elassal
*** BLDC Motor Control Information Guide ***
This folder contains the code to operate my BLDC motor controller circuit.

** This README file will guide you in testing and understanding the code **

Take a look at initLogicSim.png
	How to Open Testing Software
	- I ran the debugger using STELLARIS ICDI.
	- The Watch Window, Logic Analyzer, and ADC should be open. If not, do the following:
		Watch Window... View -> Watch Windows -> Watch 1
		ADC View... Peripherals -> TExaS ADC
		Logic Analyzer... View -> Analysis Windows -> Logic Analyzer
	How to Test
	- In the Watch Window, change the value of HallInput ranging from 0x01 to 0x06.
	- Move the ADC value cursor to change the power setting of the output.
	- Observe the PWM signals for two of the six port pins from PA7-PA2.
	- Continue changing HallInput to observe phase switching.

MCU Requirements
	1. Take ADC input from Potentiometer to control the power sent to motor.
		The MCU will have 10 power settings output as PWM signals to the respective MOSFETs
		controlling the current phase.
	2. Take 5V Hall Sensor readings as input from BLDC motor as motor position readings.
	3. Send on/off signal to six individual MOSFETs, each serving a purpose in phase transition.
		***	In the code, there are three methods of control (2 of which are commented out).
			Method One:   All six outputs drive the motor using positive logic. I used this
					  when using OpAmp gain circuits for these signals.
			Method Two:   All six outputs drive the motor using negative logic. I used this
					  when I replaced the OpAmp gain circuits with fast-switching power
					  MOSFETs for a cleaner signal. This resulted in an issue at startup
					  leading to a temporary short (very bad!). To avoid adding a
					  soft-start capacitor circuit, I used Method Three for reliable startup.
			Method Three: The high-side switching uses positive logic from MCU to avoid
					  shorting the high-power circuit at startup. The PFETs turn on using
					  N-Channel pull-down MOSFETs on their gates. The low-side switching 
					  uses negative logic as a logic '1' to the gain circuit shorts the
					  gate of the low-side N-Channel MOSFETs of the 3-HBridge.
				        