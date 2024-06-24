# OpenFrame Overview

The OpenFrame Project Example is a user project designed to showcase how to use the Caravel `OpenFrame` design, which is an alternative user project harness chip that differs from the Caravel and Caravan designs by:

1. Having no integrated SoC on chip
3. Allowing access to all GPIO controls
4. Maximizing the user project area
5. Minimizing the support circuitry to comprise only the padframe, a power-on-reset circuit, and a digital ROM containing the 32-bit project ID

The padframe design and placement of pins matches the Caravel and Caravan chips. The pin types also remain the same, with power and ground pins in the same positions, with the same power domains available. Pins which previously had functions connected with the CPU (flash controller interface, SPI interface, UART) use the same GPIO pads but allocate them to general-purpose I/O for any purpose.

One reason for choosing the Openframe harness is to implement an alternative SoC or implement an SoC with the user project integrated into the same level of hierarchy.

**Following image shows what openframe looks like when it is empty:**

<img width="256" alt="Screenshot 2024-06-24 at 12 53 39 PM" src="https://github.com/efabless/openframe_timer_example/assets/67271180/ff58b58b-b9c8-4d5e-b9bc-bf344355fa80">

## Features

1. 44 configurable GPIOs
2. User area of ~15mm2
3. Supports Digital, analog or mixed signal designs

# openframe_timer_example

This is a very simple example that implements a timer and connects it to the GPIOs

## Installation and setup

First clone the repo

```
git clone https://github.com/efabless/openframe_timer_example.git
cd openframe_timer_example
```

Then download all dependencies

```
make setup
```

## Harden the design

In this case we are going to harden the timer, you will be hardening your own design

```
make user_proj_timer
```

Then after you are done with hardening your design, integrate it inside the openframe wrapper

```
make openframe_project_wrapper
```

## IMPORTANT NOTES

1. While hardening the top level, make sure you have connected your design to power using the power pins on the wrapper
    - You can instantiate the `vccd1_connection` and `vssd1_connection` macros that only contains the vias and nets needed to connect to power pins
2. If you are going to flatten your design on the `openframe_project_wrapper` make sure that you don't buffer the analog pins using standard cells
3. Make sure you run the custom step in openlane that copies the power pins from the template def, if you fail to do this the precheck will fail and your design will not be powered
