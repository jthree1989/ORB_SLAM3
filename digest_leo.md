# System -- 系统类
## 0. TrackStereo() -- 系统入口函数 
```c++
cv::Mat System::TrackStereo(const cv::Mat &imLeft, const cv::Mat &imRight, const double &timestamp, const vector<IMU::Point>& vImuMeas, string filename)
```
## 1. ORBVocabulary -- DBoW词袋模块  
## 2. KeyFrameDatabase -- 关键帧管理模块  
## 3. Atlas -- 地图管理模块  
### 3.1 Map -- 地图类

## 4. 显示模块

  ### 4.1 FrameDrawer -- 帧显示类
### 4.2 MapDrawer -- 地图显示类

### 4.3 Viewer -- 显示窗口类

## 5. Tracking -- 前端模块
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
      -- 函数返回当前帧的世界坐标系变换Tcw(即`mCurrentFrame.mTcw`);
      
   7. Tracking State:
   ```c++
    // Tracking states
    enum eTrackingState{
        SYSTEM_NOT_READY=-1,
        NO_IMAGES_YET=0,
        NOT_INITIALIZED=1,
        OK=2,
        RECENTLY_LOST=3,
        LOST=4,
        OK_KLT=5
    };
   ```


## 6. LocalMapping -- 局部地图模块（独立线程）



## 7. LoopClosing -- 闭环检测模块（独立线程）

