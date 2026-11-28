# R3 三类异质资源综合验证：研究进度与模型状态

> 2026-11-28 实现状态更新：[R3 修复与新模型实际结果](R3_RESOURCE_REMEDIATION_REPORT.md)及[模型修订说明](R3_Model_Remediation_Amendment.md)。Flexibility main `338797a` 已实现 v0.3 的公开上界固定合同域、隐藏真实能力、EV执行后重排及P/Q登记、共同反馈策略适配、无二次确认和无预设询价预算。Provider四状态端点保留；20×24 h新闭环完成，仍有177期实际电压违规，不表示完整Rolling M-Cert或市场校准通过。Minimax排序、跨合同推断和严格AC不可行证书仍未实现。

> 下文保留第一轮历史证据。旧记录的372个流程成功期，准确分为191期无需采购和181期正需求采购成功；本轮修复及v0.3数据另列，不覆盖历史样本。

日期：2026-11-28。已完成本轮一次性综合验证；下列是实际结果，不是新研究阶段。

代码和证据提交：[Flexibility b80ec37](https://github.com/Fuyx661/Flexibility/commit/b80ec37ac5547b14b0b858316a8e89d4b8b6302b)。完整 [R3_RESOURCE_VALIDATION_REPORT.md](https://github.com/Fuyx661/Flexibility/blob/b80ec37ac5547b14b0b858316a8e89d4b8b6302b/R3_RESOURCE_VALIDATION_REPORT.md)，[可复现产物及 SHA-256](https://github.com/Fuyx661/Flexibility/blob/b80ec37ac5547b14b0b858316a8e89d4b8b6302b/artifacts/r3/manifest.json)。

## 实际执行与结果

- 从最新 main 26d13c9 的独立工作副本运行；原工作树未提交改动、历史代码及结果保留。
- 配置 dynamic_A + r2_seeded，seed=1…20，24×1 h。S0/S1/S2 分别执行当前认证容量的 0%/30%/80%；同 seed 共用物理参数、初值、外生数据和私有经济参数，不共用内生状态。
- 8,640 条主体时段、17,280 条逐车时段、720 次离站验证、25,920 条合同记录；128,984 项资源、成本、状态与接口残差检查未发现违规。EV 最小离站裕量约 −7.1e−15 kWh，处于数值容差内。
- 六项指定 MATLAB 测试真实运行。test_pa_r2_network_inputs 初次失败于节点索引行列方向，修复 rows(:) 后通过。其余五项通过；test_pa_sequential_procurement 内部 T1–T17 和 12 项回归通过，论文编译 T18 仍为 NOT_VERIFIED。
- 询价/确认期间状态冻结，端口私有信息隔离、种子输入逐值复现、seed 1 S1 完整轨迹重放、未来实际值扰动不提前改变决策，均有实际记录。
- 经济状态统一用 1 kWh 合同比较；不可履约合同价格记为 NaN。未调整经济系数制造算法优势。
- 20/20 个 IEEE 33-bus、MATPOWER 8.1 闭环都执行到第 24 期；372/480 期采购成功，108 期采购失败。实际 AC 潮流均收敛，但 204/480 期不满足研究电压界，最小电压 0.949248365 pu；所有不利结果保留。

## 修复及产物

原始失败、阈值测试货币容差修复、MATLAB 图形初始化失败及保存/归档编排修复，均记录于 [REPAIRS](https://github.com/Fuyx661/Flexibility/blob/b80ec37ac5547b14b0b858316a8e89d4b8b6302b/artifacts/r3/REPAIRS.md)。模型数值数据保存后用 Python 从 CSV 重绘图表；最终 MAT 独立重开验证通过，无损归档逐字节解压 SHA 核对通过。未改变采购算法、物理模型或经济参数。

输出含三类资源状态/功率/可用容量图、同能量成本及有效价格图、逐车离站表、物理/网络违规表、信息隔离和复现记录、480 期闭环表及完整数值快照。[全部产物](https://github.com/Fuyx661/Flexibility/blob/b80ec37ac5547b14b0b858316a8e89d4b8b6302b/artifacts/r3/validation_final)。

## 模型状态与研究边界

R1/R2 合成动态资源实现已获本轮有限样本物理、成本和接口验证；真实参数校准、普遍数学证明及真实数据拟合仍未完成。BESS 基线充电、CL 无反弹/预算不恢复、EV 不超登记基线补充充电、单节点聚合边界保持冻结。

当前采购协议仍为四状态反馈、连续价格区间、最终确认、旧预算/停止规则与固定 Q 潮流。原 AC_INFEASIBLE_PROVEN 仅是历史程序状态标签，不能据此证明完整 AC 问题不可行。R3 不重新设计采购算法，也不把旧接口通过解释为完整 Rolling M-Cert 通过。资源模型检验通过与系统全部时段网络安全是两个不同结论。
