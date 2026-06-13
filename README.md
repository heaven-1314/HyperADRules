# HyperADRules — Clash/Mihomo 转换版

Fork from [Lynricsy/HyperADRules](https://github.com/Lynricsy/HyperADRules)，自动将 Adblock Plus 格式规则转换为 Clash/Mihomo 兼容的域名列表。

## 生成的文件

| 文件 | 来源 | 说明 |
|------|------|------|
| `clash-domains.txt` | `rules.txt` (405k 规则) | 主拦截规则，域名最多 |
| `clash-dns-domains.txt` | `dns.txt` (228k 规则) | DNS 级别拦截 |
| `clash-combined.txt` | 合并去重 | 以上两者合并去重，推荐使用 |

## 使用方式

### Mihomo / Clash Meta / OpenClash

```yaml
rule-providers:
  antiad:
    type: http
    behavior: domain
    format: text
    url: "https://raw.githubusercontent.com/<你的用户名>/HyperADRules/master/clash-combined.txt"
    path: ./rule_provider/antiad.txt
    interval: 86400
```

或在 OpenClash 覆盖脚本中使用：

```bash
ruby_merge_hash "$CONFIG_FILE" "['rule-providers']" \
  "'antiad'=>{'type'=>'http', 'behavior'=>'domain', 'url'=>'https://raw.githubusercontent.com/<你的用户名>/HyperADRules/master/clash-combined.txt', 'path'=>'./rule_provider/antiad.txt', 'interval'=>86400, 'format'=>'text'}"
```

> 💡 国内网络可将 `raw.githubusercontent.com` 替换为 `raw.gitmirror.com` 或 `ghproxy.net` 镜像。

### 在规则中使用

```yaml
rules:
  - RULE-SET,antiad,REJECT
```

## 自动更新

GitHub Actions 每天 UTC 02:00 自动运行转换。你也可以在 Actions 页面手动触发。
