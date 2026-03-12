# md文件名称规范

_**资料书写要求：**_

1. _总体介绍类文档在蓝区仓一般是首页呈现，文件名必须固定为“README.md”，否则gitee首页默认无法展示。_
2. _其他md文档统一放在docs文件夹下，文件以工具、特性或产品等手册英文名称命名。（中文名称链接会因转义变成乱码）_
    1. _英文名称给全称，全小写，单词之间以下划线“\_”分隔，不可用其他特殊字符（包括序号，比如：01.xxx.md，删除前面的01.）_

        _特殊情况：单个API文件名直接以API名称命名，API名称中一般含有“.”，则“.”用英文格式的中划线“-”代替（例如：API名称为torch\_npu.scatter\_update，那么对应的md文件名改为torch\_npu-scatter\_update.md）_

    2. _名称缩写：当该名称有公认术语或华为术语以及对应的缩写时可以用缩写，其他情况建议用全称，保证用户可理解__（文档名称不宜过长，根据中文标题翻译，中文标题长度不超过15个汉字，英文建议不超过25\~50个字符）_

3. _所有文件名称可按照下列[表2](#table2)固定格式命名，xxx部分或其他非下面固定格式的文档，由中文名称翻译为英文全称后，发给资料审视_

**示例1：**

![](figures/zh-cn_image_0000002503708739.png)

**示例2：**

![](figures/zh-cn_image_0000002444459468.png)

# 中英文名称对照

**表 1** 黄区文档一级标题英文名称

<table><thead align="left"><tr id="row16817181064416"><th class="cellrowborder" valign="top" width="50%" id="mcps1.2.3.1.1"><p id="p10760111864415"><a name="p10760111864415"></a><a name="p10760111864415"></a>中文名称</p>
</th>
<th class="cellrowborder" valign="top" width="50%" id="mcps1.2.3.1.2"><p id="p4760111810448"><a name="p4760111810448"></a><a name="p4760111810448"></a>英文md文件名</p>
</th>
</tr>
</thead>
<tbody><tr id="row13817191044410"><td class="cellrowborder" colspan="2" valign="top" headers="mcps1.2.3.1.1 mcps1.2.3.1.2 "><p id="p10235123819453"><a name="p10235123819453"></a><a name="p10235123819453"></a><strong id="b1934961335110"><a name="b1934961335110"></a><a name="b1934961335110"></a>安装指南</strong></p>
</td>
</tr>
<tr id="row128171210184416"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p163181831174513"><a name="p163181831174513"></a><a name="p163181831174513"></a>选择安装场景</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p47843381314"><a name="p47843381314"></a><a name="p47843381314"></a>selecting_installation_scenario.md</p>
</td>
</tr>
<tr id="row28171810144419"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p12318331174510"><a name="p12318331174510"></a><a name="p12318331174510"></a>安装说明</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1478416351311"><a name="p1478416351311"></a><a name="p1478416351311"></a>installation_guide.md</p>
</td>
</tr>
<tr id="row108171010134419"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p3318153164519"><a name="p3318153164519"></a><a name="p3318153164519"></a>安装前准备</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1178414341319"><a name="p1178414341319"></a><a name="p1178414341319"></a>installation_preparations.md</p>
</td>
</tr>
<tr id="row681791011445"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p83181731134510"><a name="p83181731134510"></a><a name="p83181731134510"></a>安装规划</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1278413312138"><a name="p1278413312138"></a><a name="p1278413312138"></a>installation_planning.md</p>
</td>
</tr>
<tr id="row13817181074415"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p9318163134511"><a name="p9318163134511"></a><a name="p9318163134511"></a>安装步骤</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p878453101317"><a name="p878453101317"></a><a name="p878453101317"></a>installation_procedure.md</p>
</td>
</tr>
<tr id="row98173105441"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p169941344617"><a name="p169941344617"></a><a name="p169941344617"></a>安装XX（XXX场景）</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p157841537137"><a name="p157841537137"></a><a name="p157841537137"></a>installing_xx_for_xxx_scenario.md</p>
</td>
</tr>
<tr id="row181715109441"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p1231893116457"><a name="p1231893116457"></a><a name="p1231893116457"></a>安装后配置</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p137847381318"><a name="p137847381318"></a><a name="p137847381318"></a>post-installation_configuration.md</p>
</td>
</tr>
<tr id="row198173101447"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p5318731114520"><a name="p5318731114520"></a><a name="p5318731114520"></a>升级</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p13784173101312"><a name="p13784173101312"></a><a name="p13784173101312"></a>upgrade.md</p>
</td>
</tr>
<tr id="row381781054420"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p13181731104512"><a name="p13181731104512"></a><a name="p13181731104512"></a>卸载</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p778515311134"><a name="p778515311134"></a><a name="p778515311134"></a>uninstallation.md</p>
</td>
</tr>
<tr id="row12817101013444"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p12319193184516"><a name="p12319193184516"></a><a name="p12319193184516"></a>安全加固</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p978515331313"><a name="p978515331313"></a><a name="p978515331313"></a>security_hardening.md</p>
</td>
</tr>
<tr id="row9817191044420"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p153191331194512"><a name="p153191331194512"></a><a name="p153191331194512"></a>附录</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p77851839136"><a name="p77851839136"></a><a name="p77851839136"></a>appendixes.md</p>
</td>
</tr>
<tr id="row1181791084418"><td class="cellrowborder" colspan="2" valign="top" headers="mcps1.2.3.1.1 mcps1.2.3.1.2 "><p id="p10923205725019"><a name="p10923205725019"></a><a name="p10923205725019"></a><strong id="b35056159519"><a name="b35056159519"></a><a name="b35056159519"></a>模型迁移指南</strong></p>
</td>
</tr>
<tr id="row19817191017444"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p157084875113"><a name="p157084875113"></a><a name="p157084875113"></a>概述</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p145738532125"><a name="p145738532125"></a><a name="p145738532125"></a>overview.md</p>
</td>
</tr>
<tr id="row1281761014442"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p770144816514"><a name="p770144816514"></a><a name="p770144816514"></a>迁移流程</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p65733539120"><a name="p65733539120"></a><a name="p65733539120"></a>migration_process.md</p>
</td>
</tr>
<tr id="row198171210114410"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p67018486511"><a name="p67018486511"></a><a name="p67018486511"></a>模型选取</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p4573205381216"><a name="p4573205381216"></a><a name="p4573205381216"></a>model_selection.md</p>
</td>
</tr>
<tr id="row1281741024412"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p77014483514"><a name="p77014483514"></a><a name="p77014483514"></a>迁移前分析</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p857305321216"><a name="p857305321216"></a><a name="p857305321216"></a>pre-migration_analysis.md</p>
</td>
</tr>
<tr id="row208188108444"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p770134865118"><a name="p770134865118"></a><a name="p770134865118"></a>模型迁移</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p45731553131215"><a name="p45731553131215"></a><a name="p45731553131215"></a>model_migration.md</p>
</td>
</tr>
<tr id="row10818141017442"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p370104810519"><a name="p370104810519"></a><a name="p370104810519"></a>特性使能</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p155731553121214"><a name="p155731553121214"></a><a name="p155731553121214"></a>feature_enabling.md</p>
</td>
</tr>
<tr id="row781831094415"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p10701848105118"><a name="p10701848105118"></a><a name="p10701848105118"></a>模型训练</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p18573155318121"><a name="p18573155318121"></a><a name="p18573155318121"></a>model_training.md</p>
</td>
</tr>
<tr id="row148189103448"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p37017484512"><a name="p37017484512"></a><a name="p37017484512"></a>模型保存与导出</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p257365313124"><a name="p257365313124"></a><a name="p257365313124"></a>model_saving_and_exporting.md</p>
</td>
</tr>
<tr id="row1681810108440"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p5561112055215"><a name="p5561112055215"></a><a name="p5561112055215"></a>精度调试</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p14573553181219"><a name="p14573553181219"></a><a name="p14573553181219"></a>accuracy_debugging.md</p>
</td>
</tr>
<tr id="row5818610104417"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p51321226185216"><a name="p51321226185216"></a><a name="p51321226185216"></a>性能调优</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p75731853151220"><a name="p75731853151220"></a><a name="p75731853151220"></a>performance_tuning.md</p>
</td>
</tr>
<tr id="row48182010184414"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p1966283065214"><a name="p1966283065214"></a><a name="p1966283065214"></a>附录</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p457315331217"><a name="p457315331217"></a><a name="p457315331217"></a>appendixes.md</p>
</td>
</tr>
<tr id="row10818910174419"><td class="cellrowborder" colspan="2" valign="top" headers="mcps1.2.3.1.1 mcps1.2.3.1.2 "><p id="p1881915132547"><a name="p1881915132547"></a><a name="p1881915132547"></a><strong id="b1583010415518"><a name="b1583010415518"></a><a name="b1583010415518"></a>特性指南</strong></p>
</td>
</tr>
<tr id="row10818171034418"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p8126520165417"><a name="p8126520165417"></a><a name="p8126520165417"></a>特性描述</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p812394611214"><a name="p812394611214"></a><a name="p812394611214"></a>feature_description.md</p>
</td>
</tr>
<tr id="row4818111054414"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p51261620155418"><a name="p51261620155418"></a><a name="p51261620155418"></a>软件编译</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p111231846181213"><a name="p111231846181213"></a><a name="p111231846181213"></a>software_compilation.md</p>
</td>
</tr>
<tr id="row1818110154419"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p91261620165417"><a name="p91261620165417"></a><a name="p91261620165417"></a>部署软件/安装软件</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p12123124618123"><a name="p12123124618123"></a><a name="p12123124618123"></a>software_deployment_and_installation.md</p>
</td>
</tr>
<tr id="row14818151084415"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p1312612045410"><a name="p1312612045410"></a><a name="p1312612045410"></a>使用特性</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p14123184631216"><a name="p14123184631216"></a><a name="p14123184631216"></a>feature_usage.md</p>
</td>
</tr>
<tr id="row188180101443"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p9126320165415"><a name="p9126320165415"></a><a name="p9126320165415"></a>维护特性</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p14123124631214"><a name="p14123124631214"></a><a name="p14123124631214"></a>features_maintenance.md</p>
</td>
</tr>
<tr id="row481881014417"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p712622013548"><a name="p712622013548"></a><a name="p712622013548"></a>安全管理</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1912316469124"><a name="p1912316469124"></a><a name="p1912316469124"></a>security_management.md</p>
</td>
</tr>
<tr id="row58181710164420"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p4126420185415"><a name="p4126420185415"></a><a name="p4126420185415"></a>故障排除</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p91230469121"><a name="p91230469121"></a><a name="p91230469121"></a>troubleshooting.md</p>
</td>
</tr>
<tr id="row8818910174411"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p1812622019546"><a name="p1812622019546"></a><a name="p1812622019546"></a>参考信息</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p51233462126"><a name="p51233462126"></a><a name="p51233462126"></a>reference.md</p>
</td>
</tr>
<tr id="row11818121044417"><td class="cellrowborder" colspan="2" valign="top" headers="mcps1.2.3.1.1 mcps1.2.3.1.2 "><p id="p161267204541"><a name="p161267204541"></a><a name="p161267204541"></a><strong id="b17562194395614"><a name="b17562194395614"></a><a name="b17562194395614"></a>命令行和API工具</strong></p>
</td>
</tr>
<tr id="row08183108448"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p1522341215710"><a name="p1522341215710"></a><a name="p1522341215710"></a>工具使用导航</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1379819366126"><a name="p1379819366126"></a><a name="p1379819366126"></a>tool_usage_overview.md</p>
</td>
</tr>
<tr id="row581861054410"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p722351218571"><a name="p722351218571"></a><a name="p722351218571"></a>简介</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1179813615123"><a name="p1179813615123"></a><a name="p1179813615123"></a>overview.md</p>
</td>
</tr>
<tr id="row14818161010443"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p622391214573"><a name="p622391214573"></a><a name="p622391214573"></a>使用前准备</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p2798236131210"><a name="p2798236131210"></a><a name="p2798236131210"></a>preparations.md</p>
</td>
</tr>
<tr id="row1481831018447"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p522371235710"><a name="p522371235710"></a><a name="p522371235710"></a>快速入门</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p779813368129"><a name="p779813368129"></a><a name="p779813368129"></a>quick_start.md</p>
</td>
</tr>
<tr id="row78181010144414"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p1922331212572"><a name="p1922331212572"></a><a name="p1922331212572"></a>xxx功能介绍</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p57983362126"><a name="p57983362126"></a><a name="p57983362126"></a>xxx_description.md</p>
</td>
</tr>
<tr id="row138181101443"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p62239123575"><a name="p62239123575"></a><a name="p62239123575"></a>输出结果文件说明</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p7798103671213"><a name="p7798103671213"></a><a name="p7798103671213"></a>output_file_description.md</p>
</td>
</tr>
<tr id="row1881851013448"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p1822381219571"><a name="p1822381219571"></a><a name="p1822381219571"></a>案例</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p679813621210"><a name="p679813621210"></a><a name="p679813621210"></a>case_study.md</p>
</td>
</tr>
<tr id="row198181210184418"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p022311214573"><a name="p022311214573"></a><a name="p022311214573"></a>扩展功能</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1379813612122"><a name="p1379813612122"></a><a name="p1379813612122"></a>extended_functions.md</p>
</td>
</tr>
<tr id="row181819103448"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p22234123576"><a name="p22234123576"></a><a name="p22234123576"></a>附录</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p11798123617125"><a name="p11798123617125"></a><a name="p11798123617125"></a>appendixes.md</p>
</td>
</tr>
<tr id="row1081951084414"><td class="cellrowborder" colspan="2" valign="top" headers="mcps1.2.3.1.1 mcps1.2.3.1.2 "><p id="p15342141714591"><a name="p15342141714591"></a><a name="p15342141714591"></a><strong id="b920144319591"><a name="b920144319591"></a><a name="b920144319591"></a>界面工具</strong></p>
</td>
</tr>
<tr id="row138191010124418"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p422441213579"><a name="p422441213579"></a><a name="p422441213579"></a>简介</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p7470142810128"><a name="p7470142810128"></a><a name="p7470142810128"></a>overview.md</p>
</td>
</tr>
<tr id="row188199108442"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p152241112145718"><a name="p152241112145718"></a><a name="p152241112145718"></a>安装说明</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p134701128131214"><a name="p134701128131214"></a><a name="p134701128131214"></a>installation_guide.md</p>
</td>
</tr>
<tr id="row281941012448"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p17224141214577"><a name="p17224141214577"></a><a name="p17224141214577"></a>基础操作</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p947020281123"><a name="p947020281123"></a><a name="p947020281123"></a>basic_operations.md</p>
</td>
</tr>
<tr id="row78197102446"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p9224121210572"><a name="p9224121210572"></a><a name="p9224121210572"></a>工具使用</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p147032813124"><a name="p147032813124"></a><a name="p147032813124"></a>tool_usage.md</p>
</td>
</tr>
<tr id="row7819131084418"><td class="cellrowborder" colspan="2" valign="top" headers="mcps1.2.3.1.1 mcps1.2.3.1.2 "><p id="p112241912205720"><a name="p112241912205720"></a><a name="p112241912205720"></a><strong id="b15798311002"><a name="b15798311002"></a><a name="b15798311002"></a>术语规范</strong></p>
</td>
</tr>
<tr id="row1881911015448"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p184401161509"><a name="p184401161509"></a><a name="p184401161509"></a>术语和缩略语规范</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p4930142415124"><a name="p4930142415124"></a><a name="p4930142415124"></a>terms_and_acronyms.md</p>
</td>
</tr>
<tr id="row20819101004412"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p174401161908"><a name="p174401161908"></a><a name="p174401161908"></a>术语运作规则</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1793018247129"><a name="p1793018247129"></a><a name="p1793018247129"></a>term_management_rules.md</p>
</td>
</tr>
<tr id="row1782020107441"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p7440216301"><a name="p7440216301"></a><a name="p7440216301"></a>术语变更</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p14930102421212"><a name="p14930102421212"></a><a name="p14930102421212"></a>term_change.md</p>
</td>
</tr>
<tr id="row982061010445"><td class="cellrowborder" colspan="2" valign="top" headers="mcps1.2.3.1.1 mcps1.2.3.1.2 "><p id="p450953216"><a name="p450953216"></a><a name="p450953216"></a><strong id="b99178118117"><a name="b99178118117"></a><a name="b99178118117"></a>版本说明书</strong></p>
</td>
</tr>
<tr id="row5820410164411"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p1765213173110"><a name="p1765213173110"></a><a name="p1765213173110"></a>版本配套说明</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p2995317131217"><a name="p2995317131217"></a><a name="p2995317131217"></a>version_mapping.md</p>
</td>
</tr>
<tr id="row14820111016444"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p186531817213"><a name="p186531817213"></a><a name="p186531817213"></a>版本兼容性说明</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p6995201761214"><a name="p6995201761214"></a><a name="p6995201761214"></a>version_compatibility.md</p>
</td>
</tr>
<tr id="row98201310184414"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p11653117718"><a name="p11653117718"></a><a name="p11653117718"></a>版本使用注意事项</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p139951017151217"><a name="p139951017151217"></a><a name="p139951017151217"></a>version_precautions.md</p>
</td>
</tr>
<tr id="row3820181017443"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p665311711115"><a name="p665311711115"></a><a name="p665311711115"></a>xxx更新说明</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1995181761214"><a name="p1995181761214"></a><a name="p1995181761214"></a>xxx_updates.md</p>
</td>
</tr>
<tr id="row11820151015447"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p9653917515"><a name="p9653917515"></a><a name="p9653917515"></a>升级影响</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1995131781220"><a name="p1995131781220"></a><a name="p1995131781220"></a>upgrade_impact.md</p>
</td>
</tr>
<tr id="row982013105449"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p2653317917"><a name="p2653317917"></a><a name="p2653317917"></a>xxx版本配套文档</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p16995191721216"><a name="p16995191721216"></a><a name="p16995191721216"></a>xxx_documentation.md</p>
</td>
</tr>
<tr id="row2820181017445"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p46166532114"><a name="p46166532114"></a><a name="p46166532114"></a>漏洞修补列表</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p17995151701219"><a name="p17995151701219"></a><a name="p17995151701219"></a>vulnerability_fixes.md</p>
</td>
</tr>
<tr id="row16820141019442"><td class="cellrowborder" colspan="2" valign="top" headers="mcps1.2.3.1.1 mcps1.2.3.1.2 "><p id="p291517561815"><a name="p291517561815"></a><a name="p291517561815"></a><strong id="b431612413204"><a name="b431612413204"></a><a name="b431612413204"></a>开发指南</strong></p>
</td>
</tr>
<tr id="row18205104441"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p12905142510209"><a name="p12905142510209"></a><a name="p12905142510209"></a>XXX简介</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1062411081218"><a name="p1062411081218"></a><a name="p1062411081218"></a>xxx_introduction.md</p>
</td>
</tr>
<tr id="row6820161019447"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p119051254202"><a name="p119051254202"></a><a name="p119051254202"></a>准备环境</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1962519104121"><a name="p1962519104121"></a><a name="p1962519104121"></a>environment_setup.md</p>
</td>
</tr>
<tr id="row18820131094417"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p390542513204"><a name="p390542513204"></a><a name="p390542513204"></a>快速入门</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1762510107123"><a name="p1762510107123"></a><a name="p1762510107123"></a>quick_start.md</p>
</td>
</tr>
<tr id="row8820121074414"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p99052259201"><a name="p99052259201"></a><a name="p99052259201"></a>编程接口与调用流程</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p9625141014126"><a name="p9625141014126"></a><a name="p9625141014126"></a>programming_apis_and_call_sequence.md</p>
</td>
</tr>
<tr id="row17820510194419"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p189051725132018"><a name="p189051725132018"></a><a name="p189051725132018"></a>精度/性能调优</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1862501012129"><a name="p1862501012129"></a><a name="p1862501012129"></a>accuracy_and_performance_tuning.md</p>
</td>
</tr>
<tr id="row10841202594520"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p1684115254452"><a name="p1684115254452"></a><a name="p1684115254452"></a>xx数据监控</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p38411225154515"><a name="p38411225154515"></a><a name="p38411225154515"></a>xx_data_monitoring</p>
</td>
</tr>
<tr id="row1795851620218"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p10959121614217"><a name="p10959121614217"></a><a name="p10959121614217"></a>服务化调优</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p10959161614218"><a name="p10959161614218"></a><a name="p10959161614218"></a>serving_tuning.md</p>
</td>
</tr>
<tr id="row9773161112512"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p177316119516"><a name="p177316119516"></a><a name="p177316119516"></a>xx样例集合</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p577315111552"><a name="p577315111552"></a><a name="p577315111552"></a>xx_samples.md</p>
</td>
</tr>
<tr id="row382019109442"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p5905182572011"><a name="p5905182572011"></a><a name="p5905182572011"></a>XXX样例参考</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p36251410171215"><a name="p36251410171215"></a><a name="p36251410171215"></a>xxx_sample.md</p>
</td>
</tr>
<tr id="row118214107442"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p16905152513205"><a name="p16905152513205"></a><a name="p16905152513205"></a>附录</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1162511011215"><a name="p1162511011215"></a><a name="p1162511011215"></a>appendixes.md</p>
</td>
</tr>
<tr id="row79261621904"><td class="cellrowborder" colspan="2" valign="top" headers="mcps1.2.3.1.1 mcps1.2.3.1.2 "><p id="p1692682118013"><a name="p1692682118013"></a><a name="p1692682118013"></a><strong id="b2073112382020"><a name="b2073112382020"></a><a name="b2073112382020"></a>API接口</strong></p>
</td>
</tr>
<tr id="row48119433011"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p181194311015"><a name="p181194311015"></a><a name="p181194311015"></a>总体说明</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1181144310017"><a name="p1181144310017"></a><a name="p1181144310017"></a>overview.md</p>
</td>
</tr>
<tr id="row187076561729"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p97078564214"><a name="p97078564214"></a><a name="p97078564214"></a>xx类型</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p770765615212"><a name="p770765615212"></a><a name="p770765615212"></a>英文可不翻译，保留xx</p>
</td>
</tr>
<tr id="row168204174220"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p5683945422"><a name="p5683945422"></a><a name="p5683945422"></a>xx系列</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p191373512718"><a name="p191373512718"></a><a name="p191373512718"></a>英文可不翻译，保留xx</p>
</td>
</tr>
<tr id="row3489144620"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p4489184424"><a name="p4489184424"></a><a name="p4489184424"></a>宏定义</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p289114594362"><a name="p289114594362"></a><a name="p289114594362"></a>macro_definitions</p>
</td>
</tr>
<tr id="row149818591925"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p5981759529"><a name="p5981759529"></a><a name="p5981759529"></a>输出结果说明</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p108911859133612"><a name="p108911859133612"></a><a name="p108911859133612"></a>output_description</p>
</td>
</tr>
<tr id="row2045711219310"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p67995141135"><a name="p67995141135"></a><a name="p67995141135"></a>示例代码</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.2 "><p id="p1289114595366"><a name="p1289114595366"></a><a name="p1289114595366"></a>sample_code</p>
</td>
</tr>
</tbody>
</table>
**表 2** 特定文档的固定文件名格式<a name="table2"></a>

<table><thead align="left"><tr id="row78099019712"><th class="cellrowborder" valign="top" width="30.509999999999998%" id="mcps1.2.3.1.1"><p id="p188101101879"><a name="p188101101879"></a><a name="p188101101879"></a>中文名称</p>
</th>
<th class="cellrowborder" valign="top" width="69.49%" id="mcps1.2.3.1.2"><p id="p581014011719"><a name="p581014011719"></a><a name="p581014011719"></a>英文md文件名</p>
</th>
</tr>
</thead>
<tbody><tr id="row177113480182"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p1871748111812"><a name="p1871748111812"></a><a name="p1871748111812"></a><em id="i965525203018"><a name="i965525203018"></a><a name="i965525203018"></a>xxx模型迁移指南</em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p1711148141818"><a name="p1711148141818"></a><a name="p1711148141818"></a><em id="i1365685263017"><a name="i1365685263017"></a><a name="i1365685263017"></a>xxx_model_migration_guide</em><em id="i46501677199"><a name="i46501677199"></a><a name="i46501677199"></a>.md</em></p>
</td>
</tr>
<tr id="row173761619112311"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p837691910237"><a name="p837691910237"></a><a name="p837691910237"></a><em id="i16658752103011"><a name="i16658752103011"></a><a name="i16658752103011"></a>xxx模型迁移调优指南</em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p10376121919239"><a name="p10376121919239"></a><a name="p10376121919239"></a><em id="i665975233010"><a name="i665975233010"></a><a name="i665975233010"></a>xxx_model_migration_and_tuning_guide</em><em id="i6839023192410"><a name="i6839023192410"></a><a name="i6839023192410"></a>.md</em></p>
</td>
</tr>
<tr id="row14109854112519"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p15109105413251"><a name="p15109105413251"></a><a name="p15109105413251"></a><em id="i96601752203013"><a name="i96601752203013"></a><a name="i96601752203013"></a>xxx特性指南</em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p810925410258"><a name="p810925410258"></a><a name="p810925410258"></a><em id="i366014522308"><a name="i366014522308"></a><a name="i366014522308"></a>xxx_feature_guide</em></p>
</td>
</tr>
<tr id="row19914518308"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p942410410820"><a name="p942410410820"></a><a name="p942410410820"></a><em id="i140343495"><a name="i140343495"></a><a name="i140343495"></a><span>xxx版本说明书</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p10424114112816"><a name="p10424114112816"></a><a name="p10424114112816"></a><em id="i1976019451095"><a name="i1976019451095"></a><a name="i1976019451095"></a><span>xxx_release_notes.md</span></em></p>
</td>
</tr>
<tr id="row1662617344192"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p1781010011717"><a name="p1781010011717"></a><a name="p1781010011717"></a><em id="i14408160583"><a name="i14408160583"></a><a name="i14408160583"></a>xxx快速入门</em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p108101701671"><a name="p108101701671"></a><a name="p108101701671"></a><em id="i0544751881"><a name="i0544751881"></a><a name="i0544751881"></a>xxx_quick_start.md</em></p>
</td>
</tr>
<tr id="row1629620912816"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p189471942488"><a name="p189471942488"></a><a name="p189471942488"></a><em id="i17269194816918"><a name="i17269194816918"></a><a name="i17269194816918"></a><span>昇腾产品形态说明</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p494711421486"><a name="p494711421486"></a><a name="p494711421486"></a><em id="i1550318531796"><a name="i1550318531796"></a><a name="i1550318531796"></a><span>ascend_product_models.md</span></em></p>
</td>
</tr>
<tr id="row5185133615194"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p38101010719"><a name="p38101010719"></a><a name="p38101010719"></a><em id="i4565681814"><a name="i4565681814"></a><a name="i4565681814"></a><span>xxx安装指南</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p12810201276"><a name="p12810201276"></a><a name="p12810201276"></a><em id="i523117111680"><a name="i523117111680"></a><a name="i523117111680"></a><span>xxx_install_guide.md</span></em></p>
</td>
</tr>
<tr id="row11410153319194"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p9811123916819"><a name="p9811123916819"></a><a name="p9811123916819"></a><em id="i13646677920"><a name="i13646677920"></a><a name="i13646677920"></a><span>xxx开发指南</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p118121939085"><a name="p118121939085"></a><a name="p118121939085"></a><em id="i979716120916"><a name="i979716120916"></a><a name="i979716120916"></a><span>xxx_development_guide.md</span></em></p>
</td>
</tr>
<tr id="row19583193711914"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p1881013019717"><a name="p1881013019717"></a><a name="p1881013019717"></a><em id="i7669013189"><a name="i7669013189"></a><a name="i7669013189"></a><span>xxx用户指南（xxx使用指南应统一改为xxx用户指南）</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p4810901072"><a name="p4810901072"></a><a name="p4810901072"></a><em id="i1761316168816"><a name="i1761316168816"></a><a name="i1761316168816"></a><span>xxx_user_guide.md</span></em></p>
</td>
</tr>
<tr id="row9810509716"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p781011017719"><a name="p781011017719"></a><a name="p781011017719"></a><em id="i441120191184"><a name="i441120191184"></a><a name="i441120191184"></a><span>xxx工具用户指南</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p2810803711"><a name="p2810803711"></a><a name="p2810803711"></a><em id="i2676222987"><a name="i2676222987"></a><a name="i2676222987"></a><span>xxx_tool_user_guide.md</span></em></p>
</td>
</tr>
<tr id="row48104012710"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p28101901578"><a name="p28101901578"></a><a name="p28101901578"></a><em id="i1476218149716"><a name="i1476218149716"></a><a name="i1476218149716"></a><span>介绍说明类</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p118101201073"><a name="p118101201073"></a><a name="p118101201073"></a><em id="i166839251376"><a name="i166839251376"></a><a name="i166839251376"></a><span>xxx_introduct.md</span></em></p>
</td>
</tr>
<tr id="row18101501870"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p13810501877"><a name="p13810501877"></a><a name="p13810501877"></a><em id="i19591137571"><a name="i19591137571"></a><a name="i19591137571"></a>操作指导类</em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p1381018012711"><a name="p1381018012711"></a><a name="p1381018012711"></a><em id="i182621841572"><a name="i182621841572"></a><a name="i182621841572"></a>xxx_instruct.md</em></p>
</td>
</tr>
<tr id="row118107010719"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p457814491086"><a name="p457814491086"></a><a name="p457814491086"></a><em id="i57600151498"><a name="i57600151498"></a><a name="i57600151498"></a><span>xxxAPI参考</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p35789491784"><a name="p35789491784"></a><a name="p35789491784"></a><em id="i7695720796"><a name="i7695720796"></a><a name="i7695720796"></a><span>xxx_api_reference.md</span></em></p>
</td>
</tr>
<tr id="row1781011014716"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p481020015718"><a name="p481020015718"></a><a name="p481020015718"></a><em id="i960216443720"><a name="i960216443720"></a><a name="i960216443720"></a>单独API文档</em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p18810301278"><a name="p18810301278"></a><a name="p18810301278"></a><em id="i47151748477"><a name="i47151748477"></a><a name="i47151748477"></a>直接以API名称命名文件，API名称中有“.”的转为英文格式的中划线“-”</em></p>
</td>
</tr>
<tr id="row7810609719"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p6409247386"><a name="p6409247386"></a><a name="p6409247386"></a><em id="i836514376910"><a name="i836514376910"></a><a name="i836514376910"></a><span>故障处理</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p154095471689"><a name="p154095471689"></a><a name="p154095471689"></a><em id="i168453394918"><a name="i168453394918"></a><a name="i168453394918"></a><span>troubleshooting.md</span></em></p>
</td>
</tr>
<tr id="row17810150570"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p6928845587"><a name="p6928845587"></a><a name="p6928845587"></a><em id="i587353119918"><a name="i587353119918"></a><a name="i587353119918"></a><span>日志参考</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p169281445984"><a name="p169281445984"></a><a name="p169281445984"></a><em id="i1112011351998"><a name="i1112011351998"></a><a name="i1112011351998"></a><span>log_reference.md</span></em></p>
</td>
</tr>
<tr id="row58111439189"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p175328445812"><a name="p175328445812"></a><a name="p175328445812"></a><em id="i64879233916"><a name="i64879233916"></a><a name="i64879233916"></a><span>环境变量参考</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p185321441681"><a name="p185321441681"></a><a name="p185321441681"></a><em id="i19744162610912"><a name="i19744162610912"></a><a name="i19744162610912"></a><span>environment_variable_reference.md</span></em></p>
</td>
</tr>
<tr id="row1257713499816"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p2435338880"><a name="p2435338880"></a><a name="p2435338880"></a><em id="i5836533102"><a name="i5836533102"></a><a name="i5836533102"></a><span>术语和缩略语</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p15436238981"><a name="p15436238981"></a><a name="p15436238981"></a><em id="i819237121014"><a name="i819237121014"></a><a name="i819237121014"></a><span>terms_acronyms_abbreviations.md</span></em></p>
</td>
</tr>
<tr id="row188931924081"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p889318241583"><a name="p889318241583"></a><a name="p889318241583"></a><em id="i7619560916"><a name="i7619560916"></a><a name="i7619560916"></a><span>安全声明</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p189311245813"><a name="p189311245813"></a><a name="p189311245813"></a><em id="i1332714018107"><a name="i1332714018107"></a><a name="i1332714018107"></a><span>security_statement.md（这个文档里包含了通信矩阵、安全声明、漏洞机制说明</a><span>）</span></em></p>
</td>
</tr>
<tr id="row961755595815"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p1617555185820"><a name="p1617555185820"></a><a name="p1617555185820"></a><em id="i1621717274212"><a name="i1621717274212"></a><a name="i1621717274212"></a>漏洞处理流程.png</em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p561775517586"><a name="p561775517586"></a><a name="p561775517586"></a><em id="i12188271021"><a name="i12188271021"></a><a name="i12188271021"></a>vulnerability_handling_process.png</em></p>
</td>
</tr>
<tr id="row16729155318585"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p1872914531589"><a name="p1872914531589"></a><a name="p1872914531589"></a><em id="i1921811273211"><a name="i1921811273211"></a><a name="i1921811273211"></a>MindStudio漏洞机制说明</em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p972945316580"><a name="p972945316580"></a><a name="p972945316580"></a><em id="i1321819271629"><a name="i1321819271629"></a><a name="i1321819271629"></a>mindstudio_vulnerability_handling_procedure.md</em></p>
</td>
</tr>
<tr id="row128482536119"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p1084919535111"><a name="p1084919535111"></a><a name="p1084919535111"></a><em id="i172192275217"><a name="i172192275217"></a><a name="i172192275217"></a>公网地址</em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p1984919531319"><a name="p1984919531319"></a><a name="p1984919531319"></a><em id="i1521915272028"><a name="i1521915272028"></a><a name="i1521915272028"></a>public_ip_address.md</em></p>
</td>
</tr>
<tr id="row087415365818"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p2087473612817"><a name="p2087473612817"></a><a name="p2087473612817"></a><em id="i13224101020104"><a name="i13224101020104"></a><a name="i13224101020104"></a><span>FAQ</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p1587443614813"><a name="p1587443614813"></a><a name="p1587443614813"></a><em id="i359321231011"><a name="i359321231011"></a><a name="i359321231011"></a><span>faq.md</span></em></p>
</td>
</tr>
<tr id="row377118261385"><td class="cellrowborder" valign="top" width="30.509999999999998%" headers="mcps1.2.3.1.1 "><p id="p2771426785"><a name="p2771426785"></a><a name="p2771426785"></a><em id="i152001315171013"><a name="i152001315171013"></a><a name="i152001315171013"></a><span>xxx性能基线报告</span></em></p>
</td>
<td class="cellrowborder" valign="top" width="69.49%" headers="mcps1.2.3.1.2 "><p id="p12771426586"><a name="p12771426586"></a><a name="p12771426586"></a><em id="i147071814107"><a name="i147071814107"></a><a name="i147071814107"></a><span>xxx_perf_baseline.md</span></em></p>
</td>
</tr>
</tbody>
</table>
