

<div align="center">
  <br />
  <h3>MindStudio msModelslim</h3>
  <p align="center">
  <h4>昇腾模型压缩工具</h4>

[📖工具文档](https://msmodelslim.readthedocs.io/zh-cn/latest/) |
[🤔Ask DeepWiki](https://deepwiki.com/Keithwwa/msmodelslim) |
[🔥昇腾社区](https://www.hiascend.com/developer/software/mindstudio)|
[🌐软件下载](https://gitcode.com/Ascend/msinsight/releases)

</div>

<br>

## ✨ 最新消息

* [2025.12.30]：MindStudio Operator Tools 项目首次上线 

## ℹ️ 简介

MindStudio Operator Tools（msOT）算子开发工具链，致力于解决算子开发中的关键挑战。通过提供高效算子设计、开发框架自动生成、全面功能调试、精准异常检测与多维性能调优等能力，
降低算子开发复杂度，提升高性能算子的交付效率。

## 🗺️ 目录结构
关键目录如下，详细目录可以参考[目录文件](docs/zh/dir_structure.md)
```
├─config             # 配置文件
└─test               # 测试目录
```

## ⚙️ 功能介绍

算子开发工具链提供以下系列化工具：

| 工具名称 | 功能简介 | 源码仓库 |
| --- | --- | :---: |
| **msKPP** | **【性能预测】** 支持输入算子描述，预测算子在特定算法实现下的性能上限。 | [点击查看](https://gitcode.com/Ascend/mskpp) |
| **msOpGen** | **【工程生成】** 算子开发效率提升工具，提供模板工程生成能力，简化工程搭建。 | [点击查看](https://gitcode.com/Ascend/msopgen) |
| **msSanitizer** | **【异常检测】** 提供内存、竞争、未初始化及同步检测，支持多核程序内存问题的精准定位。 | [点击查看](https://gitcode.com/Ascend/mssanitizer) |
| **msDebug** | **【原生调试】** 基于昇腾处理器的原生环境调试，支持变量查看、单步执行及上板调试。 | [点击查看](https://gitcode.com/Ascend/msdebug) |
| **msOpProf** | **【性能分析】** 支持上板与仿真数据采集，通过 MindStudio Insight 可视化工具定位性能瓶颈。 | [点击查看](https://gitcode.com/Ascend/msopprof) |
| **msKL** | **【快捷调用】** 提供 Python 接口，快速实现 Kernel 的代码生成、编译及下发运行。 | [点击查看](https://gitcode.com/Ascend/mskl) |


## 🚀 快速入门

以简单的加法算子开发为例，贯穿算子开发全流程，10分钟快速体验msKPP(设计)、msOpGen(开发)、msSanitizer(检测)、msDebug(调试)、msOpProf(调优)工具的核心功能。详细操作步骤请参见 [《算子开发工具链快速入门》](docs/zh/quick_start/op_tool_quick_start.md)。

## 📦 安装指南

介绍 msOT 工具的环境依赖与安装方法，具体请参见 [《msOT安装指南》](./docs/zh/install_guide/msot_install_guide.md)。

## 📘 使用指南
工具的详细使用方法，具体请参见 [《msSanitizer 使用指南》](docs/zh/user_guide/msanitizer_user_guide.md)。

## 💡 典型案例
通过典型问题场景帮助用户理解并掌握工具使用，具体请参见 [《msSanitizer 典型案例》](docs/zh/cases/msanitizer_cases.md)。

## 📚 API 参考
包含 sanitizer 接口和 mstx 扩展接口两种类型，具体请参见 [《msSanitizer 对外接口使用说明》](docs/zh/api/msanitizer_api_ref.md)。

## ❓ 常见问题
常见问题详见 [《msSanitizer FAQ》](docs/zh/faq/msanitizer_faq.md)。

## 🛠️ 贡献指南
若您有意参与项目贡献， 具体请参见 [《贡献指南》](./docs/zh/contributing/contributing_guide.md)。  

## ⚖️ 相关说明

- [《License声明》](docs/zh/legal/license_notice.md) 

- [《安全声明》](docs/zh/legal/security_statement.md) 

- [《免责声明》](docs/zh/legal/disclaimer.md)  

## 🤝 建议与交流

欢迎大家为社区做贡献。如果有任何疑问或建议，请提交 [Issues](https://gitcode.com/Ascend/msot/issues)，我们会尽快回复。感谢您的支持。

## 🙏 致谢

本工具由华为公司的下列部门贡献：   

- 计算产品线   

感谢来自社区的每一个PR，欢迎贡献。

## 关于MindStudio团队

华为MindStuido全流程开发工具链团队致力于提供端到端的昇腾AI应用开发解决方案，使能开发者高效完成训练开发、推理开发和算子开发。您可以通过以下渠道更深入了解华为MindStudio团队：
<div style="display: flex; align-items: center; gap: 10px;">
    <span>昇腾论坛：</span>
    <a href="https://www.hiascend.com/forum/" rel="nofollow">
        <img src="https://camo.githubusercontent.com/dd0b7ef70793ab93ce46688c049386e0755a18faab780e519df5d7f61153655e/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f576562736974652d2532333165333766663f7374796c653d666f722d7468652d6261646765266c6f676f3d6279746564616e6365266c6f676f436f6c6f723d7768697465" data-canonical-src="https://img.shields.io/badge/Website-%231e37ff?style=for-the-badge&amp;logo=bytedance&amp;logoColor=white" style="max-width: 100%;">
    </a>
    <span style="margin-left: 20px;">昇腾小助手：</span>
    <a href="https://gitcode.com/Ascend/msinsight/blob/master/docs/zh/user_guide/figures/readme/xiaozhushou.png">
        <img src="https://camo.githubusercontent.com/22bbaa8aaa1bd0d664b5374d133c565213636ae50831af284ef901724e420f8f/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5765436861742d3037433136303f7374796c653d666f722d7468652d6261646765266c6f676f3d776563686174266c6f676f436f6c6f723d7768697465" data-canonical-src="./docs/zh/user_guide/figures/readme/xiaozhushou.png" style="max-width: 100%;">
    </a>
</div>

<br>
<br>

## 修改理由

本次修改的核心逻辑是从“社交媒体/通用风格”转向“企业级开源/工程技术风格”，旨在提升文档的专业度、语义精准度和视觉一致性。

| 章节标题 | 修改前 Emoji | 修改后 Emoji | 新 Emoji 含义 | 原 Emoji 的不足                                                            | 修改理由与专业性提升 |
| :--- | :---: | :---: | :--- |:-----------------------------------------------------------------------| :--- |
| 最新消息 | 📢 (喇叭) | ✨ (火花) | 亮点/更新：象征新版本的发布、亮点特性或新鲜事，低调而有质感。 | 过于喧闹：📢 像广播通知或促销广告，缺乏技术文档的沉稳感，略显娱乐化。                                   | 去噪提质：技术更新日志（Changelog）应强调内容的价值而非音量。✨ 既表示“新”，又保持了界面的清爽和专业。 |
| 简介 | 📌 (图钉) | ℹ️ (信息) | 标准信息：国际通用的“关于/信息”符号，代表客观的项目概述。 | 随意性强：📌 通常用于标记重点或便签，作为正式文档的“简介”标题显得不够庄重。                               | 规范统一：ℹ️ 是开源社区（如 CNCF 项目）中“About/Introduction”的标准符号，体现中立和官方属性。 |
| 目录结构 | 🔍 (放大镜) | 🗺️ (地图) | 项目导航：象征项目的“地图”或“全景图”，指引用户了解文件分布。 | 侧重搜索：🔍 意为“查找”，而目录结构展示的是整体架构和层级关系，不仅仅是用来“找”文件的。                        | 架构视角：🗺️ 更准确地隐喻了“项目蓝图”的概念，帮助用户建立对项目整体骨架的认知，符合工程思维。 |
| 功能介绍 | 📖 (打开的书) | ⚙️ (齿轮) | 核心机制：象征工具链的内部运作、引擎、处理能力及工程化属性。 | 偏向教程：📖 更像“阅读材料”或“故事书”，无法体现算子开发工具链的“硬核技术”和“自动化能力”。                     | 突显硬核：算子开发涉及底层编译、调试和优化，⚙️ 能更好地传达“精密工具”、“底层机制”和“高性能”的技术特征。 |
| 安装指南 | 🛠️ (锤扳手) | 📦 (包裹) | 软件交付：象征软件包（Package）、部署单元或发行版，专指安装过程。 | 语义重复/偏差：🛠️ 更适合“维修”或“构建”。若功能介绍用了工具类图标，这里再用具象工具会显得重复且不够精准（安装是获取包，不是修包）。 | 精准对应：在 DevOps 领域，📦 是“Install/Deploy”的事实标准符号，清晰区分了“功能（造工具）”和“安装（拿工具）”。 |
| 相关说明 | 📝 (备忘录) | ⚖️ (天平) | 合规契约：象征法律、平衡、规则、License 及安全声明，体现严肃性。 | 过于轻量：📝 像个人笔记或待办事项，无法承载 License、安全声明等法律文件的重量感。                         | 强化合规：涉及华为及开源协议的法律文档需要庄重感。⚖️ 强调了规则的公平性和约束力，提升法律板块的专业形象。 |
| 建议交流 | 💬 (气泡) | 🤝 (握手) | 协作共建：象征合作、伙伴关系、社区贡献及双向互动。 | 偏向闲聊：💬 像即时聊天或论坛灌水，未能充分体现开源社区“共同贡献（Contribution）”的核心精神。                | 升华主题：开源不仅是提问题，更是共同建设。🤝 呼应了下文的“致谢”和“贡献指南”，体现了开发者社区的协作文化。 |
| 致谢 | 🤝 (握手) | 🙏 (合十) | 诚挚感谢：象征敬意、感恩和对他人的认可，情感色彩浓厚且得体。 | 位置冲突：若“交流”用了🤝，此处再用则重复。且🤝侧重“合作动作”，🙏侧重“感谢情感”。                         | 情感分层：将“合作（交流）”与“感恩（致谢）”在视觉上区分开。🙏 在文档末尾表达对贡献者的敬意，更符合礼仪规范。 |
| *(新增)* 使用指南 | (无) | 📘 (蓝书) | 用户手册：区别于简介的ℹ️，专门代表详细的操作手册或参考书。 | -                                                                      | 体系完整：引入📘 构建完整的文档矩阵（简介ℹ️ -> 手册📘 -> 案例💡 -> 接口📚），提升文档的可检索性。 |
| *(新增)* 典型案例 | (无) | 💡 (灯泡) | 灵感方案：象征解决问题的思路、最佳实践和启发。 | -                                                                      | 价值外显：案例不仅是步骤，更是解决方案。💡 暗示阅读后能获得“灵光一现”的解题思路。 |
| *(新增)* API参考 | (无) | 📚 (书堆) | 权威字典：象征密集的知识点、接口定义库和权威参考。 | -                                                                      | 强调权威：📚 比单本书📘更显厚重，适合存放大量接口定义的“字典”类文档，体现严谨性。 |
| *(新增)* 常见问题 | (无) | ❓ (问号) | 疑问解答：全球通用的 Q&A 符号，零认知成本。 | -                                                                      | 高效识别：❓ 是最直观的符号，让用户在遇到阻碍时能第一时间找到帮助入口。 |

### 💡 总结：修改带来的核心价值

1.  **语义精准化**：从“泛化的图形”（如喇叭、图钉）转变为“具有工程隐喻的符号”（如齿轮、地图、包裹），使图标真正服务于内容理解。
2.  **去娱乐化**：移除了带有强烈社交媒体属性（📢, 💬）的图标，替换为更符合企业级开源项目（✨, 🤝, ⚖️）的稳重符号，提升了昇腾工具链的品牌形象。
3.  **视觉层级清晰**：通过引入一套颜色协调（蓝、灰、红、黄点缀）、风格统一的图标体系，构建了清晰的文档导航地图，降低了用户的认知负荷。
4.  **符合国际惯例**：新选用的 Emoji（如 📦 安装, ⚙️ 功能, ℹ️ 简介）高度契合 GitHub 顶级开源项目（如 Kubernetes, PyTorch, TensorFlow）的文档规范，有助于提升项目的国际化观感。

