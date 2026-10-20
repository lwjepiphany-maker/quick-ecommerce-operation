# 上手：从七份文件开始

首次使用可先阅读 [电商指标与字段说明](metrics-glossary.md)，了解 ROAS、CLV、CPA 和贡献额，以及报表里的字段。

使用可创建个人 Space 和自定义 Chat Agent 的 Quick 账号。此路径在 Web 操作，不依赖 Desktop 或企业连接器。界面名称可能随版本变化。

## 1. 准备资料

下载仓库并解压。初始只使用 [data 目录](../examples/pod-operations/data/) 的七份文件：

| 文件 | 用途 |
| --- | --- |
| business_brief.md | 业务目标、活动范围和模拟负责人回答 |
| metric_contract.md | 指标口径 |
| campaign_performance.csv | SKU 级经营与广告聚合 |
| fulfillment_snapshot.csv | 产能、质量、授权与确认稿 |
| product_catalog.csv | 商品规格和生产运输信息 |
| dingtalk_campaign.md | 模拟交接消息，无实时连接 |
| market_signals_mock.md | 待验证的市场假设 |

案例时点固定在 2026-09-10，不是今天的真实经营数据。先不要上传 updates，避免提前改变初始条件。

## 2. 创建 Space 并检查读取

在 Quick 的 Spaces 创建个人空间，上传七份文件并等待处理完成。在该 Space 的聊天中提问：

```text
请列出实际读到的七份文件、business_brief.md 的业务时点和本次活动 SKU。
再引用 campaign_performance.csv 中 POD-MUG-02 的 gross_sales_usd 和 ad_spend_usd。
读不到时请说明，不要猜测。
```

预期核对值：活动涉及 T 恤、杯子、连帽衫三个 SKU；杯子销售额为 2500 USD，广告花费为 400 USD。商品总目录有五款，但本次活动只有三款。

若读不到，检查文件处理状态、Space 选择和资源访问权限，再继续。

## 3. 创建助手

在 Chat agents 创建自定义助手，名称可用“电商运营复盘助手”。将 [参考指令](../examples/pod-operations/agent-instructions.md) 放入 Instructions，关联上述 Space。检查是否混入其他知识来源，保持本例仅使用模拟资料，不配置外部写入 Actions。

预览取数后，使用当前界面的保存或 Launch 操作，确认可从列表重新打开助手。指令文件存在不代表 Agent 已保存。

## 4. 试三个场景

每个场景开一个新对话：

1. [经营复盘](../examples/pod-operations/prompts/01-business-review.txt)：不加附件，让助手使用 Space。将原始回答保存为本地 `previous-review.md`。如被问到英文市场，本例可选择美国英语 `en-US`，这只是演示选择。
2. [数据不足](../examples/pod-operations/prompts/02-missing-data.txt)：观察助手是否识别客户数据不足。
3. [状态更新](../examples/pod-operations/prompts/03-review-update.txt)：上传 [update_v2.md](../examples/pod-operations/updates/update_v2.md) 和你自己的 `previous-review.md`，复查旧建议。更新文件不加入初始 Space，便于重试第一种场景。

用 [核对清单](review-checklist.md) 检查答案。这里给的是预期检查点，不是已测得的 Quick 表现。

## 5. 用完之后

需要继续使用可保留助手和 Space；不再需要时，只删除自己为此案例创建的资源。若另行做了 Desktop 进阶尝试，移除不再需要的目录授权，停用练习任务，检查并删除本次新建且不再需要的 Memory。不要清理他人共享资源。
