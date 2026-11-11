<!-- ppt-master-schema: spec-lock/v1 -->
# Execution Lock

## canvas
- format: PPT 16:9
- viewBox: 0 0 1280 720

## communication
- primary_language: zh-CN
- audience: 技术 meetup / 团队 meeting 听众 — 工程师、技术负责人与预算决策者，部分偏工程部分偏业务；已知道大模型与提示工程 basics，未读《Agentic Design Patterns》第 17 章「推理技术」的中文对照
- objective: 让听众把四台阶推理技术谱系（会想 → 会查错 → 会动手 → 会协作）与推理扩展定律 into 一个成本-效果 决策 框架, 并把 Deep Research 的四步流程与 LangGraph 图 as 已落地形态, 最后选择一条团队行动建议
- core_message: 从「一口气回答」到「想清楚再动手」：把内部推理显式化并把推理（inference）阶段愿意投入更多思考预算，性能可预期地提升；推理扩展定律把技术选择变成成本决策，Deep Research 证明它已经落地
- consumption_mode: balanced

## mode
- mode: custom
- mode_references: narrative, pyramid
- mode_behavior: 以 narrative 负责 the whole arc page by page（每 act 是 situation → tension → resolution）with the source's metaphor library as the carrier of each turn，pyramid 负责 the closing decision gate；titles are turns rather than labels, evidence arrives as annotated exhibits with visible source lines, and the four-step ladder is re-narrated at each act boundary.

## visual_style
- visual_style: blueprint

## colors
- background: #FBF8F1
- secondary_bg: #F1EBDD
- primary: #1F111A
- accent: #B4321F
- secondary_accent: #2F5D50
- body_text: #24211C
- secondary_text: #6B655C
- divider: #D8D0C0
- grid: #E7E0D0
- image_rendering: custom
- image_rendering_references: editorial, screen-print
- image_rendering_behavior: 以 editorial 负责 the magazine infographic line and flat ink fields，screen-print 负责 the halftone dot texture and the 3-color flatness；one warm ink plus one seal-red accent only, no gradients and no cast shadows, paper grain allowed at low opacity；reads as printed essay art.

## typography
- font_family: Microsoft YaHei, Arial, sans-serif
- title_family: SimSun, Cambria, serif
- body_family: Microsoft YaHei, Arial, sans-serif
- subtitle_family: SimSun, Cambria, serif
- lead_family: Microsoft YaHei, Arial, sans-serif
- annotation_family: Microsoft YaHei, Arial, sans-serif
- footnote_family: Microsoft YaHei, Arial, sans-serif
- code_family: Consolas, "Courier New", monospace
- display_family: SimSun, Cambria, serif
- body: 24
- title: 42
- subtitle: 32
- lead: 26
- annotation: 18
- code: 18
- footnote: 14
- display: 96

## icons
- library: tabler-outline
- inventory: alert-triangle, arrow-right, arrows-exchange, books, brain, bulb, calculator, chart-line, clock, code, coin, crosshair, flag, gauge, git-branch, hierarchy, history, messages, network, pencil, refresh, scale, search, share, shield, sitemap, stack-3, target, trending-up, users, simple-icons/github, simple-icons/google, simple-icons/langchain, simple-icons/openai, simple-icons/perplexity
- stroke_width: 2

## images
- p-logo: images/logo_lockup_ink.png | source=user | crop=no-crop
- p-pelican-a: images/A.svg | source=user | crop=no-crop
- p-pelican-b: images/B.svg | source=user | crop=no-crop

## page_rhythm
- P01: anchor
- P02: breathing
- P03: anchor
- P04: dense
- P05: dense
- P06: dense
- P07: dense
- P08: dense
- P09: dense
- P10: dense
- P11: dense
- P12: dense
- P13: dense
- P14: dense
- P15: anchor

## pptx_structure
- mode: flat

## forbidden
- `mask`, `<style>`, `class`, external CSS, `<foreignObject>`, `textPath`, `@font-face`, `<animate*>`, `<set>`, `<script>` / event attributes, `<iframe>`
- HTML named entities in text; write typography as raw Unicode and escape XML reserved characters
- No web search, no placeholders, no decorative lettering (titles stay native and searchable) (user)
- No real company / product screenshot is required (user)
