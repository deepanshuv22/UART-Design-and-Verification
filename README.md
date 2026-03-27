Overview
This repository contains the Register-Transfer Level (RTL) design of a Universal Asynchronous Receiver-Transmitter (UART) and a comprehensive, fully layered SystemVerilog verification environment. The design is parameterized for flexible clock frequencies and baud rates, ensuring adaptability across different system architectures.

RTL Design Architecture
The hardware implementation is divided into distinct modular components, written in Verilog/SystemVerilog:

uarttx (Transmitter): A finite state machine (FSM) that serializes 8-bit parallel data into a standard UART transmission frame (Start bit, Data bits, Stop bit).

uartrx (Receiver): An FSM responsible for oversampling the rx line, detecting the start bit, and deserializing the incoming frame back into parallel 8-bit data.

uart_top: The top-level wrapper instantiating both the transmitter and receiver, parameterized by default for a 1MHz clock and 9600 baud rate.

SystemVerilog Verification Methodology
The core of this repository is a robust, object-oriented testbench built from scratch in SystemVerilog. It mirrors standard UVM methodologies by utilizing a layered architecture to ensure maximum reusability and scalability.

Testbench Components:
Interface (uart_if): A physical interface bundle used to connect the Design Under Test (DUT) to the testbench.

Transaction: Defines the base data packet, utilizing rand modifiers for data payloads (dintx) and randomized operation types (Read/Write).

Generator: Randomizes transaction packets and pushes them via a mailbox to the Driver.

Driver: Receives transactions and drives the protocol-level pin wiggles onto the virtual interface, handling the specific timing required for UART transmission.

Monitor: Passively observes the virtual interface, captures valid transactions, and routes the extracted data to the Scoreboard.

Scoreboard: Acts as the automated checker. It receives expected data from the Driver and actual sampled data from the Monitor, comparing them to verify functional correctness.

Environment (env): The top-level verification container that instantiates, connects, and orchestrates the Generator, Driver, Monitor, and Scoreboard using SystemVerilog mailboxes and synchronization events.

Simulation & Testing
The environment (tb) automatically executes a sequence of randomized read/write tests. Data consistency is verified on-the-fly by the Scoreboard, which flags DATA MATCHED or DATA MISMATCHED in the simulation console. Waveforms are automatically dumped to a .vcd file for debug analysis.
