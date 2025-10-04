本项目 clone 自 [River Runner](https://github.com/sdl60660/river-runner), 添加了本地化处理。

## River Runner 项目结构分析

### 📋 **项目概述**

这是一个基于 Svelte 的 Web 应用，用于可视化雨滴从世界任何点到终点（通常是海洋或内陆水体）的路径。它使用 Mapbox GL JS 进行地图渲染和动画。

### 🏗️ **技术栈**

- **前端框架**: Svelte 3.x
- **地图引擎**: Mapbox GL JS 2.13.0
- **构建工具**: Rollup
- **地理数据处理**: Turf.js (各种地理计算)
- **数据可视化**: D3.js 6.7.0
- **3D 渲染**: Three.js 0.113.0
- **后端 API**: Node.js + Express (name_server)

### 📁 **目录结构分析**

```
river-runner/
├── 📂 src/                          # 前端源码
│   ├── 📄 App.svelte                # 主应用组件
│   ├── 📄 main.js                   # 应用入口点
│   ├── 📄 config.json               # 配置文件
│   ├── 📄 state.js                  # Svelte状态管理
│   ├── 📄 access_tokens.js          # Mapbox访问令牌
│   ├── 📄 mapbox.js                 # Mapbox配置
│   ├── 📄 utils.js                  # 工具函数
│   └── 📂 components/               # UI组件
│       ├── 📄 Map.svelte            # 主地图组件 (1461行)
│       ├── 📄 InsetMap.svelte       # 小地图组件
│       ├── 📄 Controls.svelte       # 控制面板
│       ├── 📄 NavigationInfo.svelte # 导航信息
│       ├── 📄 WaterLevelDisplay.svelte # 水位显示
│       ├── 📄 Legend.svelte         # 图例
│       ├── 📄 Loader.svelte         # 加载器
│       └── 📂 utils/                # 组件工具
│           ├── 📄 mapboxUtils.js    # Mapbox相关工具
│           ├── 📄 mapUtils.js       # 地图工具
│           ├── 📄 geoUtils.js       # 地理计算工具
│           └── 📄 popupFormats.js   # 弹窗格式化
├── 📂 public/                       # 静态资源
│   ├── 📄 index.html               # HTML模板
│   ├── 📄 global.css               # 全局样式
│   ├── 📂 data/                    # 地理数据文件
│   │   ├── 📄 global_stopping_features.json
│   │   ├── 📄 rivers_simple.json
│   │   ├── 📄 us_states.json
│   │   └── 📄 active_nwis_sites.json
│   ├── 📂 shaders/                 # WebGL着色器
│   │   ├── 📂 caustics/           # 水波效果
│   │   ├── 📂 simulation/         # 水流模拟
│   │   └── 📂 water/              # 水体渲染
│   └── 📂 textures/               # 纹理资源
├── 📂 data_processing/             # 数据处理脚本
│   ├── 📄 create_global_stop_feature_file.py
│   ├── 📄 create_stop_feature_file.py
│   ├── 📄 create_coordinate_set.py
│   ├── 📄 parse_vaa_data.py
│   └── 📂 data/                   # 原始地理数据
├── 📂 name_server/                # 后端API服务
│   ├── 📄 package.json
│   ├── 📂 src/                    # 后端源码
│   │   ├── 📄 index.js            # 服务器入口
│   │   ├── 📄 app.js              # Express应用
│   │   ├── 📂 config/             # 配置
│   │   ├── 📂 models/             # 数据模型
│   │   ├── 📂 routes/             # API路由
│   │   ├── 📂 middlewares/        # 中间件
│   │   └── 📂 utils/              # 工具函数
│   └── 📄 ecosystem.config.json   # PM2配置
├── 📂 scripts/                    # 构建脚本
│   ├── 📄 gatherActiveNWISSites.js
│   └── 📄 setupTypeScript.js
├── 📄 package.json                # 前端依赖
├── 📄 rollup.config.js           # 构建配置
└── 📄 Procfile                   # Heroku部署配置
```

### 🧩 **核心组件分析**

#### 1. **Map.svelte** (主地图组件 - 1461 行)

- **功能**: 地图渲染、河流路径动画、用户交互
- **关键特性**:
  - Mapbox GL JS 集成
  - 3D 地形渲染
  - 河流路径动画
  - 状态保存/恢复
  - 中文界面支持

#### 2. **数据流架构**

```
用户点击/搜索 → 获取坐标 → 查询河流API → 计算路径 → 动画播放
```

#### 3. **状态管理** (state.js)

```javascript
- coordinates: 坐标数据
- stoppingFeature: 停止特征
- startLocation: 起始位置
```

### 🔧 **关键技术实现**

#### 1. **地图渲染**

- **地形**: DEM 数据 + 3D 地形层
- **水体**: WebGL 着色器渲染
- **动画**: 平滑的相机移动和路径跟踪

#### 2. **数据处理**

- **地理数据**: GeoJSON 格式
- **河流数据**: 基于 NHDPlus 数据集
- **高程数据**: Mapbox 地形 DEM

#### 3. **API 集成**

- **河流 API**: 全球河流路径查询
- **名称服务**: 用户建议处理
- **数据缓存**: localStorage 状态保存

### 📊 **数据源**

1. **河流数据**: River Runner API (基于 NHDPlus)
2. **地形数据**: Mapbox 地形 DEM
3. **地理边界**: Natural Earth 数据集
4. **水体数据**: OpenStreetMap + 自定义处理

### 🚀 **部署架构**

- **前端**: 静态文件部署 (Heroku/Vercel)
- **后端**: Node.js API 服务 (Heroku)
- **数据**: JSON 文件 + 外部 API
- **CDN**: 静态资源分发

### 🎯 **项目特点**

1. **实时动画**: 流畅的河流路径跟踪
2. **3D 渲染**: 地形和水体效果
3. **全球覆盖**: 支持世界范围内的河流
4. **响应式设计**: 移动端适配
5. **状态持久化**: 用户会话保存
6. **国际化**: 中文界面支持

这是一个非常复杂和精密的 Web 地图应用，结合了现代前端技术、地理信息系统(GIS)和 3D 渲染技术。

# River Runner

This project visualizes the path of a rain droplet from any point in the world to its end point (usually an ocean or an inland water features). It will find the closest river/stream flowline coordinate to a click/search and then animate along that flowline's downstream path. The data used in this project comes from the [River Runner API](https://ksonda.github.io/global-river-runner/), which is based on several open source projects and datasets. Similar data, initially used for the project, came from the USGS's [NHDPlus data](https://www.usgs.gov/core-science-systems/ngp/national-hydrography/nhdplus-high-resolution) and their [NLDI API](https://waterdata.usgs.gov/blog/nldi-intro/)

I've used mapbox to animate the downstream path, but needed to make all sorts of adjustments for elevation and bearing changes to prevent jerkiness/nausea (just moving from point to point feels a little like flying through turbulence while shaking your head side-to-side).

I've hosted a dataset with NHDPlus [Value Added Attributes](https://www.usgs.gov/core-science-systems/ngp/national-hydrography/value-added-attributes-vaas) on Firebase, which allows me to group flowlines into their parent features and determine distances quickly.

**Note**: The newly-released, global version of this project is in beta. We currently have relatively poor coverage of river names outside of the United States, which we are hoping to fill out, as well as some UX edge-cases and bugs that we hope to resolve.

## Examples

Here are a couple of examples of what it looks like in action.

This is a section of the path from eastern Turkey to the Persian Gulf:

![Screenshot of the river runner in progress from eastern Turkey to the Persian Gulf. Mountain features and river are visible.](https://github.com/sdl60660/river-runner/blob/main/public/images/preview_image.png?raw=true)

Here's part of the path from Southwest Arizona down to the Mexican border:

![Screenshot of the river runner in progress from Southwest Arizona to Mexican border. Mountain features, desert, and river are visible.](https://github.com/sdl60660/river-runner/blob/main/public/images/example-2-az.png?raw=true)

You can look at a heatmap of previous searches [here](https://river-runner-query-heatmap.vercel.app/) or find a list of some of our favorite paths [here](https://docs.google.com/document/d/1EqRNDvvCwJdfNvejHzw-0zCd6Ax-0i7nyHkU4h0M9Kg/edit?usp=sharing)

## Running this on your own

If you'd like to run this locally and play around with it, just run the following commands in your terminal (assuming you have [npm](https://www.npmjs.com/get-npm) installed):

1. `git clone https://github.com/sdl60660/river-runner.git`
2. `cd river-runner`
3. `npm install`
4. `npm run dev` (then follow the link to the local server, probably `http://localhost:5000`).
5. If you're running this on your own or forking into a new app, please replace the Mapbox Access Token strings in `src/access_tokens.js` with your own. You can generate a couple of tokens (for free), by creating a Mapbox account and visiting [this page](https://account.mapbox.com/access-tokens/). You'll need to generate two separate tokens to replace the ones in the existing file, but it does not matter which serves as the primary token and which serves as the secondary token.

## Supporters

Thank you to [Mapbox](https://www.mapbox.com/) for sponsoring this project!

<img src="https://user-images.githubusercontent.com/12772904/129089126-5c528d47-961f-427f-820f-df58974d15c3.png" alt="mapbox-logo-black" width="300"/>

## Updates

- **January 2022**: The [global version](https://river-runner-global.samlearner.com/) of this tool is now released and in beta! While some lingering issues are resolved and it remains in beta, it can be found on this branch, while the original, US-only version is preserved [here](https://github.com/sdl60660/river-runner/tree/us-only) in Github, and at its original URL: https://river-runner.samlearner.com/. This is to avoid any breaking changes to existing share links/paths due to any discrepancies and because minor US issues persist on the global version, mainly when paths involve dams, canals, or conduits.

If you'd like to be notified about major updates to the tool, you can sign up for an email list [here](https://tinyletter.com/samlearner).
