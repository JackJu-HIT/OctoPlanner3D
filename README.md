# OctoPlanner3D

基于 PCD 点云与 OctoMap 的轻量级三维全局路径规划器。

项目采用纯 C++ 实现，不依赖 ROS 生态，可方便集成到现有机器人系统、仿真平台或自主导航项目中。系统实现了从点云地图构建、可通行空间提取、障碍物碰撞检测到三维 A* 全局路径规划的完整流程，可用于四足机器人跨楼层导航场景中的导航研究。

---

## 项目特点

* 基于 PCD 点云构建 OctoMap 三维地图
* 基于 OctoMap 的占据空间建模
* 可通行空间（Traversable Space）提取
* 基于机器人尺寸的碰撞检测
* 三维 26 邻域 A* 全局路径规划
* 支持跨楼层、跨高度差场景路径搜索
* 纯 C++ 实现，不依赖 ROS
* 易于集成到现有机器人项目

---

## 规划流程

```text
PCD 点云
   │
   ▼
OctoMap 建图
   │
   ▼
可通行空间提取
   │
   ▼
碰撞检测
   │
   ▼
3D A* 搜索
   │
   ▼
全局路径
```

---

## 项目结构

```text
OctoPlanner3D
├── octomap
│   ├── include
│   │   └── pcd2octomap_converter.h
│   └── src
│       └── pcd2octomap_converter.cpp
│
├── planner
│   ├── include
│   │   └── global_planner.h
│   └── src
│       └── global_planner.cpp
│
├── pcd_files
│   └── building2_9.pcd
│
└── main.cpp
```

---

## 依赖项

### PCL

用于点云读取与处理。

```bash
sudo apt install libpcl-dev
```

---

## 编译

```bash
mkdir build
cd build

cmake ..
make -j
```

---

## 运行

```bash
./globalPlan
```

程序将自动完成：

1. 读取 PCD 点云
2. 构建 OctoMap
3. 提取可通行空间
4. 执行三维 A* 路径搜索
5. 输出规划结果

---

## 结果可视化

### 1. 查看 OctoMap 地图

程序运行后会生成：

```text
result_cleaned.bt
```

使用 OctoVis 查看：

```bash
octovis result_cleaned.bt
```

### 2. 查看规划路径

程序运行后会生成：

```text
planned_path.pcd
```

使用 PCL Viewer 查看：

```bash
pcl_viewer building2_9.pcd planned_path.pcd
```

其中：

* `building2_9.pcd`：原始环境点云
* `planned_path.pcd`：规划生成的路径点

---

## 使用示例

```cpp
auto converter =
    std::make_shared<pcd2octomap::Pcd2OctomapConverter>();

converter->convert();

auto planner =
    std::make_shared<global_planner::GlobalPlanner>();

planner->setOctomap(
    converter->getOctomap());

global_planner::PointPose start;
global_planner::PointPose goal;

planner->makePlan(start, goal);

std::vector<global_planner::PointPose> path;
planner->getPlannerResults(path);
```

---

## 应用场景

* 四足机器人跨楼层导航
* 无人机三维路径规划
* 室内数字孪生导航
* 巡检机器人自主导航
* 三维环境自主探索
* 学术研究与算法验证

---

## 参考项目

本项目参考：

* [jie_3d_nav](https://github.com/6-robot/jie_3d_nav)

感谢原作者的开源工作。

---

## License

MIT License

---

## 作者

**Juchunyu**

如果本项目对您的研究或工程开发有所帮助，欢迎 Star ⭐ 支持。
