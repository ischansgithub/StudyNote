# NXP_LPC5410X 用户手册阅读笔记
---
##Input multiplexing (INPUT MUX)

all digital pins of ports 0 and 1 can be selected as inputs to the pin interrupts

The input multiplexing makes it possible to design event-driven processes without CPU intervention by connecting peripherals like the ADC.

##Group GPIO input interrupt (GINT0/1)
The grouped interrupts can wake up the part from sleep, deep-sleep or power-down modes.

##DMA controller
22 channels, 21 of which are connected to peripheral DMA requests. These come from the USART, SPI, and I2C peripherals. One spare channels has no DMA request connected, and can be used for functions such as memory-to-memory moves.

DMA operations can be triggered by on- or off-chip events. Each DMA channel can select one trigger input from 20 sources. Trigger sources include ADC interrupts,Timer interrupts, pin interrupts, and the SCT DMA request lines.

##SCTimer/PWM (SCT0)
Priority is user selectable for each channel (up to eight priority levels).

• Counter/timer features:
– Each SCTimer is configurable as two 16-bit counters or one 32-bit counter.
– Counters clocked by system clock or selected input.
– Configurable as up counters or up-down counters.

• PWM features:
– Counters can be used in conjunction with match registers to toggle outputs and
create time-proportioned PWM signals.
– Up to 8 single-edge or 6 dual-edge PWM outputs with independent duty cycle and
common PWM cycle length.

##Standard counter/timers(CT32B0/1/2/3/4)

For each timer with pin connections, up to 4 external outputs corresponding to match
registers with the following capabilities:
– Set LOW on match.
– Set HIGH on match.
– Toggle on match.
– Do nothing on match.

PWM: for each timer with pin connections, up to 3 match outputs can be used as single edge controlled PWM outputs.

 Applications
• Interval Timer for counting internal events.
• PWM outputs
• Pulse Width Demodulator via Capture inputs.
• Free running timer.

Each Counter/timer is designed to count cycles of the peripheral clock (PCLK) or an externally supplied clock and can optionally generate interrupts or perform other actions at specified timer values based on four match registers. Each counter/timer also includes one capture input to trap the timer value when an input signal transitions, optionally generating an interrupt.

##Windowed Watchdog Timer (WWDT)
The WWDT has no external pins.

The purpose of the Watchdog Timer is to reset or interrupt the microcontroller within a programmable time if it enters an erroneous state. When enabled, a watchdog reset is generated if the user program fails to feed (reload) the Watchdog within a predetermined amount of time.

##Real-Time Clock (RTC)

The RTC contains two timers:
1. The main RTC timer. This 32-bit timer uses a 1 Hz clock and is intended to runcontinuously as a real-time clock. When the timer value reaches a match value, aninterrupt is raised. The alarm interrupt can also wake up the part from any low power mode if enabled.
2. The high-resolution/wake-up timer. This 16-bit timer uses a 1 kHz clock and operates as a one-shot down timer. Once the timer is loaded, it starts counting down to 0 at which point an interrupt is raised. The interrupt can wake up the part from any low power mode if enabled. This timer is intended to be used for timed wake-up from Deep-sleep, power-down, or Deep power-down modes. The high-resolution wake-up timer can be disabled to conserve power if not used.

##Multi-Rate Timer (MRT)

The MRT is not associated with any device pins.





