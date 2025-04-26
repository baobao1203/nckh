<launch>
  <!-- Khởi động Gazebo -->
  <include file="$(find gazebo_ros)/launch/empty_world.launch">
    <arg name="world_name" value="$(find baocao)/src/world/world.world"/>
    <arg name="paused" value="false"/>
    <arg name="use_sim_time" value="true"/>
    <arg name="gui" value="true"/>
  </include>

  <!-- Đẩy URDF lên parameter server -->
  <param name="robot_description" command="$(find xacro)/xacro $(find baocao)/src/baocao.urdf"/>
  <node name="lidar_tf" pkg="tf" type="static_transform_publisher"  args="0 0 0.15 0 0 0 base_link lidar_link 100" />
  <node name="urdf_spawner" pkg="gazebo_ros" type="spawn_model" 
        args="-urdf -model robot -param robot_description" output="screen"/>

  <!-- Chạy node gmapping -->
  <node pkg="gmapping" type="slam_gmapping" name="slam_gmapping">
    <param name="base_frame" value="base_link"/>
    <param name="odom_frame" value="odom"/>
    <param name="map_update_interval" value="5.0"/>
    <param name="maxUrange" value="16.0"/>
    <param name="sigma" value="0.05"/>
    <param name="kernelSize" value="1"/>
    <param name="lstep" value="0.05"/>
    <param name="astep" value="0.05"/>
    <param name="iterations" value="5"/>
    <param name="lsigma" value="0.075"/>
    <param name="ogain" value="3.0"/>
    <param name="lskip" value="0"/>
    <param name="srr" value="0.1"/>
    <param name="srt" value="0.2"/>
    <param name="str" value="0.1"/>
    <param name="stt" value="0.2"/>
    <param name="linearUpdate" value="1.0"/>
    <param name="angularUpdate" value="0.5"/>
    <param name="temporalUpdate" value="-1.0"/>
    <param name="resampleThreshold" value="0.5"/>
    <param name="particles" value="100"/>
    <param name="xmin" value="-50.0"/>
    <param name="ymin" value="-50.0"/>
    <param name="xmax" value="50.0"/>
    <param name="ymax" value="50.0"/>
    <param name="delta" value="0.05"/>
    <param name="llsamplerange" value="0.01"/>
    <param name="llsamplestep" value="0.01"/>
    <param name="lasamplerange" value="0.005"/>
    <param name="lasamplestep" value="0.005"/>
  </node>

  <!-- Chạy rviz để trực quan hóa -->

  <!-- Khởi động RViz -->
  <node name="rviz" pkg="rviz" type="rviz" args="-d $(find baocao)/src/rviz/slam.rviz" output="screen"/>

  <!-- Điều khiển robot bằng teleop_twist_keyboard -->
  <node name="teleop_twist_keyboard" pkg="teleop_twist_keyboard" type="teleop_twist_keyboard.py" output="screen"/>
</launch>
