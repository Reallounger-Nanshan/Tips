## Ubuntu 18.04 增加 Swap 分区

### 1. 查看内存和 swap 分区

* ```
  free -h

### 2. 创建一个 Swap 文件

* ```
  cd /
  ```

* ```
  sudo mkdir swap
  ```

* ```
  cd swap/
  ```

* ```
  sudo dd if=/dev/zero of=swapfile bs=1M count=20480

### 3. 激活 Swap 文件

* ```
  sudo swapon swapfile

### 4. 设置 Swap 开机自动挂载

#### 4.1 备份文件

* ```
  sudo cp /etc/fstab /etc/fstab.bak

#### 4.2 写入路径

* ```
  echo '/swap/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

### 5. 检查内存和 swap 分区

* ```
  free -h
