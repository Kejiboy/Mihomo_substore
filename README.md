# Mihomo Substore

完美实现了阻止 DNS 泄漏和链式代理的 Mihomo 代理配置方案。

## 功能特性

- ✅ DNS 泄漏防护
- ✅ 链式代理支持
- ✅ 自定义规则管理
- ✅ 灵活的代理分组

## 文件说明

| 文件                         | 说明                           |
|----------------------------|------------------------------|
| `grok.yaml`                | Mihomo 主配置文件，包含 DNS、规则、代理组设置 |
| `mihomo配置.json`            | Sub-Store 配置模板，用于订阅管理        |
| `sub-store组合订阅模板.json`     | Sub-Store 组合订阅模板             |
| `custom-proxy.yaml`        | 自定义代理规则                      |
| `custom-direct.yaml`       | 自定义直连规则                      |
| `链式代理.js`                  | 链式代理脚本                       |
| `grok.yaml` / `grok2.yaml` | 备用配置文件                       |

## 使用方法

### 1. 添加订阅

自行添加单条订阅，可通过脚本为节点增加前缀：

```javascript
$server.name = '落地-' + $server.name
```

### 2. 导入 Sub-Store 模板

在 Sub-Store 订阅管理页面导入 `sub-store组合订阅模板.json`

### 3. 导入 Mihomo 配置

在文件管理页面导入 `mihomo配置.json`

### 4. 自定义规则

- `custom-proxy.yaml` - 自定义代理规则（走代理）
- `custom-direct.yaml` - 自定义直连规则（直连）

根据需要编辑这两个文件。

### 5. 自定义 Gist 规则

在 `grok.yaml` 的 `rule-providers` 部分配置自定义 Gist 规则：

```yaml
MyCustomRules:
  type: http
  format: yaml
  behavior: classical
  url: https://gist.githubusercontent.com/your-username/your-gist-id/raw/your-rules.yaml
  path: ./rule-providers/my-custom-rules.yaml
  interval: 86400
```

然后在 `rules` 部分添加规则应用：

```yaml
- "RULE-SET,MyCustomRules,🔗 全局直连"
```

**注意**：Gist 规则格式应为 `DOMAIN-SUFFIX,example.com`，不需要添加代理组后缀。

## 代理组说明

| 代理组         | 说明              |
|-------------|-----------------|
| 🔰 模式选择     | 主选择器            |
| ⚙️ 节点选择     | 普通节点选择          |
| 🕊️ 落地节点    | 落地/家宽节点（支持链式代理） |
| ♻️ 延迟选优     | 自动选择延迟最低的节点     |
| 🚑 故障转移     | 节点故障时自动切换       |
| ⚖️ 负载均衡(散列) | 一致性哈希负载均衡       |
| ☁️ 负载均衡(轮询) | 轮询负载均衡          |
| 🔗 全局直连     | 直连模式            |
| 🐬 自定义直连    | 自定义直连规则         |
| 🐳 自定义代理    | 自定义代理规则         |

## 链式代理

落地节点（名称包含"落地"或"家宽"）会自动通过普通节点进行链式代理，实现更稳定的连接。

## 配置更新

所有规则和配置文件都支持自动更新，更新间隔为 86400 秒（24 小时）。
