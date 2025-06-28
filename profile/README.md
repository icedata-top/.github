# 简介

冰数据（iceData）是一个致力于构建虚拟歌手社群生态的非官方数据中台组织。本组织通过自动化采集、存储和分析虚拟歌手相关的投稿数据，并以 Web 应用、ChatBI 智能对话机器人和数据可视化视频等多种形式呈现数据洞察。

- 官方站点：[icedata.top](https://www.icedata.top/)
- GitHub 组织：[icedata-top](https://github.com/icedata-top)
- 飞书组织：FE0KZ09A1ZE

# 工程架构

## 数据采集层

### Hantang Daily

- **功能**：从 `video_static` 数据表全量获取视频基础数据，写入 `video_daily` 增量表
- **数据规模**：日均增量约 30 万条
- **技术栈**：Java
- **部署环境**：阿里云
- **GitHub 仓库**：[hantang-daily](https://github.com/icedata-top/hantang-daily)

### Hantang Dynamic

- **功能**：实时捕获关注用户发布/转发的新视频数据，写入 `video_static` 基础表
- **技术栈**：TypeScript
- **运行环境**：用户端
- **GitHub 仓库**：[hantang-dynamic](https://github.com/icedata-top/hantang-dynamic)

### Hantang SAAS

- **功能**：监控 `video_static` 表中标记视频，将分钟级数据写入 `video_minute` 表
- **数据规模**：日均增量约 1000 条
- **技术栈**：TypeScript
- **运行环境**：云端
- **GitHub 仓库**：[hantang-saas](https://github.com/icedata-top/hantang-saas)

## 数据存储层

### MySQL

- **角色**：主数据库（Master）
- **版本**：MySQL 8.0
- **用途**：承担核心业务数据的写入和持久化存储

### PostgreSQL

- **角色**：从数据库（Read Replica）
- **版本**：PostgreSQL 17.3
- **同步机制**：基于逻辑复制的准实时同步（延迟 < 5 分钟）
- **用途**：支撑分析型查询负载

> 当前设计为 MySQL 主、PostgreSQL 从，未来会变更为仅 PostgreSQL。

## 数据应用层

### Hantang Web

- **功能**：提供数据可视化看板及交互式分析
- **技术栈**：
  - 后端：Java + Spring Boot
  - 前端：TypeScript + Vue 3 + Ant Design Vue
- **临时访问入口**：[添加监测任务](http://hantang-add.streamlit.app)
- **GitHub 仓库**：[hantang-add](https://github.com/icedata-top/hantang-add)

### Hantang ChatBI

- **功能**：基于自然语言的智能数据查询系统
- **技术栈**：
  - 后端：Java + Spring Boot
  - AI 引擎：Dify + GPT-4 工作流

### 统计月报（停止维护状态）

- **技术栈**：Java + MySQL
- **历史资料**：
  - [Bilibili 专栏文集](https://www.bilibili.com/read/readlist/rl68394)
  - [GitHub 仓库](https://github.com/JingyuNankin/Icedata_Monthly_Statistical_Report)（待迁移至组织仓库）

### 洛天依新曲排行榜（低维护状态）

- **技术栈**：Python
- **资源链接**：
  - [Bilibili 视频合集](https://space.bilibili.com/67946083/channel/collectiondetail?sid=1364628)
  - [GitEE 仓库](https://gitee.com/icedata-foundation-frame/luotianyi-weekly-ranking)（待迁移至 GitHub）

## 其它设施

### 冰数据网盘

- **技术架构**：基于 Cloudreve 构建的企业级文件管理系统
- **功能**：提供组织内部文件共享与协作服务
