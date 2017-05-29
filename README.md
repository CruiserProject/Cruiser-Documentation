# Cruiser-Documentation
Documentation for Cruiser Project.

Documentation maintainer: [@hanzheteng](https://github.com/hanzheteng)

## 1. "Cruiser" Project Introduction
<table>
  <tr>
    <th> Repositories </th>
    <th> Description ( <I> Functions </I> ) </th>
    <th> Platform </th>
    <th> Collaborators </th>
  </tr>
  <tr>
    <td> <a href="https://github.com/CruiserProject/Cruiser-OnboardROS">
      OnboardROS </a> </td>
    <td> <ul>
      <li> Movement control </li>
      <li> Object detection and tracking </li>
      <li> Autonomous landing by Hough circle detection </li>
      <li> Executing command from mobile terminal </li> </ul> </td>
    <td> <ul>
      <li> <a href="http://www.dji.com/matrice100"> DJI Matrice100 </a> </li>
      <li> <a href="http://www.dji.com/manifold"> Manifold </a> ( Ubuntu 14.04 ) </li>
      <li> <a href="http://wiki.ros.org/"> Robot Operating System </a> ( indigo ) </li>
      <li> <a href="https://github.com/dji-sdk/Onboard-SDK-ROS"> Onboard-SDK-ROS </a> </li> </ul> </td>
    <td> <a href="https://github.com/Cuijie12358"> @Cuijie12358 </a>
      <br> <a href="https://github.com/hanzheteng"> @hanzheteng </a>
      <br> <a href="https://github.com/XiangqianMa"> @XiangqianMa </a>
      <br> <a href="https://github.com/ShoupingShan"> @ShoupingShan </a> </td>
  </tr>
  <tr>
    <td> <a href="https://github.com/CruiserProject/Cruiser-GroundStation">
      GroundStation </a> </td>
    <td> <ul>
      <li> Object detection and classification </li>
      <li> Abnormal behavior detection </li> </ul> </td>  
    <td> <ul>
      <li> Ubuntu 14.04 </li>
      <li> <a href="http://opencv.org/"> OpenCV </a> ( 2.4.10 ) </li>
      <li> <a href="http://www.nvidia.cn/object/cudazone-cn.html"> CUDA </a> ( 8.0 ) </li>
      <li> <a href="https://pjreddie.com/darknet"> Darknet YOLO CNN </a> </li> </ul> </td>
    <td> <a href="https://github.com/finaldong"> @finaldong </a>
      <br> <a href="https://github.com/ShoupingShan"> @ShoupingShan </a> </td>
  </tr>
  <tr>
    <td> <a href="https://github.com/CruiserProject/Cruiser-MobileController">
      MobileController </a> </td>
    <td> <ul>
      <li> Autonomous cruising mission </li>
      <li> Object detection and tracking </li> </ul> </td>
    <td> <ul>
      <li> Android 5.1 or above </li> </ul> </td>
    <td> <a href="https://github.com/TrafalgarZZZ"> @TrafalgarZZZ </a>
      <br> <a href="https://github.com/hwding"> @hwding </a> </td>
  </tr>
</table>


## 2. CDT Protocol
Cruiser Data Transmission(CDT) Protocol.
### 2.1 Protocol Introduction
DJI Onboard SDK OPEN Protocol provide a method for communication between Mobile and Onboard device called Data Transparent Transmission.

Under this mechanism, we could send message to Autopilot first and Autopilot will transfer this message to Mobile or Onboard device.

<table align="center">
  <tr>
    <th colspan=3> CMD Frame </th>
  </tr>
  <tr>
    <td> CMD SET </td>
    <td> CMD ID </td>
    <td> CMD VAL
      <br> ( CDT DATA ) </td>
  </tr>
</table>

DJI Onboard SDK OPEN Protocol is only used between Onboard and Autopilot. With CMD SET `0x00` CMD ID `0xFE` we could send message from Onboard to Mobile. With CMD SET `0x02` CMD ID `0x02` Onboard device could get message from Mobile.

So we define a new set of command and acknowledge messages within CMD VAL called Cruiser Data Transmission (CDT).

### 2.2 CDT Frame
<table>
  <tr>
    <th colspan=3> CDT DATA </th>
  </tr>
  <tr>
    <td> CDT SET </td>
    <td> CDT ID </td>
    <td> CDT VAL </td>
  </tr>
</table>

<table>
  <tr>
    <th> Field </th>
    <th> Offset(byte) </th>
    <th> Size(byte) </th>
  </tr>
  <tr>
    <td> CDT SET </td>
    <td> 0 </td>
    <td> 1 </td>
  </tr>
  <tr>
    <td> CDT ID </td>
    <td> 1 </td>
    <td> 1 </td>
  </tr>
  <tr>
    <td> CDT VAL </td>
    <td> 2 </td>
    <td> vary by CDTs </td>
  </tr>
</table>

### 2.3 Function List
- Data type : unsigned char[ ]
- M-O : from Mobile to Onboard -- CDT ID is an odd number
- O-M : from Onboard to Mobile -- CDT ID is an even number
- CMD : command
- ACK : acknowledge
<table>
  <tr>
    <th> CDT SET </th>
    <th> CDT ID </th>
    <th> CDT VAL
      <br> Size(byte) </th>
    <th> Direction </th>
    <th> Description </th>
  </tr>
  <tr>
    <td> 0x00 </td>
    <td> --- </td>
    <td> --- </td>
    <td> --- </td>
    <td> --- ( <I> reserved </I> ) </td>
  </tr>

 <!-------SET 0x01-------->
  <tr>
    <td rowspan=8> 0x01
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
    <td> Stop visual detection - CMD </td>
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
    <td> Land vertically </td>
  </tr>
  <tr>
    <!--SET 0x01-->
    <td> 0x08 </td>
    <td> --- </td>
    <td> O-M </td>
    <td> Land successfully </td>
  </tr>
  <tr>
    <!--SET 0x01-->
    <td> 0x42 </td>
    <td> 2 + 2 </td>
    <td> O-M </td>
    <td> Delta X and Y position (meter)
      <br> ( <I> 2 bytes char instead of 1 float </I> ) </td>
  </tr>
  <tr>
    <!--SET 0x01-->
    <td> 0x44 </td>
    <td> 2 + 1 </td>
    <td> O-M </td>
    <td> Circle center and radius
      <br> ( <I> 1 point and 1 length </I> ) </td>
  </tr>

  <!-------SET 0x02------->
  <tr>
    <td rowspan=8> 0x02
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
    <td> 2 + 2 </td>
    <td> M-O </td>
    <td> Object position to be tracking
      <br> ( <I> 2 diagonal points of a rectangle </I> ) </td>
  </tr>
  <tr>
    <!--SET 0x02-->
    <td> 0x12 </td>
    <td> --- </td>
    <td> O-M </td>
    <td> Position - ACK </td>
  </tr>
  <tr>
    <!--SET 0x02-->
    <td> 0x42 </td>
    <td> 2 + 2 </td>
    <td> O-M </td>
    <td> Delta X and Y position (meter)
      <br> ( <I> 2 bytes char instead of 1 float </I> ) </td>
  </tr>
  <tr>
    <!--SET 0x02-->
    <td> 0x44 </td>
    <td> 2 + 2 </td>
    <td> O-M </td>
    <td> Current position of object
      <br> ( <I> 2 diagonal points of a rectangle </I> ) </td>
  </tr>
</table>


## 3. Robot Operating System
### 3.1 ROS Node Info
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
    <td> ( <I> many </I> ) </td>
    <td> ( <I> many </I> )
      <br> /dji_sdk/local_position </td>
    <td> Official SDK </td>
    <td> DJI </td>
  </tr>
  <tr>
    <td> dji_sdk_read_cam </td>
    <td> --- </td>
    <td> /dji_sdk/image_raw
      <br> /dji_sdk/camera_info </td>
    <td> Get camera image and publish </td>
    <td> DJI </td>
  </tr>
  <tr>
    <td> mobile_msg </td>
    <td> /dji_sdk/data_received_from_remote_device
      <br> cruiser/landing_move
      <br> cruiser/tracking_move
      <br> cruiser/tracking_position_now </td>
    <td> cruiser/landing_flag
      <br> cruiser/tracking_flag
      <br> cruiser/tracking_position </td>
    <td> Read message from Mobile and pub </td>
    <td> @Cuijie12358
      <br> @hanzheteng </td>
  </tr>

  <!-- Landing Mission -->
  <tr>
    <td> landing_alg_node </td>
    <td> /dji_sdk/image_raw
      <br> /dji_sdk/local_position
      <br> cruiser/landing_flag </td>
    <td> cruiser/landing_move </td>
    <td> Hough circle detection </td>
    <td> @ShoupingShan
      <br> @XiangqianMa </td>
  </tr>
  <tr>
    <td> landing_move_node </td>
    <td> /dji_sdk/local_position
      <br> cruiser/landing_move </td>
    <td> --- </td>
    <td> Movement control </td>
    <td> @Cuijie12358 </td>
  </tr>

  <!-- Tracking Mission -->
  <tr>
    <td> tracking_alg_node </td>
    <td> /dji_sdk/image_raw
      <br> /dji_sdk/local_position
      <br> cruiser/tracking_flag
      <br> cruiser/tracking_position </td>
    <td> cruiser/tracking_move
      <br> cruiser/tracking_position_now </td>
    <td> Object tracking algorithm </td>
    <td> @ShoupingShan
      <br> @XiangqianMa </td>
  </tr>
  <tr>
    <td> tracking_move_node </td>
    <td> /dji_sdk/local_position
      <br> cruiser/tracking_flag
      <br> cruiser/tracking_move </td>
    <td> --- </td>
    <td> Movement control </td>
    <td> @Cuijie12358 </td>
  </tr>
</table>

### 3.2 ROS Topic Format
<table>
  <tr>
    <th> Topic name </th>
    <th> Format </th>
    <th> Message file </th>
    <th> Description </th>
  </tr>
  <tr>
    <td> dji-sdk/ ... </td>
    <td> ( <I> many </I> ) </td>
    <td> ( <I> many </I> ) </td>
    <td> Official SDK </td>
  </tr>

  <!-- Landing Mission -->
  <tr>
    <td> cruiser/landing_flag </td>
    <td> bool flag </td>
    <td> Flag.msg </td>
    <td> Start or stop landing flag </td>
  </tr>
  <tr>
    <td> cruiser/landing_move </td>
    <td> bool state
      <br> float32 delta_X_meter
      <br> float32 delta_Y_meter </td>
    <td> DeltaPosition.msg </td>
    <td> Delta X and Y distance
      <br> in ground coordinate system </td>
  </tr>

  <!-- Tracking Mission -->
  <tr>
    <td> cruiser/tracking_flag </td>
    <td> bool flag </td>
    <td> Flag.msg </td>
    <td> Start or stop tracking flag </td>
  </tr>
  <tr>
    <td> cruiser/tracking_move </td>
    <td> bool state
      <br> float32 delta_X_meter
      <br> float32 delta_Y_meter </td>
    <td> DeltaPosition.msg </td>
    <td> Delta X and Y distance
      <br> in ground coordinate system </td>
  </tr>
  <tr>
    <td> cruiser/tracking_position </td>
    <td> float32 a_width_percent
      <br> float32 a_height_percent
      <br> float32 b_width_percent
      <br> float32 b_height_percent </td>
    <td> TrackingPosition.msg </td>
    <td> Two points on screen in percentage </td>
  </tr>
  <tr>
    <td> cruiser/tracking_position_now </td>
    <td> float32 a_width_percent
      <br> float32 a_height_percent
      <br> float32 b_width_percent
      <br> float32 b_height_percent </td>
    <td> TrackingPosition.msg </td>
    <td> Two points on screen in percentage </td>
  </tr>
</table>
