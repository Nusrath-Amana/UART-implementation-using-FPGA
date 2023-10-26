# UART Transceiver Using FPGA

## Overview
This repository contains Verilog code for implementing a UART (Universal Asynchronous Receiver Transmitter) communication protocol in an FPGA. UART is commonly used for serial communication between devices such as FPGAs, microcontrollers, computers, and sensors. This README provides an overview of UART, its functionality, and code examples for implementing UART in an FPGA using Verilog.

## Table of Contents
- [Introduction to UART](#introduction-to-uart)
- [Method](#method)
  - [Sampling Data in an FPGA](#sampling-data-in-an-fpga)
  - [Transmitter](#transmitter)
  - [Serializing Data](#serializing-data)
  - [Receiver](#receiver)
- [UART Implementation in Verilog](#uart-implementation-in-verilog)
- [Testbench](#testbench)

## Introduction to UART
UART is referred to as Universal Asynchronous Serial Transmitter. It is also referred to as Serial Port, COM 
port and RS-232 Interface. UART operates in an asynchronous manner, meaning it does not rely on a 
clock signal to synchronize data transmission. Instead, it sends data one bit at a time over a single wire. 
UART parameters that need to be set for proper communication include:
- Baud Rate: Determines the rate at which data is transmitted.
- Number of Data Bits: Typically set to 7 or 8.
- Parity Bit: Optional error-checking bit.
- Stop Bits: Number of bits at the end of each data frame.
- Flow Control: Optional mechanism for managing data flow.
To recover data correctly it must be sampled, because a clock is not sent along with the data of 
asynchronous interfaces. Moreover, data sampling rate must be eight times faster than the rate of the 
data bits. Hence faster sampling clock can be used.

## Method

### Sampling Data in an FPGA
To receive data correctly in an FPGA, the receiver must sample the data at the right time. In UART, the 
FPGA continuously samples the line and looks for the start bit (a transition from high to low). After 
detecting the start bit, the FPGA waits for one-half of a bit period and then samples the data at regular 
bit intervals based on the baud rate.
The code given below is structured as the shown data stream. This code is for one Start Bit, one Stop Bit, 
eight Data Bits, and no parity. The transmitter modules below both have a signal called o_tx_active 
which is used to infer a tri-state buffer for half-duplex communication. The selection of the duplex mode 
depends on the application.
It needs to be sampled at least eight times faster than the rate of the data bits. This means that 
for an 115200 baud UART, the data needs to be sampled at at least 921.6 KHz (115200 baud * 
8). A faster sampling clock can be used.

### Transmitter
The transmitter converts the 8-bit parallel data into serial data and adds start bit at start of the data 
frame and stop bit at the end of the data frame.

### Serializing Data
Serializing data refers to the process of converting parallel data into a serial bit stream. In a UART 
implementation, this is achieved by sending each bit one at a time in a sequential manner. The data is 
usually transmitted LSB (Least Significant Bit) first, followed by the subsequent bits until all bits are 
transmitted.


### Receiver
The receiver module is responsible for converting the received serial data back into parallel data, performing the reverse process of the transmitter.

# UART Implementation in Verilog
The repository contains Verilog code for implementing UART. 

- [Verilog codes](https://github.com/Nusrath-Amana/UART-implementation-using-FPGA/commit/12f310066228b552daeaceacf767cada46db9be6)

## Testbench
A testbench is provided in the [testbench directory](https://github.com/Nusrath-Amana/UART-implementation-using-FPGA/blob/main/Verilog%20code/testbench%20(1).v) to simulate the functionality of the UART transmitter and receiver. The testbench operates at a Baud Rate of 115200 and is for simulation purposes only.
