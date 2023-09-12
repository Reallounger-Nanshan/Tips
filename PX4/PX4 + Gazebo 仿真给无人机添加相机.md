## PX4 + Gazebo 仿真给无人机添加相机

### 1. 修改 mavros_posix_sitl.launch 文件

#### 1.1 进入目标文件夹

* ```
  cd ~/PX4_Firmware/launch
  ```

#### 1.2 添加参数

* ```
  gedit ./mavros_posix_sitl.launch
  ```

* ```xml
  # 添加
  
  <arg name="my_model" default="iris_fpv_cam"/>
  ```

* ```xml
  #修改
  
  # <arg name="sdf" default="$(find mavlink_sitl_gazebo)/models/$(arg vehicle)/$(arg vehicle).sdf"/>
  <arg name="sdf" default="$(find mavlink_sitl_gazebo)/models/$(arg my_model)/$(arg my_model).sdf"/>
  
  # <arg name="fcu_url" default="udp://:14540@localhost:14557"/>
  <arg name="fcu_url" default="udp://:24540@localhost:14557"/>
  
  ```

### 2. 修改模型文件

#### 2.1 进入目标文件夹

* ```
  cd ../Tools/sitl_gazebo/models/iris_fpv_cam
  ```

#### 2.2 修改相机朝向

* ```xml
  # 相机朝下
  # <pose> 标签参数分别为 x, y, z, Φ, θ, ψ
  
  <include>
        <uri>model://fpv_cam</uri>
        <pose>0 0 0 0 1.570796 0</pose>
  </include>
  ```

#### 2.3 如果没有该模型文件

##### 2.3.1 新建文件夹

* ```
  cd ../Tools/sitl_gazebo/models

* ```
  mkdir ./iris_fpv_cam && cd ./iris_fpv_cam

##### 2.3.2 添加 iris_fpv_cam.sdf 文件

* ```
  gedit ./iris_fpv_cam.sdf

* ```xml
  <?xml version='1.0'?>
  <sdf version='1.5'>
    <model name='iris_fpv_cam'>
  
      <include>
        <uri>model://iris</uri>
      </include>
  
      <include>
        <uri>model://fpv_cam</uri>
        <pose>0 0 0 0 1.570796 0</pose>
      </include>
      <joint name="fpv_cam_joint" type="fixed">
        <child>fpv_cam::link</child>
        <parent>iris::base_link</parent>
        <axis>
          <xyz>0 0 1</xyz>
          <limit>
            <upper>0</upper>
            <lower>0</lower>
          </limit>
        </axis>
      </joint>
  
    </model>
  </sdf>

##### 2.3.3 添加 model.config 文件

* ```
  gedit ./model.config
  ```

* ```xml
  <?xml version="1.0"?>
  <model>
    <name>3DR Iris with FPV camera</name>
    <version>1.0</version>
    <sdf version='1.4'>iris_fpv_cam.sdf</sdf>
  
    <author>
     <name>Amy Wagoner</name>
     <email>arwagoner@gmail.com</email>
    </author>
  
    <description>
      This is a model of the 3DR Iris Quadrotor with an FPV camera. The original model has been created by
      Thomas Gubler and is maintained by Lorenz Meier.
    </description>
  </model>

### 3. 测试

#### 3.1 运行

* ```
  roslaunch px4 mavros_posix_sitl.launch

#### 3.2 查看图像

* ```
  rqt_image_view