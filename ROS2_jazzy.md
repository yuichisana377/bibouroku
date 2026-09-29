# このファイルについて
- ubuntu24.04向けに、ros2 jazzyのインストール、その他インストールしておくと便利なもの、また、それらがインストールできているか確認する。（Ubuntu 24.04以降であれば、動くはず）
-# Ubuntu 22.04以前はHumble 、Ubuntu 24.04以降はjazzyのため

# 作業
## ROS2を入れる。

**sudo コマンドは、パスワードの入力を求められる場合はログインするときのパスワード。**

Ubuntuでターミナルを開き、(Ctrl + alt + t)
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

**これでROS２自体のインストールは終了！！**

## あるとメチャクチャ便利！「Terminator」
### どういうもの？
- もとのUbuntuに入っているものはターミナルという、１つしかターミナルが開けないが、ターミネーターだと、たくさんのターミナルが開ける！__**ROS2では、４個以上開くのは当たり前なので入れておくことを推奨！**__

### インストール
```code
sudo apt update
sudo apt install terminator
```
### ショートカットキーで開くものを変更
- インストールしただけでは、Ctrl + alt + tで開くのは普通のターミナルです。ターミネーターを開くようにしましょう
まず、次のものを実行します（ターミナルでもターミネーターでも可）

```code
sudo update-alternatives --config x-terminal-emulator
```

そうすると、番号の選択肢と、ファイルのパスが表示されるので、

```code
/usr/bin/terminator
```

になっているところの番号を入力。
**完了！！**

### ターミネーターの主なショートカットキー
- 水平分割: Ctrl + Shift + O
- 垂直分割: Ctrl + Shift + E
- ペイン移動: Alt + 矢印キー (上下左右)
- 現在のペインを閉じる: Ctrl + Shift + W

# ROS2のインストール確認
- ROS2で有名なもので、turtleノードがあります。試そう！！（ターミネーター推奨）

```code
ros2 run turtlesim turtlesim_node
```

エラーなく実行され、青い画面にカメ（？）が表示されたら成功！！！
Ctrl + Shift + O を入力し、ターミナルを分割。そして、

```code
ros2 run turtlesim turtle_teleop_key
```

を入力し、そうすると、そのターミナルを選択していれば、キーボードで前の手順で出したカメが動き始める！
