# 

基本用法

- 节点：创建节点、添加节点属性（位置、相机、光照、几何体），将节点添加到父节点（rootnode是祖宗节点）
- 动画：
    - SCNAction，行为（即添加简单的重复动画）。一些常用的物体属性变化（例如，周期性的旋转、平移缩放等）被打包成动画，通过创建SCNAction，可以指定简易动画的持续类型、周期、速度等。在上述动画基础上，还可以创建：
        - 动画序列squence：按序先后播放的一组动画
        - 动画组：同时播放的一组动画
- ScenKit中的三维数据类型
    - 矩阵/向量数据类型，SCN的向量和矩阵
- 物体变换：旋转、平移、缩放
    - 旋转rotation
        - 旋转矩阵旋转matrix4
        - 旋转向量旋转vector3
    - 平移translation
    - 缩放scale
    - 变换组合transform
- 一些问题
  
    关于坐标系的问题（物体坐标系local、世界坐标系world）
    
    - 向node添加“灯光、相机、几何体等”时，指定的位置是相对该节点local坐标系的位置
    
    关于先旋转后平移、先平移后旋转的顺序问题
    
- implicit动画
- explicit动画

问题