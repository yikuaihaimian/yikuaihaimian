# 👋 你好，我是唐语哲 (Yuzhe Tang)

<img align="right" src="https://komarev.com/ghpvc/?username=yikuaihaimian&label=Profile%20views&color=0e75b6&style=flat" alt="Profile views" />

🔭 **电子科技大学 | 电子信息硕士**  
💼 **后端开发工程师 (Java + AI)**  
📍 **中国 · 成都**  

---

## 📖 关于我

> 电子科技大学（985）电子信息专业硕士研究生，专注于 **Java后端开发** 与 **AI应用开发**。  
> 具备扎实的计算机系统基础，熟悉Spring生态、分布式系统设计与优化。  
> 在AI领域有深入实践，包括大模型应用开发、RAG系统、多模态智能体等前沿技术。

- 🎓 **教育背景**
  - 🎓 **硕士** - 电子科技大学（985）- 电子信息 - 2024.09-2027.06
  - 🎓 **本科** - 中国农业大学（985）- 电子信息工程 - 2020.09-2024.06

- 📧 **联系方式**
  - 📧 Email: [tyz1388@163.com](mailto:tyz1388@163.com)
  - 💬 微信: T15090851388
  - 📱 手机: 15090851388

---

## 🚀 核心项目

### 🎯 项目一：多模态智心Agent智能体（心理咨询AI助手）

**项目简介**：基于SpringAI构建的多模态心理关怀智能体，通过多模态感知、Agentic RAG与MCP外部服务联动，实现情绪识别、心理咨询、风险预警、自动干预的完整业务闭环。

**技术栈**：`Java` `SpringBoot` `SpringAI` `Flux` `Whisper` `MediaPipe` `SpringSecurity` `Chroma` `Ollama` `MCP`

#### 🎯 核心亮点

| 亮点                     | 说明                                  |
| ------------------------ | ------------------------------------- |
| 🎯 **SpringAI原生技术栈** | 对接大模型（Java后端 + AI），非常加分 |
| 🎨 **多模态感知**         | 语音 + 图像 + 文本，非常前沿          |
| 🧠 **Agentic RAG**        | 比普通RAG强不止一个档次               |
| 🎯 **LoRA微调模型**       | 领域适配优化，准确率提升85%→90%       |
| ⚠️ **高风险自动预警**     | 有业务价值，送达率98%                 |
| 💬 **流式输出接口**       | AI标配，打字机式回复                  |

#### 📊 性能指标

```
✅ 情绪识别准确率：90% (微调后，提升30%)
✅ 预警邮件送达率：98%
✅ 支持并发用户数：1000+
✅ 系统响应时间：<200ms (流式输出首字)
```

#### 🏗️ 系统架构

```
用户多模态输入 (文本/语音/视频)
    ↓
【多模态处理层】
├─ 语音 → Whisper → 文本情绪标签
├─ 图像/视频 → MediaPipe → 情绪标签+分数
└─ 文本 → 情感分析 → 文本情绪标签
    ↓
【多模态情绪融合】→ 输出：情绪标签 + 风险等级
    ↓
=================================================
          Agentic RAG 大脑
=================================================
    ↓（第一层：意图判断）
    ↙----------------------------↘
【非心理闲聊】                【心理咨询/情绪倾诉】
直接返回回答                  进入专业处理流程
(不写Excel、不预警)              ↓
                          （第二层：知识判断）
                          ↙----------------------↘
                       无需专业知识              需要查知识库
                          ↓                      ↓
                       直接生成回答            去Chroma向量库检索
                          ↓                      ↓
                       生成低幻觉回答              ↓
                                             （第三层：风险判断）
                                             ↙----------------------↘
                                          【正常/轻微情绪】    【高风险】
                                             ↓                      ↓
                                          MCP：写Excel记录    MCP：写Excel + 发送预警邮件
                                          (对话存档)          (数据存档 + 安全干预)
    ↓
【最终返回流式回答】
```

#### 🔧 技术实现细节

<details>
<summary>🔹 多模态感知模块</summary>


- **语音识别**：集成OpenAI Whisper API，实现高精度语音转文本
- **视觉情绪分析**：使用MediaPipe检测468个面部关键点，分析微表情和头部姿态
- **多模态融合**：采用加权策略（视觉50% + 语音40% + 文本10%）计算综合情绪分数

</details>

<details>
<summary>🔹 Agentic RAG智能中枢</summary>


- **意图理解**：通过Prompt工程实现零样本分类，判断用户意图（CHAT/CONSULT/RISK）
- **智能路由**：自主决策是否需要查询知识库，避免无效检索
- **多步推理**：支持分步骤、多轮检索，结合知识库内容生成专业回答
- **幻觉抑制**：通过严格Prompt约束 + 知识库检索，减少模型虚构内容

</details>

<details>
<summary>🔹 LoRA微调大模型</summary>


- **基座模型**：Qwen2.5-7B
- **数据集**：PsychQA校园心理对话数据集（2000~3000条）
- **微调策略**：冻结模型主体，只训练Q/V注意力层，rank=8, alpha=16
- **部署方式**：合并LoRA权重 → 导出GGUF格式 → Ollama部署
- **效果提升**：情绪识别准确率从60%提升至90%

</details>

<details>
<summary>🔹 MCP集成服务</summary>


- **Excel自动记录**：所有对话记录、情绪状态通过MCP服务自动写入Excel
- **邮件预警服务**：识别到高风险学生时，自动发送邮件给管理员/辅导员
- **数据一致性保障**：通过状态机和重试机制（3次重试）保证最终一致性

</details>

#### 📚 项目价值

```
✅ 解决学生不敢咨询、老师顾不过来、风险无法及时发现的痛点
✅ 提供匿名倾诉渠道，保护学生隐私
✅ 主动式心理健康辅助，实现"全员覆盖、主动干预、隐私保护"
✅ 在Java后端简历中属于绝对的亮点项目
```

---

### 🎯 项目二：StreamCore 泛娱乐内容生态平台

**项目简介**：一款支撑海量用户的泛娱乐短视频与直播互动平台，核心实现高并发视频流转、直播间营销秒杀、创作者海量数据调度及基于大模型的智能交互闭环。

**技术栈**：`SpringCloudAlibaba` `SpringAI` `Redis/Redisson` `RabbitMQ` `MySQL` `XXL-Job` `MyBatis-Plus`

#### 🎯 核心功能实现

| 功能模块                  | 技术实现                                               | 性能指标               |
| ------------------------- | ------------------------------------------------------ | ---------------------- |
| 📹 **视频播放进度追踪**    | Redis合并写请求 + DelayQueue延迟队列异步批量落库       | 高并发下进度误差 < 30s |
| 🎁 **VIP兑换码生成与验证** | 按位加权算法 + BitMap防重放                            | 支持千万级数据高效验证 |
| ⚡ **直播间秒杀系统**      | 乐观锁防超发 + Redisson分布式锁保障"一人一单"          | 支持高并发秒杀场景     |
| 📊 **创作者排行榜**        | Redis ZSet实现实时火力榜/粉丝贡献榜                    | 海量数据按月分库分表   |
| 🤖 **AI智能交互**          | 阿里云百炼大模型 + Redis短时会话记忆 + FunctionCalling | 支持信息查询和商品推荐 |

#### 📊 系统架构特点

```
✅ 高并发处理：Redis + MQ异步处理，有效削峰填谷
✅ 数据一致性：分布式锁 + 乐观锁，保障并发安全
✅ 可扩展性：分库分表 + 微服务架构，支持水平扩展
✅ AI集成：SpringAI统一接口，支持多模型策略
```

---

## 🔬 实习研究：航空制造中的智能优化算法

### 📝 论文标题

**《融合高保真预测与Agent-MOEA/D的复杂曲面喷涂轨迹控制方法》**

### 🎯 研究背景

针对大型客机舱门等大曲率航空构件自动化喷涂存在的漆膜厚度一致性差、机械臂易引发运动奇异、航空涂料浪费严重等技术难题，提出一种融合高保真厚度预测与大语言模型智能体（Agent）驱动多目标分解进化算法（Agent-MOEA/D）的喷涂轨迹协同优化方法。

### 🏗️ 技术创新点

#### 1️⃣ Agent-MOEA/D双层协同架构

```
		  ┌─────────────────────────────────────┐
		  │   顶层元控制器：LLM Agent (DeepSeek-V3)   │
		  │  - 状态感知  - 因果推理  - 策略下发      │
		  └─────────────────┬───────────────────────┘
							│ Function Calling (JSON指令)
		  ┌─────────────────▼───────────────────────┐
		  │   底层求解器：MOEA/D框架				  │
		  │  - 切比雪夫分解  - 邻域搜索			   │
		  │  - SBX交叉  - 多项式变异			   │
		  └─────────────────────────────────────┘
```

**Agent动态调度逻辑**：

- **Trigger_Singularity_Escape_Mutation**：当检测到种群陷入"姿态死锁"时，注入非线性高斯变异扰动
- **Rebuild_Tchebycheff_Weights**：当Pareto前沿分布不均时，动态重构分解权重集

#### 2️⃣ 高保真度厚度预测模型

**创新点**：打破传统静态平面假设，构建融合姿态耦合矩阵的参数自适应厚度预测模型

```
传统模型问题：
❌ 基于理想正交平面假设
❌ 无法表征倾斜喷涂导致的射流畸变
❌ 在曲率突变区预测误差极大

本文改进：
✅ 引入姿态耦合矩阵，解耦喷枪倾斜畸变效应
✅ 建立改进椭圆双β分布模型，考虑射流横截面几何畸变
✅ 在曲率极值区，预测误差削减76%以上
```

**厚度累积积分方程**：

```
T(P) = ∫₀ᵀ Σᵢ [f(xᵢ, yᵢ, zᵢ, vᵢ, θᵢ, φᵢ)] dt
```

#### 3️⃣ 融合雅可比矩阵的运动学约束机制

**问题**：在接近奇异位形时，雅可比矩阵行列式趋于0，导致理论关节速度趋于无穷大

**解决方案**：设计基于阶跃指数分布的动态惩罚函数

```
P_penalty = λ · exp(k · |J(q)|)
```

该机制强制将引发运动奇异的实数矩阵视为劣势基因，在进化迭代初期予以淘汰。

### 📊 实验结果

| 指标                       | 传统示教工艺 | Agent-MOEA/D优化 | 提升幅度 |
| -------------------------- | ------------ | ---------------- | -------- |
| **漆膜厚度标准差**         | 15.2 μm      | 7.1 μm           | ↓ 53.3%  |
| **涂料消耗**               | 1598.5 g     | 1206.7 g         | ↓ 24.51% |
| **预测误差（曲率极值区）** | >40%         | <10%             | ↓ 76%    |
| **HV指标（超体积）**       | -            | -                | ↑ 23%    |

### 🏆 学术价值

```
✅ 提出了Agent-MOEA/D双层协同架构，突破了高维受限空间的寻优"死锁"瓶颈
✅ 重构了复杂曲面漆膜高保真数字孪生环境，解决了评估失真痛点
✅ 实现了单次极低代价寻优下的卓越降本增效，具备直接工业转化价值
✅ 为航空复杂构件的高质量、低成本制造提供了智能化技术方案
```

---

## 💻 技术技能

### 🔧 后端开发

| 类别         | 技能栈                                                       |
| ------------ | ------------------------------------------------------------ |
| **编程语言** | ![Java](https://img.shields.io/badge/Java-ED8B00?style=flat&logo=java&logoColor=white) ![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white) |
| **框架**     | ![Spring](https://img.shields.io/badge/Spring-6DB33F?style=flat&logo=spring&logoColor=white) ![SpringBoot](https://img.shields.io/badge/SpringBoot-6DB33F?style=flat&logo=springboot&logoColor=white) ![MyBatis](https://img.shields.io/badge/MyBatis-black?style=flat) |
| **数据库**   | ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white) ![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=redis&logoColor=white) |
| **中间件**   | ![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat&logo=rabbitmq&logoColor=white) ![RocketMQ](https://img.shields.io/badge/RocketMQ-C72D25?style=flat) |
| **微服务**   | ![SpringCloud](https://img.shields.io/badge/SpringCloud-6DB33F?style=flat&logo=spring&logoColor=white) ![Nacos](https://img.shields.io/badge/Nacos-2592C9?style=flat) |

### 🤖 AI应用开发

| 类别           | 技能栈                                                       |
| -------------- | ------------------------------------------------------------ |
| **AI框架**     | ![SpringAI](https://img.shields.io/badge/SpringAI-6DB33F?style=flat) ![LangChain4j](https://img.shields.io/badge/LangChain4j-000000?style=flat) |
| **大模型**     | ![Ollama](https://img.shields.io/badge/Ollama-000000?style=flat&logo=ollama&logoColor=white) ![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat&logo=openai&logoColor=white) ![DeepSeek](https://img.shields.io/badge/DeepSeek-4D6BFF?style=flat) |
| **向量数据库** | ![Chroma](https://img.shields.io/badge/Chroma-FF5C39?style=flat) ![Milvus](https://img.shields.io/badge/Milvus-008F95?style=flat) |
| **AI技术**     | `RAG` `Agentic RAG` `Function Calling` `MCP协议` `LoRA微调` `多模态融合` |

### 🛠️ 工具与平台

![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat&logo=linux&logoColor=black)
![RobotStudio](https://img.shields.io/badge/RobotStudio-FFB900?style=flat)

### 📚 专业知识

- ✅ 掌握Java语法、集合、反射、多线程基础框架
- ✅ 了解JUC、锁机制、线程池原理、ThreadLocal等
- ✅ 熟悉Spring、SpringMVC、SpringBoot、MyBatis等开源框架，理解IOC、AOP机制
- ✅ 熟悉MySQL数据库及库表设计，掌握SQL优化技术
- ✅ 理解事务管理、索引、锁等数据库操作
- ✅ 了解Redis常用数据结构、持久化机制，理解缓存穿透、雪崩、击穿等问题
- ✅ 熟悉RocketMQ消息中间件，了解消息模型、消息持久化、ACK消息确认机制

---

## 🏆 竞赛与证书

- 🎖️ **英语六级**
- 🎖️ **计算机专业基础扎实**：数据结构、操作系统、计算机网络、数据库系统
- 🎖️ **2021 美国大学生数学建模竞赛 M 奖**
- 🎖️ **2022 全国大学生数学建模竞赛 省级二等奖**



---

## 📫 联系我

<div align="center">


  [![Email](https://img.shields.io/badge/Email-tyz1388%40163.com-red?style=flat&logo=gmail&logoColor=white)](mailto:tyz1388@163.com)
  [![WeChat](https://img.shields.io/badge/WeChat-T15090851388-brightgreen?style=flat&logo=wechat&logoColor=white)](https://)

</div>

---

## 🎯 研究方向

```
🔬 大模型应用开发（RAG、Agent、微调）
🔬 多模态智能感知与融合
🔬 智能优化算法（进化算法、多目标优化）
🔬 数字孪生与仿真建模
🔬 高并发系统设计与优化
```

---

<div align="center">
  <img src="https://komarev.com/ghpvc/?username=yikuaihaimian&label=Thanks%20for%20visiting!&color=0e75b6&style=flat" alt="Visitors" />
</div>


---

> 💡 **正在寻找后端开发/AI应用开发相关的实习/全职机会**  
> 如果您对我的背景感兴趣，欢迎随时联系我！ 🙏
