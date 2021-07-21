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
       `ParseCamParamFile`解析相机参数，
       `ParseORBParamFile`解析ORB特征提取参数，
       `ParseIMUParamFile`解析IMU相关参数，



## 6. LocalMapping -- 局部地图模块（独立线程）



## 7. LoopClosing -- 闭环检测模块（独立线程）

