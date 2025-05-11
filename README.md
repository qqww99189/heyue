# 网红签约平台系统设计方案

## 1. 系统概述

本项目是一个面向东南亚市场的网红签约平台，支持多语言、多币种，提供网红资源管理、任务发布与合作、MCN 机构对接等功能。

## 2. 核心功能模块

### 2.1 用户认证系统

- 登录：用户名 + 密码 + 图形验证码
- 注册功能：
  - 用户名（实时检测重复）
  - 密码（实时强度检测）
  - 二次验证密码
  - 支付密码
  - 货币类型选择
  - 图形验证码

### 2.2 多语言支持

- 中文简体
- 中文繁体
- 英语
- 日语
- 韩语
- 马来语
- 泰语
- 越南语

### 2.3 多币种支持

```javascript
const currencies = [
  { code: "CNY", name: "人民币", symbol: "¥" },
  { code: "TWD", name: "新台币", symbol: "NT$" },
  { code: "HKD", name: "港币", symbol: "HK$" },
  { code: "MOP", name: "澳门币", symbol: "MOP$" },
  { code: "MYR", name: "马来西亚令吉", symbol: "RM" },
  { code: "THB", name: "泰铢", symbol: "฿" },
  { code: "VND", name: "越南盾", symbol: "₫" },
  { code: "SGD", name: "新加坡元", symbol: "S$" },
  { code: "IDR", name: "印尼盾", symbol: "Rp" },
  { code: "PHP", name: "菲律宾比索", symbol: "₱" },
  { code: "MMK", name: "缅甸元", symbol: "K" },
  { code: "KHR", name: "柬埔寨瑞尔", symbol: "៛" },
  { code: "LAK", name: "老挝基普", symbol: "₭" },
  { code: "JPY", name: "日元", symbol: "¥" },
  { code: "KRW", name: "韩元", symbol: "₩" },
];
```

### 2.4 主页功能

- 滚动通知公告
- 自动切换宣传海报
- 网红排名展示
  - 头像
  - 名字
  - 粉丝数
  - 月收益
  - 排名
- MCN 公司排名
  - 机构头像
  - 名称
  - 地区
  - 签约人数
  - 机构等级(S/A/B/C)
- 任务展示区
  - 展示 10 个热门任务
  - 任务类型（如 TIKTOK 创作）
  - 粉丝要求
  - 预算收入
  - 任务进度

### 2.5 个人中心

- 滚动通知
- 账户余额
  - 可用余额
  - 待收余额
  - 冻结金额
- 合作状态
  - 合作中数量
  - 等待合作数量
  - 完成合作数量
- 银行账户绑定
  - 国家选择（基于注册货币）
  - 银行选择
  - 银行账号
  - 开户人姓名

### 2.6 客服系统

- 悬浮可拖动按钮
- 用户信息显示
  - 注册用户名
  - 用户 IP
  - 设备类型
  - 地区
  - 来源
- 实时对话功能

## 3. 技术架构

### 3.1 前端技术栈

- Vue3 + Vue Router + Pinia
- Element Plus UI 库
- WebSocket 实时通信
- i18n 国际化方案

### 3.2 后端技术栈

- NestJS + TypeORM
- MySQL (核心数据)
- MongoDB (日志系统)
- Redis (缓存层)

### 3.3 数据库设计

```sql
-- 用户表
CREATE TABLE users (
  id VARCHAR(36) PRIMARY KEY,
  username VARCHAR(50) UNIQUE,
  password VARCHAR(100),
  payment_password VARCHAR(100),
  currency VARCHAR(3),
  created_at TIMESTAMP
);

-- 银行账户表
CREATE TABLE bank_accounts (
  id VARCHAR(36) PRIMARY KEY,
  user_id VARCHAR(36),
  country VARCHAR(2),
  bank_code VARCHAR(20),
  account_no VARCHAR(50),
  account_name VARCHAR(100),
  FOREIGN KEY (user_id) REFERENCES users(id)
);

-- 任务表
CREATE TABLE tasks (
  id VARCHAR(36) PRIMARY KEY,
  title VARCHAR(200),
  platform VARCHAR(50),
  min_followers INT,
  budget DECIMAL(15,2),
  currency VARCHAR(3),
  status VARCHAR(20),
  created_at TIMESTAMP
);
```

## 4. 安全措施

- 密码加密存储
- 图形验证码防刷
- 数据库字段加密
- 操作日志记录

## 5. 管理后台功能

- 用户管理
  - 用户搜索/筛选
  - 货币修改
- 银行数据管理
  - 银行数据导入
  - 字段验证规则
- 任务管理
- 财务审核
- 系统监控
