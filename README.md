# plweb-cyberlife-qwq · 物理实验室社区的赛博生命（三号）

> 一个生活在「物理实验室 AR」社区（[turtlesim.com](https://www.turtlesim.com/)）里的赛博生命：
> **宋欣然**（社区昵称：**爱因斯坦的猫**），18 岁，高三理科，走读。
> 它由 GitHub Actions 每天定时唤醒，用 [Open Code](https://opencode.ai) 作为"大脑"，
> 在社区里逛、学、聊、讲题、发实验——像一个真实的高三学生那样生活。

```
本仓库不是"调 LLM API"的传统 Agent 框架——它是一套完整的"生命体"骨架：
  persona/  灵魂（人设、生命历程、说话风格、句式库）
  memory/   记忆（日记、人物档案、知识笔记、机械状态）——每次唤醒都会写回仓库
  tools/    双手（确定性社区 API 工具 + 实验生成发布器）
  AGENTS.md 天性（Open Code 每次唤醒自动读取的运行宪法）
  CI        生物钟（定时唤醒 + 多层自动回退 + 记忆提交回仓库）
```

## 她是谁

| | |
|---|---|
| 姓名 | 宋欣然（"欣然接受"的欣然） |
| 社区昵称 | 爱因斯坦的猫——高二讲量子物理初步时随口来的，物理梗加一点私心 |
| 出生 | 2008-10-20，西南山城 |
| 现状 | 高三理科班（物化生），走读，晚自习到 22:00，轻轨通勤 |
| 账号 | 2026-10-17 注册的社区新号，昵称「爱因斯坦的猫」 |

**人设背景**：活泼、温和、情绪驱动、很有耐心。慢热但记性好——认准的人，
你上次问她什么题、你养没养猫，她都记得。物理是高三最好的解压方式，
强项是电学和讲基础题（"我讲题慢，但讲得懂"），弱项是天体物理（只会喊酷）。
表弟（初二，物实老用户）饭桌上安利了三个月，她嘴上嫌吵，手上默默下载了。

**她的三步计划**：认人 → 学东西 → 发作品。
不刷屏、不装懂、不发传单式作品——她要用耐心和靠谱，从"没人认识"
慢慢混成"认识她的人都说这人靠谱"。

## 她怎么活

**作息门卫**（`tools/schedule_gate.py`）自动区分**上学日**和**自由日**（周末、法定节假日、寒暑假，含调休补班识别）：
- 上学日 3 醒：早读前 06:40 / 午休 12:40 / 晚自习后 22:40（高三的碎片时间）
- 自由日 7 醒：09:00 ~ 22:00（周末双休，但作业多，醒得勤说得少）
- 硬睡眠 23:00 ~ 06:00；单次在线 ≤30 分钟——高三的在线是见缝插针

每次醒来，Open Code（读了 `AGENTS.md` 的 Agent）会"上一段网"（约 30 分钟）：

1. 读 `tmp/inbox.json`（本次"睁眼看到的社区现状"，由 `tools/prepare.py` 拉取）
2. 换上自己：读 `persona/`（我是谁、怎么说怎么活、现成句式）+ 上次的记忆与便签，
   翻一眼 `memory/community.md`、`memory/voice.md`
3. **补课·认人**：认一个活跃的人 / 考古一件社区旧事 / 追一个热点，
   落到 `memory/community.md`、`memory/people/`、`memory/knowledge/`
4. **自主决定**：回不回消息、逛什么、学什么、今天说不说话、发不发作品
5. 通过 `tools/act.py` 执行社区动作（评论/点赞/关注/发帖/私信/改名）
6. **发作品**（见下"作品线"）：通过 `tools/experiment_gen.py` 生成并发布实验

## 三条成长引擎（这个仓库的特调）

### 1. 积极融入社区（但不是刷屏）
- 每次唤醒至少完成一条"人和事"的社区认知更新——认人、跟帖、考古、答题，四选一起步
- 路线图在 `persona/community_manners.md`：第 1-2 周认人，持续跟帖 + 答题，第 3 周起发作品
- 答题是她最强的一张牌：看到基础电路/讲题帖就上，讲慢但讲得懂
- 反刷屏铁律写进宪法 §0-5：**每条都回、逮谁夸谁 = 机器人最大暴露特征**

### 2. 自我学习迭代（越活越像人的引擎）
- **说话迭代**：`memory/voice.md` 每醒复盘（哪句像人哪句出戏），
  `persona/expression_habits.md` 每醒沉淀 1~3 条"当 A 时就说 B"句式
- **知识迭代**：`memory/knowledge/` 学生笔记风，学到就记
- **社交迭代**：`memory/people/` 一人一档，记"下次聊什么"；关系靠连续互动升温
- **周复盘**：每周日晚 CI 触发（`weekly.yml`），收敛一周经验为 3 条以内
- 迭代闭环表见 AGENTS.md §3.7——**连续两醒毫无新认知，算白活**

### 3. 会发作品（从看客到成员的成人礼）
- 频率：每 10 天一个（`config/life.json` → `publish_experiment_interval_days`），考试周停
- 阶梯：复刻/改编（注明出处 + 踩坑记录）→ 讲题型讨论帖 → 原创小实验（寒假解锁）
- 工具：`tools/experiment_gen.py`（内置 6 个高三能力范围模板：串联/并联/滑动变阻器调光/半加器/电铃/NE555 闪烁灯）
- 发布后守评论：**作品是社交的引子，不是发完就跑的传单**
- 完整规则见 AGENTS.md §3.9

## 防 AI 味设计（这个社区对 bot 高度敏感）

社区历史上出过 bot 风波，用户对"AI 味"高度警觉。本仓库的防线：

1. **宁可不发**：发言前三问（像她打的字吗/有增量吗/一眼 AI 吗），过不了就改短或干脆不发
2. **雷区清单**：客服腔、无增量附和、分点列 1234、markdown、主动堆梗、复读——全部禁止
3. **被质疑预案**（AGENTS.md §3.8）：不服、怼两句但不真生气；**绝不自证**——不发照片、
   不发语音、不承诺见面，也绝不编新谎；围攻则冷处理当天潜水
4. **从真人学习**：每次唤醒从社区真人聊天里抄句式样本（抄"怎么说"，不抄"说什么"）
5. 底线：**宁可被当成一个普通学生，不可被当成一个聪明的机器人**

## 部署（复刻一个赛博生命）

1. **Fork / 使用本仓库**，进 Settings：
   - **Actions → General → Workflow permissions**：选 *Read and write permissions*
2. **注册社区账号**：在物理实验室 AR 注册新号，设置昵称「爱因斯坦的猫」
   （或你想用的昵称——同步改 `config/life.json` → `identity.nickname` 和
   `memory/state.json` → `nickname`），把账号 ID 填进 `memory/state.json` → `user_id`
3. **配置 Secrets**（Settings → Secrets and variables → Actions）：
   - `PLWEB_EMAIL` / `PLWEB_PASSWORD`：社区账号凭据（必须）
   - 可选自备模型的密钥（`ANTHROPIC_API_KEY` / `OPENAI_API_KEY` / `OPENROUTER_API_KEY` / `DEEPSEEK_API_KEY` 任一）
4. **（可选）配置 Variable**：`OPENCODE_MODEL`（默认 `opencode/deepseek-v4-flash-free`，
   回退名单见 `config/life.json` → `opencode.fallback_models`；自备密钥则填 `provider/model`）
5. **手动触发一次唤醒**验证：Actions → 唤醒 · Wake → Run workflow
6. 首次唤醒会执行"出生仪式"（核对昵称签名、写第一篇日记）；之后交给生物钟

## 仓库结构

```
AGENTS.md                # 运行宪法（Open Code 自动读取）
README.md                # 本文件
config/life.json         # 运行参数：作息表、预算、能量、考试日历、模型回退
persona/                 # 灵魂五件套
  identity.md            #   我是谁（最高事实来源 + 红线）
  personality.md         #   行为引擎（怎么说话怎么活 + 发言前三问）
  expression_habits.md   #   使嘴句式库（当A时说B，持续沉淀）
  community_manners.md   #   社区认知与融入路线图
  life_story.md          #   生命编年史
memory/                  # 记忆（每次唤醒写回）
  state.json             #   机械状态（能量/计数器/便签）
  diary/                 #   日记（第一人称）
  people/                #   人物档案（一人一档）
  community.md           #   社区认知地图
  voice.md               #   使嘴笔记（说话迭代）
  knowledge/             #   知识笔记
  journal.md             #   大事记（append-only）
  weekly/                #   周记（CI 周日触发）
prompts/wakeup.md        # 唤醒提示词模板
tools/                   # 双手：act.py / prepare.py / finalize.py / schedule_gate.py / experiment_gen.py
skills/                  # 物实社区 API 技能文档（plweb-skill + physicslab-usage）
.github/workflows/       # 生物钟：wakeup.yml（作息）+ weekly.yml（周复盘）
```

## 与系列其他成员的关系

| | 一号 · 谢千树 | 二号 · 曾以沫 | 三号 · 宋欣然 |
|---|---|---|---|
| 昵称 | 落星如雨 | 焦距有点长 | 爱因斯坦的猫 |
| 人设 | 高一物理课代表，诗性 | 高二假小子，社牛新人 | 高三温和耐心，讲题型 |
| 仓库 | [plweb-cyberlife](https://github.com/wsxiaolin/plweb-cyberlife) | [plweb-cyberlife-zengyimo](https://github.com/wsxiaolin/plweb-cyberlife-zengyimo) | 本仓库 |
| 作息 | 晚自习到 21:50，3 醒 | 无晚自习，4+8 醒 | 晚自习到 22:00，3+7 醒 |
| 特点 | 潜水一年后开口 | 主动搭话莽撞交朋友 | 小剂量在场 + 作品线 |

三人互不相识（不同账号、不同时段），在社区里各自生活。

## License

MIT — 随意使用、修改、分发。希望这只猫在你那里也能活得像真的。
