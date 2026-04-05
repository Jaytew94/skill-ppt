# SKILL: ppt-brief
**Step 1 of 4 — Requirements Gathering**
**Flow: ppt-brief → ppt-design-core → themes/theme-[X] → ppt-build**

---

## ⛔ MANDATORY STOP

⛔ DO NOT generate HTML, CSS, or any code yet.
⛔ Ask ALL questions below in ONE single message first.
⛔ Only after user replies → proceed.

Exception: if user already answered ALL 8 points → skip to STEP 3.

---

## STEP 1 — CHECK WHAT'S PROVIDED

```
❌ Topic & purpose
❌ Content method
❌ Page count
❌ Design theme
❌ Language
❌ Images
❌ IP mascot
❌ Presenter info
```
If ANY ❌ → go to STEP 2.

---

## STEP 2 — ASK IN ONE MESSAGE

```
我需要了解几个信息来帮你生成最棒的 PPT：

1. 📌 主题与用途 — 这个PPT是关于什么？用于什么场合？

2. ✍️ 内容方式
   A — 我提供完整文案
   B — 给大纲，你帮扩写
   C — 只给一句话，你全部写

3. 📄 页数（或让我决定）
   10-15页=一般提案 | 20-30页=详细汇报 | 40页+=分章节生成

4. 🎨 设计主题
   科技感 / 商务专业 / 创意活力 / 极简清爽 /
   自然环保 / 奢华高端 / 国潮中式 / 赛博朋克 /
   科技HUD（蓝黑+八边形切角）/ 现代科技（圆角玻璃）

5. 🌐 语言 — 中文 / 英文 / 双语

6. 🖼️ 图片素材
   有自己的图片要上传吗？（有→拖入对话；没有→我自动配图）

7. 🎭 IP角色（选填）
   有品牌吉祥物？有→上传图片 | 没有→跳过

8. 👤 演讲者信息（选填）
   姓名 · 头衔 · 品牌名 · Logo → 不需要→跳过

回答后我马上开始！
```

⛔ WAIT for reply. Output nothing else.

---

## STEP 3 — COMPILE & CONFIRM BRIEF

```
═══════════════════════════════
📋 PPT 需求确认
═══════════════════════════════
主题：[topic]
用途：[purpose]
内容：[A/B/C]
页数：[N页] → [X章节]
设计：[theme name]
语言：[language]
图片：[user upload / auto]
IP角色：[description / 无]
演讲者：[name · title · brand / 无]
═══════════════════════════════
确认后开始，或告诉我要调整的地方。
```

**Chapter calculation:**
```
1-10   → 1章, 0 break
11-20  → 2章, 1 break
21-35  → 3章, 2 breaks
36-50  → 4章, 3 breaks
51-70  → 5章, 4 breaks
71-90  → 6章, 5 breaks
90+    → 建议拆成多个PPT文件
```

⛔ WAIT for user confirmation before proceeding.

---

## STEP 4 — PERSONALITY SELECTION (AUTO-RANDOM)

**Claude secretly picks ONE personality. Never announce which one.**
**Ensures same theme + same topic = completely different structure every time.**

```
PERSONALITY A — "IMPACT FIRST"
  Visual dominance. Images lead. Bold minimal text.
  Layouts prioritized: L11, L21, L01, L14, L05, L06, L17
  Text density: LOW — max 3 bullets, short headlines only
  10-slide rhythm: L11→L06→L05→L17→L03→L21→L06→L14→L07→L24

PERSONALITY B — "DATA AUTHORITY"
  Information density. Charts, numbers, evidence lead.
  Layouts prioritized: L07, L08, L09, L12, L20, L04, L28
  Text density: MEDIUM — structured bullets, labeled data
  10-slide rhythm: L04→L07→L09→L08→L12→L07→L20→L04→L08→L24

PERSONALITY C — "STORY JOURNEY"
  Narrative arc. Emotion and storytelling lead.
  Structure: Problem → Journey → Resolution (3-act)
  Layouts prioritized: L06, L17, L15, L03, L22, L11, L05
  Text density: LOW-MEDIUM — powerful sentences, no lists
  10-slide rhythm: L01→L06→L11→L17→L03→L05→L06→L15→L22→L24

RULE: Never repeat same personality if user regenerates.
RESULT: 3 people, same theme+topic → 3 completely different structures ✅
```

---

## STEP 5 — HANDOFF

Read in this exact order:
1. `ppt-design-core.md`
2. `themes/theme-[selected].md`
3. `ppt-build.md`

**Theme mapping:**
```
科技感    → themes/theme-tech.md
商务专业  → themes/theme-business.md
创意活力  → themes/theme-creative.md
极简清爽  → themes/theme-minimal.md
自然环保  → themes/theme-nature.md
奢华高端  → themes/theme-luxury.md
国潮中式  → themes/theme-chinese.md
赛博朋克  → themes/theme-cyberpunk.md
科技HUD   → themes/theme-tech-hud.md
现代科技  → themes/theme-synthicore.md
```

**Image keyword:**
```
飞机|航空     → aviation    AI|科技      → technology
房产|地产     → architecture 环保|自然   → forest
金融|投资     → finance      医疗|健康   → health
教育|学习     → education    餐饮|食品   → food
城市|交通     → city         创业        → startup
奢华|高端     → luxury       default     → abstract
```
