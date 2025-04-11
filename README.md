# 加密货币资金费率套利机器人

## 项目概述

这是一个专业的加密货币资金费率套利机器人，专注于利用不同交易所之间的资金费率差异进行套利交易。目前支持Hyperliquid和Backpack两个交易所，通过实时监控资金费率和价格差异，自动执行套利策略。

### 核心特点

- **多交易所支持**：同时连接Hyperliquid和Backpack交易所
- **实时监控**：持续监控资金费率和价格差异
- **智能套利**：基于预设条件自动执行套利交易
- **风险控制**：完善的仓位管理和风险控制机制
- **滑点控制**：智能分析订单深度，控制交易滑点
- **详细日志**：完整的交易记录和操作日志

## 系统架构

### 目录结构
```
funding_arbitrage_bot/
├── core/                    # 核心功能模块
│   ├── arbitrage_engine.py  # 套利引擎
│   ├── exchange.py         # 交易所接口
│   └── risk_manager.py     # 风险管理
├── utils/                   # 工具函数
│   ├── logger.py           # 日志工具
│   └── helpers.py          # 辅助函数
├── config.yaml             # 配置文件
└── main.py                 # 主程序入口
```

### 核心模块说明

1. **套利引擎 (arbitrage_engine.py)**
   - 实现套利策略逻辑
   - 管理交易执行
   - 处理开平仓条件
   - 计算套利机会

2. **交易所接口 (exchange.py)**
   - 封装交易所API
   - 处理订单管理
   - 获取市场数据
   - 执行交易操作

3. **风险管理 (risk_manager.py)**
   - 仓位控制
   - 风险限制
   - 资金管理
   - 止损策略

## 安装指南

### 环境要求
- Python 3.8+
- 操作系统：Windows/Linux/MacOS

### 安装步骤

1. **克隆仓库**
```bash
git clone https://github.com/yourusername/funding-arbitrage-bot.git
cd funding-arbitrage-bot
```

2. **安装依赖**
```bash
pip install -r requirements.txt
```

3. **配置API密钥**
在`config.yaml`中配置交易所API密钥：
```yaml
exchanges:
  hyperliquid:
    api_key: "你的Hyperliquid钱包地址"
    api_secret: "你的Hyperliquid私钥"
  backpack:
    api_key: "你的Backpack API密钥"
    api_secret: "你的Backpack API密钥"
```

## 配置说明

### 基础配置
```yaml
strategy:
  # 基本参数
  min_funding_diff: 0.01    # 最小资金费率差异(1%)
  min_price_diff_pct: 0.001 # 最小价格差异(0.1%)
  
  # 交易参数
  trade_size_usd: 100      # 单笔交易金额(美元)
  max_position_usd: 500    # 最大仓位(美元)
  trade_cooldown: 3600     # 交易冷却时间(秒)
```

### 开仓条件
```yaml
open_conditions:
  condition_type: "funding_only"  # 条件类型
  min_funding_diff: 0.01         # 最小资金费率差异
  min_price_diff_percent: 0.2    # 最小价格差异
  max_slippage_percent: 0.15     # 最大允许滑点
  ignore_high_slippage: false    # 是否忽略高滑点
```

### 平仓条件
```yaml
close_conditions:
  condition_type: "any"          # 条件类型
  funding_diff_sign_change: true # 资金费率符号变化时平仓
  min_funding_diff: 0.005       # 最小资金费率差异
  min_profit_percent: 0.1       # 最小利润百分比
  max_loss_percent: 0.3         # 最大损失百分比
```

## 运行指南

### 启动程序
```bash
python run_bot.py
```

### 测试模式
```bash
python run_bot.py --test
```

### 指定配置文件
```bash
python run_bot.py --config path/to/config.yaml
```

## 风险控制

### 仓位管理
- 单个币种最大仓位限制
- 总体仓位限制
- 动态仓位调整

### 滑点控制
- 订单深度分析
- 滑点预估
- 动态滑点限制

### 止损机制
- 固定止损
- 追踪止损
- 时间止损

## 日志系统

### 日志级别
- DEBUG: 调试信息
- INFO: 一般信息
- WARNING: 警告信息
- ERROR: 错误信息

### 日志内容
- 交易执行记录
- 资金费率变化
- 套利机会分析
- 错误和异常信息

## 常见问题

1. **如何处理API连接失败？**
   - 检查网络连接
   - 验证API密钥
   - 查看错误日志

2. **如何优化套利策略？**
   - 调整资金费率差异阈值
   - 优化开平仓条件
   - 调整交易金额

3. **如何处理高滑点情况？**
   - 增加最小流动性要求
   - 调整最大滑点限制
   - 使用限价单替代市价单

## 更新日志

### 2025-04-09
- 优化界面排序
- 改进滑点控制
- 增加资金费率符号变化检测

### 2025-04-08
- 添加新的平仓条件
- 优化风险管理
- 改进日志系统

### 2025-04-10
- 修复了Hyperliquid API下市价单的问题
- 将原本调用不存在的`market_order`方法改为使用正确的`order`方法
- 更新了限价单的实现，使用更加标准的参数格式
- 确保平仓操作能够正确执行

### 2025-04-12
- 修复了Hyperliquid价格格式错误问题
- 实现了基于配置的tick_size和价格精度调整
- 优化了市价单和限价单的处理逻辑，确保交易能够正确执行
- 改进了日志记录，提高系统可观察性

## 问题修复日志

### 2025-04-10: 修复Hyperliquid市价单下单问题
- **问题**：Hyperliquid平仓失败，日志显示错误"'Exchange' object has no attribute 'market_order'"
- **原因**：代码中使用了不存在的`market_order`和`limit_order`方法
- **解决方案**：
  - 修改了`place_order_sync`方法，将`market_order`和`limit_order`调用替换为正确的`order`方法
  - 使用正确的参数格式：`order_type="market"`和`order_type="limit"`
  - 这种修改适应了Hyperliquid SDK的最新版本
- **文件修改**：`funding_arbitrage_bot/exchanges/hyperliquid_api.py`

### 2025-04-11: 修复Hyperliquid无效订单类型问题
- **问题**：下单时出现"Invalid order type, market"错误
- **原因**：经测试发现Hyperliquid API不支持市价单（Market Order），只支持限价单（Limit Order）
- **解决方案**：
  - 修改了`place_order_sync`方法中的市价单处理逻辑
  - 当用户请求市价单时，使用限价单模拟市价单效果
  - 使用当前市场价格加上一定的滑点（买入+5%，卖出-5%）来确保订单能够快速成交
  - 使用正确的限价单格式：`order_type={"limit": {"tif": "Gtc"}}`
- **文件修改**：`funding_arbitrage_bot/exchanges/hyperliquid_api.py`
- **注意事项**：
  - 所有对Hyperliquid的交易，包括开仓和平仓，现在都使用限价单
  - 系统会自动计算合适的价格，确保类似市价单的快速成交体验
  - 详细的技术文档已添加在`funding_arbitrage_bot/Hyperliquid 订单创建指南.md`中

### 2025-04-12: 修复Hyperliquid价格格式错误问题
- **问题**：下单时出现"Order has invalid price"错误
- **原因**：Hyperliquid对价格格式有严格要求，必须按照币种配置的tick_size和price_precision进行调整
- **解决方案**：
  - 修改了`place_order_sync`方法中的价格处理逻辑
  - 从配置文件中读取每个币种的tick_size和price_precision
  - 按照tick_size调整价格以符合交易所的最小价格变动单位
  - 控制价格的小数位数保持在配置的精度范围内
  - 完善了日志记录，显示价格调整前后的变化
- **文件修改**：`funding_arbitrage_bot/exchanges/hyperliquid_api.py`
- **相关配置**：
  - 交易对的价格精度和tick_size在`config.yaml`的`trading_pairs`部分配置
  - 例如：FARTCOIN的price_precision=3, tick_size=0.001
  - 例如：HYPE的price_precision=3, tick_size=0.001

## 贡献指南

欢迎提交Issue和Pull Request来帮助改进项目。在提交代码前，请确保：
1. 代码符合PEP 8规范
2. 添加适当的测试
3. 更新相关文档

## 许可证

本项目采用MIT许可证。详见LICENSE文件。

## 联系方式

如有问题或建议，请通过以下方式联系：
- 提交Issue
- 发送邮件至：your.email@example.com 