# AzureFirewallRule-Structured-ForSentinel
> 目的
本レポジトリは Microsoft Sentinel で提供されている Azure Firewall のテンプレートルール (Analytics Rule) を　Structured 形式に対応した形式に書き換えた分析ルールサンプルを提供しています。

- AzureFirewall 構造型ファイアウォールログとは
構造化ログ (Structured Log) は、特定の形式で構成されたログデータの一種です。これまでの AzureFirewall ログは AzureDiagnostic テーブルに含まれていたため、5-tuples (送受信 IP, ポート, プロトコル) などの情報は AzureDiagnostic テーブルのカラムをユーザー側で正規化して抽出する作業が必要でした。<BR>
Azure Firewall の構造化ログでは、ネットワークルール、アプリケーションルール専用に Log Analytics ワークスペースのテーブルが作成され、標準で送信/宛先の IP アドレス、プロトコル、ポート番号、ファイアウォールによって実行されたアクションなどの情報が含まれます。また、イベントの時刻や Azure Firewall インスタンスの名前など、さらに多くのメタデータも含まれます。

<img width="872" alt="image" src="https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/assets/55295601/cb247e61-713d-4b1d-8abc-a4ad185763aa">


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
