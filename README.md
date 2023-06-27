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
| Port Scan | Done | - [Port Scan - AZFWNetworkRule.json](https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/blob/main/Port%20Scan%20-%20AZFWNetworkRule.json)<BR>※AZFWApplicationRule は 80,443 のみのため、Port Scan を書ける必要無しと判断 |

# 例
**PortScan ルール(標準)**
```sql
let RunTime = 1h;
let StartRunTime = 1d;
let EndRunTime = StartRunTime - RunTime;
let MinimumDifferentPortsThreshold = 100;
let BinTime = 30s;
AzureDiagnostics
| where TimeGenerated between (ago(StartRunTime) .. ago(EndRunTime))
| where OperationName == "AzureFirewallNetworkRuleLog"
| where OperationName == "AzureFirewallApplicationRuleLog" or OperationName == "AzureFirewallNetworkRuleLog"
| parse msg_s with * "from " srcip ":" srcport " to " dsturl ":" dstport
| where isnotempty(dsturl) and isnotempty(srcip)
| summarize AlertTimedCountPortsInBinTime = dcount(dstport) by srcip, bin(TimeGenerated, BinTime), dsturl
| where AlertTimedCountPortsInBinTime > MinimumDifferentPortsThreshold
| extend IPCustomEntity = srcip, URLCustomEntity = dsturl
```

Structured 形式に変換する場合、テーブルが別なので、join するか 2 つのルールに分けるかを確認する必要がある<BR>

**PortScan - AZFWNetworkRule**
```sql
let RunTime = 1h;
let StartRunTime = 1d;
let EndRunTime = StartRunTime - RunTime;
let MinimumDifferentPortsThreshold = 100;
let BinTime = 30s;
AZFWNetworkRule
| where TimeGenerated between (ago(StartRunTime) .. ago(EndRunTime))
| summarize AlertTimedCountPortsInBinTime = dcount(DestinationPort) by SourceIp, bin(TimeGenerated, BinTime), DestinationIp
| where AlertTimedCountPortsInBinTime > MinimumDifferentPortsThreshold
```
