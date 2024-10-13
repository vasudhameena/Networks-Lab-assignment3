
# CS342 Assignment 3 Task 1
Done by Group 37:
1. Adarsh Gupta (220101003)
2. Tanvi Doshi (220101102)
3. Vasudha Meena (220101108)
4. Yash Jain (220101115)

## The Question:

You are working for a tech company developing a sophisticated monitoring and control system for a fleet of autonomous drones. The system needs to support three types of communication:

1. Control Commands: These commands need to be sent from a central server to the drones with minimal delay to ensure real-time responsiveness.

2. Telemetry Data: Drones will periodically send telemetry data (e.g., location, speed, status) back to the central server. This data must be reliable and delivered in order, as it will be used for diagnostics and analysis.

3. File Transfers: Occasionally, drones will need to send large files (such as captured images or video data) to the central server. This requires a protocol that can handle efficient file transfer with low latency and high reliability, such as QUIC (Quick UDP Internet Connections). Your task is to develop the client (drone) and server (control center) application. The system should handle multiple drones simultaneously. 

Additionally, implement a security feature where all messages are encrypted and decrypted using a simple XOR cipher.
 
**Objective**: Develop a tri-mode communication system within a single application:

• Mode 1 (Control Commands): A high-performance mode where the server sends control commands to drones with minimal latency.
• Mode 2 (Telemetry Data): A reliable mode where drones send telemetry data to the server. Ensure data is delivered accurately and in order.
• Mode 3 (File Transfers): Efficiently transfer large files from drones to the server using the QUIC protocol

## How to run
Make sure you have python & pip installed if you want to use File Transfer using QUIC.

In the folder, run
```console
make 
make server
```


## The Implementation
The server provides a user interface to control each of the drone application.

The server provides the following interface:
- quit: To quit the program
- start n: Creates n drone application, ./drone{i} all with a UUID. Each drone application needs to be seperately started in it's own terminal.
- list: Lists all active drones along with their UUID.
- show: Lists last received telemetry data of all drones.
- stop print: This stops printing of telemetry information.
- resume print: This resumes printing of telemetry information.
- clear: Clears the interface.
- [UUID] set [pos/vel] [x/y/z] [data]: This is the implementation of the control commands. The commands will be sent to the particular drone application via the specific port it's active on, via a UDP connection.
- [UUID] set state [Sleeping/Active]: This is implementation of state of drone (another control command). Avalaible states are Flying (Default), Stopped. In Stopped state, the position of the drone does not get updated with time.
- [UUID] fetch: This fetch command is sent over a UDP connection. Following this the video data is sent from the particular drone application to the user application, via a QUIC connection.
- [UUID] show: Shows last received telemetry data of this particular drone.
  
## Example Usage
See Example.png


## Implementation Details

For details about the implementation refer to https://drive.google.com/drive/folders/14G-sVuGipDREOW7os-ooKKKGUgzy5x8-?usp=drive_link


Development TODO:
    - Implement start function DONE
    - Implement user interface, and call to start function DONE
    - Implement help function DONE
    - Control Commands implementation DONE
        Server --> Drone
        so, drone needs to listen on some port, and server needs to send data to that port based on UUID
        Drone listening part DONE
        Server sending part DONE
        Implement command behaviour in Drone DONE
    - Telemetry Data implementation
        Drone --> Server
        so, server has multiple threads for each drone process
        so, on each thread, server needs to listen on some port, and drone needs to send data to that port based on UUID
        Server listening part DONE
        Drone sending port DONE
        Implement saving and updating behaviour in server DONE
        Add encrypt Decrypt DONE
        Implement show and UUID show DONE
        Additional Details about implementation:
        On some thread of client (drone) process, after every defined time interval, data sent to server
        For each drone, we need to start listening at some port (UUID + 1) on a thread
        Server listens and collects the data, dumps it into a file and maintains a vector of
        The server also dumps all the data received in some file with timestamp
    - File Transfer implementation  
        (python3 helper/QUICHelper/examples/http3_client.py --ca-certs helper/QUICHelper/tests/pycacert.pem https://localhost:4433/ --fileName a.out)
        (python3 helper/QUICHelper/examples/http3_server.py --certificate helper/QUICHelper/tests/ssl_cert.pem --private-key helper/QUICHelper/tests/ssl_key.pem --dronePort 8080)
        [UUID] fetch: command sent via UDP connection to drone. This is already setup we will use existing control
            command infrastructure.
        Both server and drone needs to do something
        Drone --> Server
        Server listening part DONE
        Drone sending part DONE
        Behaviour implementation DONE
        Drone Creates it's data subfolder and subfile and stores some random large data in the file and continously updates that file DONE
        EncryptDecrypt DONE
    - Drone Behaviour implementation DONE
    - Implement exit command DONE
    - Update help print from final above thingy DONE

    # Weather Data Client-Server Application

This project is a client-server application that simulates the transmission of weather data using Boost.Asio and Zlib. The client generates random weather data, compresses it using Zlib, and sends it to the server. The server receives the data, prints the client's information, and sends an acknowledgment.

## Features

- **Client:** 
  - Generates random weather data (temperature and humidity).
  - Compresses data using Zlib before sending.
  - Implements TCP Reno congestion control algorithm for dynamic adjustment of sending rate.
  
- **Server:** 
  - Receives weather data from the client.
  - Prints the data and the client's IP and port information.
  - Sends acknowledgment back to the client.

## Requirements

- C++ compiler supporting C++11 or later.
- Boost.Asio library.
- Zlib library.

## Installation

### Dependencies

Ensure you have the following dependencies installed:

- **Boost.Asio**: Used for network communication.
- **Zlib**: Used for compressing the weather data.

On Linux, you can install these using:

```bash
sudo apt-get install libboost-all-dev zlib1g-dev
