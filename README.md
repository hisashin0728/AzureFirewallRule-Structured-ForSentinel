# AzureFirewallRule-Structured-ForSentinel
> 目的
本レポジトリは Microsoft Sentinel で提供されている Azure Firewall のテンプレートルール (Analytics Rule) を　Structured 形式に対応した形式に書き換えた分析ルールサンプルを提供しています。

- 通常の AzureFirewall 診断ログクエリー例
```
AzureDiagnostics
| where OperationName == "AzureFirewallApplicationRuleLog" or OperationName == "AzureFirewallNetworkRuleLog"
| parse msg_s with * "from " srcip ":" srcport " to " dsturl ":" dstport
| where isnotempty(dsturl) and isnotempty(srcip)
```

- 新たに提供された Structured 形式のAzureFirewall 診断ログクエリー例
```
AZFWNetworkRule
```
```
AZFWApplicationRule
```

# テンプレートルールリスト

|  テンプレートルール名  |  Status  | リンク |
| ---- | ---- | ---- |
| TI map IP entity to AzureFirewall | TBD | - |
| Several deny actions registered | TBD | - |
| Several deny actions registered | TBD | - |
| Multiple Sources Affected by the Same TI Destination | TBD | - |
| Port Sweep | TBD | - |
| Abnormal Deny Rate for Source IP | TBD | - |
| Abnormal Port to Protocol | TBD | - |
| Port Scan | TBD | - |

# 例
