# 使用 Amazon Quick 辅助电商运营

Amazon Quick 可以结合用户提供的文件回答问题、分析数据和起草内容。通过 Space（知识空间）集中保存业务资料，再把 Space 关联到自定义 Chat Agent（聊天助手），可以让助手按设定的工作要求反复处理日常问题。

本仓库以按需定制商品（POD）业务为例，提供模拟经营数据、助手指令和操作步骤。案例包括 T 恤、杯子和连帽衫，涉及商品推广、广告预算、履约检查和英文商品文案。

## 本仓库使用哪些 Quick 功能

| Quick 功能 | 本例中的用法 | 对应的电商工作 |
| --- | --- | --- |
| Space 与文件上传 | 保存商品目录、经营报表、指标定义和交接记录 | 将分散的商品、广告和生产信息放在一起查询 |
| 自定义 Chat Agent | 设置助手指令，关联业务 Space | 按约定口径分析商品贡献、广告表现和推广条件 |
| 对话分析与内容生成 | 提问并要求引用资料，生成分析表和文案草稿 | 辅助选品、预算讨论、英文卖点编写和任务交接 |
| 对话附件 | 上传业务更新和上一轮回答 | 复查产能或业务状态变化后，原建议是否还适用 |

本例中的钉钉内容是模拟交接记录，输出也是待人工发送的草稿。没有接入真实店铺、广告平台或钉钉，也不会执行投放、调价或发布。

## 适合哪些人员

- **电商运营和店铺负责人**：一起查看商品、广告和履约情况，准备日常复盘和行动清单。
- **广告投放人员**：结合商品贡献和生产条件讨论预算，检查广告归因数据是否完整。
- **商品与内容运营人员**：根据商品规格起草英文卖点，检查材质、交期等描述是否有依据。
- **生产和履约协调人员**：整理产能、质量、授权及确认稿的待办事项，明确需要谁确认。

不需要编程经验。使用者需要能够核对自己负责的业务数据，并判断生成的建议是否适用。

## 案例中要完成的事情

1. **经营复盘**：未来 48 小时最多增加 500 美元广告预算，判断哪些商品可以考虑推广，并给出依据、英文文案和行动草稿。
2. **检查数据是否足够**：尝试用现有报表识别沉睡客户、预测客户终身价值，观察助手是否指出缺少客户级数据。
3. **复查旧建议**：收到产能下降、广告归因补齐等更新后，检查哪些建议需要调整，以及哪些推广条件仍未满足。

资料中的 ROAS 指广告支出回报率，CLV 指客户终身价值。具体含义、公式和字段解释见 [电商指标说明](docs/metrics-glossary.md)。

## 前提条件

- 一个可以创建 Space、上传文件和创建自定义 Chat Agent 的 Quick 账号。
- 能通过浏览器访问 Quick Web；本例不需要安装 Desktop。
- 账号有足够的文件存储和对话使用额度。套餐功能和额度以 [Quick 官方说明](https://aws.amazon.com/quick/pricing/) 及实际账号为准。
- 下载本仓库，准备好 `examples/pod-operations/data/` 中的七份文件。

每人使用自己的 Space 即可，不需要团队共享 Space。所有数据、品牌和人物都是模拟的，不需要准备真实订单或客户资料。

## 操作步骤

### 1. 创建 Space 并上传资料

在 Quick Web 的 Spaces 中创建一个个人 Space，上传 [data 目录](examples/pod-operations/data/) 中的全部七份文件，等待处理完成。

文件包括业务背景、指标定义、经营与广告数据、履约状态、商品目录、模拟钉钉记录和市场假设。先不要上传 `updates` 目录中的更新文件。

### 2. 检查资料能否读取

在该 Space 中提问：

```text
请按 business_brief.md 列出本次活动的 SKU 和业务时点，再引用 campaign_performance.csv 中 POD-MUG-02 的 gross_sales_usd 和 ad_spend_usd。
如果无法读取文件，请明确说明。
```

核对回答：本次活动包含三个 SKU；杯子的销售额为 2500 美元，广告花费为 400 美元。取数不正确时，先检查文件处理状态和 Space 选择。

### 3. 创建电商运营助手

在 Chat agents 中创建助手，将 [agent-instructions.md](examples/pod-operations/agent-instructions.md) 的正文粘贴到 Instructions，关联刚才创建的 Space。预览确认能读取资料后，保存或 Launch，并检查是否能从列表重新打开。

### 4. 运行经营复盘和数据不足两个问题

打开助手，发送 [经营复盘问题](examples/pod-operations/prompts/01-business-review.txt)。核对回答后，将原始内容保存为本地文件 `previous-review.md`。

另开新对话，发送 [客户数据问题](examples/pod-operations/prompts/02-missing-data.txt)，检查助手如何处理数据不足。

### 5. 上传更新，复查上一轮建议

另开新对话，上传 [update_v2.md](examples/pod-operations/updates/update_v2.md) 和自己的 `previous-review.md`，再发送 [状态更新问题](examples/pod-operations/prompts/03-review-update.txt)。

更新文件仅用于本轮对话，不加入初始 Space，方便之后重复测试。

### 6. 核对结果

使用 [结果核对清单](docs/review-checklist.md) 检查计算、引用、推广条件和更新判断。可以用 [使用记录模板](examples/pod-operations/reference/usage-template.md) 保存错误、修改内容及人工处理时间。

更详细的操作、排错和清理说明见 [上手指南](docs/getting-started.md)。

## 换成自己的业务资料

完成案例后，可以替换商品、广告和履约文件，并修改助手指令中的业务规则。例如，现货零售需要检查可售库存和补货周期，定制生产需要检查产能、授权和确认稿。

具体说明见 [业务迁移指南](docs/adapt-to-your-business.md)。使用真实资料前，确认自己有权上传和处理这些数据。

## 当前状态

本仓库已检查文档链接和模拟数据的计算结果，整理后的操作流程和指令尚未在 Quick 中重新完整验证。[回答示例](examples/pod-operations/reference/review-example.md) 是根据材料人工整理的核对参考，不是 Quick 实测输出。

材料来源见 [来源说明](docs/provenance.md)，复用许可状态见 [复用说明](docs/reuse.md)。
