# Cruiser-Documentation
Documentation for Cruiser Project.

maintainer: [@hanzheteng](https://github.com/hanzheteng)
## CDT Protocol
Cruiser Data Transmission(CDT) Protocol.
### Introduction
DJI Onboard SDK OPEN Protocol provide a method for communication between Mobile and Onboard device called Data Transparent Transmission.

Under this mechanism, we could send message to Autopilot first and Autopilot will transfer this message to Mobile or Onboard device.

<table align="center">
  <tr>
    <th colspan=3>CMD Frame</th>
  </tr>
  <tr>
    <td>CMD SET</td>
    <td>CMD ID</td>
    <td>CMD VAL<br>( CDT DATA )</td>
  </tr>
</table>

DJI Onboard SDK OPEN Protocol is only used between Onboard and Autopilot. With CMD SET `0x00` CMD ID `0xFE` we could send message from Onboard to Mobile. With CMD SET `0x02` CMD ID `0x02` Onboard device could get message from Mobile.

So we define a new set of command and acknowledge messages within CMD VAL called Cruiser Data Transmission (CDT).

### CDT Frame
<table>
  <tr>
    <th colspan=3>CDT DATA</th>
  </tr>
  <tr>
    <td>CDT SET</td>
    <td>CDT ID</td>
    <td>CDT VAL</td>
  </tr>
</table>

| Field   | Offset(byte) | Size(byte)   |
| ------  | ------------ | ------------ |
| CDT SET | 0            | 1            |
| CDT ID  | 1            | 1            |
| CDT VAL | 2            | vary by CDTs |

### Function List
- Data type : unsigned char[ ]
- M-O : from Mobile to Onboard
- O-M : from Onboard to Mobile
<table>
  <tr>
    <th>CDT SET</th>
    <th>CDT ID</th>
    <th>CDT VAL</th>
    <th>Direction</th>
    <th>Description</th>
  </tr>
  <tr>
    <td> 0x00 </td>
    <td> --- </td>
    <td> --- </td>
    <td> --- </td>
    <td> Reserved </td>
  </tr>
  <tr>
    <td rowspan=2>0x01
      <br> Visual Landing </td>
    <td> 0x01 </td>
    <td> --- </td>
    <td> M-O </td>
    <td> Start </td>
  </tr>
  <tr>
    <td> 0x02 </td>
    <td> --- </td>
    <td> O-M </td>
    <td> Start successfully </td>
  </tr>
  <tr>
    <td rowspan=4> 0x02 <br> Object Tracking </td>
    <td> 0x01 </td>
    <td> 0xXXXX </td>
    <td> M-O </td>
    <td> Start <br> ( upper-left and lower-right point ) </td>
  </tr>
  <tr>
    <td> 0x02 </td>
    <td> --- </td>
    <td> O-M </td>
    <td> Start successfully </td>
  </tr>
  <tr>
    <td> 0x03 </td>
    <td> 0xXXXX </td>
    <td> O-M </td>
    <td> Current position <br>( continuously send )</td>
  </tr>
  <tr>
    <td> 0x04 </td>
    <td> --- </td>
    <td> M-O </td>
    <td> Received position </td>
  </tr>
</table>
