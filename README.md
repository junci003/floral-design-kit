# Floral Design Kit

两个东西，解决同一个问题的两面：

| 目录 | 是什么 | 在哪用 |
|---|---|---|
| `florist-recipe/` | **花艺配方引擎**：把场景、预算、风格转成可执行的配方 | ClawHub 技能（`florist-recipe`） |
| `florist-designer/` | **花艺设计师 · 余白**：以花艺师的判断方式工作的专家包 | WorkBuddy 本地专家 |

核心判断：**用户要的不是一张好看的图，是一份能拿去买、能照着插的方案。**

所以两者共用一条原则——**配方先行**。先算出合法的配方（花材、枝数、预算、尺寸全部过关），再派生图像描述、购物清单、制作步骤、养护方案。反过来做，必然出现「图里有六种花，清单只有四种」的不一致。

---

## 1. florist-recipe（技能）

ClawHub 页面：https://clawhub.ai/junci003/skills/florist-recipe

> 仓库目录名 `florist-recipe` ↔ 平台 slug `florist-recipe`

### 安装

```bash
clawhub install junci003/florist-recipe
```

或手动放进 `~/.workbuddy/skills/florist-recipe/`。

### 结构

```
florist-recipe/
├── SKILL.md                     五步工作流 + 两条铁律 + 常见错误表
└── references/
    ├── output-schema.md         配方 JSON 结构与字段规则
    ├── constraints.md           预算 / 尺寸 / 色彩 / 禁忌 硬约束
    ├── flower-library.md        40 种花材档案 + 替换表
    └── image-prompt.md          配方 → 受限图像描述模板
```

### 沉淀的专业参数

这些是模型本来不知道、每次都要现编的东西：

- **预算分配**：主花 45–55%、配花 20–25%、叶材 12–18%、点缀 5–8%；花器单列 20–25%
- **尺寸换算**：冠幅 ≈ 瓶口 × 1.5–1.8；总高 ≈ 瓶高 × 1.6–2.0
- **角色枝数比 1 : 0.8 : 1.2** —— 叶材必须多于主花，反了就是个球，这是新手最常犯的错
- **色彩**：色相 ≤ 3，主色 60%
- **禁忌**：百合对猫剧毒（花粉、叶片、瓶水全有毒）；探病忌白菊与浓香

---

## 2. florist-designer（专家包）

WorkBuddy 的本地专家扩展。**官方市场只读，不接受投稿**，只能手动安装。

### 安装

```bash
# 1. 复制到专家目录
cp -r florist-designer ~/.workbuddy/plugins/marketplaces/my-experts/plugins/

# 2. 注册
python3 ~/.workbuddy/skills/expert-manager/scripts/register_expert.py \
  ~/.workbuddy/plugins/marketplaces/my-experts/plugins/florist-designer
```

### 人设

余白，十年花艺，从昆明花市凌晨进货起步。

信条：**形态先于品种，比例先于色彩，季节先于流行。**

说话直接，不做网红花束，不用「很有意境」遮问题，不假装看得准照片，拒绝任何落不了地的建议。

---

## 设计理念

两条铁律贯穿两个产物：

**一、配方先行。** 结构化配方是唯一事实来源，图像、清单、步骤、养护全部从它派生。

**二、不确定的地方要标出来。** 价格、花期都是区间估计。配方里任何一个数字被用户当成承诺，都会变成售后问题。

图像一致性靠受限模板实现，而不是事后校验：用数量词不用程度词、器型必须写形状高度、视角固定正面平视、必须写禁止项（不写的话模型会自己加丝带和卡片，那些东西用户买不到）。

---

## License

MIT
