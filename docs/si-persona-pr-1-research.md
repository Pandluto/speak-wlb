# PR #1：祀人设与 STC 写作说明评估

研究日期：2026-10-14。范围：只读审阅 PR 和当前本机实现，保存本报告；不合并、不推送、不发 PR 评论，不修改生产代码、人设或根目录 AGENTS.md。

## 结论

**以最新 `e071cda` 为准：方向比旧版更贴合当前祀人设。建议再补少量适用边界后，作为参考文档合并；不将文档合并视为人设已上线。**

新版明确“祀保持语气，STC 把内容写清楚”，不再按严肃程度整条二选一。**旧报告对这一切换方式的意见已解决，应撤回，不再作为当前问题。** 当前还需处理的是人物语气的作用范围、完成示例的事实条件、两个不可用的外部路径、无法衡量的“50% 原设”以及任务回执的编号和状态。只需修订该文档，无需 STC 运行时或新路由。

## 核对对象与边界

- PR：[Pandluto/si-bot #1](https://github.com/Pandluto/si-bot/pull/1)。最新固定审阅提交为 [`e071cda0d79d2dbecb8b8a9fc011bb7e7051e132`](https://github.com/Pandluto/si-bot/commit/e071cda0d79d2dbecb8b8a9fc011bb7e7051e132)，分支 `chara/si-persona`，仍只新增 [docs/si-persona-AGENTS.md，46 行](https://github.com/Pandluto/si-bot/blob/e071cda0d79d2dbecb8b8a9fc011bb7e7051e132/docs/si-persona-AGENTS.md)。最新全文已读。相对旧版 `d7424c4`，本次替换前 18 行中的 10 行；第 20—46 行的 STC 块未变。
- 远端基线仍为 `main` 的 `d4c8e4ac70e4750fc0e52c85c4fc8576b84a8cf1`。最新 head 重新查询后已确认 `MERGEABLE/CLEAN`；`gh pr checks` 仍没有已报告的检查。技术上可合并，但这个状态只针对远端基线，不等于本文的语义兼容或真实模型验收。[最新元数据](/Users/sailstellar/Documents/ChatGPT/Si_Bot/.local/research/pr-1/metadata-e071cda.json:1)
- 当前本机是 `codex/qq-bot-pi-routing`，HEAD 为 `05e70ab381a78615be2c93247e18655d034d7d71`，另有未提交工作。本文的“当前架构”指本机当前文件，不把远端主分支视为同一版本。
- 远端基线递归树返回完整的 690 条记录，按 AGENTS/persona/stc/chara 筛选没有对应文件；PR head 才新增本次文档，远端根 AGENTS.md 查询为 404。本机根 AGENTS.md 和 src 则已受 Git 跟踪。因而 `CLEAN` 不能证明当前本机机器人及协议已经包含在远端或已通过兼容检查。此差异不构成本轮同步、提交大量本机代码的理由。[远端基线树](https://github.com/Pandluto/si-bot/tree/d4c8e4ac70e4750fc0e52c85c4fc8576b84a8cf1)

## 是否生效，是否改变路由

当前 Pi 的路径是 `src/persona.txt` → `identityInstruction()` → `piPrompt()` → `runPi.prepareRequest`。人设与事实校准由模块显式读取；每次模型请求，包括工具查询后的请求，都重新构造包含人设的系统消息。[agent-profile.mjs](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/agent-profile.mjs:10)、[pi-prompt.mjs](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/pi-prompt.mjs:69)、[pi-agent.mjs](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/pi-agent.mjs:77)

本次另构造管理员和 VIP 的实际 `piPrompt` 作只读核对：两者均包含现行 `PERSONA/PERSONA_REALITY`，均不包含本 PR 文件名、STC 或其外部绝对路径。该核对没有请求模型。

在本机 src、scripts、test、根 AGENTS.md 和 docs/main-agent.md 中没有发现本 PR 文档、角色卡绝对路径或 STC 路径的加载引用；主对话指引安装器也只安装既有身份、人格和转交协议。该文件名并非根目录的 AGENTS.md。因此，按当前查到的接线，**新增此文档不会自动改变 Pi、QQ 回复或主对话指引**。人为将其复制到系统提示或要求开发助手遵守，是另一次行为变更。[根 AGENTS.md](/Users/sailstellar/Documents/ChatGPT/Si_Bot/AGENTS.md:3)、[指引安装器](/Users/sailstellar/Documents/ChatGPT/Si_Bot/scripts/install-main-guidance.mjs:9)

新版区分“祀的语气”和“STC 的清晰表达”，属于写作约定。文档没有实现 GPT 路由，也没有授予 VIP 任务权限。现有权限取自可信 QQ 身份和宿主校验：VIP 留在 Pi，群聊不能进入含私聊历史的 GPT 主对话，只有管理员私聊能进入固定主对话及唯一开发 Codex。不能把“技术内容用 STC”解释为“技术内容转 GPT”。[新版第 3—15 行](https://github.com/Pandluto/si-bot/blob/e071cda0d79d2dbecb8b8a9fc011bb7e7051e132/docs/si-persona-AGENTS.md#L3)、[routing-policy.mjs](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/routing-policy.mjs:3)、[risk.mjs](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/risk.mjs:3)、[任务入口](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/tasks.mjs:20)、[Codex 转交校验](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/isa-client.mjs:116)

## 兼容性与需要调整的地方

| PR 内容 | 对当前实现的判断 | 最小调整 |
|---|---|---|
| “祀的语气始终优先”，开场和收尾保持祀（第 3、7、9 行） | 解决了旧版整条切换问题，符合当前“认真时专注利落”的人物设定。仍需说明这种优先只相对于写作风格，不能优于事实、权限、协议或用户指定格式。 | 限定为“可自由表述的用户正文”；不为 JSON、代码、固定回执标记或用户要求仅输出正文的结果添加开场收尾。角色感不要求每条都有开场或尾句。 |
| “刚才在帮你跑仓库……现在活干完了”示例（第 11 行） | 作为确已完成仓库工作的结果示例可以成立。当前 Pi 没有执行脚本或任意文件操作的工具，不能单凭角色台词自称跑过仓库或已经完成任务。问题在事实条件，不在角色语气本身。 | 加注“仅适用于已有真实执行与完成证据的场景”；普通聊天、已转交或待完成场景不复用这一事实陈述。 |
| “约 50% 贴合原设”及角色卡绝对路径（第 13、17 行） | 本机没有该角色卡，无法核对“原设”的性格、身份或世界观，百分比也没有可执行的衡量标准。不能据此断言其角色内容与现有人设冲突。 | 删除百分比和不可用的硬依赖，引用当前人设作为本轮基准；保留“聪明坦率、温柔有判断、俏皮有分寸、认真时利落”等可观察特征。 |
| “呆货”偶尔使用（第 13 行） | 与当前人设方向一致，但缺少现有“已验证管理员、气氛轻松且对方接受、不喜欢就停用”的条件。 | 直接引用现有人设称呼规则，不另立一套更宽的规则。 |
| STC 外部目录及完整 skill（第 18、31、41 行） | `/home/box/agent-data/workflows/stc/upstream/SKILL.md` 本机不存在；不能声称已读完整 STC、知道 C10 全文或完成按其要求的检查。已列出的自然、简短、准确规则本身可以独立讨论。 | 将此文明确为自足的写作约定摘录，删去不可用的“必须加载/按完整 skill 检查”和 C10 引用。以后确需完整 STC，再固定上游来源、版本和可访问入口；本 PR 不必安装插件。 |
| 任务回执必须分“做完了/需要你/知会”（第 35 行） | 若这些栏目放在结果标记之后，可与现有协议兼容；文档未明确这一顺序，采用时可能覆盖固定首行。且排队、处理中、失败都不能因栏目名被说成已做完。不是当前已发生的解析故障。 | 明写编号协议优先；最终开发结果首字符必须是 `[SI_DEV_RESULT:原编号]`，其后再写实际改动、验证和未完成项。进度按事实写“已转交，待完成”等，禁止把接收当完成。删去强制空栏目“无”，用有内容的栏目即可。 |
| 纯文档或代码正文严格 STC；文档主语等规则（第 9、44—45 行） | 新版已明确正文与外围回复的区别，较旧版清楚。后半段仍写技术问答和回执按 for chat，需避免被理解成又让整条回复套另一种语气。 | 保留新版分工，补充“STC 约束内容清晰度，人物语气只用于可自由表述部分；用户要求纯成品时不额外加外围回复”。 |

角色与事实规则的本机依据为 [persona.txt](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/persona.txt:1) 和 [persona-reality.txt](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/persona-reality.txt:1)。其中第 6 行限定“呆货”的使用范围，第 11 行明确不带入游戏组织、地名、战斗和任务剧情；在未见外部角色卡前，不宜扩大这些设定。

执行与完成的边界已写在 [pi-prompt.mjs](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/pi-prompt.mjs:59)：Pi 没有执行脚本、操作电脑或任意文件/SQL 工具；第 62 行禁止在结果确认前声称外部操作成功。新版的完成示例应服从这项既有事实约束。

回执问题有实际接线依据：根 AGENTS.md 第 5 行要求最终答复以编号标记开头；`isa-client.mjs` 第 106 行用 `text.startsWith(marker)` 提取结果，`task-rollout.mjs` 第 31 行同样检查开头。标记前增加“做完了”等正文会使该答复不被识别为对应开发结果。保留首行后，正文采用简洁技术写作完全可以兼容。[根协议](/Users/sailstellar/Documents/ChatGPT/Si_Bot/AGENTS.md:5)、[结果识别](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/isa-client.mjs:106)、[会话记录识别](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/task-rollout.mjs:31)、[待完成约定](/Users/sailstellar/Documents/ChatGPT/Si_Bot/docs/main-agent.md:36)

主对话已经区分“Pi 普通聊天加载完整人格”和“GPT/Codex 开发转交仅带简短身份”。PR 可以补充交付正文的写作偏好，但不应要求所有开发任务再读一套全量外部角色卡。若以后修改主对话的持久指引，根 AGENTS.md 已要求使用既有安装器；本轮文档评估不需要改动该链路。[docs/main-agent.md](/Users/sailstellar/Documents/ChatGPT/Si_Bot/docs/main-agent.md:6)、[开发转交内容](/Users/sailstellar/Documents/ChatGPT/Si_Bot/docs/main-agent.md:35)、[根 AGENTS.md](/Users/sailstellar/Documents/ChatGPT/Si_Bot/AGENTS.md:7)

## 建议怎样修改这个 PR

仍只修改 `docs/si-persona-AGENTS.md`，无需新增文件或安装器。建议标题改成“祀的人设与中文写作约定（提案）”，开头交代：

> 本文记录写作建议，不是自动加载的运行配置。祀的语气只作用于可自由表述的用户正文；其优先仅指不被技术写作风格覆盖，不能覆盖事实、权限、协议和用户指定格式，也不要求每条回复额外添加开场或收尾。写作方式不决定模型、工具、QQ 权限、聊天记录范围或任务路由。部署工作区的根 AGENTS.md 与 docs/main-agent.md 规定开发协议；src/agent-profile.json 规定身份与固定目标；src/persona.txt 和 src/persona-reality.txt 规定当前人物表达与事实校准。本文不能覆盖这些规则。

这些源文件在本机当前开发工作区存在，远端本 PR 基线尚未包含；文档应明确这个适用前提，避免为远端读者制造“仓库里已有这些文件”的假象。可写成路径说明，不必添加指向远端不存在文件的相对链接。

保留新版“祀负责语气，STC 负责写清楚”的分工，并补两项：

1. **事实条件**：涉及执行、记忆、工具或完成的角色台词必须有实际证据；“跑仓库、活干完了”只作为已验证完成的特定示例。人物特征及昵称条件引用当前人设，不使用“50% 原设”。
2. **回执条件**：开发结果先保留编号标记，再组织清楚的正文；进度、失败和未完成项如实说明，不强制所有问答出现三栏。用户要求仅交付文件、代码或其他固定格式时遵守其格式。

再删除不可用的外部角色卡/STC 必需加载指令，保留文档中已经写明、能够独立理解的简洁中文原则。这就足以让该 PR 作为提案有用，不需要先改根 AGENTS.md、Pi 提示词或生产配置。

## 与最小人设方案及已有验证的关系

此前两版真实模型实验均未达预先门槛：24 对中第一版新优/旧优/持平为 14/6/4，第二版为 11/4/9；门槛要求新优至少 14、旧优最多 3、硬性退步为 0。共 128 轮真实模型测试，正式人设未替换。第二版一次搜索失败场景把 `needsWeb` 设为 false，回复绕开既有固定失败文案；这说明即使只改人设，仍需观察同轮工具决策，不能只比较好不好听。[实验结果](/Users/sailstellar/Documents/ChatGPT/Si_Bot/.local/persona-eval-20261014/result.json:5)、[第二版失败记录](/Users/sailstellar/Documents/ChatGPT/Si_Bot/.local/persona-eval-20261014/result.json:60)、[Pi 失败回复条件](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/pi-agent.mjs:68)

实验是小样本、由助手评判的定性对比；搜索资料是固定模拟样本，GPT 转交为隔离桩，没有发送 QQ 消息或使用生产历史。22 项通过的是现有正式基线回归，不等于任一候选已经通过完整验收。这些实验早于本 PR，**没有测试本次 `e071cda` 文档**，只能作为保持上线门槛的背景。[验证方法和限制](/Users/sailstellar/Documents/ChatGPT/Si_Bot/.local/persona-eval-20261014/result.json:12)、[基线回归说明](/Users/sailstellar/Documents/ChatGPT/Si_Bot/.local/persona-eval-20261014/result.json:112)

之后若继续改进人设，最小方案仍是：

1. 从现行 `src/persona.txt` 出发，只吸收已经明确的表达原则和少量“用户—祀”对话示例。控制变量，不整段注入 STC，也不追加新模型调用、路由分类器或角色系统。
2. 复用现有隔离真实模型对比流程和原验收门槛，加入此次暴露的“搜索失败仍需正确标记”“处理中不能说做完”“事实回答仍有自然语气”场景。固定资料和模型参数；新场景独立于写示例的素材，避免只对示例有改善。
3. 除表达质量，还核对 `intent/needsWeb/needsCodex`、失败文案、管理员私聊与 VIP/群聊边界、回执编号。历史键和数据不改，仍按 QQ 跨群共享、私聊隔离。[现有记忆规则](/Users/sailstellar/Documents/ChatGPT/Si_Bot/src/memory-policy.mjs:1)
4. 只在效果稳定改善且协议没有退步后替换单一人设文件并按现有方式重启；未通过就保留旧版。此次 PR 文档合并本身不构成上线，也不需要重启。

## 本次实际操作

阅读最新固定 PR 文档并复用已核对的本机源码与实验记录；仅更新本研究报告。未执行 PR 文档中的外部 skill 指令，未合并、推送、评论或部署，未修改生产人设、权限、历史或路由；本次没有重新付费调用模型或发送 QQ 消息。由于没有运行代码变更，未重复运行功能测试。
