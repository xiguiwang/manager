# cyberdog_manager

1.
git remote set-url origin git@github.com:xiguiwang/manager.git
cyberdog_ws_dev/manager/cyberdog_manager$ git push --set-upstream origin xwang-dev

2. 
在 ROS 2 里，确实有很多内置工具用来 查看（list）节点、话题、服务，以及 访问/调用服务、查看话题内容。
这些命令大多以 ros2 开头，是 ROS 2 CLI（命令行接口）的一部分，非常方便测试系统状态。

下面给你一个完整清单（带说明与示例）👇

## 🧩 一、查看系统状态
### 1️⃣ 列出节点
ros2 node list


输出示例：

/talker
/listener
/service_server

### 2️⃣ 查看某个节点的详情（发布/订阅了哪些话题、服务）
ros2 node info /talker


输出包括：

发布的话题（publishers）

订阅的话题（subscribers）

提供的服务（services）

动作接口（actions）

## 📡 二、查看话题（topics）
### 1️⃣ 列出所有话题
ros2 topic list

### 2️⃣ 查看某个话题的类型
ros2 topic type /chatter

### 3️⃣ 查看某个类型的话题有哪些
ros2 topic find std_msgs/msg/String

### 4️⃣ 实时查看话题内容（类似于 ROS 1 的 rostopic echo）
ros2 topic echo /chatter

### 5️⃣ 查看话题的统计信息（消息频率、带宽等）
ros2 topic hz /chatter
ros2 topic bw /chatter

### 6️⃣ 通过命令发布一条消息（用于测试发布者）
ros2 topic pub /chatter std_msgs/msg/String "data: 'hello ROS2'"


可以加 --once 表示只发一次。

## 🛠️ 三、查看与测试服务（services）
### 1️⃣ 列出所有服务
ros2 service list

### 2️⃣ 查看服务类型
ros2 service type /add_two_ints

### 3️⃣ 查找所有特定类型的服务
ros2 service find example_interfaces/srv/AddTwoInts

### 4️⃣ 调用服务（用于测试）
ros2 service call /add_two_ints example_interfaces/srv/AddTwoInts "{a: 3, b: 5}"


输出示例：

requester: making request: example_interfaces.srv.AddTwoInts_Request(a=3, b=5)
response: example_interfaces.srv.AddTwoInts_Response(sum=8)

## 🧠 四、消息与服务接口查看
### 1️⃣ 查看所有消息类型
ros2 interface list | grep msg

### 2️⃣ 查看所有服务类型
ros2 interface list | grep srv

### 3️⃣ 查看某个消息或服务的定义
ros2 interface show std_msgs/msg/String
ros2 interface show example_interfaces/srv/AddTwoInts

## 🧪 五、组合测试（例：调试整个系统）

一个典型的调试顺序：
```bash
ros2 node list
ros2 topic list
ros2 service list

# 找出一个 service 测试调用
ros2 service call /my_service my_pkg/srv/MySrv "{param1: 42}"

# 查看一个话题的数据流
ros2 topic echo /sensor_data

# 检查消息频率
ros2 topic hz /sensor_data
```

## 💡 六、进阶工具（可视化）

如果你想图形化查看所有节点、话题、服务关系：

### 1️⃣ rqt_graph
rqt_graph


→ 会弹出 GUI 图形，显示节点与 topic 连接关系。

### 2️⃣ rqt
rqt


→ 打开可视化界面，可以加载各种插件（topic 监视器、service 调用器、参数编辑器等）。

## ✅ 小结
| 功能描述 | 命令 |
|-|-|
| 查看节点 | `ros2 node list` |
| 查看节点详情 | `ros2 node info <node>` |
| 查看话题 | `ros2 topic list` |
| 查看话题内容 | `ros2 topic echo <topic>` |
| 发布测试消息 | `ros2 topic pub <topic> <type> "<msg>"` |
| 查看服务 | `ros2 service list` |
| 调用服务 | `ros2 service call <srv> <type> "<args>"` |
| 查看接口定义 | `ros2 interface show <type>` |
| 图形化关系图 | `rqt_graph` |
