

# Weltuna
 Diagnostic toolkit for Mitsubishi PLC based on SLMP

## Table of contents

- [License](#license)
- [Usage](#usage)
  - [1 Gagharv](#1-gagharv)
    - [1-1 Introduction](#1-1-introduction)
    - [1-2 Data Structure and Socket Interface](#1-2-data-structure-and-socket-interface)
      - [1-2-1 MESSAGE_FRAME_TYPE_T](#1-2-1-message_frame_type_t)
      - [1-2-2 MESSAGE_DATA_CODE_T](#1-2-2-message_data_code_t)
      - [1-2-3 DESTINATION_ADDRESS_T](#1-2-3-destination_address_t)
      - [1-2-4 SocketInterface](#1-2-4-socketinterface)
      - [1-2-5 SLMP_EXCEPTION_CODE_T](#1-2-5-slmp_exception_code_t)
      - [1-2-6 REMOTE_CONTROL_MODE_T](#1-2-6-remote_control_mode_t)
      - [1-2-7 REMOTE_CLEAR_MODE_T](#1-2-7-remote_clear_mode_t)
    - [1-3 TCP](#1-3-tcp)
      - [1-3-1 Constructors](#1-3-1-constructors)
      - [1-3-2 Methods](#1-3-2-methods)
    - [1-4 UDP](#1-4-udp)
      - [1-4-1 Constructors](#1-4-1-constructors)
      - [1-4-2 Methods](#1-4-2-methods)
    - [1-5 DeviceAccessMaster](#1-5-deviceaccessmaster)
      - [1-5-1 Constructors](#1-5-1-constructors)
      - [1-5-2 Properties](#1-5-2-properties)
      - [1-5-3 Methods](#1-5-3-methods)
    - [1-6 RemoteOperationMaster](#1-6-remoteoperationmaster)
      - [1-6-1 Constructors](#1-6-1-constructors)
      - [1-6-2 Properties](#1-6-2-properties)
      - [1-6-3 Methods](#1-6-3-methods)
    - [1-7 SLMPException](#1-7-slmpexception)
      - [1-7-1 Constructors](#1-7-1-constructors)
      - [1-7-2 Properties](#1-7-2-properties)
      - [1-7-3 Methods](#1-7-3-methods)
    - [1-8 Reference](#1-8-reference)
      - [1-8-1 Device Code for Local Device Access](#1-8-1-device-code-for-local-device-access)
      - [1-8-2 Example](#1-8-2-example)
  - [2 Mcvein](#2-mcvein)
    - [2-1 Introduction](#2-1-introduction)
    - [2-2 Install Plug-in](#2-2-install-plug-in)
    - [2-3 Project Management](#2-3-project-management)
      - [2-3-1 New](#2-3-1-new)
      - [2-3-2 Open](#2-3-2-open)
      - [2-3-3 Save](#2-3-3-save)
      - [2-3-4 Save As](#2-3-4-save-as)
    - [2-4 Target Management](#2-4-target-management)
      - [2-4-1 Add](#2-4-1-add)
      - [2-4-2 Remove](#2-4-2-remove)
      - [2-4-3 Property](#2-4-3-property)
      - [2-4-4 Activate](#2-4-4-activate)
    - [2-5 Tool Management](#2-5-tool-management)
      - [2-5-1 Add](#2-5-1-add)
      - [2-5-2 Remove](#2-5-2-remove)
      - [2-5-3 Layout](#2-5-3-layout)
    - [2-6 Operation](#2-6-operation)
      - [2-6-1 Connect](#2-6-1-connect)
      - [2-6-2 Disconnect](#2-6-2-disconnect)
  - [3 Numeros](#3-numeros)
    - [3-1 Introduction](#3-1-introduction)
    - [3-2 Enable](#3-2-enable)
    - [3-3 Device Control](#3-3-device-control)
    - [3-4 Channel Control Panel](#3-4-channel-control-panel)
      - [3-4-1 Monitor](#3-4-1-monitor)
      - [3-4-2 PID Auto Tuning](#3-4-2-pid-auto-tuning)
      - [3-4-3 PID Constants](#3-4-3-pid-constants)

## License

TBD

## Usage

### 1 Gagharv

#### 1-1 Introduction

*Gagharv* is a .Net 5 library which implements the SLMP client, written in C# language.



#### 1-2 Data Structure and Socket Interface

##### 1-2-1 MESSAGE_FRAME_TYPE_T

Namespace: AMEC.PCSoftware.CommunicationProtocol.CrazyHein.SLMP.Message

Specifies the frame type that the SLMP client adopts.

```c#
public enum MESSAGE_FRAME_TYPE_T
```

**Enumeration values**

| Value                 | Description                                                  |
| :-------------------- | ------------------------------------------------------------ |
| MC_3E                 | The message format is the same as the QnA-compatible 3E frame in MC protocol.<br />・A number 121 or higher cannot be set to the request destination station No. |
| MC_4E                 | The message format is the same as the QnA-compatible 4E frame in MC protocol.<br />4E frame is the message format that extends 3E frame and corresponds to the serial No.<br />・A number 121 or higher cannot be set to the request destination station No. |
| STATION_NUM_EXTENSION | The message format that extends 4E frame and corresponds to only CC-Link IE TSN.<br/>・A number 121 or higher can be set to the request destination station No.<br/>・A device that is not supported by the station number extension frame cannot send, receive, or relay the message using the station number extension frame. |



##### 1-2-2 MESSAGE_DATA_CODE_T

Namespace: AMEC.PCSoftware.CommunicationProtocol.CrazyHein.SLMP.Message

Specifies the communication data code that the SLMP client adopts.

```C#
public enum MESSAGE_DATA_CODE_T
```

**Enumeration values**

| Value  | Description |
| ------ | ----------- |
| ASCII  | ASCII code  |
| BINARY | Binary code |

**Remarks**

When using binary codes, the communication time will decrease since the amount of communication data is reduced by approximately half comparing to using ASCII codes.



##### 1-2-3 DESTINATION_ADDRESS_T

Namespace: AMEC.PCSoftware.CommunicationProtocol.CrazyHein.SLMP.Message

Specifies the detailed address information corresponding to the access destination.

```c#
public readonly struct DESTINATION_ADDRESS_T
{
	public readonly byte network_number;
	public readonly byte station_number;
	public readonly ushort module_io;
	public readonly byte multidrop_number;
	public readonly ushort extension_station_number;
}
```

**Fields**

| Field                    | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| network_number           | Request destination network No.<br />**00H**: Connected network<br />**01H to EFH**:  Network No. The stations of network No.240 to 255 cannot be accessed. |
| station_number           | Request destination station No.<br />**FFH**: Connected station<br />**01H to 78H**: Station No.<br />**7CH**: Set a station No.121 or higher at the area of the request destination extension station No. of the station number extension frame <br />**7DH**: Assigned control station/Master station <br />**7EH**: Present control station/Master station |
| module_io                | Specify the module of the access destination.<br />**03FFH**: CPU module - Own station<br />**03FFH**: CPU module - Control CPU<br />**03E0H**: CPU module - Multiple system CPU No.1<br />**03E1H**: CPU module - Multiple system CPU No.2<br />**03E2H**: CPU module - Multiple system CPU No.3<br />**03E3H**: CPU module - Multiple system CPU No.4<br />**0000H to 01FFH**: CPU module - Multidrop connection station via a CPU module in multidrop connection<sup><a href="#multidrop-relayed-station-no">[1]</a></sup><br />**03D0H**: CPU module - Control system CPU<br />**03D1H**: CPU module - Standby system CPU<br />**03D2H**: CPU module - System A CPU<br />**03D3H**: CPU module - System B CPU<br />**03FFH**: CC-Link IE Field Network remote head module - Own station<br />**03E0H**: CC-Link IE Field Network remote head module - Module No.1<br />**03E1H**: CC-Link IE Field Network remote head module - Module No.1<br />**0000H to 01FFH**: CC-Link IE Field Network remote head module - Multidrop connection station via a CPU module in multidrop connection<sup><a href="#multidrop-relayed-station-no">[1]</a></sup><br />**03D0H**: CC-Link IE Field Network remote head module - Control system<br />**03D1H**: CC-Link IE Field Network remote head module - Standby system |
| multidrop_number         | Request destination multidrop station No.<br />**00H to 1FH**: Station No. of multidrop connection station<br />**00H**: The station that relays the multidrop connection and network<br />**00H**: Station other than the multidrop connection station |
| extension_station_number | Request destination extension station No. (only for station number extension frame)<br />**0000H**: Specify the station No. at the area of the request destination station No.<br />**0000H**: Assigned control station/Master station<br />**0001H to FFFEH**: Station No.<br />**FFFFH**: Own station |

<p id="multidrop-relayed-station-no" name="multidrop-relayed-station-no">[1] When the CPU module in multidrop connection is relayed, specify the value in 4 digits (hexadecimal) obtained by dividing the I/O No. of the serial communication module of the multidrop connection source by 16.</p>

**Remarks**

Please refer to ***SLMP reference Manual*** for a comprehensive explanation.



##### 1-2-4 SocketInterface

Namespace: AMEC.PCSoftware.CommunicationProtocol.CrazyHein.SLMP.IOUtility

Represents a socket interface which is typically used to send and receive byte arrays.

```c#
public interface SocketInterface: IDisposable
{
    int Send(byte[] buffer, int offset, int size, SocketFlags socketFlags = SocketFlags.None);
    int Receive(byte[] buffer, int offset, int size, SocketFlags socketFlags = SocketFlags.None);
    string Name();
}
```

**Remarks**

The library build-in [UDP](#1-4-udp) class  and [TCP](#1-3-tcp) class implement the interface.



##### 1-2-5 SLMP_EXCEPTION_CODE_T

Namespace: AMEC.PCSoftware.CommunicationProtocol.CrazyHein.SLMP

Represents the error code and error name of the exception.

```c#
 public enum SLMP_EXCEPTION_CODE_T: UInt32
```

**Enumeration values**

| Value                                  | Code       | Description |
| :------------------------------------- | ---------- | ----------- |
| NO_ERROR                               | 0x00000000 |             |
| RUNTIME_ERROR                          | 0xFFFFFFFF |             |
| INVALID_SUBHEADER                      | 0x00000001 |             |
| INVALID_DATA_CODE                      | 0x00000002 |             |
| INVALID_FRAME_TYPE                     | 0x00000003 |             |
| MESSAGE_FRAME_CORRUPTED                | 0x00000004 |             |
| INVALID_COMMAND_CODE                   | 0x00000010 |             |
| COMMAND_MESSAGE_CORRUPTED              | 0x00000011 |             |
| DEVICE_ACCESS_OUT_OF_HEAD_RANGE        | 0x00000020 |             |
| INVALID_DEVICE_CODE                    | 0x00000021 |             |
| INVALID_DEVICE_INDIRECT_SPECIFICATION  | 0x00000023 |             |
| INVALID_DEVICE_EXTENSION_SPECIFICATION | 0x00000024 |             |
| INVALID_DEVICE_EXTENSION_MODIFICATION  | 0x00000025 |             |
| INVALID_DEVICE_MODIFICATION            | 0x00000026 |             |
| DEVICE_REGISTER_DATA_CORRUPTED         | 0x00000027 |             |
| INVALID_DEVICE_REGISTER_DATA           | 0x00000028 |             |
| REMOTE_STATION_DISCONNECTED            | 0x00000030 |             |
| INVALID_REMOTE_OPERATION               | 0x00000040 |             |
| INVALID_MODEL_CODE                     | 0x00000041 |             |
| RECEIVED_UNMATCHED_MESSAGE             | 0x00000080 |             |
| INSUFFICIENT_DATA_ARRAY_BUFFER         | 0x000000F0 |             |
| INVALID_ASCII_CODE_VALUE               | 0x000000F1 |             |



##### 1-2-6 REMOTE_CONTROL_MODE_T

Namespace: AMEC.PCSoftware.CommunicationProtocol.CrazyHein.SLMP.Command

Specifies whether the remote operation can be executed forcibly by the device other than the external device which performed the remote STOP/remote PAUSE. If the forced execution is not allowed, remote operation can be executed only by the external device which performed the remote STOP/remote PAUSE.

```c#
public enum REMOTE_CONTROL_MODE_T:ushort
```

**Enumeration values**

| Value                        | Description                                                  |
| ---------------------------- | ------------------------------------------------------------ |
| FORCED_EXECUTION_NOT_ALLOWED | Forced execution not allowed. (Remote operation cannot be executed when other device is performing the remote STOP/remote PAUSE.) |
| FORCED_EXECUTION_ALLOWED     | Forced execution allowed. (Remote operation can be executed even when other device is performing the remote STOP/remote PAUSE.) |



##### 1-2-7 REMOTE_CLEAR_MODE_T

Namespace: AMEC.PCSoftware.CommunicationProtocol.CrazyHein.SLMP.Command

Specifies whether the clear (initialization) processing of device is executed or not when starting the calculation for the remote RUN. The device which received the remote RUN request turns to the RUN state after the clear (initialization) processing of device.
When the device initial value is set in the parameters of the CPU module, the clear (initialization) processing of device is executed according to the setting.

```c#
public enum REMOTE_CLEAR_MODE_T : byte
```

**Enumeration values**

| Value                       | Description                                          |
| --------------------------- | ---------------------------------------------------- |
| DO_NOT_CLEAR_DEVICE         | Do not clear the device.                             |
| CLEAR_DEVICE_EXCEPT_LATCHED | Clear all devices except that in the latch range.    |
| CLEAR_ALL_DEVICE            | Clear all devices including that in the latch range. |



#### 1-3 TCP

Namespace: AMEC.PCSoftware.CommunicationProtocol.CrazyHein.SLMP.IOUtility

Implements the [SocketInterface](#1-2-4-socketinterface) via TCP.

```c#
public class TCP : SocketInterface
```



##### 1-3-1 Constructors

**TCP(IPEndPoint, IPEndPoint, Int32, Int32)**

Initializes a new instance of the [TCP](#1-3-tcp) class using specific local endpoint, destination endpoint, send timeout and receive timeout.

```c#
public TCP(IPEndPoint source, IPEndPoint destination, int sendTimeout, int receiveTimeout)
```

​	**Parameters**

​	**`source`**  [IPEndPoint](https://docs.microsoft.com/en-us/dotnet/api/system.net.ipendpoint?view=net-5.0) 

​	The local [IPEndPoint](https://docs.microsoft.com/en-us/dotnet/api/system.net.ipendpoint?view=net-5.0) to associate with the [TCP](#1-3-tcp). If the parameter is null, the underlying service provider will assign the local network address and port 	number when you call [Connect](#1-3-2-4-connect) method.

​	**`destination`** [IPEndPoint](https://docs.microsoft.com/en-us/dotnet/api/system.net.ipendpoint?view=net-5.0) 

​	The [IPEndPoint](https://docs.microsoft.com/en-us/dotnet/api/system.net.endpoint?view=net-5.0) that represents the destination for the data.

​	**`sendTimeout`** Int32

​	Specifies the amount of time in milliseconds after which a synchronous [Send](#1-3-2-1-send) call will time out.

​	**`receiveTimeout`** Int32

​	Specifies the amount of time in milliseconds after which a synchronous [Receive](#1-3-2-2-receive) call will time out.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)

​	

##### 1-3-2 Methods

###### 1-3-2-1 Send

Sends the specified number of bytes of data to a socket interface, starting at the specified offset, and using the specified [SocketFlags](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socketflags?view=net-5.0).

```c#
public int Send(byte[] buffer, int offset, int size, SocketFlags socketFlags = SocketFlags.None)
```

​	**Parameters**

​	**`buffer`** Byte[]

​	An array of type byte that contains the data to be sent.

​	**`offset`** Int32

​	The position in the data buffer at which to begin sending data.

​	**`size`** Int32

​	The number of bytes to send.

​	**`	socketFlags`**  [SocketFlags](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socketflags?view=net-5.0) 			 		

​	A bitwise combination of the [SocketFlags](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socketflags?view=net-5.0) values.

​	**Returns**

​	Int32

​	The number of bytes sent.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)

​	**Remarks**

​	The method will block until the requested number of bytes are sent. 

​	If the time-out value was exceeded, the method will throw a [SLMPException](#1-7-slmpexception).



###### 1-3-2-2 Receive

Receives the specified number of bytes from a bound socket into the specified offset position of the receive buffer, using the specified [SocketFlags](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socketflags?view=net-5.0).

```c#
public int Receive(byte[] buffer, int offset, int size, SocketFlags socketFlags = SocketFlags.None)
```

​	**Parameters**

​	**`buffer`** Byte[]

​	An array of type byte that is the storage location for received data.

​	**`offset`** Int32

​	The location in buffer to store the received data.

​	**`size`** Int32

​	The number of bytes to receive.

​	**`	socketFlags`**  [SocketFlags](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socketflags?view=net-5.0) 			 		

​	A bitwise combination of the [SocketFlags](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socketflags?view=net-5.0) values.

​	**Returns**

​	Int32

​	The number of bytes received.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)

​	**Remarks**

​	The method will read as much data as is available, up to the number of bytes specified by the size parameter. 

​	If no data is available for reading, the method will block until data is available. 

​	If the time-out value was exceeded, the [Receive](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socket.receive?view=net-5.0) call will throw a [SLMPException](#1-7-slmpexception).



###### 1-3-2-3 Name

Returns the name of the socket interface.

```c#
public string Name()
```

​	**Returns**

​	[String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​	The name of the socket interface. The default value is "TCP".



###### 1-3-2-4 Connect

Establishes a connection to a destination endpoint specified in [Constructors](#1-3-1-constructors)

```c#
public void Connect()
```

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)

​	**Remarks**

​	The method will block and synchronously establishes a network connection between local and destination [IPEndPoint](https://docs.microsoft.com/zh-cn/dotnet/api/system.net.ipendpoint?view=net-5.0) specified in [Constructors](#1-3-1-constructors).



#### 1-4 UDP

Namespace: AMEC.PCSoftware.CommunicationProtocol.CrazyHein.SLMP.IOUtility

Implements the [SocketInterface](#1-2-4-socketinterface) via UDP.

```c#
public class UDP : SocketInterface
```



##### 1-4-1 Constructors

**UDP(IPEndPoint, IPEndPoint, Int32, Int32, Int32)**

Initializes a new instance of the [UDP](#1-4-udp) class using specific local endpoint, destination endpoint, receive buffer size, send timeout and receive timeout.

```c#
public UDP(IPEndPoint source, IPEndPoint destination, int internalBufferSize, int sendTimeout, int receiveTimeout)
```

​	**Parameters**

​	**`source`**  [IPEndPoint](https://docs.microsoft.com/en-us/dotnet/api/system.net.ipendpoint?view=net-5.0) 

​	The local [IPEndPoint](https://docs.microsoft.com/en-us/dotnet/api/system.net.ipendpoint?view=net-5.0) to associate with the [UDP](#1-4-udp).

​	**`destination`** [IPEndPoint](https://docs.microsoft.com/en-us/dotnet/api/system.net.ipendpoint?view=net-5.0) 

​	The [IPEndPoint](https://docs.microsoft.com/en-us/dotnet/api/system.net.endpoint?view=net-5.0) that represents the destination for the data.

​	**`internalBufferSize`** Int32

​	An Int32 that contains the size, in bytes, of the receive buffer. 

​	**`sendTimeout`** Int32

​	Specifies the amount of time in milliseconds after which a synchronous [Send](#1-3-2-1-send) call will time out.

​	**`receiveTimeout`** Int32

​	Specifies the amount of time in milliseconds after which a synchronous [Receive](#1-3-2-2-receive) call will time out.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)

​	

##### 1-4-2 Methods

###### 1-4-2-1 Send

Sends the specified number of bytes of data to a socket interface, starting at the specified offset, and using the specified [SocketFlags](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socketflags?view=net-5.0).

```c#
public int Send(byte[] buffer, int offset, int size, SocketFlags socketFlags = SocketFlags.None)
```

​	**Parameters**

​	**`buffer`** Byte[]

​	An array of type byte that contains the data to be sent.

​	**`offset`** Int32

​	The position in the data buffer at which to begin sending data.

​	**`size`** Int32

​	The number of bytes to send.

​	**`	socketFlags`**  [SocketFlags](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socketflags?view=net-5.0) 			 		

​	A bitwise combination of the [SocketFlags](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socketflags?view=net-5.0) values.

​	**Returns**

​	Int32

​	The number of bytes sent.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)

​	**Remarks**

​	The method will block until the requested number of bytes are sent. 

​	If the time-out value was exceeded, the method will throw a [SLMPException](#1-7-slmpexception).



###### 1-4-2-2 Receive

Receives the specified number of bytes from a bound socket into the specified offset position of the receive buffer, using the specified [SocketFlags](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socketflags?view=net-5.0).

```c#
public int Receive(byte[] buffer, int offset, int size, SocketFlags socketFlags = SocketFlags.None)
```

​	**Parameters**

​	**`buffer`** Byte[]

​	An array of type byte that is the storage location for received data.

​	**`offset`** Int32

​	The location in buffer to store the received data.

​	**`size`** Int32

​	The number of bytes to receive.

​	**`	socketFlags`**  [SocketFlags](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socketflags?view=net-5.0) 			 		

​	A bitwise combination of the [SocketFlags](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socketflags?view=net-5.0) values.

​	**Returns**

​	Int32

​	The number of bytes received.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)

​	**Remarks**

​	The method will read as much data as is available, up to the number of bytes specified by the size parameter. 

​	If no data is available for reading, the method will block until data is available. 

​	If the time-out value was exceeded, the [Receive](https://docs.microsoft.com/en-us/dotnet/api/system.net.sockets.socket.receive?view=net-5.0) call will throw a [SLMPException](#1-7-slmpexception).



###### 1-4-2-3 Name

Returns the name of the socket interface.

```c#
public string Name()
```

​	**Returns**

​	[String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​	The name of the socket interface. The default value is "UDP".



#### 1-5 DeviceAccessMaster

Namespace: AMEC.PCSoftware.CommunicationProtocol.CrazyHein.SLMP.Master

Implements the ***Device Access*** client of SLMP.

```c#
public class DeviceAccessMaster
```

Supported device access commands:

| Operation    | Command Code | Description                                                  |
| ------------ | ------------ | ------------------------------------------------------------ |
| Read         | 0x0401       | Reads value from the bit devices (consecutive device No.) in 1-point units.<br />Reads value from the bit devices (consecutive device No.) in 16-point units.<br />Reads value from the word devices (consecutive device No.) in one-word units. |
| Write        | 0x1401       | Writes value to the bit devices (consecutive device No.) in 1-point units.<br />Writes value to the bit devices (consecutive device No.) in 16-point units.<br />Writes value to the word devices (consecutive device No.) in one-word units. |
| Read Random  | 0x0403       | Specifies the device No. and reads the device value. This can be specified with inconsecutive device No.<br />Reads value from the word devices in one-word units or two-word units. |
| Write Random | 0x1402       | Specifies the device No. to bit device in 1-point units and writes value. This can be specified with inconsecutive device No.<br />Specifies the device No. to bit device in 16-point units and writes value. This can be specified with inconsecutive device No.<br />Specifies the device No. to word device in one-word units or two-word units and writes value. This can be specified with inconsecutive device No. |
| Read Block   | 0x0406       | Reads data by treating n points of word devices or bit devices (one point is equivalent to 16 bits) as one block and specifying multiple blocks. This can be specified with inconsecutive device No. |
| Write Block  | 0x1406       | Writes data by treating n points of word devices or bit devices (one point is equivalent to 16 bits) as one block and specifying multiple blocks. This can be specified with inconsecutive device No. |



##### 1-5-1 Constructors

**DeviceAccessMaster(MESSAGE_FRAME_TYPE_T, MESSAGE_DATA_CODE_T, Boolean, SocketInterface, ref DESTINATION_ADDRESS_T, Int32, Int32, Object)**

Initializes a new instance of the [DeviceAccessMaster](#1-5-deviceaccessmaster) class.

```c#
public DeviceAccessMaster(MESSAGE_FRAME_TYPE_T frameType, 
                          MESSAGE_DATA_CODE_T dataCode, 
                          bool dedicationR, 
                          SocketInterface sc, 
                          ref DESTINATION_ADDRESS_T destination, 
                          int sendBufferSize = 4096, int receiveBufferSize = 4096, object sync = null)
```

​	**Parameters**

​	**`frameType`** [MESSAGE_FRAME_TYPE_T](#1-2-1-message_frame_type_t)

​	Specifies the frame type that the [DeviceAccessMaster](#1-5-deviceaccessmaster) adopts.

​	**`dataCode`** [MESSAGE_DATA_CODE_T](#1-2-2-message_data_code_t)

​	Specifies the communication data code that the [DeviceAccessMaster](#1-5-deviceaccessmaster) adopts.

​	**`dedicationR`** bool

​	Specifies whether to active R dedicated message format or not.

​	`true`: R dedicated message format.

​	`false`: Q/L compatible message format.

​	**`sc`** [SocketInterface](#1-2-4-socketinterface)

​	Specifies the communication interface that the [DeviceAccessMaster](#1-5-deviceaccessmaster) associates with.

​	**`destination`** [DESTINATION_ADDRESS_T](#1-2-3-destination_address_t)

​	Specifies  the remote controller station.

​	**`sendBufferSize`** Int32

​	An Int32 that contains the size, in bytes, of the send buffer. 

​	**`receiveBufferSize`** Int32

​	An Int32 that contains the size, in bytes, of the receive buffer. 

​	**`sync`** [Object](https://docs.microsoft.com/en-us/dotnet/api/system.object?view=net-5.0)

​	This object is used as a mutex variable that prevents data corruption by simultaneous device access.

​	If this parameter is null,  the constructor will create a new instance for you.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



##### 1-5-2 Properties

###### 1-5-2-1 COM

Set a socket interface that the [DeviceAccessMaster](#1-5-deviceaccessmaster) associates with.

```c#
public SocketInterface COM {set;}
```

​	**Property Value**

​	[SocketInterface](#1-2-4-socketinterface)



##### 1-5-3 Methods

Notes:

All methods here have a asynchronous version,  the method name of asynchronous version ends in "Async".

###### 1-5-3-1 ReadLocalDeviceInWord

**Overloads**

------

**ReadLocalDeviceInWord(UInt16, String, UInt32, UInt16, out UInt16, Span\<UInt16\>)**

Reads value from the bit devices (consecutive device No.) in 16-point units.

Reads value from the word devices (consecutive device No.) in one-word units.

```c#
public void ReadLocalDeviceInWord(ushort monitoringTimer, string deviceCode, uint headDevice, ushort devicePoints, out ushort endCode, Span<ushort> data)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`deviceCode`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​	Specify the type of the target device of reading. Refer to [Device Code for Local Device Access](#1-8-1-device-code-for-local-device-access) for details.

​	**`headDevice`** UInt32

​	Specify the head No. of the target device of reading. 

​	**`devicePoints`** UInt16

​	Specify the number of target device points of reading.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	When normally completed, 0 is stored. When failed, an error code of the access destination is stored. Refer to SLMP-Compatible Device Manual.

​	**`data`** [Span\<UInt16\>](https://docs.microsoft.com/en-us/dotnet/api/system.span-1?view=net-5.0)

​	The storage location for data read from devices.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)

​	

**ReadLocalDeviceInWord(UInt16, IEnumerable<(String, UInt32)>, IEnumerable<(String, UInt32)>, out UInt16, Span\<UInt16\> , Span\<UInt32\>)**

Specifies the device No. and reads the device value. This can be specified with inconsecutive device No. 

Reads value from the word devices in one-word units or two-word units.

```c#
public void ReadLocalDeviceInWord(ushort monitoringTimer, 
                                  IEnumerable<(string deviceCode, uint headDevice)> wordDevice, 
                                  IEnumerable<(string deviceCode, uint headDevice)> dwordDevice,
                                  out ushort endCode, Span<ushort> worddata, Span<uint> dworddata)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`wordDevice`** IEnumerable<(String deviceCode, UInt32 headDevice)>

​	**`dwordDevice`** IEnumerable<(String deviceCode, UInt32 headDevice)>

​	Specify the device to be read in order from the word access to the double-word access.

​			**`deviceCode`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​			Specify the type of the target device of reading. Refer to [Device Code for Local Device Access](#1-8-1-device-code-for-local-device-access) for details.

​			**`headDevice`** UInt32

​			Specify the head No. of the target device of reading. 

​	**`endCode`** UInt16

​	The command processing result is stored.

​	**`worddata`** [Span\<UInt16\>](https://docs.microsoft.com/en-us/dotnet/api/system.span-1?view=net-5.0)

​	The storage location for data of word access read from devices. The parameter can be null if **`wordDevice`** is null.

​	**`worddata`** [Span\<UInt32\>](https://docs.microsoft.com/en-us/dotnet/api/system.span-1?view=net-5.0)

​	The storage location for data of double-word access read from devices. The parameter can be null if **`dwordDevice`** is null.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



**ReadLocalDeviceInWord(UInt16,** **IEnumerable<(String, UInt32, UInt16)>,** **IEnumerable<(String, UInt32, UInt16)>, ** **out UInt16, Memory\<UInt16\>\[\], Memory\<UInt16\>\[\])** 

Reads data by treating n points of word devices or bit devices (one point is equivalent to 16 bits) as one block and specifying multiple blocks. This can be specified with inconsecutive device No.

```c#
public void ReadLocalDeviceInWord(ushort monitoringTimer, 
                                  IEnumerable<(string deviceCode, uint headDevice, ushort devicePoints)> wordDeviceBlock,
                                  IEnumerable<(string deviceCode, uint headDevice, ushort devicePoints)> bitDeviceBlock,
                                  out ushort endCode, Memory<ushort>[] wordDataBlock, Memory<ushort>[] bitDataBlock)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`wordDeviceBlock`** IEnumerable<(String deviceCode, UInt32 headDevice, UInt16 devicePoints)>

​	**`bitDeviceBlock`** IEnumerable<(String deviceCode, UInt32 headDevice, UInt16 devicePoints)>

​	Specify the target device of reading.

​		**`deviceCode`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​		Specify the type of the target device of reading. Refer to [Device Code for Local Device Access](#1-8-1-device-code-for-local-device-access) for details.

​		**`headDevice`** UInt32

​		Specify the head No. of the target device of reading. 

​		**`devicePoints`** UInt16

​		Specify the number of target device points of reading.

​	**`wordDataBlock`** Memory\<UInt16\>\[\]

​	The storage location for data read from specified word device blocks. The parameter can be null if **`wordDeviceBlock`** is null.

​	**`bitDataBlock`** Memory\<UInt16\>\[\]

​	The storage location for data read from specified bit device blocks.  The parameter can be null if **`bitDeviceBlock`** is null.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



###### 1-5-3-2 ReadLocalDeviceInBit

Reads value from the bit devices (consecutive device No.) in 1-point units.

```c#
public void ReadLocalDeviceInBit(ushort monitoringTimer, string deviceCode, uint headDevice, ushort devicePoints, out ushort endCode, Span<byte> data)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`deviceCode`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​	Specify the type of the target device of reading. Refer to [Device Code for Local Device Access](#1-8-1-device-code-for-local-device-access) for details.

​	**`headDevice`** UInt32

​	Specify the head No. of the target device of reading. 

​	**`devicePoints`** UInt16

​	Specify the number of target device points of reading.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	When normally completed, 0 is stored. When failed, an error code of the access destination is stored. Refer to SLMP-Compatible Device Manual.

​	**`data`** [Span\<Byte\>](https://docs.microsoft.com/en-us/dotnet/api/system.span-1?view=net-5.0)

​	The storage location for data read from devices. 

​	Data is stored as the specified number of device points from the specified head device from the upper bit. 

​	One bit device data occupies one byte. ON is expressed as 01H (1) and OFF is expressed as 00H (0). 

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



###### 1-5-3-3 ReadModuleAccessDeviceInWord

**Overloads**

------

**ReadModuleAccessDeviceInWord(UInt16, string, UInt32, UInt16, out UInt16, Span\<UInt16\>)**

Reads value from the buffer memory of SLMP-Compatible devices or intelligent function modules (consecutive device No.) in one-word units.

```c#
public void ReadModuleAccessDeviceInWord(ushort monitoringTimer, string extensionSepcification, uint headDevice, ushort devicePoints, out ushort endCode, Span<ushort> data)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`extensionSepcification`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​	Specify the start I/O number in hexadecimal (upper 3-digit ASCII code) starting with 'U'. 

​	**`headDevice`** UInt32

​	Specify the head No. of the target device of reading. 

​	**`devicePoints`** UInt16

​	Specify the number of target device points of reading.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	When normally completed, 0 is stored. When failed, an error code of the access destination is stored. Refer to SLMP-Compatible Device Manual.

​	**`data`** [Span\<UInt16\>](https://docs.microsoft.com/en-us/dotnet/api/system.span-1?view=net-5.0)

​	The storage location for data read from devices.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



**ReadModuleAccessDeviceInWord(UInt16, IEnumerable<(string, UInt32)>, IEnumerable<(String, UInt32)>,  out UInt16, Span\<UInt16\>, Span\<UInt32\>)**

Specifies the device No. and reads the device value. This can be specified with inconsecutive device No. 

Reads value from the buffer memory of SLMP-Compatible devices or intelligent function modules in one-word units or two-word units.

```c#
public void ReadModuleAccessDeviceInWord(ushort monitoringTimer, 
                                         IEnumerable<(string extensionSpecification, uint headDevice)> wordDevice,
                                         IEnumerable<(string extensionSpecification, uint headDevice)> dwordDevice,
                                         out ushort endCode, Span<ushort> worddata, Span<uint> dworddata)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`wordDevice`** IEnumerable<(String extensionSpecification, UInt32 headDevice)>

​	**`dwordDevice`** IEnumerable<(String extensionSpecification, UInt32 headDevice)>

​	Specify the device to be read in order from the word access to the double-word access.

​			**`extensionSepcification`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​			Specify the start I/O number in hexadecimal (upper 3-digit ASCII code) starting with 'U'. 

​			**`headDevice`** UInt32

​			Specify the head No. of the target device of reading. 

​	**`endCode`** UInt16

​	The command processing result is stored.

​	**`worddata`** [Span\<UInt16\>](https://docs.microsoft.com/en-us/dotnet/api/system.span-1?view=net-5.0)

​	The storage location for data of word access read from devices. The parameter can be null if **`wordDevice`** is null.

​	**`worddata`** [Span\<UInt32\>](https://docs.microsoft.com/en-us/dotnet/api/system.span-1?view=net-5.0)

​	The storage location for data of double-word access read from devices. The parameter can be null if **`dwordDevice`** is null.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



**ReadModuleAccessDeviceInWord(UInt16, IEnumerable<(String, UInt32, UInt16)>,  out UInt16, Memory\<UInt16\>\[\])**

Reads the buffer memory of SLMP-Compatible devices or intelligent function modules by treating n points of word devices as one block and specifying multiple blocks. This can be specified with inconsecutive device No.

```c#
public void ReadModuleAccessDeviceInWord(ushort monitoringTimer, 
                           IEnumerable<(string extensionSpecification, uint headDevice, ushort devicePoints)> wordDeviceBlock,
                           out ushort endCode, Memory<ushort>[] wordDataBlock)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`wordDevice`** IEnumerable<(String extensionSpecification, UInt32 headDevice, UInt16 devicePoints)>

​	**`dwordDevice`** IEnumerable<(String extensionSpecification, UInt32 headDevice, UInt16 devicePoints))>

​	Specify the device to be read in order from the word access to the double-word access.

​			**`extensionSepcification`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​			Specify the start I/O number in hexadecimal (upper 3-digit ASCII code) starting with 'U'. 

​			**`headDevice`** UInt32

​			Specify the head No. of the target device of reading. 

​			**`devicePoints`** UInt16

​			Specify the number of target device points of reading.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	**`wordDataBlock`** [Memory\<UInt16\>](https://docs.microsoft.com/en-us/dotnet/api/system.memory-1?view=net-5.0)\[\]

​	The storage location for data read from specified word device blocks.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



###### 1-5-3-4 WriteLocalDeviceInWord

**Overloads**

------

**WriteLocalDeviceInWord(UInt16, String, UInt32, UInt16, out UInt16, ReadOnlySpan\<UInt16\>)**

Writes value to the bit devices (consecutive device No.) in 16-point units.
Writes value to the word devices (consecutive device No.) in one-word units.

```c#
public void WriteLocalDeviceInWord(ushort monitoringTimer, string deviceCode, uint headDevice, ushort devicePoints, out ushort endCode, ReadonlySpan<ushort> data)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`deviceCode`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​	Specify the type of the target device of writing. Refer to [Device Code for Local Device Access](#1-8-1-device-code-for-local-device-access) for details.

​	**`headDevice`** UInt32

​	Specify the head No. of the target device of writing. 

​	**`devicePoints`** UInt16

​	Specify the number of target device points of writing.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	When normally completed, 0 is stored. When failed, an error code of the access destination is stored. Refer to SLMP-Compatible Device Manual.

​	**`data`**  [ReadOnlySpan\<UInt16\>](https://docs.microsoft.com/en-us/dotnet/api/system.readonlyspan-1?view=net-5.0)

​	The storage location contains the data to be written to devices.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)

​	

**WriteLocalDeviceInWord(UInt16, IEnumerable<(string, UInt32, UInt16)>, IEnumerable<(string, UInt32, UInt32)>, out UInt16)**

Specifies the device No. to bit device in 16-point units and writes value. This can be specified with inconsecutive device No.

Specifies the device No. to word device in one-word units or two-word units and writes value. This can be specified with inconsecutive device No.

```c#
public void WriteLocalDeviceInWord(ushort monitoringTimer, 
                                   IEnumerable<(string deviceCode, uint headDevice, ushort value)> wordDevice,
                                   IEnumerable<(string deviceCode, uint headDevice, uint value)> dwordDevice,
                                   out ushort endCode)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`wordDevice`** IEnumerable<(String deviceCode, UInt32 headDevice, UInt16 value)>

​	**`dwordDevice`** IEnumerable<(String deviceCode, UInt32 headDevice, UInt32 value)>

​	Specify the device and data value to be written in order from the word access to the double-word access.

​			**`deviceCode`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​			Specify the type of the target device of writing. Refer to [Device Code for Local Device Access](#1-8-1-device-code-for-local-device-access) for details.

​			**`headDevice`** UInt32

​			Specify the head No. of the target device of writing. 

​			**`value`** UInt16

​			**`value`** UInt32

​			Specify the data of device value.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



**WriteLocalDeviceInWord(UInt16,** **IEnumerable<(String, UInt32, UInt16)>,** **IEnumerable<(String, UInt32, UInt16,  ReadOnlyMemory\<UInt16\>)>, IEnumerable<(String, UInt32, UInt16,  ReadOnlyMemory\<UInt16\>)>** **out UInt16**) 

Writes data by treating n points of word devices or bit devices (one point is equivalent to 16 bits) as one block and specifying multiple blocks. This can be specified with inconsecutive device No.

```c#
 public void WriteLocalDeviceInWord(ushort monitoringTimer, 
           	IEnumerable<(string deviceCode, uint headDevice, ushort devicePoints, ReadOnlyMemory<ushort> data)> wordDeviceBlock, 
            IEnumerable<(string deviceCode, uint headDevice, ushort devicePoints, ReadOnlyMemory<ushort> data)> bitDeviceBlock,
            out ushort endCode)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`wordDeviceBlock`** IEnumerable<(String deviceCode, UInt32 headDevice, UInt16 devicePoints, ReadOnlyMemory\<UInt16\> data))>

​	**`bitDeviceBlock`** IEnumerable<(String deviceCode, UInt32 headDevice, UInt16 devicePoints, ReadOnlyMemory\<UInt16\> data))>

​	Specify the target device of writing.

​		**`deviceCode`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​		Specify the type of the target device of writing. Refer to [Device Code for Local Device Access](#1-8-1-device-code-for-local-device-access) for details.

​		**`headDevice`** UInt32

​		Specify the head No. of the target device of writing. 

​		**`devicePoints`** UInt16

​		Specify the number of target device points of writing.

​		**`data`** [ReadOnlyMemory\<UInt16\>](https://docs.microsoft.com/en-us/dotnet/api/system.readonlyspan-1?view=net-5.0)

​		The storage location contains the data to be written to specified word device blocks or bit device blocks.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



###### 1-5-3-5 WriteLocalDeviceInBit

**Overloads**

------

**WriteLocalDeviceInBit(UInt16, string, UInt32, UInt16, out UInt16, ReadOnlySpan\<byte\>)**

Writes value to the bit devices (consecutive device No.) in 1-point units.

```c#
public void WriteLocalDeviceInBit(ushort monitoringTimer, string deviceCode, uint headDevice, ushort devicePoints, out ushort endCode, ReadOnlySpan<byte> data)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`deviceCode`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​	Specify the type of the target device of writing. Refer to [Device Code for Local Device Access](#1-8-1-device-code-for-local-device-access) for details.

​	**`headDevice`** UInt32

​	Specify the head No. of the target device of writing. 

​	**`devicePoints`** UInt16

​	Specify the number of target device points of writing.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	When normally completed, 0 is stored. When failed, an error code of the access destination is stored. Refer to SLMP-Compatible Device Manual.

​	**`data`** [ReadOnlySpan\<Byte\>](https://docs.microsoft.com/en-us/dotnet/api/system.readonlyspan-1?view=net-5.0)

​	The storage location contains the data to be written to bit devices. 

​	Data should be stored as the specified number of device points from the specified head device from the upper bit. 

​	One bit device data occupies one byte. ON is expressed as 01H (1) and OFF is expressed as 00H (0).

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



**WriteLocalDeviceInBit(UInt16, IEnumerable<(String, UInt32, Byte)>, out UInt16)**

Specifies the device No. to bit device in 1-point units and writes value. This can be specified with inconsecutive device No.

```c#
public void WriteLocalDeviceInBit(ushort monitoringTimer, 
                                  IEnumerable<(string deviceCode, uint headDevice, byte value)> bitDevice,
                                  out ushort endCode)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`bitDevice`** IEnumerable<(String deviceCode, UInt32 headDevice, Byte value)>

​	Specify the device and data value to be written.

​			**`deviceCode`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​			Specify the type of the target device of writing. Refer to [Device Code for Local Device Access](#1-8-1-device-code-for-local-device-access) for details.

​			**`headDevice`** UInt32

​			Specify the head No. of the target device of writing. 

​			**`value`** Byte

​			Specify the data of device value. 

​			One bit device data occupies one byte. ON is expressed as 01H (1) and OFF is expressed as 00H (0).

​	**Exceptions**

​	**`endCode`** UInt16

​	The command processing result is stored.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



###### 1-5-3-6 WriteModuleAccessDeviceInWord

**Overloads**

------

**WriteModuleAccessDeviceInWord(UInt16, String, UInt32, UInt16, out UInt16, ReadOnlySpan\<UInt16\>)**

Writes value to the buffer memory of SLMP-Compatible devices or intelligent function modules (consecutive device No.) in one-word units.

```c#
public void WriteModuleAccessDeviceInWord(ushort monitoringTimer, string extensionSepcification, uint headDevice, ushort devicePoints, out ushort endCode, ReadOnlySpan<ushort> data)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`extensionSepcification`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​	Specify the start I/O number in hexadecimal (upper 3-digit ASCII code) starting with 'U'. 

​	**`headDevice`** UInt32

​	Specify the head No. of the target device of writing. 

​	**`devicePoints`** UInt16

​	Specify the number of target device points of writing.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	When normally completed, 0 is stored. When failed, an error code of the access destination is stored. Refer to SLMP-Compatible Device Manual.

​	**`data`**  [ReadOnlySpan\<UInt16\>](https://docs.microsoft.com/en-us/dotnet/api/system.readonlyspan-1?view=net-5.0)

​	The storage location contains the data to be written to devices.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)

​	

**WriteModuleAccessDeviceInWord(UInt16, IEnumerable<(String, UInt32, UInt16 )> , IEnumerable<(String, UInt32, UInt32)>, out UInt16)**

Specifies the device No. and writes the device value. This can be specified with inconsecutive device No. 

Writes value to the buffer memory of SLMP-Compatible devices or intelligent function modules in one-word units or two-word units.

```c#
public void WriteModuleAccessDeviceInWord(ushort monitoringTimer, 
                         IEnumerable<(string extensionSpecification, uint headDevice, ushort value)> wordDevice,
                         IEnumerable<(string extensionSpecification, uint headDevice, uint value)> dwordDevice,
                         out ushort endCode)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`wordDevice`** IEnumerable<(String extensionSpecification, UInt32 headDevice, UInt16 value)>

​	**`dwordDevice`** IEnumerable<(String extensionSpecification, UInt32 headDevice, UInt32 value)>

​	Specify the device and data value to be written in order from the word access to the double-word access.

​			**`extensionSpecification`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​			Specify the start I/O number in hexadecimal (upper 3-digit ASCII code) starting with 'U'. 

​			**`headDevice`** UInt32

​			Specify the head No. of the target device of writing. 

​			**`value`** UInt16

​			**`value`** UInt32

​			Specify the data of device value.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



**WriteModuleAccessDeviceInWord(UInt16, IEnumerable<(String, UInt32, UInt16, ReadOnlyMemory\<UInt16 \>)>, out UInt16)**

Writes the buffer memory of SLMP-Compatible devices or intelligent function modules by treating n points of word devices as one block and specifying multiple blocks. This can be specified with inconsecutive device No.

```c#
public void WriteModuleAccessDeviceInWord(ushort monitoringTimer, 
IEnumerable<(string extensionSpecification, uint headDevice, ushort devicePoints, ReadOnlyMemory<ushort> data)> wordDeviceBlock, out ushort endCode)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`wordDeviceBlock`** IEnumerable<(String extensionSpecification, UInt32 headDevice, UInt16 devicePoints, ReadOnlyMemory\<UInt16\> data))>

​	Specify the target device of writing.

​		**`extensionSpecification`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​		Specify the start I/O number in hexadecimal (upper 3-digit ASCII code) starting with 'U'. 

​		**`headDevice`** UInt32

​		Specify the head No. of the target device of writing. 

​		**`devicePoints`** UInt16

​		Specify the number of target device points of writing.

​		**`data`** [ReadOnlyMemory\<UInt16\>](https://docs.microsoft.com/en-us/dotnet/api/system.readonlyspan-1?view=net-5.0)

​		The storage location contains the data to be written to specified word device blocks.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



#### 1-6 RemoteOperationMaster

Namespace: AMEC.PCSoftware.CommunicationProtocol.CrazyHein.SLMP.Master

Implements the ***Remote Operation*** client of SLMP.

```c#
public class RemoteOperationMaster
```

Supported remote operation commands:

| Operation          | Command Code | Description                                                  |
| ------------------ | ------------ | ------------------------------------------------------------ |
| Remote Run         | 0x1001       | This command executes the remote RUN to the access destination module. |
| Remote Stop        | 0x1002       | This command executes the remote STOP to the access destination module. |
| Remote Pause       | 0x1003       | This command executes the remote PAUSE to the access destination module. |
| Remote Latch Clear | 0x1005       | This command executes the remote latch clear to the access destination module. |
| Remote Reset       | 0x1006       | This command executes the remote RESET to the access destination module. Remote RESET is used to restore when an error occurred in the module. |
| Read Type Name     | 0x0101       | This command reads the model name and model code of the access destination module. |



##### 1-6.1 Constructors

**RemoteOperationMaster(MESSAGE_FRAME_TYPE_T, MESSAGE_DATA_CODE_T, Boolean, SocketInterface, ref DESTINATION_ADDRESS_T, Int32, Int32, Object)**

Initializes a new instance of the [RemoteOperationMaster](#1-6-remoteoperationmaster) class.

```c#
public RemoteOperationMaster(MESSAGE_FRAME_TYPE_T frameType, MESSAGE_DATA_CODE_T dataCode, bool dedicationR, SocketInterface sc, ref DESTINATION_ADDRESS_T destination, int sendBufferSize = 4096, int receiveBufferSize = 4096, object sync = null)
```

​	**Parameters**

​	**`frameType`** [MESSAGE_FRAME_TYPE_T](#1-2-1-message_frame_type_t)

​	Specifies the frame type that the [DeviceAccessMaster](#1-5-deviceaccessmaster) adopts.

​	**`dataCode`** [MESSAGE_DATA_CODE_T](#1-2-2-message_data_code_t)

​	Specifies the communication data code that the [DeviceAccessMaster](#1-5-deviceaccessmaster) adopts.

​	**`dedicationR`** Boolean

​	Specifies whether to active R dedicated message format or not.

​	`true`: R dedicated message format.

​	`false`: Q/L compatible message format.

​	**`sc`** [SocketInterface](#1-2-4-socketinterface)

​	Specifies the communication interface that the [DeviceAccessMaster](#1-5-deviceaccessmaster) associates with.

​	**`destination`** [DESTINATION_ADDRESS_T](#1-2-3-destination_address_t)

​	Specifies  the remote controller station.

​	**`sendBufferSize`** Int32

​	An Int32 that contains the size, in bytes, of the send buffer. 

​	**`receiveBufferSize`** Int32

​	An Int32 that contains the size, in bytes, of the receive buffer. 

​	**`sync`** [Object](https://docs.microsoft.com/en-us/dotnet/api/system.object?view=net-5.0)

​	This object is used as a mutex variable that prevents data corruption by simultaneous device access.

​	If this parameter is null,  the constructor will create a new instance for you.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



##### 1-6-2 Properties

###### 1-6-2-1 COM

Set a socket interface that the [RemoteOperationMaster](#1-6-remoteoperationmaster) associates with.

```c#
public SocketInterface COM {set;}
```

​	**Property Value**

​	[SocketInterface](#1-2-4-socketinterface)



##### 1-6-3 Methods

###### 1-6-3-1 Run

Executes the remote RUN to the access destination module.

```c#
public void Run(ushort monitoringTimer, REMOTE_CONTROL_MODE_T controlMode, REMOTE_CLEAR_MODE_T clearMode, out ushort endCode)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`controlMode`** [REMOTE_CONTROL_MODE_T](#1-2-6-remote_control_mode_t)

​	Specify whether the remote RUN can be executed forcibly by the device other than the external device which performed the remote STOP/remote 	PAUSE. 

​	If the forced execution is not allowed, remote RUN can be executed only by the external device which performed the remote STOP/remote PAUSE.

​	Forced execution is used when the external device which performed the remote operation cannot execute the remote RUN because of a trouble on the device.

​	**`clearMode`** [REMOTE_CLEAR_MODE_T](#1-2-7-remote_clear_mode_t)

​	Specify whether the clear (initialization) processing of device is executed or not when starting the calculation for the remote RUN. 

​	The device which received the remote RUN request turns to the RUN state after the clear (initialization) processing of device.

​	When the device initial value is set in the parameters of the CPU module, the clear (initialization) processing of device is executed according to the setting.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	When normally completed, 0 is stored. When failed, an error code of the access destination is stored. Refer to SLMP-Compatible Device Manual.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



###### 1-6-3-2 Stop

Executes the remote STOP to the access destination module.

```c#
public void Stop(ushort monitoringTimer, out ushort endCode)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`endCode`** UInt16

​	The command processing result is stored.

​	When normally completed, 0 is stored. When failed, an error code of the access destination is stored. Refer to SLMP-Compatible Device Manual.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



###### 1-6-3-3 Pause

Executes the remote PAUSE to the access destination module.

```c#
public void Pause(ushort monitoringTimer, REMOTE_CONTROL_MODE_T controlMode, out ushort endCode)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`controlMode`** [REMOTE_CONTROL_MODE_T](#1-2-6-remote_control_mode_t)

​	Specify whether the remote RUN can be executed forcibly by the device other than the external device which performed the remote STOP/remote 	PAUSE. 

​	If the forced execution is not allowed, remote RUN can be executed only by the external device which performed the remote STOP/remote PAUSE.

​	Forced execution is used when the external device which performed the remote operation cannot execute the remote RUN because of a trouble on the device.

​	**`endCode`** UInt16

​	The command processing result is stored.

​	When normally completed, 0 is stored. When failed, an error code of the access destination is stored. Refer to SLMP-Compatible Device Manual.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



###### 1-6-3-4 LatchClear

 Executes the remote latch clear to the access destination module.

```c#
public void LatchClear(ushort monitoringTimer, out ushort endCode)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`endCode`** UInt16

​	The command processing result is stored.

​	When normally completed, 0 is stored. When failed, an error code of the access destination is stored. Refer to SLMP-Compatible Device Manual.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



###### 1-6-3-5 Reset

Executes the remote RESET to the access destination module. Remote RESET is used to restore when an error occurred in the module.

```c#
public void Reset(ushort monitoringTimer, out ushort endCode)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`endCode`** UInt16

​	The command processing result is stored.

​	When normally completed, 0 is stored. When failed, an error code of the access destination is stored. Refer to SLMP-Compatible Device Manual.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



###### 1-6-3-6 ReadTypeName

Reads the model name and model code of the access destination module.

```c#
public void ReadTypeName(ushort monitoringTimer, out ushort endCode, out string modelName, out ushort modelCode)
```

​	**Parameters**

​	**`monitoringTimer`** UInt16

​	This timer sets the waiting time for the external device that received a request message to wait for the response after it issued a processing request to 	the access destination.

​	0000H (0): Unlimited wait (until the processing is completed)

​	0001H to FFFFH (1 to 65535): Waiting time (Unit: 250ms)

​	**`endCode`** UInt16

​	The command processing result is stored. 

​	When normally completed, 0 is stored. When failed, an error code of the access destination is stored. Refer to SLMP-Compatible Device Manual.

​	**`modelName`** [String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​	16 characters from the upper byte of the module model are stored.

​	If the model to be read is less than 16 characters, space (20H) is stored for the remaining character.

​	**`modeCode`** UInt16

​	The 16bit model code is stored. Refer to SLMP reference manual for the detail information about the code.

​	**Exceptions**

​	[SLMPException](#1-7-slmpexception)



#### 1-7 SLMPException

Namespace: AMEC.PCSoftware.CommunicationProtocol.CrazyHein.SLMP

Represents errors that occur during application execution.

```c#
public class SLMPException : Exception
```



##### 1-7-1 Constructors

Initializes a new instance of the [SLMPException](#1-7-slmpexception) class.

**Overloads**

------

**SLMPException([SLMP_EXCEPTION_CODE_T](#1-2-5-slmp_exception_code_t))**

Initializes a new instance of the [SLMPException](#1-7-slmpexception) class with a specified [SLMP_EXCEPTION_CODE_T](#1-2-5-slmp_exception_code_t) code.

```c#
public SLMPException(SLMP_EXCEPTION_CODE_T code)
```

​	**Parameters**

​	**`code`** [SLMP_EXCEPTION_CODE_T](#1-2-5-slmp_exception_code_t)

​	The code that explains the reason for the exception.



**SLMPException([Exception](https://docs.microsoft.com/en-us/dotnet/api/system.exception?view=net-5.0))**

Initializes a new instance of the [SLMPException](#1-7-slmpexception) class with a reference to the inner runtime exception that is the cause of this exception.

```c#
public SLMPException(Exception exp)
```

​	**Parameters**

​	**`exp`** [Exception](https://docs.microsoft.com/en-us/dotnet/api/system.exception?view=net-5.0)

​	The exception that is the cause of the current runtime exception.



##### 1-7-2 Properties

###### 1-7-2-1 ExceptionCode

Gets the [SLMP_EXCEPTION_CODE_T](#1-2-5-slmp_exception_code_t) code that describes the current exception.

```c#
 public SLMP_EXCEPTION_CODE_T ExceptionCode { get; private set; }
```

​	**Property Value**

​	[SLMP_EXCEPTION_CODE_T](#1-2-5-slmp_exception_code_t) 

​	The code that explains the reason for the exception.



###### 1-7-2-2 RuntimeException

If ExceptionCode returns ***RUNTIME_ERROR***, this property will return the [Exception](https://docs.microsoft.com/en-us/dotnet/api/system.exception?view=net-5.0) instance that caused the current runtime exception, otherwise, it returns null.

```c#
 public Exception RuntimeException { get; private set; }
```

​	**Property Value**

​	[Exception](https://docs.microsoft.com/en-us/dotnet/api/system.exception?view=net-5.0) 

​	An object that describes the error that caused the current runtime exception.



###### 1-7-2-3 Message

Creates and returns a short message representation of the current exception.

```c#
 public override string Message {get;}
```

​	**Returns**

​	[String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​	A short message representation of the current exception.



##### 1-7-3 Methods

###### 1-7-3-1 ToString

Creates and returns a string representation of the current exception.

```c#
 public override string ToString()
```

​	**Returns**

​	[String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0)

​	A string representation of the current exception.



#### 1-8 Reference

##### 1-8-1 Device Code for Local Device Access

| Device                             | Device Code | Type        | Remarks                                  |
| ---------------------------------- | ----------- | ----------- | ---------------------------------------- |
| Special relay                      | SM          | Bit         |                                          |
| Special register                   | SD          | Word        |                                          |
| Input                              | X           | Bit         |                                          |
| Output                             | Y           | Bit         |                                          |
| Internal relay                     | M           | Bit         |                                          |
| Latch relay                        | L           | Bit         |                                          |
| Annunciator                        | F           | Bit         |                                          |
| Edge relay                         | V           | Bit         |                                          |
| Link relay                         | B           | Bit         |                                          |
| Data register                      | D           | Word        |                                          |
| Link register                      | W           | Word        |                                          |
| Timer contact                      | TS          | Bit         |                                          |
| Timer coil                         | TC          | Bit         |                                          |
| Timer current Value                | TN          | Word        |                                          |
| Long timer contact                 | LTS         | Bit         | Only apply to R dedicated message format |
| Long timer coil                    | LTC         | Bit         | Only apply to R dedicated message format |
| Long timer current Value           | LTN         | Double Word | Only apply to R dedicated message format |
| Retentive timer contact            | STS         | Bit         | "SS" for Q/L compatible message format   |
| Retentive timer coil               | STC         | Bit         | "SC" for Q/L compatible message format   |
| Retentive timer current value      | STN         | Word        | "SN" for Q/L compatible message format   |
| Long retentive timer contact       | LSTS        | Bit         | Only apply to R dedicated message format |
| Long retentive timer coil          | LSTC        | Bit         | Only apply to R dedicated message format |
| Long retentive timer current value | LSTN        | Double Word | Only apply to R dedicated message format |
| Link special relay                 | SB          | Bit         |                                          |
| Link special register              | SW          | Word        |                                          |
| Direct access input                | DX          | Bit         |                                          |
| Direct access output               | DY          | Bit         |                                          |
| Index register                     | Z           | Word        |                                          |
| Long index register                | LZ          | Double Word | Only apply to R dedicated message format |
| File register                      | R           | Word        |                                          |
| File register                      | ZR          | Word        |                                          |
| Refresh data register              | RD          | Word        | Only apply to R dedicated message format |
| Extended data register             | D           | Word        | Only apply to R dedicated message format |
| Extended link register             | W           | Word        | Only apply to R dedicated message format |



##### 1-8-2 Example

###### 1-8-2-1 Device Access

The following code example demonstrates how to create a  [DeviceAccessMaster](#1-5-deviceaccessmaster) instance and how to read devices with it.

```c#
var destination = DESTINATION_ADDRESS_T.CONNECTED_OWN_STATION();//get the access address of the connected station
ushort end = 0;
try
{
    var sc = new UDP(new IPEndPoint(IPAddress.Any, 5010), //bind to local UDP port 5010
                     new IPEndPoint(IPAddress.Parse("192.168.2.250"), 5010), //remote destination 192.168.2.250:5011
                     4096,
                     200, 200);
    var deviceAccessMaster = new DeviceAccessMaster(MESSAGE_FRAME_TYPE_T.MC_3E, 
                                                    MESSAGE_DATA_CODE_T.BINARY, 
                                                    false, //Q/L-Compatible message format
                                                    sc, 
                                                    ref destination, 4096, 4096);
    
    //Read value from the bit devices (consecutive device No.) in 1-point units.
    byte[] bitbuffer = new byte[64];
	deviceAccessMaster.ReadLocalDeviceInBit(1, "M", 32, 64, out end, bitbuffer); //Read M[32] - M[95]
    
    //Read value from the bit devices (consecutive device No.) in 16-point units.
    ushort[] wordbuffer = new ushort[4];
    deviceAccessMaster.ReadLocalDeviceInWord(1, "M", 32, 4, out end, wordbuffer); //Read M[32] - M[95]
    
    //Read value from the word devices (consecutive device No.) in one-word units.
    deviceAccessMaster.ReadLocalDeviceInWord(1, "D", 32, 4, out end, wordbuffer); //Read D[32] - M[35]
    
    //Read value from the word devices in one-word units or two-word units(inconsecutive device No.).
	var worddevices = new ValueTuple<string, uint>[] {("D", 100),
                                                      ("D", 101),
                                                      ("M", 100),
                                                      ("M", 116)};
	var dworddevices = new ValueTuple<string, uint>[] {("D", 200),
                                                       ("D", 202),
                                                       ("M", 200),
                                                       ("M", 232)};
    uint[] dwordbuffer = new uint[4];
    //Read D[100], D[101], M[100-115], M[116-131], D[200-201], D[202-203], M[200-231], M[232-263]
    deviceAccessMaster.ReadLocalDeviceInWord(1, worddevices, dworddevices, out end, wordbuffer, dwordbuffer);
    
    //Read data by treating n points of word devices or bit devices (one point is equivalent to 16 bits) as one block
    //and specifying multiple blocks.
    var worddeviceblocks = new ValueTuple<string, uint, ushort>[] {("D", 32, 2),
                                                                   ("W", 48, 2)};
	var bitbdevicelocks = new ValueTuple<string, uint, ushort>[] {("M", 16, 2),
                                                                  ("B", 32, 2)};
    var wordbufferblocks = new Memory<ushort>[2] { new ushort[2], new ushort[2] };
	var bitbufferblocks = new Memory<ushort>[2] { new ushort[2], new ushort[2] };
    //Read D[32-33] W[30-31] M[16-47] B[20-3F]
    deviceAccessMaster.ReadLocalDeviceInWord(1, worddeviceblocks, bitbdevicelocks, out end, wordbufferblocks, bitbufferblocks);
    
    //Read value from the buffer memory of intelligent function modules (consecutive device No.) in one-word units.
    deviceAccessMaster.ReadModuleAccessDeviceInWord(1, "U000", 0, 4, out end, wordbuffer); //Read U0000\G0 - Read U0000\G3
    
    //Read value from the buffer memory of intelligent function modules 
    //in one-word units or two-word units(inconsecutive device No.).
	var wordbuffermemorys = new ValueTuple<string, uint>[] {("U000", 1), ("U000", 2)};
    var dwordbuffermemorys = new ValueTuple<string, uint>[] {("U000", 10), ("U000", 12)};
    //Read U0000\G1, U0000\G2, U0000\G10-11, U0000\G12-13
    deviceAccessMaster.ReadModuleAccessDeviceInWord(1, wordbuffermemorys, dwordbuffermemorys, out end, wordbuffer, dwordbuffer);
    
    //Read the buffer memory of intelligent function modules by treating n points of word devices as one block
    //and specifying multiple blocks.
    var buffermemoryblocks = new ValueTuple<string, uint, ushort>[] {("U000", 0, 2), ("U000", 16, 2)};
    //Read U0000\G0-1, U0000\G16-17
    deviceAccessMaster.ReadModuleAccessDeviceInWord(1, buffermemoryblocks, out end, wordbufferblocks);
}
catch(SLMPException e)
{
    Console.WriteLine(e.ToString());
}
```



The following code example demonstrates how to create a  [DeviceAccessMaster](#1-5-deviceaccessmaster) instance and how to write devices with it.

```c#
var destination = DESTINATION_ADDRESS_T.CONNECTED_OWN_STATION();//get the access address of the connected station
ushort end = 0;
try
{
    var sc = new UDP(new IPEndPoint(IPAddress.Any, 5010), //bind to local UDP port 5010
                     new IPEndPoint(IPAddress.Parse("192.168.2.250"), 5010), //remote destination 192.168.2.250:5011
                     4096,
                     200, 200);
    var deviceAccessMaster = new DeviceAccessMaster(MESSAGE_FRAME_TYPE_T.MC_3E, 
                                                    MESSAGE_DATA_CODE_T.BINARY, 
                                                    false, //Q/L-Compatible message format
                                                    sc, 
                                                    ref destination, 4096, 4096);
    
    //Write value to the bit devices (consecutive device No.) in 1-point units.
    byte[] bitbuffer = new byte[64];
	deviceAccessMaster.WriteLocalDeviceInBit(1, "M", 32, 64, out end, bitbuffer); //Write M[32] - M[95]
    
    //Write value to the word devices in 1-point units(inconsecutive device No.).
    var bitdevices = new ValueTuple<string, uint, byte>[] {("M", 0, 1),
                                                           ("M", 100, 0),
                                                           ("M", 200, 1),
                                                           ("M", 300, 0) };
    deviceAccessMaster.WriteLocalDeviceInBit(1, bitdevices, out end); //Write M[0] M[100] M[200] M[300]
    
     //Write value to the bit devices (consecutive device No.) in 16-point units.
	ushort[] wordbuffer = new ushort[4];
	deviceAccessMaster.WriteLocalDeviceInWord(1, "M", 32, 4, out end, wordbuffer); //Write M[32] - M[95]

    //Write value to the word devices (consecutive device No.) in one-word units.
    deviceAccessMaster.WriteLocalDeviceInWord(1, "D", 32, 4, out end, wordbuffer); //Write D[32] - M[35]

    //Write value to the word devices in one-word units or two-word units(inconsecutive device No.).
    var worddevices = new ValueTuple<string, uint, ushort>[] {("D", 100, 100),
                                                              ("D", 101, 200),
                                                              ("M", 100, 300),
                                                              ("M", 116, 400)};
    var dworddevices = new ValueTuple<string, uint, uint>[] {("D", 200, 1000000),
                                                             ("D", 202, 2000000),
                                                             ("M", 200, 3000000),
                                                             ("M", 232, 4000000)};
    uint[] dwordbuffer = new uint[4];
    //Write D[100], D[101], M[100-115], M[116-131], D[200-201], D[202-203], M[200-231], M[232-263]
    deviceAccessMaster.WriteLocalDeviceInWord(1, worddevices, dworddevices, out end);

    //Write data by treating n points of word devices or bit devices (one point is equivalent to 16 bits) as one block
    //and specifying multiple blocks.
    var wordbufferblocks = new Memory<ushort>[2] { new ushort[2], new ushort[2] };
    var bitbufferblocks = new Memory<ushort>[2] { new ushort[2], new ushort[2] };
    var worddeviceblocks = new ValueTuple<string, uint, ushort, ReadOnlyMemory<ushort>>[] {("D", 32, 2, wordbufferblocks[0]),
                                                                                           ("W", 48, 2, wordbufferblocks[1])};
    var bitbdevicelocks = new ValueTuple<string, uint, ushort, ReadOnlyMemory<ushort>>[] {("M", 16, 2, bitbufferblocks[0]),
                                                                                          ("B", 32, 2, bitbufferblocks[1])};

    //Write D[32-33] W[30-31] M[16-47] B[20-3F]
    deviceAccessMaster.WriteLocalDeviceInWord(1, worddeviceblocks, bitbdevicelocks, out end);

     //Write value from the buffer memory of intelligent function modules (consecutive device No.) in one-word units.
    deviceAccessMaster.WriteModuleAccessDeviceInWord(1, "U000", 0, 4, out end, wordbuffer); //Write U0000\G0 - Read U0000\G3

    //Write value to the buffer memory of intelligent function modules 
    //in one-word units or two-word units(inconsecutive device No.).
    var wordbuffermemorys = new ValueTuple<string, uint, ushort>[] { ("U000", 1, 100), ("U000", 2, 200) };
    var dwordbuffermemorys = new ValueTuple<string, uint, uint>[] { ("U000", 10, 1000000), ("U000", 12, 2000000) };
    //Write U0000\G1, U0000\G2, U0000\G10-11, U0000\G12-13
    deviceAccessMaster.WriteModuleAccessDeviceInWord(1, wordbuffermemorys, dwordbuffermemorys, out end);

    //Write the buffer memory of intelligent function modules by treating n points of word devices as one block
    //and specifying multiple blocks.
    var buffermemoryblocks = new ValueTuple<string, uint, ushort, ReadOnlyMemory<ushort>>[] { 
        ("U000", 0, 2, wordbufferblocks[0]), 
        ("U000", 16, 2, wordbufferblocks[1]) };
    //Write U0000\G0-1, U0000\G16-17
    deviceAccessMaster.WriteModuleAccessDeviceInWord(1, buffermemoryblocks, out end);
}
catch(SLMPException e)
{
    Console.WriteLine(e.ToString());
}
```



###### 1-8-2-2 Remote Operation

The following code example demonstrates how to create a  [RemoteOperationMaster](#1-6-remoteoperationmaster) instance and how to execute remote operation with it.

```c#
var destination = DESTINATION_ADDRESS_T.CONNECTED_OWN_STATION();//get the access address of the connected station
ushort end = 0;
try
{
    var sc = new UDP(new IPEndPoint(IPAddress.Any, 5010), //bind to local UDP port 5010
                     new IPEndPoint(IPAddress.Parse("192.168.2.250"), 5010), //remote destination 192.168.2.250:5011
                     4096,
                     200, 200);
    var remoteOperationMaster = new RemoteOperationMaster(MESSAGE_FRAME_TYPE_T.MC_3E,
                                                          MESSAGE_DATA_CODE_T.BINARY,
                                                          false, //Q/L-Compatible message format
                                                          sc,
                                                          ref destination, 4096, 4096);
	
    //Remote Run
    remoteOperationMaster.Run(1, REMOTE_CONTROL_MODE_T.FORCED_EXECUTION_ALLOWED, 
                              REMOTE_CLEAR_MODE_T.DO_NOT_CLEAR_DEVICE, out end);
    //Remote Stop
    remoteOperationMaster.Stop(1, out end);
    //Remote Pause
    remoteOperationMaster.Pause(1, REMOTE_CONTROL_MODE_T.FORCED_EXECUTION_ALLOWED, out end);
    //Remote Latch Clear
    remoteOperationMaster.LatchClear(1, out end);
    //Remote Reset
    remoteOperationMaster.Reset(1, out end);
    //Read Type Name
    string modelName;
    ushort modelCode = 0;
    remoteOperationMaster.ReadTypeName(1, out end, out modelName, out modelCode);
}
catch(SLMPException e)
{
    Console.WriteLine(e.ToString());
}
```



### 2 Mcvein

#### 2-1 Introduction

*Mcvein* is a container of SLMP based diagnostic tool for Mitsubishi PLC.

![](DocImages\McveinMain.png)

<p style="text-align: center"><b><em>Figure 2.1</em></b> <em>Mcvein Main Window</em></p>

You can manage the connection targets in "**Target**" sheet, please refer to [2-4 Target Management](#2-4-target-management) for details;

You can manage the available tools "**Navigation**" sheet, please refer to [2-5 Tool Management](#2-5-tool-management) for details;

The caption of main window shows the path of the current project;

The properties of current state, which are always shown in the status bar of main window, from left to right, are "Project dirty flag", "Online/Offline flag", "Communication loop time indicator", "Data exchanging indicator",  "Program state",  "Detail exception information".



#### 2-2 Install Plug-in

To install a new plug-in (tool), please copy the whole folder of the plug-in to the folder "**toolkit**" exists in the same directory as ***Mcvein.exe*** before you launch tool. 

The compatible plug-ins (tools) will be shown in "**Navigation**" sheet, please refer to [2-5 Tool Management](#2-5-tool-management) for details;



#### 2-3 Project Management

##### 2-3-1 New

You can create a new project by a menu item of shortcut key.

> Project -> New

or

> Ctrl+N



##### 2-3-2 Open

You can open an existing project file by a menu item or shortcut key.

> Project -> Open

or

> Ctrl+O



##### 2-3-3 Save

You can save the project you are working on by a menu item or shortcut key.

> Project -> Save

or

> Ctrl+S



##### 2-3-4 Save As

You can save the project you are working on as another separate file by a menu item or shortcut key.

> Project -> Save As

or

> Ctrl+Shift+S



#### 2-4 Target Management

![](DocImages\McveinTargets.png)

<p style="text-align: center"><b><em>Figure 2.4</em></b> <em>Target Management</em></p>

##### 2-4-1 Add

You can add a new connection target by a menu item of shortcut key.

> Target -> Add

or

> Ctrl+Shift+A

A connection property dialog will show:

![](DocImages\McveinTargetProperty.png)

<p style="text-align: center"><b><em>Figure 2.4.1</em></b> <em>Target Property</em></p>

Please refer to "**SLMP Reference Manual**" for the detail information of the target property.

Please click the "**OK**" button to add the target to the available targets list;

Please click the "**Communication Test**" button to conduct a communication test with the current settings;

**Notes:**

- You can not conduct the communication test or add new connection target if the console is in **Online State**;
- You can not have two targets with the same name, please specify another name for your target if a target with same name has already been in  existence;



##### 2-4-2 Remove

You can remove the specified connection target by a menu item of shortcut key.

> Target -> Remove

or

> Ctrl+Shift+R

**Notes:**

- You can not remove the activated connection target if the console is in **Online State**;



##### 2-4-3 Property

You can review or revise the property of the specified connection target by a menu item of shortcut key.

> Target -> Property

or

> Ctrl+Shift+P

**Notes:**

- You can not conduct the communication test or revise the property of the specified connection target if the console is in **Online State**;



##### 2-4-4 Activate

You can activate the specified connection target by a menu item of shortcut key.

> Target -> Activate

or

> Ctrl+Shift+E

**Notes:**

- You can not activate the specified connection target if the console is in **Online State**;

- You must activate one connection target before going online;



#### 2-5 Tool Management

![](DocImages/McveinNavigation.png)

<p style="text-align: center"><b><em>Figure 2.5</em></b> <em>Tool Navigation</em></p>

All compatible plug-ins(tools) are shown here.

You can also review the plug-in name, version and detail description here.



##### 2-5-1 Add

Click the plus icon to instantiate the corresponding tool. You can instantiate the same tool multiple times.



##### 2-5-2 Remove

You can remove the tool by closing the corresponding tab.



##### 2-5-3 Layout

TBD

**Notes:**

- The layout of tool tabs will be saved to the project file;



#### 2-6 Operation

##### 2-6-1 Connect

You can connect to the activated connection target by a menu item of shortcut key.

> Online -> Connect

or

> Ctrl+Shift+C

**Notes:**

- You must activate one connection target before going online;



##### 2-6-2 Disconnect

You can disconnect from the activated connection target by a menu item of shortcut key.

> Online -> Disconnect

or

> Ctrl+Shift+D



### 3 Numeros

#### 3-1 Introduction

*Numeros* (Q64TCAutoTuning) is an auto-tuning utility for Q64TCTTN, Q64TCTTBWN, Q64TCRTN and Q64TCRTBWN.



#### 3-2 Enable

![](DocImages/NumerosEnable.png)

<p style="text-align: center"><b><em>Figure 3.2</em></b> <em>Enable Numeros</em></p>

You should input the correct module(Q64TCTTN, Q64TCTTBWN, Q64TCRTN Q64TCRTBWN) address and click the "**Enable**" button to enable the tool. The module address must be start with "U" and follow by three hexadecimal numbers.



#### 3-3 Device Control

![](DocImages/NumerosDeviceControl.png)

<p style="text-align: center"><b><em>Figure 3.3</em></b> <em>Device Control</em></p>

| Field              | Content                                                      |
| ------------------ | ------------------------------------------------------------ |
| **Error Code**     | Write data error code or alarm code. The error code is always given priority over the alarm code. |
| **Error Cause**    | Detail information about the "**Error Code**".               |
| **Operation Mode** | The current device operation mode(SETTING MODE/OPERATION MODE). |

Click the "**Clear Error**" button to clear  "**Error Code**";

Click the "**SETTING MODE**" button to set the device to SETTING_MODE;

Click the "**OPERAITION MODE**" button to set the device to OPERATION_MODE;

Click the "**CHx**" button to switch the control panel to corresponding channel;



#### 3-4 Channel Control Panel

##### 3-4-1 Monitor

![](DocImages/NumerosChannelMonitor.png)

<p style="text-align: center"><b><em>Figure 3.4.1</em></b> <em>Channel Monitor</em></p>

Click "**Auto Mode**" button to set the channel to automatic mode and click "**Manual Mode**" button to set the channel to manual mode.



##### 3-4-2 PID Auto Tuning

![](DocImages/NumerosChannelAT.png)

<p style="text-align: center"><b><em>Figure 3.4.2</em></b> <em>Channel Auto Tuning</em></p>

| Field                                       | Content                                                      |
| ------------------------------------------- | ------------------------------------------------------------ |
| **Set Value Setting**                       | Set the target temperature value of PID control.             |
| **AT Bias Setting**                         | The point set as the set value (SV) in the auto tuning can be rearranged by  this value. The auto tuning is performed with having the AT point (the point rearranged by the setting) as its center. When the auto tuning is completed, AT bias is not added and a control is performed toward the set value (SV). |
| **AT Loop Disconnection Detection Flag**    | This function detects loop disconnections during auto tuning (AT). With this function, a channel that is not controlled can be detected during auto tuning, thus the error channel is detected more than two hours before the auto tuning error occurs. The auto tuning continues even if an alert is output for the loop disconnection detection. |
| **AT Loop Disconnection Detection Setting** | Using this function detects an error occurring within a control system (control loop) due to reasons such as a load (heater) disconnection, an externally-operable device (such as a magnetic relay) failure, and input disconnection. |
| **AT Mode Selection**                       | Select the auto tuning mode from the following two modes according to the controlled object to be used.  <br />***Standard mode:*** The standard mode is appropriate for most controlled objects. This mode is especially suitable for controlled objects that have an extremely slow response speed or can be affected by noise or disturbance. However, PID constants of slow response (low gain) may be calculated from controlled objects whose ON time or OFF time in the auto tuning is only around 10s. In this case, PID constants of fast response can be calculated by selecting the high response mode and performing the auto tuning.<br />***High response mode:*** This mode is suitable for controlled objects whose ON time or OFF time in the auto tuning is only around 10s. PID constants of fast response (high gain) can be calculated. However, the temperature process value (PV) may oscillates near the set value (SV) because of the too high gain of the PID constants calculated. In this case, select the normal mode and perform the auto tuning. |
| **Backup of the Calculated Value**          | By enable the field at the start of auto tuning, the calculated value is automatically backed up into E2PROM on completion of auto tuning. |
| **AT Status**                               | Indicate the auto tuning status.                             |

Click "**ON**"  button to request the channel to do PID auto tuning.

If the PID auto tuning is complete normally or any error occurred while doing tuning, please click "OFF" button to withdraw the request;

**Note:**

- You can post the auto tuning request only if the device operation mode is "**OPERATION MODE**" and the channel operation mode is **Auto Mode**".



##### 3-4-3 PID Constants

![](DocImages/NumerosChannelPIDConstants.png)

<p style="text-align: center"><b><em>Figure 3.4.3</em></b> <em>Channel PID Constants</em></p>

| Field | Content                                                      |
| ----- | ------------------------------------------------------------ |
| Ph    | Set proportional band (P)/heating proportional band (Ph)/cooling proportional band (Pc) to perform PID control.  <br />Heating proportional band (Ph) setting: 0 to 10000 (0.0% to 1000.0%) . |
| Pc    | Set proportional band (P)/heating proportional band (Ph)/cooling proportional band (Pc) to perform PID control.  <br />Cooling proportional band (Pc) setting: 1 to 10000 (0.1% to 1000.0%) . |
| I     | Set integral time (I) to perform PID control.  <br />The setting range is 0 to 3600 (0 to 3600s). |
| D     | Set derivative time (D) to perform PID control.  <br />The setting range is 0 to 3600 (0 to 3600s). |
| LP    | Errors such as disconnection of resistors, malfunction of an external controller, and errors of the control system due to troubles such as disconnection of the sensor can be detected by the loop disconnection detection function. If temperature does not change by 2°C ( ) or more in the Loop disconnection detection judgment time, a loop disconnection is detected.  <br />The setting range is 0 to 7200 (s) . |

Click "**Backup PID Constants**" button to save the PID constants of all channels at once.

