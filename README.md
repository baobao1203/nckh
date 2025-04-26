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

  <!-- Spawn robot trong Gazebo -->
  <!--<node name="robot_state_publisher" pkg="robot_state_publisher" type="robot_state_publisher" />-->
  <node name="lidar_tf" pkg="tf" type="static_transform_publisher" 
        args="0 0 0.15 0 0 0 base_link lidar_link 100" />
  <node name="urdf_spawner" pkg="gazebo_ros" type="spawn_model" 
        args="-urdf -model robot -param robot_description" output="screen"/>

  <!-- Khởi động SLAM Toolbox -->
<node pkg="slam_toolbox" type="sync_slam_toolbox_node" name="slam_toolbox" output="screen">
    <param name="odom_frame" value="odom"/>
    <param name="map_frame" value="map"/>
    <param name="base_frame" value="base_link"/>
    <param name="laser_frame" value="lidar_link"/>
    <remap from="/scan" to="/scan"/>
  </node>

  <!-- Khởi động RViz -->
  <node name="rviz" pkg="rviz" type="rviz" args="-d $(find baocao)/src/rviz/slam.rviz" output="screen"/>

  <!-- Điều khiển robot bằng teleop_twist_keyboard -->
  <node name="teleop_twist_keyboard" pkg="teleop_twist_keyboard" type="teleop_twist_keyboard.py" output="screen"/>
</launch>
