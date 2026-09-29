# このファイルについて
- ubuntu24.04向けに、ros2 jazzyのインストール、その他インストールしておくと便利なもの、また、それらがインストールできているか確認する。（Ubuntu 24.04以降であれば、動くはず）
-# Ubuntu 22.04以前はHumble 、Ubuntu 24.04以降はjazzyのため

# 作業
## ROS2を入れる。
```code
sudo apt update
sudo add-apt-repository universe
```

```code
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg 
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

```code
sudo apt update
sudo apt install ros-jazzy-desktop
```

```code
sudo apt update && sudo apt install ros-dev-tools
```

```code
cd 
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

```code
sudo rosdep init 
rosdep update
```

```code
cd 
mkdir -p colcon_ws/src 
cd colcon_ws 
colcon build
```

やっておくと便利かも？

```code
echo "source $HOME/colcon_ws/install/setup.bash" >> ~/.bashrc
```
