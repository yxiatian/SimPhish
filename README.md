# SimPhish

**企业安全意识培训工具** | Enterprise Security Awareness Tool

> 一个用于企业内部安全意识培训和授权测试的钓鱼邮件模拟系统

---

## 目录

- [项目简介](#项目简介)
- [功能特性](#功能特性)
- [技术架构](#技术架构)
- [快速开始](#快速开始)
- [配置说明](#配置说明)
- [使用指南](#使用指南)
- [API 文档](#api-文档)
- [数据管理](#数据管理)
- [重要声明与免责](#重要声明与免责)
- [贡献](#贡献)
- [联系方式](#联系方式)

---

## 项目简介

SimPhish 是一个专为企业内部安全意识培训设计的钓鱼邮件模拟平台。通过模拟真实的钓鱼攻击场景，帮助组织提高员工的安全意识，识别和防范网络钓鱼威胁。

### 核心价值

- **教育导向**：通过模拟真实攻击场景，提升员工安全意识
- **安全可控**：仅在授权范围内使用，确保测试环境安全
- **数据追踪**：详细记录打开、点击、提交等行为数据
- **灵活定制**：支持自定义邮件模板、钓鱼页面和变量替换

---

## 功能特性

### 邮件管理
- **模板编辑**：支持 HTML 和富文本编辑器
- **变量替换**：内置多种变量（邮箱、姓名、链接、二维码等）
- **附件支持**：可添加邮件附件（最多 10 个，单个最大 10MB）
- **SMTP 配置**：支持多种加密方式（SSL/TLS、STARTTLS）
- **发送控制**：支持并发控制和速率限制
<img width="1545" height="892" alt="image" src="https://github.com/user-attachments/assets/22d4c61f-b22e-4167-b1ee-12b67c7565de" />


### 钓鱼页面
- **页面编辑**：支持自定义 HTML 内容或从 URL 导入
- **变量支持**：页面内容支持变量替换
- **服务器配置**：可配置独立的钓鱼服务器地址和端口
- **重定向**：支持提交后重定向到指定 URL
<img width="1545" height="893" alt="image" src="https://github.com/user-attachments/assets/00098e51-da8a-4ab2-af9b-3da87ad8e7ed" />
<img width="1547" height="891" alt="image" src="https://github.com/user-attachments/assets/e269f49d-7822-439b-b7b8-196fefc4fe05" />


### 目标管理
- **批量导入**：支持 CSV 文件批量导入目标
- **自定义变量**：为每个目标设置自定义变量
- **追踪 ID**：自动为每个目标生成唯一追踪 ID
<img width="1546" height="893" alt="image" src="https://github.com/user-attachments/assets/1942b84a-ed73-4f82-a7f8-07e9b8f5da4e" />


### 活动管理
- **活动创建**：创建钓鱼邮件活动，关联模板、页面和目标
- **发送任务**：异步发送邮件，实时查看发送进度
- **数据统计**：查看打开率、点击率、提交率等统计信息
- **时间线**：查看每个目标的完整交互时间线
- **数据导出**：支持导出为 HTML、PDF 或 Excel 格式
<img width="2518" height="1506" alt="88bfcb95fe0baf8dd8f5a9758011c2b9" src="https://github.com/user-attachments/assets/284390c2-af08-443d-aa94-aacec689ef0a" />
<img width="2536" height="1528" alt="31e6619581f61bd2eabec7d46b61413c" src="https://github.com/user-attachments/assets/2724810e-9035-4511-812b-ceb5325f5e9e" />

### Webhook 集成
- **事件通知**：支持邮件发送、打开、点击、提交等事件
- **自定义 URL**：可配置多个 Webhook 接收地址
<img width="1547" height="891" alt="image" src="https://github.com/user-attachments/assets/1d61adcb-d5da-4ef1-b5ac-e623bba02eb9" />


---

## 技术架构

### 后端
- **语言**：Go 1.25+
- **框架**：Gin Web Framework
- **存储**：JSON 文件存储（无需数据库）
- **认证**：JWT Token 认证
- **邮件**：gomail (SMTP)
- **二维码**：go-qrcode
- **PDF 导出**：ChromeDP (headless Chrome)

### 前端
- **框架**：React 19
- **构建工具**：Vite
- **UI 库**：Radix UI + Tailwind CSS
- **富文本编辑**：TipTap
- **图表**：Recharts
- **路由**：React Router v7

### 运行时目录结构

运行可执行文件后，会在可执行文件所在目录自动创建 `data/` 目录，用于存储所有配置和项目数据：

```
simphish/                    # 可执行文件所在目录
├── simphish                 # 可执行文件（Linux/macOS）
│   # 或 simphish.exe (Windows)
│
└── data/                    # 数据目录（首次运行自动创建）
    ├── config/              # 系统配置文件
    │   ├── admin.hash       # 管理员密码哈希
    │   ├── jwt.secret       # JWT 认证密钥
    │   └── disclaimer.accepted  # 免责声明接受标记
    │
    └── <project-id>/        # 项目数据目录（每个项目一个）
        │                    # 例如：63ccc5df-0310-4466-8f96-89b7dd8914be/
        ├── project_details.json      # 项目基本信息
        ├── email_config.json          # 邮件服务器配置
        ├── targets.json               # 目标列表
        ├── templates.json             # 邮件模板列表
        ├── phishing_pages.json        # 钓鱼页面列表
        ├── campaigns.json             # 活动列表
        ├── campaign_targets_<campaign-id>.json  # 活动目标记录
        ├── clicks_<campaign-id>.json            # 点击事件记录
        ├── opens_<campaign-id>.json             # 打开事件记录
        └── credentials_<campaign-id>.json       # 提交凭证记录
```

**说明**：
- `data/` 目录在首次运行时自动创建
- 所有数据以 JSON 格式存储，便于备份和迁移
- 每个项目有独立的目录，以项目 ID 命名
- 活动相关的数据文件以活动 ID 作为文件名的一部分

---

## 快速开始

### 系统要求

- **操作系统**：Linux、macOS、Windows
- **内存**：建议 512MB 以上
- **磁盘**：建议 100MB 以上可用空间

### 使用预编译版本

1. **下载二进制文件**
   ```bash
   # 从 releases 下载对应平台的二进制文件
   # Linux: simphish-linux-amd64
   # macOS: simphish-darwin-amd64 或 simphish-darwin-arm64
   # Windows: simphish-windows-amd64.exe
   ```

2. **运行程序**
   ```bash
   # Linux/macOS
   chmod +x simphish
   ./simphish
   
   # Windows
   simphish.exe
   ```

3. **访问界面**
   - 打开浏览器访问：`http://localhost:5173`
   - 默认管理员账号：`admin`
   - 默认密码：`woaixiatian`（首次运行后请立即修改）

---

## 配置说明

### 追踪链接随机化说明

- 每个活动都会生成 **随机的追踪路径 `trackPath` 和查询参数名 `trackParam`**，示例：`https://yourhost.com/ab12?x=aB3xK9mL`。
- 追踪 ID 采用短随机字符串（非 Base64），链接更短且更难被指纹识别。
- 发送时使用活动已持久化的 `trackPath`/`trackParam`；解析端会扫描所有 query 值并匹配追踪 ID，无需固定参数名。
- 钓鱼页、全局打开/点击、二维码、邮件链接和像素均复用该随机路径与参数名，保持一致。
- **不兼容旧的 `rid`/`id` 参数**：历史邮件中的旧链接将无法解析。

### 环境变量

| 变量名 | 说明 | 默认值 |
|--------|------|--------|
| `PORT` | 服务端口 | `5173` |
| `ALLOWED_ORIGINS` | CORS 允许的来源（逗号分隔） | 无 |
| `LOGIN_RATE` | 登录速率限制（次数） | `5` |
| `LOGIN_WINDOW_SECONDS` | 登录时间窗口（秒） | `60` |
| `PUBLIC_PAGE_RATE` | 公开页面访问速率限制 | `60` |
| `PUBLIC_SUBMIT_RATE` | 公开提交速率限制 | `60` |
| `PUBLIC_TRACK_RATE` | 追踪请求速率限制 | `120` |
| `SUBMIT_MAX_BYTES` | 提交数据最大字节数 | `1048576` (1MB) |
| `ENVIRONMENT` | 运行环境（development/production） | `development` |

### 数据目录管理

程序会在可执行文件所在目录自动创建 `data/` 目录用于存储所有数据。详细的目录结构请参考[运行时目录结构](#运行时目录结构)部分。

#### 切换数据目录

目前数据目录固定在可执行文件所在目录。如需切换：

1. **方法一：符号链接**（推荐）
   ```bash
   # 备份现有数据
   mv data data-backup
   
   # 创建符号链接指向新位置
   ln -s /path/to/new/data data
   ```

2. **方法二：移动数据目录**
   ```bash
   # 停止服务
   # 移动数据目录
   mv data /path/to/new/location
   
   # 创建符号链接
   ln -s /path/to/new/location/data data
   ```

---

## 使用指南

### 1. 初始设置

#### 修改管理员密码

1. 登录系统（默认账号：`admin`，默认密码：`woaixiatian`）
2. 进入"设置"页面
3. 点击"修改密码"
4. 输入旧密码和新密码

#### 配置邮件服务器

1. 创建或选择一个项目
2. 进入"邮件配置"页面
3. 填写 SMTP 服务器信息：
   - **主机**：SMTP 服务器地址
   - **端口**：SMTP 端口（如 25、465、587）
   - **加密方式**：选择 SSL/TLS、STARTTLS 或无
   - **用户名/密码**：SMTP 认证信息
   - **发件人名称/地址**：显示的发件人信息
4. 点击"测试连接"验证配置
5. 保存配置

### 2. 创建邮件模板

1. 进入"邮件模板"页面
2. 点击"新建模板"
3. 填写模板信息：
   - **名称**：模板名称
   - **主题**：邮件主题（支持变量）
   - **内容类型**：选择 HTML 或富文本
   - **内容**：邮件正文（支持变量）
4. 添加变量（可选）：
   - 在"变量"区域输入变量名
   - 点击"添加"按钮
5. 添加附件（可选）：
   - 点击"添加附件"
   - 选择文件（最大 10MB）
6. 点击"保存"

#### 支持的变量

**目标信息变量**
- `{{.email}}` - 目标邮箱地址
- `{{.firstName}}` - 目标名字
- `{{.lastName}}` - 目标姓氏

**功能变量**（需要配置 Hosting URL）
- `{{.phishingLink}}` 或 `{{.link}}` - 钓鱼链接
- `{{.trackingId}}` - 追踪 ID
- `{{.trackingPixel}}` - 邮件打开追踪像素（隐藏图片）
- `{{.qrCode}}` - 二维码图片（自动生成）

**时间变量**
- `{{.year}}` - 当前年份（4位）
- `{{.month}}` - 当前月份（2位）
- `{{.day}}` - 当前日期（2位）
- `{{.date}}` - 当前日期（YYYY-MM-DD）
- `{{.hour}}` - 当前小时（2位）
- `{{.minute}}` - 当前分钟（2位）
- `{{.second}}` - 当前秒数（2位）

**自定义变量**
- 在目标中设置的自定义变量，使用 `{{.变量名}}` 格式

> **注意**：发送邮件时，系统会自动从所选钓鱼页面的服务器配置中获取 Hosting URL。如果未配置，相关变量（phishingLink、trackingPixel、qrCode）将为空。

### 3. 创建钓鱼页面

1. 进入"钓鱼页面"页面
2. 点击"新建页面"
3. 填写页面信息：
   - **名称**：页面名称
   - **URL**：页面 URL（可选）
   - **内容来源**：选择"自定义"或"从 URL 导入"
   - **内容**：HTML 内容（支持变量）
4. **配置钓鱼服务器**（重要）：
   - **Hosting URL**：外部可访问的服务器地址（如 `https://yourdomain.com`）
   - **Hosting Port**：服务器端口（如 `9090`）
   - 这些信息将用于生成追踪链接和二维码
5. 设置重定向 URL（可选）
6. 点击"保存"

### 4. 添加目标

#### 单个添加

1. 进入"目标"页面
2. 点击"新建目标"
3. 填写目标信息：
   - **邮箱**：必填
   - **名字/姓氏**：可选
   - **自定义变量**：可选，键值对形式
4. 点击"保存"

#### 批量导入

1. 进入"目标"页面
2. 点击"导入 CSV"
3. 准备 CSV 文件，格式：
   ```csv
   email,firstName,lastName,customVar1,customVar2
   user1@example.com,张,三,部门,IT
   user2@example.com,李,四,部门,HR
   ```
4. 粘贴 CSV 内容或上传文件
5. 点击"导入"

### 5. 创建活动

1. 进入"活动"页面
2. 点击"新建活动"
3. 填写活动信息：
   - **名称**：活动名称
   - **邮件模板**：选择已创建的模板
   - **钓鱼页面**：选择已创建的页面
   - **目标**：选择要发送的目标（可多选）
4. 点击"保存"

### 6. 发送活动

1. 在活动列表中，找到要发送的活动
2. 点击"发送"按钮
3. 确认发送（系统会提示将使用所选钓鱼页面的服务器地址）
4. 等待发送完成，可查看发送进度

### 7. 查看结果

1. 进入活动详情页面
2. 查看统计信息：
   - 发送数量
   - 打开数量/率
   - 点击数量/率
   - 提交数量/率
3. 查看目标列表：查看每个目标的状态
4. 查看时间线：查看每个目标的完整交互记录
5. 导出数据：支持导出为 HTML、PDF 或 Excel

---

## API 文档

### 认证

所有 API 请求（除登录和健康检查外）需要在 Header 中携带 JWT Token：

```
Authorization: Bearer <token>
```

### 主要端点

#### 认证
- `POST /api/auth/login` - 登录
- `POST /api/auth/logout` - 登出
- `GET /api/auth/me` - 获取当前用户信息
- `POST /api/auth/change-password` - 修改密码

#### 项目
- `GET /api/projects` - 获取项目列表
- `POST /api/projects` - 创建项目
- `GET /api/projects/:id` - 获取项目详情
- `PUT /api/projects/:id` - 更新项目
- `DELETE /api/projects/:id` - 删除项目
- `GET /api/projects/:id/dashboard` - 获取仪表板统计

#### 邮件配置
- `GET /api/projects/:id/email-config` - 获取邮件配置
- `POST /api/projects/:id/email-config` - 保存邮件配置
- `POST /api/projects/:id/email-config/test` - 测试邮件连接

#### 模板
- `GET /api/projects/:id/templates` - 获取模板列表
- `POST /api/projects/:id/templates` - 创建模板
- `PUT /api/projects/:id/templates/:templateId` - 更新模板
- `DELETE /api/projects/:id/templates/:templateId` - 删除模板

#### 钓鱼页面
- `GET /api/projects/:id/phishing-pages` - 获取页面列表
- `POST /api/projects/:id/phishing-pages` - 创建页面
- `PUT /api/projects/:id/phishing-pages/:pageId` - 更新页面
- `DELETE /api/projects/:id/phishing-pages/:pageId` - 删除页面

#### 目标
- `GET /api/projects/:id/targets` - 获取目标列表
- `POST /api/projects/:id/targets` - 创建目标
- `PUT /api/projects/:id/targets/:targetId` - 更新目标
- `DELETE /api/projects/:id/targets/:targetId` - 删除目标
- `POST /api/projects/:id/targets/import-csv` - 导入 CSV

#### 活动
- `GET /api/projects/:id/campaigns` - 获取活动列表
- `POST /api/projects/:id/campaigns` - 创建活动
- `GET /api/projects/:id/campaigns/:campaignId` - 获取活动详情
- `PUT /api/projects/:id/campaigns/:campaignId` - 更新活动
- `DELETE /api/projects/:id/campaigns/:campaignId` - 删除活动
- `POST /api/projects/:id/campaigns/:campaignId/send` - 发送活动
- `GET /api/projects/:id/campaigns/:campaignId/send-status` - 获取发送状态
- `GET /api/projects/:id/campaigns/:campaignId/export?format=xlsx` - 导出活动数据

---

## 部署

### Docker 部署（示例）

```dockerfile
FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY simphish .
EXPOSE 5173
CMD ["./simphish"]
```

### 系统服务部署

#### Linux (systemd)

创建服务文件 `/etc/systemd/system/simphish.service`：

```ini
[Unit]
Description=SimPhish Service
After=network.target

[Service]
Type=simple
User=your-user
WorkingDirectory=/path/to/simphish
ExecStart=/path/to/simphish/simphish
Restart=always
RestartSec=10
Environment="PORT=5173"

[Install]
WantedBy=multi-user.target
```

启用并启动服务：

```bash
sudo systemctl daemon-reload
sudo systemctl enable simphish
sudo systemctl start simphish
```

---

## 数据管理

### 备份

```bash
# 备份整个数据目录
cp -r data data-backup-$(date +%Y%m%d)

# 或使用 tar 压缩
tar -czf data-backup-$(date +%Y%m%d).tar.gz data/
```

### 恢复

```bash
# 停止服务
# 恢复数据目录
cp -r data-backup-20250101/* data/

# 或解压备份
tar -xzf data-backup-20250101.tar.gz
```

### 迁移

数据目录包含所有项目数据，可以直接复制到新服务器使用。

---

## 重要声明与免责

### 使用目的

**SimPhish 仅用于以下合法目的：**

1. **企业内部安全意识培训**
2. **授权范围内的安全测试**
3. **安全研究和教育**

### 禁止用途

**严禁将本工具用于以下任何目的：**

1. **任何非法活动**
2. **未经授权的网络攻击**
3. **真实的钓鱼诈骗**
4. **恶意软件分发**
5. **任何违反法律法规的行为**

### 免责声明

1. **用户责任**：使用者需对使用本工具的所有行为承担全部责任。任何滥用本工具的行为均由使用者自行承担法律后果。

2. **作者免责**：作者不对任何滥用本工具的行为承担责任。作者不保证本工具不会被用于非法目的，也不对因使用本工具而产生的任何损失、损害或法律后果负责。

3. **无担保**：本工具按"现状"提供，不提供任何明示或暗示的担保，包括但不限于对适销性、特定用途的适用性和非侵权性的担保。

4. **风险提示**：使用本工具存在被误判为垃圾邮件的风险。建议：
   - 配置 SPF、DKIM、DMARC 记录
   - 使用信誉良好的邮件服务器
   - 控制发送频率和数量
   - 确保收件人已授权接收测试邮件

5. **合规要求**：在使用本工具前，请确保：
   - 已获得所有相关方的明确授权
   - 遵守当地法律法规
   - 遵守组织的安全政策
   - 已告知收件人这是安全意识培训邮件

### 使用条款

通过使用本工具，您确认：

- 已阅读、理解并同意遵守上述所有条款
- 仅将本工具用于合法和授权的目的
- 对使用本工具的所有行为承担全部责任
- 不会将本工具用于任何非法或恶意目的

**违反上述条款可能导致法律后果，作者不承担任何责任。**

---

## 贡献

欢迎提交问题报告和改进建议。但在提交代码前，请确保：

1. 代码符合项目规范
2. 已通过所有测试
3. 已更新相关文档

---

## 联系方式

- **作者**：xiatian
- **项目**：SimPhish - Enterprise Security Awareness Tool
<img src="https://github.com/user-attachments/assets/255cebc2-f35d-4fdf-ae62-e3b080b0bfa7" width="350" alt="微信二维码">
---

## 更新日志

### v1.3.0
- 支持二维码变量自动生成
- 优化邮件模板变量系统
- 改进发送任务管理
- 增强数据导出功能
- 修复重复发送问题
- 优化钓鱼页面服务器配置集成
- 初始版本发布
- 基础功能实现

---

**最后更新**：2025-12-15

---

> **再次提醒**：请务必遵守法律法规，仅将本工具用于合法和授权的目的。任何滥用行为均由使用者自行承担全部责任。

