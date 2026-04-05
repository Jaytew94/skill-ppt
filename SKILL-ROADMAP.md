# SKILL ROADMAP
**持续优化指南 — 每次在新平台或新对话继续改进**

---

## 当前版本状态

```
版本：v3.0
文件：13个 .md
主题：10个
Layout：29种
系统：排版个性A/B/C + 主题身份识别 + 可编辑数据 + html2canvas PDF
已知平台：Codex (Claude) ✅ | Antigravity (Gemini Pro) ⚠️ 持续优化
```

---

## 如何在新平台继续优化

### 第一步 — 读文件，了解现状

```
新对话开始时，先让 AI 读：
1. ppt-brief.md         → 了解需求流程
2. ppt-design-core.md   → 了解布局规则
3. ppt-build.md         → 了解引擎和已知 bug
4. SKILL-ROADMAP.md     → 了解优化方向（本文件）

然后说：「我要继续优化这套 skill，从上次结束的地方开始。」
```

### 第二步 — 告诉 AI 测试发现了什么问题

```
格式：
「我测试了 [主题名] 在 [平台]，发现：
  问题1：[描述] + [截图]
  问题2：[描述] + [截图]
请修复并更新对应的 .md 文件。」

AI 会：
1. 诊断根本原因
2. 修改对应文件
3. 输出完整更新后的文件
4. 说明修了什么
```

---

## 优化优先级（从高到低）

### 🔴 P0 — 必须修（影响基本使用）

```
[ ] Antigravity 空白页问题 — 已加 scaleToFit 修复，需测试确认
[ ] PDF 导出 — 已改 html2canvas，需在 Antigravity 测试
[ ] Remote control 翻页 — 已加 tabindex + focus，需测试真实遥控
[ ] 空白 slide 问题 — validateSlides() 已加，测试是否还出现
```

### 🟡 P1 — 应该做（影响质量）

```
[ ] 主题视觉差异化测试
    → 同内容用科技HUD + 赛博朋克生成，截图对比
    → 如果结构还是雷同 → 加强对应主题 LAYOUT IDENTITY
    → 目标：不看颜色也能分辨主题

[ ] 三种 Personality 验证
    → 同内容生成3次，每次说「换个风格」
    → 检查是否 A/B/C 真的不同结构
    → 如果雷同 → 加强 ppt-brief STEP4 强制规则

[ ] 中文字体渲染
    → 测试国潮中式主题 ZCOOL XiaoWei 是否加载
    → 如果不显示 → 改用 Noto Serif SC 作备选

[ ] 图片质量
    → picsum.photos 有时图片不相关
    → 测试不同 keyword 效果
    → 整理一份「效果好的 seed 列表」加进 ppt-brief
```

### 🟢 P2 — 可以做（锦上添花）

```
[ ] 更多主题
    → 每次发新设计 PDF → 分析 → 新建 theme-xxx.md
    → 目标：20个主题覆盖所有场景

[ ] 图表系统升级
    → 现在：counter + progress bar 可编辑
    → 目标：柱状图 + 折线图 + 饼图可编辑
    → 方案：Chart.js 内嵌 + JSON 数据存储

[ ] 团队页专属 Layout
    → 现在 L16/L23 比较基础
    → 新增：头像网格 + 个人卡片 hover 展开效果

[ ] 过渡动画多样化
    → 现在：slide/fade 两种
    → 新增：cube翻转 / wipe擦除 / zoom缩放

[ ] 手机端适配
    → 现在：只在桌面浏览器使用
    → 如需手机预览：加 meta viewport + touch-only UI

[ ] 演讲者备注
    → 添加 speaker notes 面板（按 N 键显示）
    → 演示模式：观众看主屏，演讲者看备注
```

---

## 加新主题的标准流程

```
1. 用户发 PDF/截图（至少5页）
2. AI 分析：
   - 背景色系 + 主色 + 强调色
   - 标志性形状（切角/圆角/有机曲线/几何体）
   - 特殊装饰元素（L框/扫描线/粒子/笔刷）
   - 排版风格（衬线/无衬线/等宽/手写）
   - 动画感觉（动感/安静/科技/优雅）

3. AI 生成 themes/theme-[name].md 包含：
   - CSS :root 变量
   - 3-5个签名装饰元素（带 HTML/CSS 代码）
   - Ambient 动画函数（必须有 GSAP）
   - LAYOUT IDENTITY（MANDATORY + FORBIDDEN）
   - Google Fonts CDN 链接

4. 更新 ppt-brief.md 主题映射

5. 用户在 Codex 测试 → 截图反馈 → 调整
   通常需要 3-5 轮调整才能达到预期
```

---

## 平台差异对照表

```
功能                  | Codex (Claude)  | Antigravity (Gemini)
---------------------|-----------------|--------------------
基本生成              | ✅ 稳定          | ✅ 基本可用
主题准确度            | ✅ 高            | ⚠️ 有时跳过规则
scaleToFit           | ✅ 正常          | ⚠️ 需 setTimeout 100ms
动画                  | ✅ 流畅          | ⚠️ 有时生成静态
图片                  | ✅ 正常          | ✅ 用 picsum 正常
PDF 导出              | ✅ html2canvas  | ⚠️ 需测试
Remote control       | ✅ 修复后正常    | ⚠️ 需测试
字体加载              | ✅ 正常          | ⚠️ 偶尔回退字体

Gemini 专用补丁（已写入 ppt-build.md 顶部）：
  - scaleToFit setTimeout 100ms
  - 强制最小字体尺寸
  - 强制 class="animate" 检查
  - 强制 GSAP ambient
```

---

## 版本更新记录

```
v1.0  单文件 SKILL.md，所有内容混在一起
v2.0  拆分 4 文件：brief + design + build + (1 theme)
v2.5  加 IP 人设系统（A吉祥物 + B演讲者栏）
v2.6  加图片卡三列 Layout + 删除按钮修复
v2.7  加11种文化 SVG 角色（中华/日本/韩国/西方）
v2.8  加 PDF 导出 + 空白页防护
v3.0  重构为 13 文件
      + 10个主题各有 LAYOUT IDENTITY
      + 3种 Personality 系统（A/B/C随机）
      + html2canvas PDF（一键导出）
      + 全局图片点击替换
      + Remote control 修复
      + Antigravity scaleToFit 修复
      + 可编辑数据（counter + progress bar）
      + 29种 Layout
```

---

## 日常优化习惯建议

```
每次测试后做这3件事：
  1. 截图保存（好的和坏的都保存）
  2. 记录平台 + 主题 + 问题描述
  3. 发给 AI 修复 → 更新对应 .md

每加一个新主题后：
  1. 同内容生成 3 次验证 Personality A/B/C 是否不同
  2. 和另一个相近主题对比（结构是否真的不同）
  3. 测试 PDF 导出是否完整

每个月做一次：
  1. 回顾版本记录，更新版本号
  2. 整理「效果好的设计」截图库
  3. 检查是否有新的 Google Fonts 可以升级
  4. 检查 CDN 链接是否还有效
```

---

## 快速问题诊断

```
症状：打开HTML空白
  → 检查 scaleToFit 是否用 parentElement
  → 检查是否有 setTimeout(scaleToFit, 100)
  → 检查 .scaler 的 width/height 是否 1920/1080

症状：动画不播放
  → 检查元素是否有 class="animate"
  → 检查 GSAP 是否从 CDN 加载成功
  → 检查 triggerAnim() 是否被调用

症状：PDF 内容缺失
  → 确认用 html2canvas 方案（不是 window.print）
  → 检查图片是否 CORS 允许（picsum.photos 支持）
  → 检查每页截图前是否等待 250ms

症状：所有主题排版一样
  → 检查 theme 文件是否有 LAYOUT IDENTITY 章节
  → 检查 ppt-brief STEP4 Personality 是否被执行
  → 测试：让 AI 说明它选了哪个 Personality

症状：Remote control 不工作
  → 确认 body 有 tabindex="0"
  → 确认 startPresent() 里有 document.body.focus()
  → 测试 PageDown/PageUp 键是否工作
```
