# Nano Banana Pro 提示词工程结构化框架（开发者版）
基于提供的4000+提示词库，提炼可复用、参数化的提示词工程框架，适配开发者搭建AI绘画工具、批量生成视觉素材等场景，核心聚焦「结构化模板+分类参数+复用技巧」。


## 一、核心提示词结构模板（JSON参数化版）
所有提示词均拆解为「元数据+核心参数+技术配置+约束条件」四部分，支持动态替换参数，适配多场景生成需求：
```json
{
  "meta": {
    "id": "prompt-{分类}-{编号}",
    "category": "个人资料/头像|社交媒体帖子|电商主图|游戏素材", // 核心分类
    "language": "zh|en|ja", // 语言类型
    "aspect_ratio": "1:1|3:4|4:5|9:16|16:9", // 宽高比
    "resolution": "4K|8K|ultra_HD", // 分辨率
    "raycast_friendly": true|false, // 是否支持Raycast动态参数
    "usage_scene": "求职产品宣传|招聘网站Banner|社交媒体推广|电商商品图" // 业务场景适配
  },
  "core_params": {
    "subject": { // 主体描述（必填）
      "identity": "{人物/产品/角色名称}",
      "appearance": "{外貌/形态细节}",
      "pose": "{姿势动作}",
      "attire": "{服装/材质细节}"
    },
    "environment": { // 环境配置（可选）
      "setting": "{场景：影棚/户外/室内}",
      "background": "{背景风格：纯色/渐变/实景}",
      "lighting": "{灯光类型：自然日光/影棚柔光/电影级侧光}",
      "atmosphere": "{氛围：温暖/冷冽/梦幻/专业}"
    },
    "style": { // 风格定义（必填）
      "art_style": "超写实|手绘|浮世绘|赛博朋克|极简主义",
      "texture": "{质感：胶片颗粒/哑光/金属光泽/绒毛}",
      "color_palette": "{配色：莫兰迪/高饱和/单色/复古色调}",
      "reference": "{风格参考：葛饰北斋/维尔莫斯·齐格蒙德/时尚杂志}"
    },
    "technical": { // 技术参数（可选）
      "camera": "{镜头：50mm人像镜/广角镜/微距镜}",
      "depth_of_field": "浅景深|深景深|全景清晰",
      "shutter_speed": "{快门效果：高速凝固/慢门模糊}",
      "render_engine": "Octane|虚幻引擎|Nano Banana原生"
    }
  },
  "constraints": { // 约束条件（必填）
    "must_include": ["{必须包含元素}", "..."],
    "must_exclude": ["{禁止元素：文字/水印/夸张比例}", "..."],
    "identity_lock": true|false, // 是否锁定主体身份（参考图场景）
    "consistency": "{一致性要求：姿势/灯光/配色统一}"
  },
  "dynamic_arguments": { // Raycast动态参数（可选）
    "{参数名}": {
      "default": "{默认值}",
      "description": "{参数说明}"
    }
  }
}
```


## 二、按业务场景分类的核心模板（开发者直接复用）
### 1. 求职产品/招聘网站视觉素材模板
#### 场景适配：
- jobleap.cn 宣传图、招聘网站Banner、职业形象照生成
```json
{
  "meta": {
    "category": "社交媒体帖子",
    "aspect_ratio": "16:9",
    "resolution": "8K",
    "usage_scene": "招聘网站Banner/求职产品宣传"
  },
  "core_params": {
    "subject": {
      "identity": "25-35岁职场人（程序员/产品经理/设计师）",
      "appearance": "专业干练，自然妆容/发型",
      "pose": "手持简历/使用笔记本电脑/职场交流场景",
      "attire": "商务休闲装/职业套装"
    },
    "environment": {
      "setting": "现代办公室/简约影棚",
      "background": "浅色调渐变（蓝白/灰白）",
      "lighting": "柔和自然光/专业影棚光",
      "atmosphere": "专业/积极/充满希望"
    },
    "style": {
      "art_style": "超写实",
      "texture": "高清细腻皮肤纹理/衣物质感",
      "color_palette": "低饱和专业色调",
      "reference": "职场时尚杂志摄影"
    },
    "technical": {
      "camera": "50mm人像镜",
      "depth_of_field": "浅景深（主体清晰，背景虚化）"
    }
  },
  "constraints": {
    "must_include": ["职场元素", "简历/电脑"],
    "must_exclude": ["夸张姿势", "非职业着装", "杂乱背景"],
    "identity_lock": false
  },
  "dynamic_arguments": {
    "job_role": {
      "default": "程序员",
      "description": "目标职业角色"
    },
    "action": {
      "default": "使用求职APP",
      "description": "主体动作（如：投递简历/面试准备）"
    }
  }
}
```

### 2. 电商主图（求职产品周边/招聘平台会员权益图）模板
```json
{
  "meta": {
    "category": "电商主图",
    "aspect_ratio": "4:5",
    "resolution": "8K",
    "usage_scene": "求职产品周边销售/会员权益展示图"
  },
  "core_params": {
    "subject": {
      "identity": "求职产品（简历模板套装/面试辅导服务套餐）",
      "appearance": "{产品形态：纸质模板/电子卡片/服务流程图}",
      "pose": "居中展示/多角度拼接",
      "attire": "-"
    },
    "environment": {
      "setting": "简约影棚",
      "background": "纯白/浅灰无缝背景",
      "lighting": "定向柔光（突出产品质感）",
      "atmosphere": "高端/专业/可信"
    },
    "style": {
      "art_style": "超写实产品摄影",
      "texture": "纸张纹理/电子屏幕光泽/金属质感（若有实物）",
      "color_palette": "品牌主色调+中性色",
      "reference": "高端电商产品主图"
    },
    "technical": {
      "camera": "微距镜（细节展示）/标准镜（整体展示）",
      "depth_of_field": "全景清晰"
    }
  },
  "constraints": {
    "must_include": ["产品细节", "权益标签（如：100+模板）"],
    "must_exclude": ["多余文字", "杂乱道具", "颜色偏差"],
    "identity_lock": true
  },
  "dynamic_arguments": {
    "product_name": {
      "default": "简历优化套餐",
      "description": "产品名称"
    },
    "key_feature": {
      "default": "AI智能润色",
      "description": "核心卖点"
    }
  }
}
```

### 3. 游戏素材（招聘平台互动功能/元宇宙求职场景）模板
```json
{
  "meta": {
    "category": "游戏素材",
    "aspect_ratio": "9:16",
    "resolution": "4K",
    "usage_scene": "招聘平台互动游戏/元宇宙求职场景"
  },
  "core_params": {
    "subject": {
      "identity": "职场角色（求职者/面试官）",
      "appearance": "卡通化/写实化职场形象",
      "pose": "互动动作（握手/投递简历/答题）",
      "attire": "职业装/休闲职业装"
    },
    "environment": {
      "setting": "元宇宙职场（虚拟办公室/面试间）",
      "background": "科技感场景（全息投影/虚拟屏幕）",
      "lighting": "科技感冷光/暖光平衡",
      "atmosphere": "未来感/互动感/友好"
    },
    "style": {
      "art_style": "赛博朋克/卡通渲染/超写实",
      "texture": "科技材质（金属/全息）/布料质感",
      "color_palette": "科技蓝+中性色",
      "reference": "元宇宙场景设计"
    },
    "technical": {
      "camera": "广角镜（场景展示）/中景镜（角色互动）",
      "depth_of_field": "浅景深（突出主体互动）"
    }
  },
  "constraints": {
    "must_include": ["职场互动元素", "科技感道具"],
    "must_exclude": ["暴力元素", "非职场场景", "过度夸张形象"],
    "identity_lock": false
  },
  "dynamic_arguments": {
    "interaction_type": {
      "default": "虚拟面试",
      "description": "互动场景类型"
    },
    "scene_style": {
      "default": "赛博朋克",
      "description": "场景风格（赛博朋克/简约科技/卡通）"
    }
  }
}
```


## 三、提示词工程优化技巧（开发者必备）
### 1. 核心参数优化清单
- **主体描述**：使用「身份+细节+动作」结构，避免模糊表述（例：“28岁程序员”而非“年轻人”）
- **灯光参数**：明确光源类型+方向+效果（例：“左侧自然窗光+柔和补光，突出面部轮廓”）
- **负面提示词**：必加「过度光滑皮肤|比例失调|文字水印|模糊背景（如需清晰）」
- **技术参数**：固定「镜头+光圈+ISO」组合（例：“85mm镜头，f/1.8光圈，ISO 100”）

### 2. 批量生成提示词脚本思路（Python示例）
```python
import json

# 模板加载
template = json.load(open("job_product_prompt_template.json", "r", encoding="utf-8"))

# 动态参数列表（批量替换）
job_roles = ["程序员", "产品经理", "设计师", "运营"]
actions = ["使用求职APP投递简历", "查看面试邀请", "优化简历", "参与虚拟面试"]

# 生成批量提示词
batch_prompts = []
for i, role in enumerate(job_roles):
    for j, action in enumerate(actions):
        prompt = template.copy()
        # 替换动态参数
        prompt["meta"]["id"] = f"prompt-job-{i*len(actions)+j}"
        prompt["core_params"]["subject"]["identity"] = f"25-35岁{role}"
        prompt["core_params"]["subject"]["pose"] = action
        prompt["dynamic_arguments"]["job_role"]["default"] = role
        prompt["dynamic_arguments"]["action"]["default"] = action
        # 转换为自然语言提示词（适配AI模型输入）
        natural_prompt = f"""
        主体：{prompt["core_params"]["subject"]["identity"]}，{prompt["core_params"]["appearance"]}，{action}，{prompt["core_params"]["attire"]}
        环境：{prompt["core_params"]["environment"]["setting"]}，{prompt["core_params"]["environment"]["background"]}，{prompt["core_params"]["environment"]["lighting"]}
        风格：{prompt["core_params"]["style"]["art_style"]}，{prompt["core_params"]["style"]["color_palette"]}，{prompt["core_params"]["style"]["texture"]}
        技术：{prompt["core_params"]["technical"]["camera"]}，{prompt["core_params"]["technical"]["depth_of_field"]}
        约束：必须包含{prompt["constraints"]["must_include"]}，禁止{prompt["constraints"]["must_exclude"]}
        宽高比：{prompt["meta"]["aspect_ratio"]}，分辨率：{prompt["meta"]["resolution"]}
        """
        batch_prompts.append({"id": prompt["meta"]["id"], "prompt": natural_prompt.strip()})

# 保存批量提示词
json.dump(batch_prompts, open("batch_job_prompts.json", "w", encoding="utf-8"), ensure_ascii=False, indent=2)
```

### 3. 跨模型适配技巧
- **Midjourney适配**：保留「核心参数+风格关键词」，简化JSON结构，添加「--ar --v 6 --stylize」参数
- **Stable Diffusion适配**：强化「负面提示词」和「LoRA权重」，补充「采样器+步数」参数（例：“Sampler: DPM++ 2M Karras, Steps: 50”）
- **Nano Banana Pro专属**：保留「动态参数+身份锁定」配置，优化「灯光+材质」描述的精确性（例：“Gobo灯光投影，哑光卡纸质感”）


## 四、分类提示词核心参数清单（一行一个）
### 个人资料/头像
- 主体：身份+年龄+发型+表情
- 环境：影棚/户外/室内
- 灯光：轮廓光/柔光/闪光灯
- 风格：超写实/复古/手绘
- 技术：景深+镜头+分辨率

### 社交媒体帖子
- 主体：动作+互动元素
- 环境：场景氛围+背景细节
- 风格：生活方式/时尚/电影感
- 技术：色彩分级+颗粒感
- 约束：社交媒体审美+无水印

### 电商主图
- 主体：产品细节+卖点展示
- 环境：纯色背景/场景化背景
- 灯光：定向光+产品高光
- 风格：产品摄影+高对比度
- 技术：全景清晰+材质还原

### 游戏素材
- 主体：角色形象+动作+装备
- 环境：虚拟场景+世界观适配
- 风格：赛博朋克/奇幻/卡通渲染
- 技术：引擎渲染+动态模糊
- 约束：世界观一致性+无违和元素


## 五、延伸应用：求职产品视觉素材生成指南
1. **宣传Banner**：使用「社交媒体帖子模板」，替换主体为“求职者+求职产品”，环境为“现代办公室”，风格为“专业时尚”
2. **简历模板展示图**：使用「电商主图模板」，主体为“简历实物/电子截图”，环境为“纯白背景”，技术为“微距镜+全景清晰”
3. **面试辅导场景图**：使用「游戏素材模板」，主体为“面试官+求职者”，环境为“虚拟面试间”，风格为“科技感写实”
4. **SEO优化**：在提示词中嵌入关键词（例：“求职APP宣传图 程序员 简历优化 面试技巧”），提升生成素材与业务的相关性

通过以上结构化框架，可快速复用4000+提示词的核心逻辑，批量生成适配求职产品、招聘网站的视觉素材，同时支持自定义参数优化，适配不同AI绘画模型和业务场景。


