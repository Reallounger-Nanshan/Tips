## 创建自定义 msg 类型功能包

### 1. 创建 msg 类型功能包

#### 1.1 进入工作空间

* ```
  cd [WorkSpace]/src

#### 1.2 创建功能包

* ```
  catkin_create_pkg [package_name] roscpp rospy message_generation std_msgs
  ```

  * 根据实际需要选择依赖的功能包
    * roscpp: C++ 接口
    * rospy: Python 接口
    * message_generation: 生成消息（必须添加）
    * std_msgs: 基础消息类型
    * geometry_msgs: 提供常见图元（点、向量、姿态）消息
    * nav_msg: 用于与导航堆栈交互的通用消息



### 2. 新建 msg 文件

#### 2.1 新建文件

* ```
  cd [package_name]

* ```
  mkdir msgs
  ```

#### 2.2 设置消息类型及消息名

* ```
  gedit msgs/[msg_name].msg
  
  # 按照以下格式设置消息类型及消息名
  [msg_type] [msg_name]



### 3. 修改 package.xml

* ```xml
  gedit package.xml
  
  # 添加以下内容
  <build_depend>message_generation</build_depend>
  <exec_depend>message_runtime</exec_depend>



### 4. 修改 CMakeList.txt

* ```cmake
  gedit CMakeList.txt
  
  # 添加以下内容
  find_package(catkin REQUIRED COMPONENTS
    ...
    message_generation
  )
  add_message_files(
    FILES
    [msg_name].msg
  )
  generate_messages(
    DEPENDENCIES
    std_msgs
  )
  catkin_package(
    CATKIN_DEPENDS
    std_msgs
    message_runtime
  )



### 5. 编译

#### 5.1 编译全部功能包

* ```
  catkin_make

#### 5.2 编译自定义 msg 类型功能包

* ```
  catkin_make -DCATKIN_WHITELIST_PACKAGES="[package_name]"