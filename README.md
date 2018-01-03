# node-red-contrib-roc
# ROC Drivers for Node-RED.

This repository contains flows for use in Node-RED for interacting with Emerson Remote Automation Solutions devices that support the ROC protocol. This is a work in progress, with an initial focus on reading and writing TLP's in the FB107 and ROC800 devices. Support for other opcodes and other devices will follow.

The flows don't use any nodes that are not included in Node-RED by default. The goal is to support the ROC Protocol without having to install any additional nodes, meaning it can be done as a copy-and-paste operation from a computer and doesn't require an active internet connection.

# Adding the drivers to a Node-RED flow
