### 3.1 误差度量 - Hausdorff 距离详解

Hausdorff 距离是衡量两个点集之间相似度的重要指标。它可以直观地理解为："将一个点集中的每个点移动到另一个点集所需的最小距离"。

#### 3.1.1 数学定义

给定两个点集 A 和 B，Hausdorff 距离定义为：

$$
H(A,B) = max{ h(A,B), h(B,A) }
$$

其中 h(A,B) 是单向 Hausdorff 距离：
h(A,B) = max(min(d(a,b)))
a∈A b∈B

这里 d(a,b) 是两点间的欧氏距离。

#### 3.1.2 直观理解

1. **单向距离 h(A,B)**

   - 对于集合 A 中的每个点，找到它到集合 B 中最近的点的距离
   - 在这些最小距离中取最大值

2. **双向距离 H(A,B)**
   - 计算两个方向的单向距离：h(A,B) 和 h(B,A)
   - 取两者的最大值作为最终的 Hausdorff 距离

#### 3.1.3 示例实现

```cpp
class HausdorffDistance {
public:
    // 计算两个点集间的 Hausdorff 距离
    static double compute(const vector<Point2D>& setA, const vector<Point2D>& setB) {
        // 计算双向距离并取最大值
        double distAB = computeDirectionalDistance(setA, setB);
        double distBA = computeDirectionalDistance(setB, setA);
        return max(distAB, distBA);
    }

private:
    // 计算单向 Hausdorff 距离
    static double computeDirectionalDistance(const vector<Point2D>& setA,
                                          const vector<Point2D>& setB) {
        double maxDist = 0.0;

        // 对集合A中的每个点
        for (const auto& pointA : setA) {
            double minDist = numeric_limits<double>::max();

            // 找到到集合B中最近的点的距离
            for (const auto& pointB : setB) {
                double dist = euclideanDistance(pointA, pointB);
                minDist = min(minDist, dist);
            }

            // 更新最大距离
            maxDist = max(maxDist, minDist);
        }

        return maxDist;
    }

    // 计算两点间的欧氏距离
    static double euclideanDistance(const Point2D& p1, const Point2D& p2) {
        double dx = p1.x - p2.x;
        double dy = p1.y - p2.y;
        return sqrt(dx*dx + dy*dy);
    }
};
```

#### 3.1.4 应用场景

Hausdorff 距离在以下场景特别有用：

1. **形状匹配**

   - 评估两个形状的相似度
   - 模式识别中的轮廓匹配

2. **点云配准**

   - 评估点云对齐质量
   - 三维重建中的配准评估

3. **简化质量评估**

   - 评估简化后的模型与原始模型的差异
   - 确保简化结果的质量

4. **图像处理**
   - 图像匹配和识别
   - 目标跟踪

#### 3.1.5 优化考虑

1. **计算优化**

   - 使用空间分区（如 k-d 树）加速最近点查找
   - 并行计算不同点的距离

2. **近似计算**
   - 对于大规模点集，可以采用采样策略
   - 使用早期终止条件优化计算
