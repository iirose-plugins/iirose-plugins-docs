# 开发指南

本页面为开发者提供使用 IIROSE 适配器进行插件开发的详细指南。

## 环境准备

### 前置要求

- Node.js >= 22.0.0
- Koishi 4.18.8+
- TypeScript 

### 安装依赖
```bash
yarn clone iirose-plugins/koishi-plugin-adapter-iirose
```
### 添加到工作区
```bash
cd external
cd adapter-iirose
code .
```
:::tip

此步骤会打开VScode，并将`adapter-iirose`添加至工作区。

此步骤可以让 `.vscode/settings.json` 生效，统一项目的代码风格。

详见 -> [.vscode/settings.json](https://github.com/iirose-plugins/koishi-plugin-adapter-iirose/blob/main/.vscode/settings.json)

:::


### 开发模式启动
```bash
cd ..
cd ..
yarn dev
```


## 代码规范问题

请前往 -> [sonarcloud.io/project/security_hotspots](https://sonarcloud.io/project/security_hotspots?id=iirose-plugins_koishi-plugin-adapter-iirose&branch=main&issueStatuses=OPEN,CONFIRMED&sinceLeakPeriod=true)

