# System -- 系统类
## 0. TrackStereo() -- 系统入口函数 
```c++
cv::Mat System::TrackStereo(const cv::Mat &imLeft, const cv::Mat &imRight, const double &timestamp, const vector<IMU::Point>& vImuMeas, string filename)
```
## 1. `class ORBVocabulary` -- DBoW词袋模块  
## 2. `class KeyFrameDatabase` -- 关键帧管理模块  
## 3. `class Atlas` -- 地图管理模块  
### 3.1 `class Map` -- 地图类

## 4. 显示模块

  ### 4.1 `class FrameDrawer` -- 帧显示类
### 4.2 `class MapDrawer` -- 地图显示类

### 4.3 `class Viewer` -- 显示窗口类

## 5. `class Tracking` -- 前端模块
  Tracking构造函数包含：
   1. System/ORBVocabulary/FrameDrawer/MapDrawer/Altas/KeyFrameDatabase等类的对象作为参数;  
   2. 包含yaml配置文件作为参数，其中

  ````c++
  ParseCamParamFile();                  // 解析相机参数
  ParseORBParamFile();                  // 解析ORB特征提取参数
  ParseIMUParamFile();                  // 解析IMU相关参数
    -- class IMU::Calib;                // IMU标定参数类，包含IMU噪声、随机游走和协方差变量
    -- class IMU::Preintegrated;        // IMU预积分类
    -- class IMU::Bias;                 // IMU陀螺仪和加速度计零偏类
  ````
   3. `InformOnlyTracking(const bool &flag)`:是否开启局部地图模块？ 
       true：只有tracking没有局部地图
       false： 开启局部地图
       
   4. `Reset()` `ResetActiveMap()`函数

   5. IMU输入函数`void Tracking::GrabImuData(const IMU::Point &imuMeasurement)`
      将IMU数据存入`std::list<IMU::Point> mlQueueImuData`容器
      
   6. 图像输入函数`cv::Mat Tracking::GrabImageStereo(const cv::Mat &imRectLeft, const cv::Mat &imRectRight, const double &timestamp, string filename)`
      #### 6.1 函数返回当前帧的世界坐标系变换Tcw(即`mCurrentFrame.mTcw`);
      #### 6.2 构造`class Frame`类;

      - 多线程调用`Frame::ExtractORB`提取双目特征点 -> 左右目特征点和特征描述子vector
      - 计算左右目内外参数，`mTrl`/`mTlr`等
      - 双目三角化特征点`ComputeStereoFishEyeMatches()`
      - 合并左右特征描述子
      - 特征点网格划分`AssignFeaturesToGrid()`
      - 特征点去畸变`UndistortKeyPoints()`

      #### 6.3 **Tracking类的主函数** `void Tracking::Track()`;
    
      - 检测图像时序问题，并作相应处理
      - 将系统Tracking状态从从NO_IMGAES_YET状态切换到NOT_INITIALIZED状态
      - IMU预积分`void Tracking::PreintegrateIMU()` -- `class Preintegrated`
        - 相邻两帧之间的IMU预积分(`mCurrentFrame.mpImuPreintegratedFrame = pImuPreintegratedFromLastFrame;`)；
        - 如果存在上一个关键帧，进行关键帧到当前帧的预积分(`mCurrentFrame.mpImuPreintegrated = mpImuPreintegratedFromLastKF`)；
      - 如果未初始化(`mState==NOT_INITIALIZED`)，进入初始化流程
        - 双目初始化`void Tracking::StereoInitialization()` 
      - 否则进入跟踪(Track)流程
        - `mbOnlyTracking == false` 局部地图(local map)可用，使用局部地图进行Tracking
          - 处理Tracking处于正常状态的情况
          - 处理Tracking处于异常状态的情况
        - `mbOnlyTracking == true`  局部地图(local map)不可用，使用VO进行Tracking


​      
   7. Tracking State:
   ```c++
    // Tracking states
    enum eTrackingState{
        SYSTEM_NOT_READY=-1,
        NO_IMAGES_YET=0,      // 未接收到有效图像帧
        NOT_INITIALIZED=1,
        OK=2,
        RECENTLY_LOST=3,
        LOST=4,
        OK_KLT=5
    };
   ```
   1. `class ORBextractor` -- ORB特征子提取类
   2. `class GeometricCamera` -- 相机类
   3.  `class KeyFrame` -- 关键帧类


## 6. LocalMapping -- 局部地图模块（独立线程）



## 7. LoopClosing -- 闭环检测模块（独立线程）

