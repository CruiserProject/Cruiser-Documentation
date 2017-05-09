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
- M-O : from Mobile to Onboard -- CDT ID is an odd number
- O-M : from Onboard to Mobile -- CDT ID is an even number
- CMD : command
- ACK : acknowledge
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
    <td> --- ( *reserved* ) </td>
  </tr>

 <!-------SET 0x01-------->
  <tr>
    <td rowspan=7> 0x01
      <br> <br> Visual Landing </td>
    <td> 0x01 </td>
    <td> --- </td>
    <td> M-O </td>
    <td> Start - CMD </td>
  </tr>
  <tr>
    <!--SET 0x01-->
    <td> 0x02 </td>
    <td> --- </td>
    <td> O-M </td>
    <td> Start - ACK </td>
  </tr>
  <tr>
    <!--SET 0x01-->
    <td> 0x03 </td>
    <td> --- </td>
    <td> M-O </td>
    <td> Stop - CMD </td>
  </tr>
  <tr>
    <!--SET 0x01-->
    <td> 0x04 </td>
    <td> --- </td>
    <td> O-M </td>
    <td> Stop - ACK </td>
  </tr>
  <tr>
    <!--SET 0x01-->
    <td> 0x06 </td>
    <td> --- </td>
    <td> O-M </td>
    <td> Landing successfully </td>
  </tr>
  <tr>
    <!--SET 0x01-->
    <td> 0x42 </td>
    <td> 0xXXXX ... </td>
    <td> O-M </td>
    <td> Delta X and Y distance <br>( *continuously send* )</td>
  </tr>
  <tr>
    <!--SET 0x01-->
    <td> 0x44 </td>
    <td> 0xXXXX ... </td>
    <td> O-M </td>
    <td> Circle center and radius <br>( *continuously send* )</td>
  </tr>

  <!-------SET 0x02------->
  <tr>
    <td rowspan=6> 0x02
      <br> <br> Object Tracking </td>
    <td> 0x01 </td>
    <td> --- </td>
    <td> M-O </td>
    <td> Start - CMD </td>
  </tr>
  <tr>
    <!--SET 0x02-->
    <td> 0x02 </td>
    <td> --- </td>
    <td> O-M </td>
    <td> Start - ACK </td>
  </tr>
  <tr>
    <!--SET 0x02-->
    <td> 0x03 </td>
    <td> --- </td>
    <td> M-O </td>
    <td> Stop - CMD </td>
  </tr>
  <tr>
    <!--SET 0x02-->
    <td> 0x04 </td>
    <td> --- </td>
    <td> O-M </td>
    <td> Stop - ACK </td>
  </tr>
  <tr>
    <!--SET 0x02-->
    <td> 0x11 </td>
    <td> 0xXXXX </td>
    <td> M-O </td>
    <td> Object position percentage <br> ( *upper-left and lower-right point* ) </td>
  </tr>
  <tr>
    <!--SET 0x02-->
    <td> 0x44 </td>
    <td> 0xXXXX ... </td>
    <td> O-M </td>
    <td> Current position of object <br>( *continuously send* )</td>
  </tr>
</table>


## ROS Node Info
<table>
  <tr>
    <th> Node name </th>
    <th> Sub topic </th>
    <th> Pub topic </th>
    <th> Description </th>
    <th> Collaborators </th>
  </tr>
  <tr>
    <td> dji-sdk/... </td>
    <td> ( *many* ) </td>
    <td> ( *many* ) </td>
    <td> Official SDK </td>
    <td> DJI </td>
  </tr>
  <tr>
    <td> image_raw </td>
    <td> --- </td>
    <td> /dji_sdk/image_raw
      <br> /dji_sdk/camera_info </td>
    <td> Get cam image and pub </td>
    <td> DJI
      <br> @ShoupingShan </td>
  </tr>
  <tr>
    <td> mobile_msg </td>
    <td> /dji_sdk/data_received_from_remote_device </td>
    <td> cruiser/landing_flag
      <br> cruiser/tracking_flag
      <br> cruiser/tracking_position </td>
    <td> Read message from Mobile and pub </td>
    <td> @Cuijie12358 </td>
  </tr>

  <!-- Landing Mission -->
  <tr>
    <td> landing_alg_node </td>
    <td> /dji_sdk/image_raw
      <br> cruiser/landing_flag </td>
    <td> cruiser/landing_move </td>
    <td> Hough circle detection </td>
    <td> @ShoupingShan
      <br> @hanzheteng </td>
  </tr>
  <tr>
    <td> landing_move_node </td>
    <td> cruiser/landing_move </td>
    <td> --- </td>
    <td> Movement control </td>
    <td> @Cuijie12358 </td>
  </tr>

  <!-- Tracking Mission -->
  <tr>
    <td> tracking_alg_node </td>
    <td> /dji_sdk/image_raw
      <br> cruiser/tracking_flag
      <br> cruiser/tracking_position </td>
    <td> cruiser/tracking_move </td>
    <td> Object tracking algorithm </td>
    <td> @ShoupingShan
      <br> @hanzheteng </td>
  </tr>
  <tr>
    <td> tracking_move_node </td>
    <td> cruiser/tracking_move </td>
    <td> --- </td>
    <td> Movement control </td>
    <td> @Cuijie12358 </td>
  </tr>
</table>

## ROS Topic Format
<table>
  <tr>
    <th> Topic name </th>
    <th> Format </th>
    <th> Message file </th>
    <th> Description </th>
  </tr>
  <tr>
    <td> dji-sdk/ ... </td>
    <td> ( *many* ) </td>
    <td> ( *many* ) </td>
    <td> Official SDK </td>
  </tr>

  <!-- Landing Mission -->
  <tr>
    <td> cruiser/landing_flag </td>
    <td> bool flag </td>
    <td> flag.msg </td>
    <td> Start or stop landing flag </td>
  </tr>
  <tr>
    <td> cruiser/landing_move </td>
    <td> float32 delta_X_meter
      <br> float32 delta_Y_meter </td>
    <td> move.msg </td>
    <td> Delta X and Y distance in ground coordinate system </td>
  </tr>

  <!-- Tracking Mission -->
  <tr>
    <td> cruiser/tracking_flag </td>
    <td> bool flag </td>
    <td> flag.msg </td>
    <td> Start or stop tracking flag </td>
  </tr>
  <tr>
    <td> cruiser/tracking_position </td>
    <td> float32 width_percent
      <br> float32 height_percent </td>
    <td> position.msg </td>
    <td> Point position in percentage </td>
  </tr>
  <tr>
    <td> cruiser/tracking_move </td>
    <td> float32 delta_X_meter
      <br> float32 delta_Y_meter </td>
    <td> move.msg </td>
    <td> Delta X and Y distance in ground coordinate system </td>
  </tr>
</table>
