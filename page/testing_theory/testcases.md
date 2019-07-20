# 测试用例设计

### 测试用例的格式

普遍是 Excel 或 Xmind。

Excel 优势是比较细化，可以突出更多的测试要素，适用于等价划分类等黑盒测试设计思路，也适用于输入输出的场景；缺点是结构化不直观，不好体现功能需求，用例数过于臃肿。

Xmind 优势是大部分只需要列出测试点，更加注重探索性测试，能够更好的去描述功能需求，结构化展示比较直观，比较契合产品PRD；缺点是不太适用于输入输出的场景，测试细节不好表达。

------

### 测试用例优先级

- 高（优先执行）：产品基本的功能验证，即关键路径的测试用例，包括最常执行的功能、基本流程的输入（正向流程+正向数据）。
- 中（次级执行）：包括界面数据有效性校验、默认值、边界值。
- 低（最后执行）：建议执行的测试用例，包括不常执行的功能、异常流程的输入以及异常数据的输入。

------

### 测试用例要素

1. 用例标识（id）
2. 用例标题
3. 重要性
4. 前置条件
5. 操作步骤
6. 预期结果
7. 作者（选填）

------

### 测试方案设计应从哪些方面去考虑？

无非是从以下方面去考虑，其中，功能测试我认为是最重要的一个环节。

- 功能测试（最重要）
- 文档测试
- UI测试
- 接口测试
- 性能测试（压力、负载）
- 安全测试
- 稳定性测试（Monkey、遍历测试等）
- 异常测试（断网/弱网）
- 兼容性测试（安卓、IOS系统版本以及APP新老版本）
- 易用性测试
- 可用性测试
- 配置测试

------

### 测试用例面试题

[IP地址-测试用例](/page/testing_theory/testcases/ip_case.md)

[登陆界面-测试用例](/page/testing_theory/testcases/login_case.md)

[电梯-测试用例](/page/testing_theory/testcases/elevator_case.md)

[客服系统-测试用例](/page/testing_theory/testcases/im_case.md)

[列表页-测试用例](/page/testing_theory/testcases/list_case.md)

[搜索-测试用例](/page/testing_theory/testcases/search_case.md)

[长视频-测试用例](/page/testing_theory/testcases/long_video_case.md)

[短视频-测试用例](/page/testing_theory/testcases/short_video_case.md)

[游戏压力测试-测试用例](/page/testing_theory/testcases/short_video_case.md)

[UI自动化测试-测试用例](/page/testing_theory/testcases/UIAuto_case.md)

