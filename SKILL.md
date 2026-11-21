---
name: figma-motion-kit
description: Automate Figma native Motion [Beta] timeline keyframe animations via localfig MCP. Covers transparent asset upload (figma_place_image), flat layer structuring, native Motion [Beta] Plugin API calls (setTimelineDuration, playbackSettings, applyManualKeyframeTrack), 1-frame snap cuts, pure-alpha glow/sparkles, and visual verification via figma_export.
---

# Figma Motion [Beta] Kit

专为 **Figma 原生 Motion [Beta] 时间轴**打造的 AI 动效自动化 Skill（基于 `localfig` MCP 驱动）。

## 核心工作流

1. **检查桥接状态**：调用 `figma_status` 确认 Figma Desktop 已连接且当前页面可读。
2. **上传透明分件**：用 `figma_eval` 创建对应尺寸占位矩形，再调用 `figma_place_image`（`scaleMode: "FIT"`）将本地透明 PNG 上传至画布。
3. **构建扁平主画板**：
   - 新建主画板（如 `480 × 480`），将所有分件、光晕、光束、矢量星芒作为**直属子图层**平铺在画板内，方便在 Figma Motion 时间轴中直接查看和微调。
4. **写入 Figma 原生 Motion [Beta] 关键帧**：
   ```javascript
   frame.setTimelineDuration(frame.id, 3.06);
   frame.playbackSettings = { autoplay: true, loop: true, muted: false };

   node.applyManualKeyframeTrack(
     { type: "PROPERTY", name: propName },
     {
       keyframes: [
         { timelinePosition: 0.0, value: { type: "FLOAT", value: 1 }, easing: { type: "LINEAR" } },
         { timelinePosition: 0.78, value: { type: "FLOAT", value: 0 }, easing: { type: "EASE_IN_AND_OUT" } }
       ]
     },
     frame.id
   );
   ```
   - **常用轨道名**：`OPACITY`、`TRANSLATION_X`、`TRANSLATION_Y`、`SCALE_X`、`SCALE_Y`、`ROTATION`、`WIDTH`、`HEIGHT`。
   - **1 帧硬切**：在 `t - 0.001` 与 `t` 处放置一对 `LINEAR` 关键帧实现瞬切（如 `0.779s: 1` → `0.780s: 0`）。
   - **纯 Alpha 防黑边光效**：径向光晕外环使用同色 `a: 0` 渐变 + `LAYER_BLUR`；星芒使用矢量 `STAR` + `DROP_SHADOW`。
5. **视觉验收**：调用 `figma_export` 导出关键帧 PNG 并用 `read_file` 核验后，选中主画板聚焦提示用户按空格预览。
