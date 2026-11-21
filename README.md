# Figma Motion Kit

> **让 AI 直接操作 Figma 原生 Motion [Beta] 时间轴：自动传图、自动构建光效/星芒、自动写入全部关键帧与缓动曲线，按空格即可实时播放！**

底层通过开源桥接插件 [LucasGorgal/localfig](https://github.com/LucasGorgal/localfig)（MIT License）直连本地 Figma Desktop Plugin API，**无需云端 API、无需 Token、无额度限制**。

---

## 实测效果（Figma 原生 Motion [Beta] 自动打帧）

以下「宝箱开启」动效完全由 AI 通过 `localfig` MCP 在 Figma 画布内自动生成，并一次性写入 `3.06s` 循环时间轴（17 个直属图层轨道）：

| 闭合待机（0.00s） | 开盖爆发光束（0.78s） | 星芒扩散（1.20s） |
| :---: | :---: | :---: |
| ![待机](assets/figma_chest_idle.png) | ![爆发](assets/figma_chest_burst.png) | ![星芒](assets/figma_chest_sparkles.png) |

---

## 傻瓜式一键安装（复制这一段发给你的 AI 助手）

```text
请帮我安装并配置 https://github.com/amiduai/figma-motion-kit ：
1. 克隆仓库到 ~/.agents/skills/figma-motion-kit（如使用 Antigravity 请同步软链到 ~/.gemini/antigravity/skills/figma-motion-kit）；
2. 在我的 MCP 配置文件中加入 localfig（command: "npx", args: ["-y", "localfig"]）；
3. 告诉我如何在 Figma Desktop 中导入 ~/.localfig/plugin/manifest.json 开启桥接面板。
```

---

## Figma 原生 Motion [Beta] Plugin API 速查

通过 `figma_eval` 可直接调用以下 Figma 原生时间轴接口：

- **设置总时长**：`frame.setTimelineDuration(frame.id, durationSeconds)`
- **设置循环与自动播放**：`frame.playbackSettings = { autoplay: true, loop: true, muted: false }`
- **写入关键帧轨道**：`node.applyManualKeyframeTrack({ type: "PROPERTY", name }, { keyframes }, frame.id)`
  - 支持的常用属性：`OPACITY`、`TRANSLATION_X`、`TRANSLATION_Y`、`SCALE_X`、`SCALE_Y`、`ROTATION`、`WIDTH`、`HEIGHT`、`CORNER_RADIUS`、`STROKE_WEIGHT` 等。
  - 支持的缓动：`LINEAR`、`EASE_IN`、`EASE_OUT`、`EASE_IN_AND_OUT`、`CUSTOM_CUBIC_BEZIER`。

---

## 致谢

- Figma 本地 MCP 桥接基于开源项目 [LucasGorgal/localfig](https://github.com/LucasGorgal/localfig)（MIT License）。
