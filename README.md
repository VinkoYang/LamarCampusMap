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

## 贡献

欢迎提交 Issue 和 Pull Request 来改进这个项目！

## 许可证

本项目采用 MIT 许可证。