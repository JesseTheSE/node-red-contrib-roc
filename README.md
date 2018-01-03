# node-red-contrib-roc
# ROC Drivers for Node-RED.

This repository contains flows for use in Node-RED for interacting with Emerson Remote Automation Solutions devices that support the ROC protocol. This is a work in progress, with an initial focus on reading and writing TLP's in the FB107 and ROC800 devices. Support for other opcodes and other devices will follow.

The flows don't use any nodes that are not included in Node-RED by default. The goal is to support the ROC Protocol without having to install any additional nodes, meaning it can be done as a copy-and-paste operation from a computer and doesn't require an active internet connection.

# Adding the drivers to a Node-RED flow
### Note: This instructions are geared towards the user who has never used github before. There are often other, easier ways to accomplish the following for those already familiar.
### 1) Download files to your computer
Go to the main the main node-red-contrib-roc directory (you're likely there now) and click on the green "Clone or download" button and select "Download ZIP." A ZIP file containing the drivers and dataType list will be downloaded. 

### 2) Import flow to Node-RED
Open the downloaded ZIP file and open the "ROC_drivers.txt" file contained within. Select all of the text (ctrl-a) and copy to your clipboard (ctrl-c). In the Node-Red interface, click on the menu and select "Import">"Clipboard. Paste the contents of your clipboard (ctrl-v) in the window and choose whether you want it to be included in the current flow or a new one. Click "Import" and the drivers will show up where you selected.

### 3) Add the dataTypes.csv file to your device
Copy the dataTypes.csv file to the device running Node-RED. The procedure to do so will vary by device. If using a FreeWave ZumLink programmable radio, you can use the file uploader on the web interface. Navigate your web browser to the IP address of the radio, click "File Upload" at the top, and choose the dataTypes.csv file from your computer. This will upload it to the /ptp/ directory. For other devices, consult the documentation for help in transferring files. Take note of the directory the file was placed in.

### 4) Point Node-RED to the dataTypes.csv file
In the Node-RED flow with the drivers, double-click the node labeled "File In: dataTypes.csv". By default it will be at the top center of the flow. In the filename field, enter the path to the dataTypes.csv file that you copied to the device. If you're on a ZumLink programmable radio and moved the file using the File Upload tool, you won't need to change this.

### 5) Deploy the flow
Click the red "Deploy" button in the Node-RED window and the flow will be copied to the device and run. If steps 1-4 were completed properly, a message saying "Datatypes Loaded" should appear in the debug tab on the right.

# Using the drivers
## Note: These drivers currently only support IP communications. To connect to a ROC via a serial connection, a terminal server must be used. If using ZumLink programmable radios, see "Using the terminal server on a ZumLink Programmable radios" below.
(/ROC_drivers.jpeg)
Each line in the flow contains an inject node, the driver subflow, and a debug node. The inject node sends the IP Address of the target ROC, the IP Port for communication, the target device Group and Address (240, 240 by default), and any information needed for the command. The info tab on each driver subflow gives more detail on additional inputs and how they are formatted. By clicking on the blue button on the inject node, you will send a message to the ROC and the response will be displayed in the debug tab. For read requests, the values requested will be the first output of the subflow node. For writes, a message indicating the write was successful will be the first output. If there is a CRC error or opcode error, it will be sent out the second output of the subflow.

**Any flow using these drivers must load the dataTypes.csv file on startup. The example includes the code to do this as the top line/wire. This must not be removed.**

Once the functionality of the subflow has been understood and tested, the inject nodes and debug nodes can be replaced with the appropriate inputs and outputs. They are included for example display purposes only, but the inject nodes in particular can be useful for showing how to format the input.

# Using the terminal server on a ZumLink programmable radio
If the connection between the ZumLink radio and the ROC is made using a serial connection, the COM Port on the radio must be configured properly. To do this, point a web browser toward xxx.xxx.xxx.xxx/config, where xxx.xxx.xxx.xxx is the IP address of the radio. You may be asked to login with your credentials. Scroll down to the COM1 or COM2 section, depending on which COM port you will be using for ROC communications. COM2 lies in between COM1 and the ethernet port on the radio. You will need to set the mode, baud rate, data bits, parity, and stop bits to match the COM port of the ROC. Choose "Setup" as the handler. For RS232 communications, set the duplex to "Full". For RS485, set to "Half". Take note of the Terminal Server Port.

In the flow that uses the ROC drivers, you'll use the IP address of the radio as the "host" field that gets injected into the driver subflow. The "port" needs to match the terminal server port for the COM port in use.

For more information on using the ZumLink programmable radio, see [the support section of the FreeWave website](www.freewave.com/support) for radio functionality and the [github wiki](https://github.com/FreeWaveTechnologies/ZumIQ/wiki) for programmability and ZumIQ related support.
