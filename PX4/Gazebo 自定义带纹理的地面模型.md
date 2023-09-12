## Gazebo 自定义带纹理的地面模型

### 1. 新建模型文件夹

* ```
  mkdir ~/.gazebo/models/xxx
  ```

* ```
  mdkir -p ~/.gazebo/models/xxx/materials/textures 
  ```

* ```
  mdkir -p ~/.gazebo/models/xxx/materials/scripts

### 2. 添加纹理图片和描述文件

#### 2.1 添加纹理图片

* 在 textures 文件夹下放想要贴的纹理图片 ground_plane.png

#### 2.2 添加描述文件

* ```
  gedit ~/.gazebo/models/xxx/materials/scripts/my_ground_plane.material 
  ```

* ```
  material textures_name/Diffuse
  {
    receive_shadows off
    technique
    {
      pass
      {
        ambient 1 1 1 1.000000
        diffuse 1 1 1 1.000000
        specular 0.03 0.03 0.03 1.000000 
        texture_unit
        {
          texture ground_plane.png
        }
      }
    }
  }

### 3. 新建模型文件

#### 3.1 新建 model.sdf

* ```
  gedit ~/.gazebo/models/xxx/model.sdf
  ```

* ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <sdf version="1.0">
    <model name="xxx">
      <static>true</static>
      <link name="link">
        <collision name="collision">
          <geometry>
            <plane>
              <normal>0 0 1</normal>
              <size>15 15</size>
            </plane>
          </geometry>
          <surface>
            <friction>
              <ode>
                <mu>100</mu>
                <mu2>50</mu2>
              </ode>
            </friction>
          </surface>
        </collision>
        <visual name="visual">
          <cast_shadows>false</cast_shadows>
          <geometry>
            <plane>
              <normal>0 0 1</normal>
              <size>15 15</size>
            </plane>
          </geometry>
          <material>
            <script>
              <uri>model://xxx/materials/scripts</uri>
              <uri>model://xxx/materials/textures/</uri>
              <name>textures_name/Diffuse</name>
            </script>
          </material>
        </visual>
      </link>
    </model>
  </sdf>

#### 3.2 新建 model.config

* ```
  gedit ~/.gazebo/models/xxx/model.config

* ```xml
  <?xml version="1.0"?>
  
  <model>
    <name>xxx</name>
    <version>1.0</version>
    <sdf version="1.0">model.sdf</sdf>
  
    <author>
      <name>MyName</name>
    </author>
  
    <description>
      Landing Mark
    </description>
  </model>