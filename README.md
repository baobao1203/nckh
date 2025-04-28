1. đoạn code đọc lidar link
2.     <gazebo reference="lidar_link">
        <sensor type="ray" name="lidar_sensor">
            <update_rate>10</update_rate>
            <ray>
                <scan>
                    <horizontal>
                        <samples>360</samples>
                        <resolution>1.0</resolution>
                        <min_angle>-1.57</min_angle>
                        <max_angle>1.57</max_angle>
                    </horizontal>
                </scan>
                <range>
                    <min>0.1</min>
                    <max>10.0</max>
                    <resolution>0.01</resolution>
                </range>
            </ray>
            <plugin name="gazebo_ros_laser" filename="libgazebo_ros_laser.so">
                <topicName>/scan</topicName>
                <frameName>lidar_link</frameName>
            </plugin>
        </sensor>
    </gazebo>
    2. đoạn code đọc camera link 
     <gazebo reference="camera_link">
  <sensor type="camera" name="camera_sensor">
    <update_rate>30</update_rate>
    <camera>
      <horizontal_fov>1.3962634</horizontal_fov>
      <image>
        <width>640</width>
        <height>480</height>
        <format>R8G8B8</format>
      </image>
      <clip>
        <near>0.1</near>
        <far>100</far>
      </clip>
    </camera>
    <plugin name="gazebo_ros_camera" filename="libgazebo_ros_camera.so">
      <alwaysOn>true</alwaysOn>
      <updateRate>30</updateRate>
      <cameraName>camera</cameraName>
      <imageTopicName>image_raw</imageTopicName>
      <cameraInfoTopicName>camera_info</cameraInfoTopicName>
      <frameName>camera_link</frameName>
    </plugin>
  </sensor>
 </gazebo>
