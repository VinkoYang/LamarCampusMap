# LamarCampusMap

## 项目描述

LamarCampusMap 是一个交互式的 Lamar University 校园地图应用，使用 MapLibre GL 库构建。提供校园建筑搜索、地图导航和图层切换功能，帮助学生和访客轻松找到校园内的位置。

## 功能特性

- **交互式地图**: 使用 MapLibre GL 显示校园地图，支持缩放和平移
- **搜索功能**: 快速搜索校园建筑和地点
- **地图控制**: 缩放控制按钮和定位功能
- **图层切换**: 多个地图图层选项（街道、卫星等）
- **图例**: 显示地图符号和颜色的说明
- **响应式设计**: 适配移动设备和桌面端

## 技术栈

- HTML5
- CSS3
- JavaScript
- [MapLibre GL](https://maplibre.org/) - 开源地图库

## 如何运行

### 方法1: 本地开发服务器（推荐）

1. 克隆项目到本地：
   ```bash
   git clone <repository-url>
   cd LamarCampusMap
   ```

2. 启动本地HTTP服务器：
   ```bash
   python3 -m http.server 8000
   ```

3. 在浏览器中打开：`http://localhost:8000`

### 方法2: 直接在浏览器中打开

直接在浏览器中双击打开 `index.html` 文件。

**注意**: 这种方式可能由于浏览器的同源策略限制而无法正常加载地图资源。

### 方法3: GitHub Pages

在线访问：https://vinkoyang.github.io/LamarCampusMap/


## 浏览器支持

- Chrome (推荐)
- Firefox
- Safari
- Edge

## 最近更新 (2026-04-16)

### 🔄 架构重构：从硬编码数据迁移到外部 GeoJSON

本次更新对应用架构进行了重大优化，将所有地理数据源从硬编码转换为外部 GeoJSON 文件加载，提高了代码维护性和可扩展性。

#### 📍 建筑数据（OSM Buildings）

**之前**：
- 建筑数据硬编码在 HTML 中（`const OSM_BUILDINGS` 全局对象）
- 每次更新需要修改 HTML 文件

**现在**：
- ✅ 从 `LamarCampus.geojson` 文件异步加载建筑数据
- ✅ 使用 `async/await` 处理异步加载
- ✅ 代码更整洁，减少了 ~450 行硬编码 GeoJSON

**实现细节**：
```javascript
const osmBuildingsRes = await fetch('LamarCampus.geojson');
const osmBuildingsData = await osmBuildingsRes.json();
map.addSource('osm-buildings', { type:'geojson', data:osmBuildingsData });
```

#### 🏢 真实 3D 建筑渲染

**新增功能**：
- ✅ `osm-bldg-3d` 图层：3D 建筑挤压效果
  - 根据 OSM 数据的 `levels` 属性计算高度
  - 每层 4.5 米，默认 12 米
  - 红色填充（#C8102E），90% 透明度
- ✅ `osm-bldg-outline` 图层：建筑轮廓线
- ✅ `osm-bldg-labels` 图层：显示真实建筑名称

#### 🛣️ 路径系统（Walkways）

**之前**：
- 使用 Dijkstra 算法预构建全局图 (`const G = buildGraph()`)
- 路径由 `walkwaysGeoJSON()` 函数动态生成
- 依赖硬编码的 `NODES` 和 `EDGES` 数据

**现在**：
- ✅ 从 `lamar_paths.geojson` 外部文件加载路径
- ✅ 删除 `buildGraph()` 函数和 `const G` 全局变量
- ✅ 删除 `walkwaysGeoJSON()` 函数
- ✅ Dijkstra 算法保留但优化为按需计算

**新增的 paths source**：
```javascript
map.addSource('paths', { 
  type: 'geojson', 
  data: './lamar_paths.geojson' 
});
```

#### 📊 数据文件结构

```
LamarCampusMap/
├── index.html                 # 主页面（删除了硬编码 OSM 数据）
├── LamarCampus.geojson       # OSM 建筑数据（方式 A：直接加载）
├── lamar_paths.geojson       # 校园路径数据
└── README.md                 # 本文件
```

#### 🎯 优化效果

| 指标 | 改进前 | 改进后 |
|------|--------|--------|
| HTML 文件大小 | ~883 KB | ~433 KB (-51%) |
| 初始加载方式 | 全硬编码 | 异步加载 |
| 代码维护性 | 低 | 高 |
| 数据更新流程 | 修改 HTML | 更新 GeoJSON 文件 |
| 代码可扩展性 | 受限 | 灵活 |

#### 🔧 技术实现

- **OSM 建筑加载**：使用 `fetch()` 和 `async/await` 异步加载
- **图层配置**：MapLibre GL 的 `fill-extrusion` 和 `symbol` 类型
- **高度计算**：`['*',['to-number',['get','levels']],4.5]` 动态表达式
- **路径渲染**：`dasharray: [2,2]` 虚线效果

#### 🚀 后续改进方向

- 🔲 支持动态图层切换
- 🔲 实时路径规划优化
- 🔲 建筑搜索功能增强
- 🔲 地理数据编辑工具

## 贡献

欢迎提交 Issue 和 Pull Request 来改进这个项目！

## 许可证

本项目采用 MIT 许可证。