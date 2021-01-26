# Intro
This is a repository for prototyping a distributed hardware + software modular control framework for robotics applications. Called the moco (MOdular COntrol) framework until someone comes up with a better name. The concept is very much still in development, so proceed at your own risk.

## Background
This project started out as an attempt to design and build a smarter brushless motor controller, and has since evolved into a new method of doing modular, distributed, robot control. This is what happens when you get stuck inside for months on end...

## Motivation
Most robots have many actuators, sensors and other components that need to be individually read and controlled, usually in coordination. Most "traditional" control strategies rely on a centralized controller having knowledge of the entire robot state and adjusting control commands accordingly. This has the following drawbacks:
* Many communication methods required: Sensors and Actuators use UART, I2C, SPI, PWM and many more to communicate. This requires custom code and wiring setups every time a new sensor is required.
* Many communication rates: Sensors and motors get polled and commanded at highly varying rates. Anywhere from one time commands/checks, to on the order of 1Hz (GPS) to potentially several kHz (incremental encoders). This generally makes it impossible for one controller to deal with communication alone.
* Delay in action on control methods requiring full state. If you wish to have a single controller update based on knowledge of the full robot state, the master must first receive the most current state, make a control decision and then send out updated control commands. (All while potentially having to update other controllers)
* Others...


## The Solution
The proposed solution to these problems is a modular control framework made up of nodes, where each node has the ability to read and write to a bus network. This means all connected nodes have access full robot state knowledge by default and can act accordingly, without a centralized controller coordinating their actions. (Feels like a more hardware based implementation of a ROS-like communication layer). Benefits (hopefully) include:
* Drastically simplified wiring: each node only needs to receive power and CAN bus.
* Easily expandable: Any sensor that can be read and published by a node can be integrated into the network. New nodes can be added without any major network changes.
* Complex control strategies: Each node can rapidly adjust its control based off of knowledge of other joint or sensor states, making this approach lend itself to fast moving multi dof arms, legs and linkages.
* Distributed computing: Each node deals with its own data processing and exposes only useful information to the network, cutting down on unnecessary communication. (For example: A high resolution, fast spinning incremental encoder can be read rapidly, but then published at whatever rate is reasonable for the system)
* Data can enter and exit the network easily: "Edge nodes" would allow for interfacing with any device outside the network. Such as a control station, an internet network etc.
* Others....

Challenges:
* Programing such a network likely requires each node to be programmed. This would lead to updating and debugging code becoming harder and harder. Steps should be taken to ensure this is as painless as possible.
* Every new node must connect to the network in a standardized way. Might potentially require configuration beforehand.
* Something like a CAN bus requires all nodes receive all data (?). Could become a problem with larger/faster updating systems.
* Most processes are quite time dependent and efforts should be made to avoid data clashing to combat time constrained loops from going out of sync with data transmission.
* Likely more that I have not considered yet
